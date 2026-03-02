- when u type a web address into the browser address bar
	1. The browser does to the DNS (domain name server) and looks up the IP address of the server of the web address
	2. The browser sends request to that IP address, asking it to send a copy of the website to the client
		- this msg and all other data send between client and server is sent using TCP/IP
	3. If the server approves the client's request, server sends `200 OK` msg then starts sending the website's files as **packets**
	4. The browser gets these packets and assembles them into small chunks to a complete web page & displays it
- **packets**
	- `header` - includes details of server/client's IP address, packet number, total num of packets, details of protocols used in the transmission
	- `payload` - actual data
- components of **url**
	- URL - uniform resource locators
	- `https://developer.mozilla.org/en-US/` -> URL
		- `https` 
			- the protocol (https is a secure version of http)
		- `developer.mozilla.org` 
			- the domain name of the URL (the top lvl location of the server u are connecting to)
			- `developer` part is a *subdomain*
		- `/en-US/`
			- the path to the resource on the server that you are accessing
- **web standards**
	- [mdn docs article about it (interesting)](https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Web_standards/The_web_standards_model)
	- The technologies we use to build websites
		- the standards exist as specifications (long technical documents, details exactly how the technology should work)
	- Created by standards bodies -> institutions that invite groups of ppl from diff tech companies & agree on how the tech should work in the best way
		- W3C, WHATWG, etc
	- key principles
		- open -> free to both contribute & use
		- accessible & interoperable -> built for everyone (including those w/ disabilities etc), and work consistently
		- don't break the web -> any new web technology should be backwards compatible
	- overview
		- [[HTML]], [[CSS]], [[JavaScript]]
	- tools
		- Developer tools
		- Testing tools
		- Frameworks and libraries on top of javascript
		- linters, formatters
# source
- [mdn's web standards section](https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Web_standards)