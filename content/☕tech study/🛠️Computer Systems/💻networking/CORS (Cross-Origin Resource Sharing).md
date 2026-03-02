
>[!Definition]
>A **security feature** built into web browsers that allows applications from one _origin_ (a domain) to access resources (like data or [[APIs]])  from a different _origin_ 

- [[HTTP Fundamentals]]
- [See more - Codeit Notion Notes](https://calm-individual-12a.notion.site/CORS-225c6b70982881108425cb3a25a70490)
- In Spring, you can implement this through Spring Security
# Same-Origin policy (SOP) & CORS
- By default, browsers enforce a "*same-origin policy*" 
	- Only allows to send requests to the *same origin* (Prevents a script on `your-website.com` from making a request to `another-api.com`)
	- A crucial defense against malicious attacks
- However, modern web applications often need to pull resources from various external sources => CORS creates a safe way to do this
	- CORS = a mechanism that *allows servers to explicitly permit cross-origin requests* that would *otherwise be forbidden by the browser's default security policy*.
- Without CORS
	- The danger was that any website could secretly make requests to any other website you were logged into, using your credentials to steal data or perform actions
		- defends against security vulnerabilities like **XSS (Cross-Site Scripting)** and **CSRF (Cross-Site Request Forgery)**.
	- CORS provides the necessary safety checks to prevent this while still allowing for modern, interconnected web applications.
- **CORS** is the exception to the SOP that allows for the use of external resources
# What is Origin?
>Protocol + Host + Port
- If any of these three parts are different between two applications, they are considered to have different origins, or be "*cross-origin*."
- Check the origin of the current page by typing `location.origin` into the browser's developer console
- Example
	![[origin-example-url.png]]
# How CORS works
> These 2 are examples of interaction between the browser and the server when [[JavaScript]] requests an [[APIs|API]]
## Simple Request
![[simple-request.png]]
- They DON'T trigger a CORS preflight -> *involves sending a request directly to the server*
	- A simple request asks the server for an API, and the server sends a response back to the browser that includes the `Access-Control-Allow-Origin` header. 
	- The browser then checks this header to determine whether to proceed with the CORS operation
- Not used that much anymore
- request header - `Origin` 
- response header - `Access-Control-Allow-Origin`
#### Conditions
1. The request **method** must be one of `GET`, `HEAD`, or `POST`.
2. Headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, `DPR`, `Downlink`, `Save-Data`, `Viewport-Width`, and `Width` must *NOT* be used.
	- excludes the `Authorization` header, which is commonly used for user authentication
3. The `Content-Type` header must be one of `application/x-www-form-urlencoded`, `multipart/form-data`, or `text/plain`.
	- difficult to meet since [[REST API|REST APIs]] use `application/json` as their `Content-Type`
## Preflight Request ⭐
>[!Definition]
>A method where a *preliminary request* is sent to the server to determine if it's safe before sending the main request. 
>- `PUT`, `PATCH`, `DELETE`
- Preflight diagram 1
	![Diagram of a request that is preflighted](https://mdn.github.io/shared-assets/images/diagrams/http/cors/preflight-correct.svg)
- Preflight diagram 2
	![[preflight-diagram2.png]]
- Preflight diagram 3
	![[preflight-diagram3.png]]
- My diagram lol
	![[preflight-request-mydiagram.png|800]]
- A sanity check using the `OPTIONS` method
	-  `OPTIONS` method determines whether to send the actual request before requesting the real resource
- If you have ever requested an API using methods like `GET`, `POST`, `PUT`, or `DELETE` and noticed a request being sent with the `OPTIONS` method in the Chrome Developer Tools' network tab, you have experienced CORS
- Preflight caching exists (read MDN docs for more)
#### CORS Preflight Headers

| Request Header (Browser → Server)                                                                                                                               | Response Header (Server → Browser)                                                                                                                     |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **`Origin`**  <br>The origin (domain) that is making the request.                                                                                               | **`Access-Control-Allow-Origin`**  <br>- The origin that is allowed to access the resource. <br>- Must match the `Origin` header or be `*` (wildcard). |
| **`Access-Control-Request-Method`**  <br>- Asks for permission for the HTTP method that the actual request will use (e.g., `PUT`, `DELETE`).                    | **`Access-Control-Allow-Methods`**  <br>- Specifies the methods (e.g., `GET`, `PUT`, `DELETE`) that are allowed for cross-origin requests.             |
| **`Access-Control-Request-Headers`**  <br>- Asks for permission for the non-standard HTTP headers that the actual request will include (e.g., `Authorization`). | **`Access-Control-Allow-Headers`**  <br>- Specifies the headers that are allowed in the actual cross-origin request.                                   |
- The browser checks the preflight response and its `Access-Control-Allow-Origin` and see if it matches the `Origin`
	- if it doesn't match, the response
#### Other Important CORS Response Headers
These headers are sent by the server to provide additional rules.

| Response Header                        | Purpose                                                                                                                     |
| -------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **`Access-Control-Allow-Credentials`** | When set to `true`, it allows cookies and other credentials to be included in cross-origin requests.                        |
| **`Access-Control-Expose-Headers`**    | Specifies a *list of headers* from the server response that the *browser should expose to the client-side JavaScript code*. |
| **`Access-Control-Max-Age`**           | Indicates how *long the results of a preflight request can be cached* by the browser, avoiding another preflight check.     |

#### Example - The preflight request/response
```http
OPTIONS /doc HTTP/1.1
Host: bar.other
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Connection: keep-alive
Origin: https://foo.example
Access-Control-Request-Method: POST
Access-Control-Request-Headers: content-type,x-pingother

HTTP/1.1 204 No Content
Date: Mon, 01 Dec 2008 01:15:39 GMT
Server: Apache/2
Access-Control-Allow-Origin: https://foo.example
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
Access-Control-Max-Age: 86400
Vary: Accept-Encoding, Origin
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
```
- request (1st block) - *client*
	- has `OPTIONS` method
	- request headers 
		- `Origin: https://foo.example`
		- `Access-Control-Request-Method: POST` 
			- tells the server what the actual request method is
		- `Access-Control-Request-Headers: content-type,x-pingother`
			- tells the server what custom headers the actual request will have
- response (2nd block) - *server*
	- request headers
		- `Access-Control-Allow-Origin: https://foo.example`
			- the server restricts access to the requesting origin domain only
			- `Access-Control-Allow-Origin: *`
		- `Access-Control-Allow-Methods` 
			- tells which are valid  methods to query the resource in question
			- in our ex it says  `POST` and `GET` are valid methods 
		- `Access-Control-Allow-Headers` 
			- headers allowed within request  
		- [`Access-Control-Max-Age`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Access-Control-Max-Age) 
			- gives the value in seconds for how long the response to the preflight request can be cached *without sending another preflight request*
- **Once the preflight request is complete, the real request is sent!**
#### Example - the actual request + response
```http
POST /doc HTTP/1.1
Host: bar.other
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Connection: keep-alive
X-PINGOTHER: pingpong
Content-Type: text/xml; charset=UTF-8
Referer: https://foo.example/examples/preflightInvocation.html
Content-Length: 55
Origin: https://foo.example
Pragma: no-cache
Cache-Control: no-cache

<person><name>Arun</name></person>

HTTP/1.1 200 OK
Date: Mon, 01 Dec 2008 01:15:40 GMT
Server: Apache/2
Access-Control-Allow-Origin: https://foo.example
Vary: Accept-Encoding, Origin
Content-Encoding: gzip
Content-Length: 235
Keep-Alive: timeout=2, max=99
Connection: Keep-Alive
Content-Type: text/plain

[Some XML content]

```
# STEPS
1. Browser sends request ([[JavaScript]])
	- When your browser makes a request (e.g., an API call from JavaScript), it automatically adds an **`Origin`** header that indicates where the request is coming from.
2. Browser checks origin
	- If that request goes to the same origin, then request proceeds as normal ✅
	- If that request goes to a different origin, its a *cross-origin request* (the CORS pathway)
The CORS pathway 
- If the request is a simple request
	1. **Browser → Server:** Sends the actual request with the `Origin` header.
	2. **Server → Browser:** The server processes the request. If it allows the origin, it includes the header `Access-Control-Allow-Origin` in its response.
	3. **Browser:** Checks if the server's response header matches the request's origin. If it matches, the request succeeds. If not, the browser blocks the response, causing a CORS error.
- If the request is a preflight request
	- request is more complex (e.g., uses methods like `PUT` or `DELETE`, or has custom headers like `Authorization`), the browser sends a "preflight" request first to ask for permission.
	1. **Preflight (Browser → Server):** The browser sends a preliminary **`OPTIONS`** request to ask if the actual request is allowed.	    
	2. **Preflight Response (Server → Browser):** The server responds, telling the browser which origins, methods, and headers it accepts.
	3. **Browser Decision:** The browser checks the preflight response. (checks if everything match)	    
	    - **If permitted:** The browser sends the **actual request** (e.g., the `PUT` request).
	    - **If not permitted:** The browser stops and throws a CORS error. The actual request is never sent.
# CORS error
> Can be REAL PAIN... Do these if you have a CORS error!
- Open the network tab > Response Header > look for the `Access-Control-Allow-Origin`
	- If this doesn't exist you should enable CORS on the server
	- If it does exist, the URL maybe a mismatch with the current website
	- If it was preflighted, it may not be allowing those methods/headers in the request (you should configure in the server)

# Sources
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS 👍
- fireship vid - https://www.youtube.com/watch?v=4KHiSt0oLJ0
- https://aws.amazon.com/what-is/cross-origin-resource-sharing/?nc1=h_ls