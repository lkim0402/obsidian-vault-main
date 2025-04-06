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
    system_message="You are thinking of an animal. You have the animal 'snake' in your mind, and I will try to guess it. "
    "If I guess incorrectly, give me a hint.",
    llm_config=llm_config,
    human_input_mode="NEVER",
)

guesser = ConversableAgent(
    name="guesser",
    is_termination_msg=lambda msg: "snake" in msg["content"].lower(),
    system_message="I have an animal in my mind, and you will try to guess it. "
    "If I give you a hint, use it to narrow down your guesses.",
    llm_config=llm_config,
    human_input_mode="NEVER",
)

riddler.initiate_chat(
    guesser, message="I am thinking of an animal. Guess which animal I am thinking of!"
)
```

```sh
PS C:\Users\artygen\Desktop\AutoGen\input_modes> python .\never_mode.py
riddler (to guesser):

I am thinking of an animal. Guess which animal I am thinking of!

--------------------------------------------------------------------------------
guesser (to riddler):

Is it a mammal?

--------------------------------------------------------------------------------
riddler (to guesser):

No, it is not a mammal.

--------------------------------------------------------------------------------
guesser (to riddler):

Is the animal you are thinking of a bird?

--------------------------------------------------------------------------------
riddler (to guesser):

No, it is not a bird.

--------------------------------------------------------------------------------
guesser (to riddler):

Is the animal you are thinking of a reptile?

--------------------------------------------------------------------------------
riddler (to guesser):

Yes, the animal I am thinking of is a reptile.

--------------------------------------------------------------------------------
guesser (to riddler):

Is the animal you are thinking of a type of lizard?

--------------------------------------------------------------------------------
riddler (to guesser):

No, the animal I am thinking of is not a type of lizard.

--------------------------------------------------------------------------------
guesser (to riddler):

Is the animal you are thinking of a type of snake?

--------------------------------------------------------------------------------
riddler (to guesser):

Yes, the animal I am thinking of is a snake!

--------------------------------------------------------------------------------
PS C:\Users\artygen\Desktop\AutoGen\input_modes> 
```