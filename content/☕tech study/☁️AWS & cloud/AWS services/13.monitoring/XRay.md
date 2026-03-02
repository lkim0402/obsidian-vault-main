
>[!XRay]
> A service that can be used to collect cross-workload traffic metrics and information.
> - allows users to understand how requests travel through their applications
> - integrates with [[CloudWatch]] where X-Ray data can be analyzed in greater detail

- You have to prepare your apps for tracing with XRay
	- add extra code
	- install a daemon service in the app environment
	- save tracing events with certain code snippets -> the daemon sends that data back to AWS & you can analyze these traces
- analyze traces
	- view traffic flow across applications/services
	- can be combined with other monitoring tools like [[CloudWatch]] for more detailed insight