
| SWE                                                    | ML systems                                                                                                                                                           |
| ------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Underlying assumption that code and data are separated | They are part code, part data, and part artifacts created from the two                                                                                               |
| Only focus on testing and versioning code              | Same, but ALSO DATA<br>- version datasets because you need to know which data are more valuable than others (just getting more data blindly often is data poisoning) |

Other challenge: 
- Size of ML models
	- 100s to billions of parameters which requires GBs of RAM to load into memory
	- Getting these large models in production, especially on [[On Device AI|edge devices]], is a **massive engineering challenge**.
- Speed
	- An autocompletion [[model]] is useless if it can't even get autocomplete in seconds
- Lack of visibility
	- debugging these models.... + lack of visibility into their work
	- hard to figure out what went wrong