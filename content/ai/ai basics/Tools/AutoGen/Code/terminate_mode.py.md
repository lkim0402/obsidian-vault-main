```python
import os
from autogen import ConversableAgent
from dotenv import load_dotenv

load_dotenv()

model = "gpt-3.5-turbo"
llm_config={
    "model":model,
    "api_key": os.getenv("OPENAI_API_KEY")
}

riddler = ConversableAgent(
    name="riddler",
    system_message="You are thinking of an animal. You have the animal 'monkey' in your mind, and I will try to guess it. "
    "If I guess incorrectly, give me a hint.",
    llm_config=llm_config,
    max_consecutive_auto_reply=2,  
    # maximum number of consecutive auto-replies of the riddler before asking for human input
    human_input_mode="TERMINATE",
)

guesser = ConversableAgent(
    name="guesser",
    is_termination_msg=lambda msg: "monkey" in msg["content"].lower(),
    system_message="I have an animal in my mind, and you will try to guess it. "
    "If I give you a hint, use it to narrow down your guesses.",
    llm_config=llm_config,
    human_input_mode="NEVER",
)

riddler.initiate_chat(
    guesser, message="I am thinking of an animal. Guess which animal I am thinking of!"
)
```

```
PS C:\Users\artygen\Desktop\AutoGen\input_modes> python .\terminate_mode.py
riddler (to guesser):

I am thinking of an animal. Guess which animal I am thinking of!

--------------------------------------------------------------------------------
guesser (to riddler):

Is it a mammal?

--------------------------------------------------------------------------------

>>>>>>>> USING AUTO REPLY...
riddler (to guesser):

Yes!

--------------------------------------------------------------------------------
guesser (to riddler):

Is the animal you're thinking of a carnivore?

--------------------------------------------------------------------------------

>>>>>>>> USING AUTO REPLY...
riddler (to guesser):

No, it is not a carnivore.

--------------------------------------------------------------------------------
guesser (to riddler):

Is the animal you're thinking of a herbivore?

--------------------------------------------------------------------------------
Please give feedback to guesser. Press enter to skip and use auto-reply, or type 'exit' to stop the conversation: idk
riddler (to guesser):

idk

--------------------------------------------------------------------------------
guesser (to riddler):

That's okay! Let me ask another question to help narrow it down. Is the animal you're thinking of commonly kept as a pet?    

--------------------------------------------------------------------------------

>>>>>>>> USING AUTO REPLY...
riddler (to guesser):

Yes, it is commonly kept as a pet!

--------------------------------------------------------------------------------
guesser (to riddler):

Is the animal you're thinking of a rabbit?

--------------------------------------------------------------------------------

>>>>>>>> USING AUTO REPLY...
riddler (to guesser):

Oops, I made a mistake in our conversation. The animal I'm thinking of is not a rabbit. Let me give you another hint: it has a tail and is known for its playful nature.

--------------------------------------------------------------------------------
guesser (to riddler):

Is the animal you're thinking of a ferret?

--------------------------------------------------------------------------------
Please give feedback to guesser. Press enter to skip and use auto-reply, or type 'exit' to stop the conversation: no
riddler (to guesser):

no

--------------------------------------------------------------------------------
guesser (to riddler):

Is the animal you're thinking of a hamster?

--------------------------------------------------------------------------------

>>>>>>>> USING AUTO REPLY...
riddler (to guesser):

No, it is not a hamster. Let me clarify that the animal I am thinking of is known for its agility and climbing skills, and it is often found in forests and jungles.

--------------------------------------------------------------------------------
guesser (to riddler):

Is the animal you're thinking of a squirrel?

--------------------------------------------------------------------------------

>>>>>>>> USING AUTO REPLY...
riddler (to guesser):

Close! The animal I am thinking of is similar to a squirrel but is often found in tropical climates and is known for its intelligence and ability to use tools. Can you guess which animal it is?

--------------------------------------------------------------------------------
guesser (to riddler):

Is the animal you're thinking of a capuchin monkey?

--------------------------------------------------------------------------------
Please give feedback to guesser. Press enter to skip and use auto-reply, or type 'exit' to stop the conversation:

>>>>>>>> NO HUMAN INPUT RECEIVED.

>>>>>>>> USING AUTO REPLY...
riddler (to guesser):

Yes, that's correct! The animal I was thinking of is a capuchin monkey. Well done!

--------------------------------------------------------------------------------
PS C:\Users\artygen\Desktop\AutoGen\input_modes> 
```