1. Setting up environment
	1. Create [[Virtual environment]]
		- Install requirements in a text file; I named mine `requirements.txt` which lists:  
			- streamlit  
			- google-cloud-aiplatform  
			- vertexai```
			- `pip install -r requirements.txt`  
	2. Setting the region to one that supports Vertex AI
		- gcloud config set ai/region us-central1
		- gcloud config list (verifying)
2. Downloaded [[Google Cloud]]
	1. `wget https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-cli-linux-x86_64.tar.gz`
	2. `tar -xvf google-cloud-cli-linux-x86_64.tar.gz`
	3. `cd google-cloud-sdk` `./install.sh`
3. Authenticating gcloud
	- `export GOOGLE_APPLICATION_CREDENTIALS="/path/to/your/service-account-key.json"`
	- `gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS`
	- `gcloud auth list` for verification
	- `nano ~/.bashrc` and adding `export GOOGLE_APPLICATION_CREDENTIALS="/path/to/your/service-account-key.json"`
	- `source ~/.bashrc` for reloading and applying changes
1. Code (in github)
2. Enable API (AI Platform, Vertex AI, Service Usage API)
	1. Connect billing information for Vertex AI
	2. Add credentials and create a service account
		1. Add specific roles to the service account
3. Run the application
	`streamlit run gemini_explorer.py`

##### Useful
- Checking all projects: `gcloud projects list`
- Checking current project config: `gcloud config list`





	
