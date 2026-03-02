- Other type of pagination is [[Cursor-based pagination (Spring)]]
>[!Overview]
>Querying large amounts of data at once can significantly impact performance. Spring Data JPA provides built-in **Paging** and **Sorting** features to enable efficient data retrieval.

- Pagination
	- overall technique itself (concept)
	- the general idea of splitting into pages
- Key terminologies
	- **`Sort`**: An interface that encapsulates sorting information.
	- The request (offset/limit)
		- **`Pageable`**: An interface that encapsulates pagination information.
	- The response (choose which to return)
		- **`Page`**: A **result set** that includes total page information (like the total number of elements).
		- **`Slice`**: A partial page result; used to check if a next page exists with `hasNext()`.
# `Pageable` interface (`PageRequest.of()`)
>[!Overview]
>An interface that holds all the necessary information for *pagination*, including the *page number, page size, and sorting details*.

- You can easily create a `Pageable` instance using the static method **`PageRequest.of()`**.
	- We use `PageRequest.of()` because we don't need to manually create an implementation like `new PageableImpl()`, `PageRequest` is officially supported by [[🐣Spring & SpringBoot|Spring]], and has good readability
- It uses a **zero-based page index** (the first page is page 0).
## Key Methods

| Method              | Description                                                                           |
| ------------------- | ------------------------------------------------------------------------------------- |
| `getPageNumber()`   | Returns the current page number (starts from 0).                                      |
| `getPageSize()`     | Returns the number of items per page.                                                 |
| `getSort()`         | Returns the `Sort` information.                                                       |
| `next()`            | Returns the `Pageable` for the next page.                                             |
| `previousOrFirst()` | Returns the `Pageable` for the previous page, or the first page if on the first page. |
## Example
```java
Pageable pageable = PageRequest.of(0, 10, Sort.by("username").descending());
// PageRequest.of() 메서드를 사용해서 페이징과 정렬 정보를 가진 Pageable 객체 생성
// 첫 번째 파라미터: 조회할 페이지 번호 (0부터 시작)
// 두 번째 파라미터: 한 페이지에 보여줄 데이터 개수
// 세 번째 파라미터: 정렬 기준과 방향을 지정하는 Sort 객체
// - Sort.by("username") : username이라는 필드를 기준으로 정렬
// - .descending() : 내림차순 정렬 (username이 Z → A 순서)
//
// 결국, "username 필드를 기준으로 내림차순 정렬된 데이터 중 0번 페이지(첫 번째 페이지)에서 10개 데이터를 가져와라"라는 의미

Pageable pageable = PageRequest.of(0, 10);
// "Give me page 0, with 10 items per page"
// - page: 0 (first page)  
// - size: 10 (10 items per page)
```
### Example Result

**Assumption:** The following `username` values exist in this order: `["zoo", "yuri", "alice", "bob"]`
```json
["zoo", "yuri", "alice", "bob"]
```
**Sorting Condition:** `username` descending.
```json
[“zoo”, “yuri”, “bob”, “alice”]
```
**Pagination Condition:** Page 0, with a size of 10 items.
```java
[“zoo”, “yuri”, “bob”, “alice”] // (최대 10개지만 데이터가 4개뿐)
```
# `Sort` interface
>[!Overview]
>The **`Sort`** interface is used to explicitly define sorting conditions. It allows you to specify sorting rules for multiple fields.
>- You can sort by multiple fields at once using **`Sort.by()`**.
>- You can specify whether the order should be **ascending** or **descending**.

```java
Sort sort = Sort.by(
    Sort.Order.asc("username"),
    Sort.Order.desc("age")
);
```
This creates a sort order that is first by `username` **ascending**, and then by `age` **descending**.

| Method         | Description |
| -------------- | ----------- |
| `Sort.by()`    | 오름차순 기본     |
| `Order.asc()`  | 오름차순 지정     |
| `Order.desc()` | 내림차순 지정     |

