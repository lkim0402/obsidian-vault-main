- [[🐣Spring & SpringBoot]]
# `multipart/form-data`

> A special encoding method that enables a browser to send a request that includes binary file data.
- Unlike the transmission of plain text data, file uploads are sent using a special HTTP request format called `multipart/form-data` (and not the typical `x-www-form-urlencoded`)
- This method is designed to allow *both text data and binary files* to be delivered to the server within a single request.
- A single file can be processed as one `MultipartFile`, while multiple files can be handled as a `List<MultipartFile>`.

>[!note]
>Think of `multipart/form-data` request as a single large shipping box (the HTTP request) that contains several smaller, individually labeled boxes inside (the "parts"). **Each part can have its own content type**.
>- This format is specifically designed to send a mix of different data types at once, like files, text, and JSON

**How Browser File Uploads Work**
1. A user selects a local file using an `<input type="file">` element.
2. The user submits the `<form>`. In response, the browser sets the request's `Content-Type` header to `multipart/form-data`.
3. Each form field and the file data are then sent as separate "parts" of the request body.

# Sample code
Directory Structure
```
src/main/java/com/springboot/coffee
├── controller
│   └── CoffeeController.java
├── dto
│   └── CoffeeImageDto.java
├── templates
│   └── uploadForm.html
│   └── uploadResult.html
```

`CoffeeImageDto.java`
```java
@Getter
@Setter
public class CoffeeImageDto {
    private String coffeeName;
    private String fileName;
}
```

`CoffeeController.java`
```java
@PostMapping("/v1/coffees/upload")
public String uploadCoffeeImage(
	@RequestParam("coffeeName") String coffeeName,
	@RequestParam("image") MultipartFile file,
	Model model) throws IOException {
    
    String fileName = file.getOriginalFilename();
    Path savePath = Paths.get("./uploads/" + fileName);
    Files.createDirectories(savePath.getParent());
    file.transferTo(savePath);

    CoffeeImageDto dto = new CoffeeImageDto();
    dto.setCoffeeName(coffeeName);
    dto.setFileName(fileName);

    model.addAttribute("coffeeName", coffeeName);
    model.addAttribute("fileName", fileName);
    return "uploadResult";
}
```
- The directory where files will be saved is `./uploads/`, and it will be automatically created at the project's root.
# Tips
- For file uploads, you **must** use `enctype="multipart/form-data"` in your [[HTML]] form.
- To prevent file name conflicts on the server, it is recommended to either use a **UUID** to generate a unique file name or to save files into **user-specific directories**.
- An **image preview** feature can be implemented on the front-end using [[JavaScript]].
- Implementing **security measures** is essential. This includes limiting file sizes and validating file extensions.