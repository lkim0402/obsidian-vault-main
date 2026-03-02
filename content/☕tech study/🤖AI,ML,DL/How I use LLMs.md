> Andrej Karpathy, Feb 28 2025
> - https://www.youtube.com/watch?v=EWvNQjAaOHw
> - [file link](https://excalidraw.com/) and [google drive](https://drive.google.com/file/d/1DN3LU3MbKI00udxoS-W5ckCHq99V0Uqs/view) 

- how user queries (+system msg) are tokenized -> converted to tokens
	- https://tiktokenizer.vercel.app/
- the user and the llm are both collaborating together to write a 1D *token stream/sequence, or the context window*
- now, when i click new chat, that *wipes* the token window (resets the tokens to 0 again)
## When we click `new chat`
![[Pasted image 20250301222350.png]]
- user and llm both contribute new 1D token stream/sequence
	- users can write into the stream (user queries)
		- when Enter is pressed, the control is transferred over to the LLM
	- llm then responds with its own token streams
	- llm has a special token that indicates it's done. when it emits that token, the chat gpt application transfers its control back to us
- the token window/token stream
	- working memory of tokens

## What is the talking entity
- what is the entity we're talking to & how should we think about it?
- first, we look at how its trained (2 major stages)
	- pre training stage
		- taking all of internet, chopping to tokens, compressing it into like a single zip file (which is lossy and probabilistic)
		- what's in the zip file are the parameters of the NN
			- for ex) 1TB zip file corresponds to 1 trillion parameters in the NN
			- this NN is taking tokens, and trying to predict the next tokens
		- this phase is not done that often because it's very costly (~10M)
			- that's why models have a knowledge cutoff... and they are generally out of date
	- post training stage
		- the :] => we want llm to take the persona of an assistant while having the knowledge of all internet (pretraining)
- The entity is fully self contained entity by default. Think of it as a 1TB file on a disk, secretly representing 1 trillion parameters, trying to give you next token in a sequence. No calculator, python, internet, www, nothing, no tool. You're talking to a zip file (which has knowledge from pre-training and style/form from post training) where if you give tokens, it will string tokens back.
- Remember, the output is just a recollection, a memory Always double check its answers, it's not guaranteed to be correct.
- If you want to switch topic, start new chat
	- the more tokens that's not related to the topic u want, it will be distracting for the llm
	- more expensive -> it takes llm more and more power to calculate the next token
	- think of tokens as a precious resource, think of it as the working memory of the model, keep it as short as you can

keep in mind what model you're using, pricing
- bigger models are usually charged more
- have a look at if you really want to pay or not
- ask same stuff to different models
- llm "counsels" lol -> if you wanna ask something, ask all of them LOL

