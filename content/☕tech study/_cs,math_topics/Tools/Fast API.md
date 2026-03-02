> Mainly notes for myself to not forget

> FastAPI is a modern, high-performance web framework for building APIs with Python 3.7+ that is designed with RESTful principles in mind ([[REST API]])
> Source: https://fastapi.tiangolo.com/tutorial/first-steps/

## Operations
#### GET
- Used to define an endpoint that handles HTTP GET requests
```python
from fastapi import FastAPI

app = FastAPI()

#this is a decorator
@app.get("/")
def read_root():
    return {"message": "Hello World"}

@app.get("/items/{item_id}")
def read_item(item_id: int, q: str = None):
    return {"item_id": item_id, "q": q}
```
- When a client (such as a web browser or another application) sends an HTTP GET request to a FastAPI endpoint, the server processes the request, executes the corresponding handler function, and returns the response to the client.
- `read_root()`
	- When you open your browser and navigate to the root URL of your FastAPI server, it will return a JSON response `{"message": "Hello World"}`.
#### POST
```python
app = FastAPI()

@app.post("/items/")
async def create_item(item: dict):
    return item
```


## Middleware
> - A "middleware" is a function that works with every **request** before it is processed by any specific _path operation_. And also with every **response** before returning it.
> - Not mandatory, but highly recommended and often necessary
> - It **enhances functionality** by providing mechanisms for logging, authentication, validation, error handling, etc
> - https://fastapi.tiangolo.com/tutorial/middleware/?h=middleware

```python 
import time

from fastapi import FastAPI, Request

app = FastAPI()


@app.middleware("http")
async def add_process_time_header(request: Request, call_next):
    start_time = time.time() #can add code before and after handling the request
    
    response = await call_next(request)
    
    process_time = time.time() - start_time
    response.headers["X-Process-Time"] = str(process_time)
    return response
```
CORSMiddleware
- If your FastAPI backend and Vite frontend are running on different ports (or domains), you need to enable CORS on your backend to allow requests from your frontend.
- For example, if your backend is running on `http://127.0.0.1:8000` and your frontend (Vite) is running on `http://127.0.0.1:3000`, they are considered different origins, and CORS needs to be configured.
- https://fastapi.tiangolo.com/tutorial/cors/