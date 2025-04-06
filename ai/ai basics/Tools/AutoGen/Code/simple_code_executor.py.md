```
pip install matplotlib numpy
```

```python
import os
from autogen import AssistantAgent, UserProxyAgent
from dotenv import load_dotenv

load_dotenv()

model="gpt-3.5-turbo"
llm_config = {
    "model": model,
    "api_key" : os.getenv("OPENAI_API_KEY"),
}

assistant = AssistantAgent(
    llm_config=llm_config,
    name="assistant"
)

user_proxy = UserProxyAgent(
    name="user",
    human_input_mode="ALWAYS",
    code_execution_config={
        "work_dir": "coding",
        "use_docker": False
    }
)

user_proxy.initiate_chat(
    recipient=assistant,
    message="Plot a chart of GOOG and NFLX stock price change."
)
```