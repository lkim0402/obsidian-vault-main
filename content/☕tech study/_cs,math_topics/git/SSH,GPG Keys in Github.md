- Both use public key cryptography

# The concept: Public/Private key
- A method for securely passing data from one place to another
- Provides a very high level of confidence that the data is coming from the place it should be coming from
- An alternative: `HTTP/HTTPS`, but they can be inconvenient because we have to keep providing our username and pw periodically to Github to authenticate
# The implementation: SSH/GPG Keys
- SSH and GPG are both **specific implementations/tools** that use this concept (the children)
	- They both use public/private key pairs, but are both different tools for different jobs
- ***SSH (Secure Shell)***
	- Purpose is to secure **authentication** and **communication** with remote servers
	- It's required for pushing/pulling code without entering your password constantly
		- When you `git push` to GitHub using SSH, your SSH key proves "yes, this is really you" without needing to type your password
	- "Let me in to do work"
- ***GPG (GNU Privacy Guard)*** - *Optional*
	- Purpose is for**signing** and **encrypting** data to prove authenticity and maintain privacy
	- Signing your Git commits so people know they genuinely came from you and weren't modified by someone else
	- "I guarantee that I made this"
	- **You don't need this** unless you care about:
	    - Proving cryptographically that YOU made those commits
	    - Contributing to projects that require signed commits
	    - Having that verified checkmark for credibility

- [Github docs - Connecting to Github with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)