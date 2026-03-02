##### Setup
- frontend
```bash
npm create vite@latest dynamocards
#select React > JavaScript
cd dynamocards
npm install

#testing vite
npm run dev
```
- backend
	- Create a [[Virtual environment]]
- download requirements
- unicorn main:app --reload

##### Backend
In `backend/main.py`:
- @app.post("/analyze_video")
	def analyze_video(request: VideoAnalysisRequest):
- Call YoutubeProcessor class from `genai.py`
In `backend`, make `genai.py` and `__init__.py`
- In genai.py
	- YoutubeProcessor class
		- `retrieve_youtube_documents` function
			- `YoutubeLoader.from_youtube_url` receives a youtube link (we use pydantic to check the format) and gets its transcripts (subtitles). 
			- We use `RecursiveCharacterTextSplitter` to split the received input into chunks. 
		- `find_key_concepts` function
			- 
	- GeminiProcessor class
		- `generate_documents_summary` function:Summarize documents using LangChain's [[load_summarize_chain#load_summarize_chain|load_summarize_chain]]
		- `count_total_tokens` function
		- `get_model` function

##### Using middleware
In `backend/main.py`:
- Use FastAPI's [[Fast API#Middleware|middleware]]: `CORSMiddleware`-> Processes the requests and responses as they pass through the application. Configures your backend to accept requests from **different origins**, like your frontend
	- frontend (npm run dev) goes to `http://localhost:5173/`
	- backend (unicorn main:app --reload) goes to `http://127.0.0.1:8000`
In `frontend/App.jsx`
- empty the file
- install axios
	- axios ->The frontend uses `axios` to send a POST request with this YouTube link to the backend API endpoint (e.g., `/analyze_video`).
- Create a frontend 
	- App() using state hooks
	- handles user input link
	- returns the metadata for the link from the backend