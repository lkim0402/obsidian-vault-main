
>[!Overview]
>A **presigned URL** is a temporary and secure link to a file in a private S3 bucket.

- Think of your S3 bucket as a secure, members-only vault. 
	- Normally, only you (your server with secret keys) can open it. 
	- A presigned URL is like a **temporary, single-use keycard** that you can generate and give to a specific person.
- This keycard has three important features:
	1. **It's signed:** It contains a cryptographic signature that proves you authorized it.
	2. **It's specific:** It only works for one action (like `GET` to download or `PUT` to upload) on one specific file.
	3. **It expires:** It's only valid for a short time (e.g., 5 minutes).
- This lets a user's browser directly access a file in your private vault without needing your secret AWS credentials.

# When to use 
## 1. Server-Side Upload (Your `put` method) - NOT USED ❌
In your `put(byte[] bytes)` method, the file is sent _through your server_.
```java
@Override  
public UUID put(UUID binaryContentId, byte[] bytes) {  
  
  S3Client s3Client = getS3Client();  
  String contentType = "application/octet-stream";  
  
  PutObjectRequest putReq = PutObjectRequest.builder()  
      .bucket(bucket)  
      .key(binaryContentId.toString())  
      .contentType(contentType)  
      .build();  
  
  s3Client.putObject(putReq,  
      RequestBody.fromBytes(bytes)  
  );  
  
  return binaryContentId;   
}
```
- **Flow:** Client → **Your Server** → S3
- **Why no presigned URL?** Your server already has the permanent "master key" (your `accessKey` and `secretKey`) to the S3 vault. It acts as a trusted middleman. The client gives the file to your server, and your server puts it in the vault.
## 2. Client-Side Download (Your `download` method) - USED ✅
In your `download()` method, your server's job is to create and hand over a temporary keycard (the presigned URL).
```java
@Override  
public ResponseEntity<?> download(BinaryContentDto metaData) {  
  String key = metaData.id().toString();  
  String presignedUrl = generatePresignedUrl(key, metaData.contentType());  
  return ResponseEntity.status(302).location(URI.create(presignedUrl)).build();  
}

public String generatePresignedUrl(String key, String contentType) {  
  S3Presigner presigner = S3Presigner  
      .builder()  
      .s3Client(getS3Client())  
      .build();  
  
  // download -> GET  
  GetObjectRequest getReq = GetObjectRequest.builder()  
      .bucket(bucket)  
      // tells the presigned URL to instruct S3 to serve the file with the correct Content-Type  
      .responseContentType(contentType)  
      .key(key)  
      .build();  
  
  GetObjectPresignRequest preReq = GetObjectPresignRequest.builder()  
      .getObjectRequest(getReq)  
      .signatureDuration(Duration.ofMinutes(5)) // 유효기간  
      .build();  
  
  return presigner.presignGetObject(preReq).url().toString();  
}
```
- **Flow:** Your Server → Client (with presigned URL) → S3
- **Why a presigned URL?** Your server offloads the heavy work. Instead of downloading the file from S3 and then sending it to the client (which uses twice the bandwidth), it just gives the client the special link. The client's browser then uses that link to download the file **directly from S3**.