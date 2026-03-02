# AWS SDK (for Java 2.x)
>[!Overview]
>A library that provides a convenient and high-level API for interacting with AWS services like S3, EC2, DynamoDB, etc., directly from your Java application. 
>- Like a translator that lets your [[🐣Spring & SpringBoot|Spring Boot]] code speak the language of AWS APIs without you needing to handle the low-level HTTP requests and authentication details manually

- **Purpose**: Simplifies connecting to and using AWS services.
- **Key Feature**: Provides service-specific client objects (e.g., `S3Client`, `DynamoDbClient`) to perform operations.
- **Version**: We're focusing on **v2**, which is the modern, recommended version. It features non-blocking I/O (`S3AsyncClient`) and improved performance over v1.
# Service-specific clients
- **`S3Client`**: Manages **Objects** in **Buckets**. Its methods are named accordingly: `putObject`, `getObject`, `deleteObject`.
- **`DynamoDbClient`**: Manages **Items** in **Tables**. Its methods are different: `putItem`, `getItem`, `deleteItem`, `query`.
- **`SQSClient`**: Manages **Messages** in **Queues**. Its methods include `sendMessage` and `receiveMessage`.
- This **Client -> Request -> Action -> Response** pattern is the same everywhere, but the names of the actions and the parameters in the requests are completely different for each service
# S3Client
>[!Overview]
>The `S3Client` is the primary interface in the AWS SDK v2 for all synchronous (blocking) operations with Amazon S3 (Simple Storage Service). If you need to upload a photo, download a config file, or list all files in a folder (bucket), the `S3Client` is the tool you'll use.

- Imagine `S3Client` is your dedicated remote control for your S3 storage. Each button on the remote corresponds to an action. 🎮
### Core Operations
- **`putObject`**: Uploads a file/object to an S3 bucket.
- **`getObject`**: Downloads an object from a bucket.
- **`deleteObject`**: Deletes an object from a bucket.
- **`listObjectsV2`**: Lists all objects within a bucket.
- **`createBucket`**: Creates a new S3 bucket.
# Key Concepts / Best Practices
- **Client Lifecycle**: Always create `S3Client` as a **singleton** bean. It's thread-safe and designed for reuse. Creating a new client for every request is inefficient as it wastes resources setting up connection pools.
- **Synchronous vs. Asynchronous**:
    - `S3Client`: **Synchronous**. Your code thread will block and wait until the S3 operation (e.g., upload) is complete. Good for most standard web request-response cycles.
    - `S3AsyncClient`: **Asynchronous**. Returns a `CompletableFuture`. Use this in reactive applications (like with Spring WebFlux) to avoid blocking threads and improve scalability.
- **Exception Handling**: Always wrap SDK calls in a `try-catch` block and handle `S3Exception` or the more general `SdkClientException`.
- **Immutable Clients**: SDK v2 clients are immutable. To change a setting (like the region), you must build a new client.

# Spring Boot Code Example
#### `monew.env` (a portion)
```env
# Application Configuration  
STORAGE_TYPE=s3  
AWS_ACCESS_KEY_ID=*
AWS_SECRET_ACCESS_KEY=*
AWS_REGION=ap-northeast-2  
  
# S3  
AWS_S3_BUCKET=monew-file-storage  
AWS_S3_PRESIGNED_URL_EXPIRATION=600
```

#### `application.yml` (a portion)
```YAML
spring:  
  profiles:  
    active: dev  
  
  config:  
    import: optional:file:.env[.properties], optional:file:monew.env[.properties]

monew:  
  backup:  
    s3:  
      access-key: ${AWS_ACCESS_KEY_ID}  
      secret-key: ${AWS_SECRET_ACCESS_KEY}  
      region: ${AWS_REGION}  
      bucket: ${AWS_S3_BUCKET}  
      presigned-url: ${AWS_S3_PRESIGNED_URL_EXPIRATION}
```

#### `S3Properties`
```java
import org.springframework.boot.context.properties.ConfigurationProperties;  
  
@ConfigurationProperties(prefix = "monew.backup.s3")  
public record S3Properties(  
    String accessKey,  
    String secretKey,  
    String region,  
    String bucket,  
    long presignedUrl  
) {  
}
```

#### `AwsConfig`
```java
@Configuration  
@EnableConfigurationProperties(S3Properties.class)  
public class AwsConfig {  
  
  /**  
   * S3Client 생성  
   */  
  @Bean  
  public S3Client s3Client(S3Properties s3Properties) {  
    // return S3Client.builder()  
    //    .region(Region.of(s3Properties.region()))  
    //    .credentialsProvider(  
    //        StaticCredentialsProvider.create(  
    //            AwsBasicCredentials.create(s3Properties.accessKey(), s3Properties.secretKey())  
    //        )  
    //    )  
    //    .build();  
    return S3Client.builder().build();
  }  
}
```
- Automatic search of credentials
	- When you call `S3Client.builder().build()` without explicitly providing a region or credentials, the AWS SDK automatically searches for them in a specific order. 
	- Since your `monew.env` file exports `AWS_REGION`, `AWS_ACCESS_KEY_ID`, and `AWS_SECRET_ACCESS_KEY` as environment variables, the SDK finds and uses them automatically.
- You create the `S3Client` as a single bean (`@Bean`) shared across the entire application
	- Objects created by `.build()` (like `S3Client` and `PutObjectRequest`) are **immutable** -> Once created, their state cannot be changed
	- Because it's immutable, you can safely use this single client instance in multiple threads simultaneously without any risk. One thread can't accidentally change the client's region while another thread is in the middle of an upload

#### `BasicArticleBackupService`
```java
@Slf4j  
@Service  
@RequiredArgsConstructor  
public class BasicArticleBackupService implements ArticleBackupService {  
  
  private final S3Client s3Client;  
  private final S3Properties s3Properties;  
  
  private final ObjectMapper objectMapper = new ObjectMapper();  
  
  @Override  
  public void backupToS3(String articleJson){  
  
    try {  
      Article article = objectMapper.readValue(articleJson, Article.class);  
      log.info("[뉴스 기사] 백업 시작 - articleId ={}", article.getId());  
  
      String contentType = "application/json";  
      String bucket = s3Properties.bucket();  
  
      PutObjectRequest putObjectRequest = PutObjectRequest.builder()  
          .bucket(bucket)  
          .key(article.getId().toString() + ".json")  
          .contentType(contentType)  
          .build();  
  
      s3Client.putObject(  
          putObjectRequest,  
          RequestBody.fromString(articleJson)  
      );  
  
    } catch (S3Exception e) {  
      log.error("[뉴스 기사] 백업 오류 (S3) : {}", e.awsErrorDetails().errorMessage());  
  
    } catch (Exception e) {  
      log.error("[뉴스 기사] 백업 오류 : ", e);  
    }  
  }  
}
```

- `PutObjectRequest`
	- the "instruction form" for uploading a file
	- You are _building_ a request object that contains all the metadata (information _about_ the file and where it should go)
- A **key** is the label you put on a specific file inside a drawer (the bucket)
	- It's the unique **name** or "**path**" for your file _within that specific bucket_.
	- **How it works:** While a bucket can't contain another bucket, the key can use slashes (`/`) to create a folder-like structure. For example, a key could be `2025/09/news-article-123.json`.