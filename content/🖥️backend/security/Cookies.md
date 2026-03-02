
>[!Overview]
>- A small piece of data a server sends to a user's web browser
>- Cookies enable web applications to store limited amounts of data and remember state information; by default the HTTP protocol is [stateless](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Overview#http_is_stateless_but_not_sessionless)
>	- [[Authentication & Authorization#Maintaining authentication state|Authentication & Authorization - Maintaining authentication state]]
>- When login succeeds, the server generates a **session ID** and sends it to the client via a **cookie**. For every subsequent request, the server uses this cookie to look up the session and **restore the user’s identity**.
    
- **Components**
	- Session store (in-memory, Redis, database)
	- cookie settings (`HttpOnly`, `Secure`, `SameSite`)
- **Advantages**
	- Relatively simple to implement. Since the server controls sessions, it allows **immediate forced logout** (by deleting the session).
- **Disadvantages**
	- Requires the server to maintain state. This means **consistency across storage** must be ensured when scaling (shared storage is essential).
- Best sources 
	- [MDN - Using HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies)
# Basic User sign-in system
1. The user sends sign-in credentials to the server, for example via a form submission.
2. If the credentials are correct, the server updates the UI to indicate that the user is signed in, and responds with a cookie containing a session ID that records their sign-in status on the browser.
3. At a later time, the user moves to a different page on the same site. The browser sends the cookie containing the session ID along with the corresponding request to indicate that it still thinks the user is signed in.
4. The server checks the session ID and, if it is still valid, sends the user a personalized version of the new page. If it is not valid, the session ID is deleted and the user is shown a generic version of the page (or perhaps shown an "access denied" message and asked to sign in again).
- Diagram - login
	![[user-sign-in-cookie.png|600]]
- Diagram - 1st request (login) + 2nd request
	![[session.png]]

# Request & Response example
## Server response 
- After receiving an HTTP request, a server can send one or more [`Set-Cookie`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Set-Cookie) headers with the response, each one of which will set a separate cookie.
	- A cookie is set by specifying a name-value pair like: `Set-Cookie: <cookie-name>=<cookie-value>`
- The following HTTP response instructs the receiving browser to store a pair of cookies:
```http
HTTP/2.0 200 OK
Content-Type: text/html
Set-Cookie: yummy_cookie=chocolate
Set-Cookie: tasty_cookie=strawberry

[page content]
```
## Future client requests
- When a new request is made, now the browser usually sends previously stored cookies for the current domain back to the server within a [`Cookie`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Cookie) HTTP header:
```http
GET /sample_page.html HTTP/2.0
Host: www.example.org
Cookie: yummy_cookie=chocolate; tasty_cookie=strawberry
```
# Cookie structure
- General format: `Name=Value`
- Attributes are separated by semicolons (`;`) and can be either `Key=Value` pairs or flags.
Example:
```
Set-Cookie: SESSIONID=abc123;
Path=/;
Domain=example.com;
Max-Age=1800;
HttpOnly; Secure;
SameSite=Lax
```
# Cookie Attributes 
> It's usually the **server** that sends a `Set-Cookie` header in its HTTP response

- Backend developers configure those cookie attributes in the server-side code

| Attribute                       | Meaning                          | Key Points                                                                                         |
| ------------------------------- | -------------------------------- | -------------------------------------------------------------------------------------------------- |
| **Name=Value**                  | Cookie name and value            | Values cannot contain spaces, semicolons, or other special characters. Use URL encoding if needed. |
| **Max-Age**                     | Time-to-live (seconds)           | Takes precedence over `Expires` if present.                                                        |
| **Expires**                     | Expiration time (absolute)       | Must be a standardized GMT date string.                                                            |
| **Secure**                      | Sent only over HTTPS             | Required for sensitive cookies.                                                                    |
| **HttpOnly**                    | Blocks JavaScript access         | Mitigates XSS. Strongly recommended for authentication cookies.                                    |
| **Domain**                      | Domain scope for sending cookies | If not set, cookie is host-only. If set, applies to the specified domain and all subdomains.       |
| **Path**                        | Restricts request path           | Matches by prefix. Commonly set to `/`.                                                            |
| **SameSite**                    | Controls cross-site transmission | Default is `Lax`. For SSO or external redirects, use `None` with `Secure`.                         |
| **Priority** (browser-specific) | Hint for removal priority        | Affects eviction order when storage limits are exceeded.                                           |
## Expiration
- ***Permanent cookies*** are deleted after the date specified in the `Expires` attribute or the period specified in the `Max-Age` attribute:
	- `Max-Age` is less error prone than `Expires`
```
Set-Cookie: id=a3fWa; Expires=Thu, 31 Oct 2021 07:28:00 GMT;
Set-Cookie: id=a3fWa; Max-Age=2592000
```
- ***Session cookies*** DON'T have a `Max-Age` or `Expires` attribute. They're deleted when the current section ends!
## Security
- Ensure that cookies are sent securely and aren't accessed by unintended parties or scripts
- `Secure`
	- Cookies are only sent to the server with an encrypted request over the *HTTPS* protocol
		- If client's request is NOT HTTPS then the cookie is not sent back to client
		- Insecure sites (with `http:` in the URL) can't set cookies with the `Secure` attribute
	- It can still be unsafe - someone with access to the client's hard disk (or JavaScript if the `HttpOnly` attribute isn't set) can read and modify the information
- `HttpOnly`
	- Cookie can't be accessed by [[JavaScript]], for example using `Document.cookie` - It can only be accessed when it reaches the server.
	- Cookies that persist user sessions for example should have the `HttpOnly` attribute set — it would be really insecure to make them available to JavaScript
```
Set-Cookie: id=a3fWa; Expires=Thu, 21 Oct 2021 07:28:00 GMT; Secure; HttpOnly
```
## Locations (where cookies are sent)
- The `Domain` and `Path` attributes define the _scope_ of a cookie: what URLs the cookies are sent to
- `Domain`
	- specifies which server can receive a cookie -> If specified, cookies are available on the specified server and its subdomains
		- a server can only set the `Domain` attribute to its own domain or a parent domain, not to a subdomain or some other domain
		- If the `Set-Cookie` header does not specify a `Domain` attribute, the cookies are available on the server that sets it *but not on its subdomains*
	- Example
		- If a server at `login.example.com` sets a cookie with `Domain=example.com`, your browser will send that cookie to any subdomain like `www.example.com`, `store.example.com`, or `login.example.com`.
		- If you don't set it, the cookie will only be sent back to the _exact_ domain that set it (e.g., `login.example.com` only, not `www.example.com`).
- `Path` 
	- dictates which URL paths _within_ a domain should receive the cookie (more specific than `Domain`)
	- indicates a URL path that must exist in the requested URL in order to send the `Cookie` header
```
// domain
Set-Cookie: id=a3fWa; Expires=Thu, 21 Oct 2021 07:28:00 GMT; Secure; HttpOnly; Domain=mozilla.org

// path
Set-Cookie: id=a3fWa; Expires=Thu, 21 Oct 2021 07:28:00 GMT; Secure; HttpOnly; Path=/docs
```
## Controlling 3rd party cookies w/ `SameSite`
- Types of requests
	- **Same-Site Request:** You are browsing `yourbank.com` and click a link that takes you to another page on `yourbank.com`. You're navigating within the same website.
	- **Cross-Site Request:** You are on `social-media.com` and click a link that takes you to `yourbank.com`. You are navigating from one site to another.
- `SameSite`
	- lets servers specify whether/when cookies are sent with *cross-site requests* — i.e., [third-party cookies](https://developer.mozilla.org/en-US/docs/Web/Privacy/Guides/Third-party_cookies).
	- helps to prevent leakage of information
- Has 3 values: `Strict`, `Lax`, `None`
	- `Strict`
		- causes the browser to only send the cookie in response to *requests originating from the cookie's origin site*
		- should be used when you have cookies relating to functionality that will always be behind an initial navigation, such as authentication or storing shopping cart information
	- `Lax` (default)
		- similar to `Strict`, except the browser also sends the cookie when the user _navigates_ to the cookie's origin site (even if the user is coming from a different site)
		- Detailed explanation/example from Gemini 🦄
			- You are logged into your favorite coffee shop's website, `DailyGrind.com`. Your browser has a cookie for it that says "User=You", and that cookie is marked `SameSite=Lax`.
			- Now, go to a totally different site, a blog called `FoodReviews.com`. That blog has a link: "Click here to see the new espresso at `DailyGrind.com`!"
			- When you click that link:
				1. **The Action:** You click a link on `FoodReviews.com` that takes you to `DailyGrind.com`. This is a **cross-site, top-level navigation** (the URL in your address bar is changing).
				2. **The Browser's Decision:** Before sending the request, your browser looks at the cookies it has for `DailyGrind.com`. It finds your "User=You" login cookie.
				3. **The Rule Check:** The browser checks the rule on the cookie. The rule is `SameSite=Lax`.
				4. **The `Lax` Permission:** The `Lax` rule says, "You can send me along if the user is clicking a main link to get here."
				5. **The Result:** ✅ Your browser **sends the cookie** with the request. When you arrive at `DailyGrind.com`, the site immediately recognizes you and says, "Welcome back!" You are already logged in.
	- `None`
		- cookies are sent on both originating and cross-site requests -> intentionally _allowing_ cookies to be sent in a cross-site context
		- The main purpose is to allow services that are *embedded inside other websites to use their own cookies*.
			- Common use cases
				- ***Embedded Content (like YouTube videos)***
					- You embed a YouTube video on your blog (`myblog.com`).
				    -  When a user visits your blog, the embedded YouTube player needs to talk to `youtube.com`. If the user is logged into YouTube, the player might need to access YouTube's login cookie (`SameSite=None`) to know their viewing history, language preferences, or whether they have YouTube Premium (so it doesn't show ads).
				- ***Advertising and Analytics***
					- An ad network needs to show personalized ads on thousands of different websites
					- The ad network uses a cookie (`SameSite=None`) to track a user's advertising ID. When you visit `news-site.com`, the embedded ad from `ad-network.com` can access this cookie to know what ads you've seen before or what your interests might be
		- Note that if `SameSite=None` is set then the `Secure` attribute must also be set — `SameSite=None` requires a _secure context_
```
// strict
Set-Cookie: cart=110045_77895_53420; SameSite=Strict

// lax
Set-Cookie: affiliate=e4rt45dw; SameSite=Lax

// none
Set-Cookie: widget_session=7yjgj57e4n3d; SameSite=None; Secure; HttpOnly
```

# Cookie Prefixes
- Cookie prefixes are special keywords at the beginning of a cookie's name that act as a security contract, forcing the browser to enforce strict rules on that cookie
- All cookie prefixes start with a double-underscore (`__`) and end in a dash (`-`)
- `__Secure-`
	- Must be set with the `Secure` attribute 
	- Must be set from a secure page (HTTPS)
- `__Host-`
	- Must be set with the `Secure` attribute from a secure page (HTTPS)
	- Must NOT have a `Domain` attribute
	- Must have `Path` attribute set to `/`
	- Provides strongest origin isolation -> guarantees that such cookies are only sent to the host that set them
- `__Http-`
	- Must be set with the `Secure` flag from a secure page (HTTPS)
	- Must have the `HttpOnly` attribute set -> Proves the cookie was set via `Set-Cookie` header (not JavaScript)
		- Cannot be modified via `Document.cookie` or Cookie Store API
- `__Host-Http-`
	- Combines all restrictions from `__Host-` and `__Http-` prefixes
	- Must be secure, `HttpOnly`, Path="/", no Domain attribute
	- Provides maximum security and origin isolation while ensuring HTTP-only scope