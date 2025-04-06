```python
#ConversableAgent.py
import os
from autogen import ConversableAgent
from dotenv import load_dotenv

load_dotenv()

model="gpt-3.5-turbo"
llm_config = {
    "model": model,
    "api_key" : os.getenv("OPENAI_API_KEY"),
}

agent = ConversableAgent(
    name="Simple_agent",
    llm_config=llm_config,
    code_execution_config=False
    )

message = [{"role":"user", "content":"Tell me a funny joke!"}]
response = agent.generate_reply(messages=message)

print(response)
```