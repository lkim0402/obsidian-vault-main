```python
import os
from autogen import UserProxyAgent, AssistantAgent
from dotenv import load_dotenv

load_dotenv()

model="gpt-3.5-turbo"
llm_config = {
    "model": model,
    "api_key" : os.getenv("OPENAI_API_KEY"),
}

assistant = AssistantAgent(name="assistant", llm_config=llm_config)
user_proxy = UserProxyAgent(
    name="user_proxy", 
    llm_config=llm_config,
    code_execution_config={
        "work_dir":"code_execution",
        "use_docker": False
    },
    human_input_mode="NEVER"
)

# Starting the convo
user_proxy.initiate_chat(
    recipient=assistant,
    message="What is the capital of France?"
)
```

```sh
//output
user_proxy (to assistant):
The capital of France is Paris.
----------------------------------------------------------------------------
assistant (to user_proxy):
Great! The capital of France is indeed Paris.
TERMINATE
----------------------------------------------------------------------------
user_proxy (to assistant):
If you have any more questions in the future, feel free to ask. Have a great day, goodbye! 
----------------------------------------------------------------------------
assistant (to user_proxy):
Thank you! Have a fantastic day! Goodbye!
TERMINATE
----------------------------------------------------------------------------
user_proxy (to assistant):
Goodbye!
----------------------------------------------------------------------------
assistant (to user_proxy):
TERMINATE
----------------------------------------------------------------------------
```