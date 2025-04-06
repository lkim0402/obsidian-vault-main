> A cloud platform offering various AI and machine learning services.

### Google Cloud SDK
![[Pasted image 20240628162134.png]]

1. Google Cloud CLI
	- Command line tools you can access and manage your cloud from the command line
2. Cloud Client Libraries
	- Client libraries for programmatically integrating Google Cloud APIs into your code
3. Tools and documentation


- [[Grounding an LLM]]

### Creating a new project
1. Add new project in https://console.cloud.google.com/
2. Menu > VertexAI > Dashboard > Enable all recommended APIs
3. Menu > IAM Admin > Service Account > Create a service account
	- Role: Give Owner (don't share anywhere, just keep it)
	- After creation, click on `keys` and add key in `JSON` (download to your computer)
4. Download Google Cloud SDK (if you haven't)
5. Setting the environment
```bash
# Authenticating gcloud
export GOOGLE_APPLICATION_CREDENTIALS="/path/to/your/service-account-key.json"

gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS

# for verification
gcloud auth list

nano ~/.bashrc 
#adding this to ~/.bashrc
export GOOGLE_APPLICATION_CREDENTIALS="/path/to/your/service-account-key.json"

# for reloading and applying changes
source ~/.bashrc
```