# `Page` interface
>[!Overview]
>A **`Page`** is a result set used when you need the data for the current page **and** the total count of all available data.

- **Key Characteristic**
	- It executes an extra `COUNT` query to get the *total number of elements* (`totalElements`) and *calculate the total number of pages* (`totalPages`). 
	- This can have a performance cost with large datasets.
- **Best For**:
    - Admin dashboards or management screens where you need to display total counts (e.g., "Showing 1-10 of 123 entries").
    - Traditional pagination UIs that show page numbers (e.g., `<< 1 2 3 ... 10 >>`).
    - Search results where the user needs to know the total number of found items.
#### Example
```java
// The repository method returns a Page
Page<Member> page = memberRepository.findAll(pageable);

// You can get the content for the current page
List<Member> content = page.getContent();

// And you can get total counts
int totalPages = page.getTotalPages();
long totalElements = page.getTotalElements();
```
# `Slice` interface
>[!Overview]
>A **`Slice`** is a simpler result set used when you only need to know if there is a **next page** of data available. It does **not** know the total count.

- **Key Characteristic**
	- More performant than `Page`  because it **does not execute an extra `COUNT` query**
- **How it Works**
	- Internally, it requests `page size + 1` items from the database. If it receives that extra item, it knows a next page exists and sets `hasNext()` to `true`.
- **Best For**:
    - **Infinite scrolling** in mobile or web applications where you load more data as the user scrolls down.
    - Implementing a **"Load More"** button.
#### Slice = One Page of Results
```java
Slice<Message> slice = repository.findAll(pageable);
// Contains:
// - content: List of actual Message objects (up to 10)
// - hasNext: boolean (are there more pages?)
// - pageable: the original request info
```

```java
// The repository method returns a Slice
Slice<Member> slice = memberRepository.findByAgeGreaterThan(20, pageable);

// You can get the content for the current page
List<Member> content = slice.getContent();

// You can only check if a next page exists
boolean hasNext = slice.hasNext();
```
# Example with [[⭐Layered Architecture#`@Repository`|Repository]]
*Note: We're talking about the repository layer WITHOUT the `@Repository` annotation but w/ [[Spring Data JPA]]*
```java
public interface MemberRepository extends JpaRepository<Member, Long> {
    Page<Member> findByAgeGreaterThan(int age, Pageable pageable);
    Slice<Member> findByUsernameContaining(String username, Pageable pageable);
}
```

Example usage
```java
Pageable pageable = PageRequest.of(0, 5, Sort.by("username").ascending());
Page<Member> result = memberRepository.findByAgeGreaterThan(20, pageable);
List<Member> members = result.getContent();
```

# Practical Tips
- **`Page`** requires an extra **count query**. Use it only when you need the total number of pages.
- **`Slice`** is suitable for "Load More" or infinite scroll features. It does not run a count query and only checks if a next page exists.
- Specify **`Sort`** conditions explicitly. For optimal performance, you need to consider your database **indexes** and how they align with the sorting order.
- A **`PageRequest`** object is **immutable**, so it can be safely reused.
- Choose the return type of your repository method (`List`, `Page`, or `Slice`) based on the specific needs of the use case.
```java
// 예시: 페이징 요청 재사용
Pageable pageable = PageRequest.of(0, 10, Sort.by("createdAt").descending());
```

# Page VS Slice

| Interface      | Role                              | Key Methods / Features                     |
| -------------- | --------------------------------- | ------------------------------------------ |
| **`Pageable`** | Transmits pagination information. | `of()`, `getPageNumber()`, `getPageSize()` |
| **`Sort`**     | Transmits sorting information.    | `by()`, `Order.asc()`, `Order.desc()`      |
| **`Page`**     | Returns the full page result.     | `getContent()`, `getTotalPages()`          |
| **`Slice`**    | Returns a partial page result.    | `getContent()`, `hasNext()`                |
