
>[!Overview] 
>True cursor-based pagination is not supported by the `Pageable` interface's default behavior. 
>- We must implement it manually by passing a "cursor" (like the last-seen `id`) and using it in a custom query. 
>- We can still use `Slice` as a return type to get the `hasNext()` behavior for free.

- Cursor-based pagination is not supported out of the box in Spring, you should write it yourself
- Another way is [[Offset-based Pagination (Spring)]]
# Example
## Strategy: Custom Repository Methods
- Since `Pageable` doesn't support cursors, you have to build the logic yourself. 
- You will **not** use `OFFSET`. You will use a `WHERE` clause with the cursor (e.g., `WHERE id > [lastSeenId]`).
- You **will** still use `Slice` and `Pageable` in a clever way:
	- You'll use `Slice` as the return type because you _only_ care about `hasNext()`.
	- You'll use `Pageable` as an input _only_ to pass the `LIMIT` (page size) and `SORT` information. The `page` number will always be `0`.
```java
public interface PostRepository extends JpaRepository<Post, Long> {

    // --- This is the "first page" method ---
    // No cursor is provided, so it just gets the first page.
    Slice<Post> findFirstByOrderByIdDesc(Pageable pageable);

    // --- This is the "next page" method ---
    // We pass the lastSeenId, and it finds all items "less than" that ID
    // (since we are sorting by ID descending, "less than" is "older").
    Slice<Post> findByIdLessThanOrderByIdDesc(Long lastSeenId, Pageable pageable);
}
```
- Explanation
	- We use `Slice` because we don't need a total count.
	- We use `OrderByIdDesc` to get the newest items first.
	- `findByIdLessThan` becomes the SQL `WHERE id < ?`.
		- Because the method name (`findByIdLessThan`) provides the `WHERE` clause, Spring is smart enough to not add an `OFFSET`.
	- The `Pageable` parameter will be used by Spring _only for the `LIMIT` and `SORT`_. 
- Or, we could use [[QueryDSL]]
## Service
```java
@Service
@RequiredArgsConstructor
public class PostService {

    private final PostRepository postRepository;

    public CursorResponse<Post> getPosts(Long cursor, int limit) {
        
        // 1. Create the Pageable object. 
        // We always ask for page 0, with the given limit, and our sort order.
        Pageable pageRequest = PageRequest.of(0, limit, Sort.by("id").descending());

        // 2. Call the correct repository method
        Slice<Post> postSlice;
        if (cursor == null) {
            // This is the first page request
            postSlice = postRepository.findFirstByOrderByIdDesc(pageRequest);
        } else {
            // This is a subsequent page request
            postSlice = postRepository.findByIdLessThanOrderByIdDesc(cursor, pageRequest);
        }

        // 3. Extract the data and determine the next cursor
        List<Post> posts = postSlice.getContent();
        Long nextCursor = null;
        if (!posts.isEmpty()) {
            // Get the ID of the last item in the list
            nextCursor = posts.get(posts.size() - 1).getId();
        }

        // 4. Build and return our custom response DTO
        return new CursorResponse<>(posts, nextCursor, postSlice.hasNext());
    }
}
```
## Controller
```java
@RestController
@RequestMapping("/api/posts")
@RequiredArgsConstructor
public class PostController {

    private final PostService postService;

    @GetMapping
    public ResponseEntity<CursorResponse<Post>> getPosts(
            @RequestParam(required = false) Long cursor, // The cursor is optional
            @RequestParam(defaultValue = "10") int limit
    ) {
        CursorResponse<Post> response = postService.getPosts(cursor, limit);
        return ResponseEntity.ok(response);
    }
}
```