## Thinking models
> Use when problem is deep and hard. Not all providers may have them.
- Multiple stages of training: Pre-training -> supervised finetuning -> reinforcement learning
- RL (3th stage of training)
	- fairly recent (1-2 ago (2025))
	- model practices on a large collection of problems that resembles practice problems in textbooks
	- practices lots of math related problems
	- in the process, llm discovers thinking strategies (resembles the inner monologue you have when you're problem solving)
	- [[DeepSeek]]
- Thinking models
	- does additional thinking - higher accuracies esp w/ problems that require higher thinking (math, science, problem solving, etc)
	- can take minutes
	- this model is additionally tuned with RL
	- for OpenAI, all models that start with `o` are thinking models (all tuned with RL)
		- `o1, o3-mini, etc`
	- Perplexity
		- Deep Research
		- Reasoning with R1 (Deepseek new model)
			- you can see the raw thoughts of the model
	- Grok -> click "Think"

## Tool use: internet search
> Can a model do internet search for you? YES! Let the models do the work FOR YOU!
> - if the thing you're doing is googling and clicking a few links, just use llm lmao
- model emits a special "search the internet" token. when this is emitted,  the application will stop sampling from the model, takes the query, does the search and takes the text and puts the in the context window! The model then utilizes this updated context to generate a more informed and accurate response.
- perplexity
- chat gpt search button
- examples
	- recent trends, news -> get the gist of what's happening
	- is the market open today?
	- does vercel offer a postgresql database (tech changes often)
	- apple news
	- why is palentir stock going up?
## Tool use: deep research
> Roughly speaking, it's internet search + thinking!!
- rn
	- chat gpt deep research
	- perplexity deep research
	- grok deepsearch
- creating giant context window
- treat it as 1st draft of papers to look at, as there still can be hallucinations
- but the graphs/numbers are a hit or miss
- examples
	- which browser is more private
	- i want to explore llm labs in the US

## Uploading files to add context
- images are more likely thrown away
- text file loaded into the token window
- claude
- example
	- research paper that you want to learn about more: "can u give me a summary of this paper?"
- A good use case -  reading books!
	- andrej says he always use an llm when reading books
	- pull the book, find the chapter you're reading on the internet (old books are on the internet is free), and let it summarize it
	- then, if you have a question while reading, you ask the llm. 
	- dramatically increases attention, understanding, etc
	- esp useful for reading documents from other fields you're not knowledgeable in, or a very old texts

## Tool use: Python interpreter
> You have to keep track of which llm has which tools 
> - claude uses js, chatgpt uses python
> - if they dont have these tools, they will hallucinate lol
- llm has ability to use and write computer programs
	- emit special tokens that chat gpt recognizes as something that's not for the human but as a computer program for the computer to run it
- for easy queries like 4 * 12, gpt has a "memorized" answer
- for hard queries (like what is 285435 * 203234), gpt will turn to tool use
- the llm writes program, and add tokens that indicate "plz run this program", then llm pauses execution and the python runs and gives result, the llm takes the result as text and displays to you
- OpenAI taught gpt to know which situations should use tools or not, by examples (human labelers are involved in curating datasets

#### ChatGPT advanced data analysis
- unique to chatgpt
- gets chatgpt to be a junior data analyst you can collaborate 
- can ask to research some stats then ask plot data
- nice because this is a VERY easy way to collect data, visualize, or show it
- but still can assume stuff and be sneaky without telling us
- so don't really use these tools if you don't have the ability to scrutinize and verify everything for themselves
#### Claude artifacts
- https://support.anthropic.com/en/articles/9487310-what-are-artifacts-and-how-do-i-use-them
- claude can write a super customized app just for us and deploys on the browser
	- no database or anything like that
	- local apps that can run in browser
	- can get fairly sophisticated and useful
- [claude artifacts showcase](https://claudeartifacts.com/)
- one useful case
	- Diagram generation!!! 
		- can be used for the book example mentioned earlier, great for visualization
		- chapters, source codes, articles, anything!
	- claude creates that mermaid diagram
#### Cursor 
- separate app that works with the files in your file system
- a program you download, references the files on  your computer, and works and edits them for you!
- under the hood uses claude 3.7 sonnet (as of feb 2025)
- lots of devs use this. you're mostly just giving commands and sitting
- becoming more and more elaborate
- composer -> like an autonomous agent on your codebase
	- executes commands, changes files

## Modalities
- we can interact with llms not only with text, but with other modalities
- most of the time we just type stuff out, but we can also speak if you're lazy to type
#### Speech and audio
- audio -> text and text-> audio
	- input
		- chatgpt (on mobile) -> microphone icon converts your speech to text
		- desktop -> SuperWhisper, WisperFlow, MacWhisper
			- download and install, its alr ready to listen to you (u can bind the key), and it will transcribe it into text
	- output
		- option to read it back to you
		- chatgpt - read aloud icon
			- or you can use another software
- what if its a model that can really handle the audio (instead of using a text token stream)
	- break down the audio into a spectrogram to see the frequencies present , go in windows, and quantize them into tokens
	- train the model with these audio chunks
Examples
- chatgpt
	- its the other audio button in chatgpt (advanced voice mode)
	- voice is handled natively in the llm
	- it understands audio chunks and predict audio chunks (no text involved)
- grok advanced voice mode (on mobile)
	- grok modes are sometimes veeeery unhinged (paid for now)
- NotebookLM google
	- upload arbitrary data on the left, which enters the context window for that model
	- you can chat with the llm with the data
	- on the right, there is a deep dive podcast! (custom podcast)
		- interesting or therapeutic, very customzable
	- we can use this feature for stuff im not an expert in (i just have a passive interest in, going out for a long walk/drive and u wanna have a customized podcast!)
	- a podcast about ANY arbitrary niche topic you like
#### Images and video
- image input
	- images (just like audio), you can re-represent images in token streams
	- the simplest possible way is to make a picture a sequence of patches, and each patch you quantize
	- for the llm, it doesn't even know that some of the tokens happen to be text/audio/images...
	- example use case
		- first ask it to transcribe it into text, so u can check if it transcribed correctly
		- then ask questions after u check
- image output
	- Dalle openai
- video input
	- advanced voice chatgpt has this feature
- video output
	- sora
	- maaany others

## Quality of Life Features
#### Chat GPT memory feature
- chatgpt has the ability to save information in chats, but it has to be invoked
	- sometimes gpt will trigger it, but sometimes u have to ask for it
	- "can u please remember this/my preference?" -> "memory updated"
	- records the text in its memory bank - separate piece of chat gpt like a database of knowledge about you
	- this database is ALWAYS PRE-APPENDED to all conversations!
	- you can add/delete memories
	- movie/book recommendations!!!!
#### Custom GPTs
- Andrej uses it for language learning the most
- all done via prompting: give a description, and give examples (few shot prompt)
- if there's a certain prompt//task that you keep reusing, just create custom gpt
- Examples of custom gpt
	- give it a sentence, extracts the vocabs in the sentence and tells their meaning with specific format
		- can be copy pasted into anki flashcards app
		- easy to turn sentence to flashcards
	- translation
		- when u wanna understand how the translation works (the nuance)
		- you can see how the part translates in more details 
		- u can ask clarifying questions!!!
		- SIGNIFICANTLY BETTER