Using Conversation
```python 
!pip install transformers
from transformers import pipeline
from transformers import Conversation

chatbot = pipeline(
	   task="conversational", 
	   model="./models/facebook/blenderbot-400M-distill"
)

user_message="""
What are some fun winter activities?
"""

conversation = Conversation(user_message)

print(conversation)
"""
Conversation id: a989b9e1-621d-4f54-b5bf-991ad508d45f
user: 
What are some fun activities I can do in the winter?
"""
conversation(chatbot(conversation))

print(conversation)
"""
Conversation id: a989b9e1-621d-4f54-b5bf-991ad508d45f
user: 
What are some fun activities I can do in the winter?

assistant:  I like snowboarding and skiing.  What do you like to do in winter?
"""

#Including prior conversations
conversation.add_message(
    {"role": "user",
     "content": """
		What else do you recommend?
		"""
    })
print(conversation)
"""
Conversation id: a989b9e1-621d-4f54-b5bf-991ad508d45f
user: 
What are some fun activities I can do in the winter?

assistant:  I like snowboarding and skiing.  What do you like to do in winter?
user: 
What else do you recommend?
"""

conversation = chatbot(conversation)
print(conversation)
"""
Conversation id: a989b9e1-621d-4f54-b5bf-991ad508d45f
user: 
What are some fun activities I can do in the winter?

assistant:  I like snowboarding and skiing.  What do you like to do in winter?
user: 
What else do you recommend?

assistant:  Snowboarding is a lot of fun.  You can do it indoors or outdoors.
"""
```