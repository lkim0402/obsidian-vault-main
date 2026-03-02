# Text Splitting/chunking
>[!Goal]
>Your goal is not to chunk for chunking sake, the goal is to get data in a format where it can be retrieved for value later!

- **Text splitting/chunking**
	- *process of splitting your data into smaller pieces*
	- The GOAL -> to prepare ur data for anticipated tasks u actually have
		- "What's the optimal way for me to pass data my LLM needs for its task?"
	- one of the most foundations an ai practitioner will do
- **Why??**
	- u cant pass all data at once, LLMs have context windows
	- LLMs also need unnecessary info removed from data (high signal to noise ratio) because if not it will destroy the performance of the overall app
- **What is retrieval?**
	- The act of gathering the right information for ur LLM
	- there are many retrieval strategies
	- *Retrieval evaluations*
		- U need proper evaluations to see if retrievals are working
		- [Ragas](https://github.com/vibrantlabsai/ragas)
- Recommended chunk size
	- 2000~6000, depending on app
- website visualizer
	- https://www.chunkviz.com/
# 5 Main ways of Text Splitting
- Outline
	1. Character splitting - split by fixed static character limit
	2. Recursive Character splitting - separate with separators
	3. Document specific text splitting
		- python/js/pdfs etc -> include multimodal
	4. Semantic splitting
		- Not naive like the previous 3 (they focus on physical positioning/structure of text chunks), but like "what" and the "why" o f the text (understanding the context of text)
	5. Agentic splitting
		- You build an agent like system that will review text and split it for us
	6. Bonus: Indexing
- [[LangChain]] will be the main tool
- Google Colab Link -> https://colab.research.google.com/drive/1OmUlatumVfBBpGqUCGblr457Vt-lg7v_?usp=sharing
## Character splitting
- split by fixed static character limit
- easy, simple, but super rigid. X used in production
- terms
	- `chunk size` = # of characters u would like in your chunks
	- `chunk overlap` = the amount you would like ur sequential chunks to overlap, to avoid cutting a single piece of context into multiple pieces. this will also lead so duplicates

Manual:
```python 
text = "This is the text I would want to chunk."
chunks = []
chunk_size = 35
for i in range(0, len(text), chunk_size):
	chunk = text[i: i + chunk_size]
	chunks.append(chunk)
```

[[LangChain]]
- u can also use other tools like `llamaIndex`
```python
from langchain_text_splitters import CharacterTextSplitter

text = "This is a sentence that I will test Langchain's text splitter."

text_splitter = CharacterTextSplitter(chunk_size = 35, chunk_overlap=0, separator='', strip_whitespace=False)

text_splitter.create_documents([text])
```
```
[Document(metadata={}, page_content='This is a sentence that I will test'),
 Document(metadata={}, page_content=" Langchain's text splitter.")]
```
- `CharacterTextSplitter`
	- `create_documents` expects a list
	- `chunk_overlap` - the tail of prev will overlap with head of next
		- https://www.chunkviz.com/
	- `separator`
		- we can split by anything! -> punctuations, space, a character, etc
	- `strip_whitespace`
		- `True` by default
- Result
	- each chunk is a `Document` object -> in [[langchain]] it's an object that can hold strings and metadata. our content is held in `page_content`
## Recursive Character splitting
- The *standard chunking method* for text embedding/RAG
	- gud resource - https://dev.to/eteimz/understanding-langchains-recursivecharactertextsplitter-2846
- Divides large documents into smaller pieces based on a maximum chunk size limit and a prioritized list of separators
	- `"\n\n"` - double new line / paragraph breaks
	- `"\n"` - next line
	- `" "` - spaces
	- `""` - characters
	- so the prioritized ordering is: *paragraph -> sentence -> word*
- what it does
	- Splits the text using the highest-priority separator first (e.g., `\n\n` to split by paragraph)
    - Checks if the resulting chunks are within the defined maximum `chunk_size`.
	    - if chunk too large, the algo recursively applies the *next* separator in the list only to that specific oversized chunk.
	- Typically uses a `chunk_overlap` parameter to duplicate a small number of characters at the boundaries of adjacent chunks to prevent cutting off context abruptly.
- Basically
	- First, split the text by the highest priority separator (e.g., `\n\n` for paragraphs).
    - Check the size of the resulting pieces.
    - **If a piece is bigger than chunk size:** Recursively split _that specific oversized piece_ using the next separator in the list (e.g., `\n`, then `" "`, then `""`).
    - **Once pieces are small enough:** Merge these smaller pieces sequentially into a single chunk until the combined length reaches the `chunk_size` limit. (This prevents having unnecessarily tiny chunks).
    - Repeat until the entire document is processed.

```python
from langchain_text_splitters import RecursiveCharacterTextSplitter


text = """What I Worked On
February 2021
Before college the two main things I worked on, outside of school, were writing and programming. I didn't write essays. I wrote what beginning writers were supposed to write then, and probably still are: short stories. My stories were awful. They had hardly any plot, just characters with strong feelings, which I imagined made them deep.
"""
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size = 100,
    chunk_overlap  = 0,
    length_function = len,
    is_separator_regex = False
)
text_splitter.create_documents([text])
```

- attributes
	- `chunk_size`
	- `chunk_overlap`
	- `length_function`
		- use `len` function to get the `length` of the string
	- `is_separator_regex`
		- put as `False` to not use regex

## Document specific text splitting

### Python docs, markdown, pdf, etc.. 
- Start with markdown - `MarkdownTextSplitter`
	- splits by headers
```python
from langchain_text_splitters import MarkdownTextSplitter

splitter = MarkdownTextSplitter(
    chunk_size = 40,
    chunk_overlap=0
)
markdown_text = """
# Fun in California

## Driving

## Hiking
"""
splitter.create_documents([markdown_text])
```

- [[Python]] docs - `PythonCodeTextSplitter`
	- `\nclass`, etc
- [[JavaScript]] docs - `RecursiveCharacterTextSplitter`, `Language`
	- `RecursiveCharacterTextSplitter.from_language(language=Language.JS, ...`
### PDFS
- PDFs are an extremely common data type for LLMs, often they have tables that contain information
- https://unstructured.io/ -> very convenient!
```python
import os
from unstructured.partition.pdf import partition_pdf
from unstructured.staging.base import elements_to_json

filename = "sonocraftar.pdf"
 
# extract
elements = partition_pdf(
    filename=filename,
    # unstructured helpers
    strategy="hi_res",
    infer_table_structure=True,
    model_name="yolox"
)
```
- `elements`
	- will give us `NarrativeText` and `Table`
	- `Table`s will be shown as [[html]] -> the LLM will understand better
### Multi-Modal (text+image)
- How will u deal with images?
```python
from typing import Any
from pydantic import BaseModel
from unstructured.partition.pdf import partition_pdf

filepath = ".../...pdf"
raw_pdf_elems = partition_pdf(
	filename = filepath,
	# using pdf format to find embedded img blocks
	extract_images_in_pdf=True,
	# use layout model to get bounding boxes
	# titles are any sub-section of the document
	
	# post process to aggregate text once we have title 
	chunking_strategy="by_title",
	# chunking params to aggregate text blocks
	# attempt to creat new chunk 3000 chars
	# attempt to keep chunks > 2000 chars 
	# marx max on chunks
	max_characters=4000,
	new_after_n_chars=3800,
	combine_text_under_n_chars=2000,
	impage_output_dir_path="static/pdfImages/"
)
```

- `extract_images_in_pdf`
	- it will chunk but treat images differently
- the images will be extracted to `static/pdfImages/`
- we can do embeddings for images, but usually models provide for just one (ex. embedding model only for text or only for images)
	- there is the CLIP model, but we can do better
- what we'll do
	- generate text summary of each img + embedding of text summary
	- https://www.youtube.com/watch?v=8OJC21T2SL4 -> 28:00
## Semantic splitting
- lvl 1-3
	-  we all took physical positioning into acc. we just assumed paragraphs have similar info. what if they dont? what if we have rlly messy info?
	- grouping books by size
- we will try *embedding based chunking method*
	- makes use of meaning + context -> grouping books by genre/author
	- **embedding** -> represent the semantic meaning of a string. so semantically similar chunks should be held together
- https://colab.research.google.com/drive/1OmUlatumVfBBpGqUCGblr457Vt-lg7v_?usp=sharing
## Agentic splitting
- Can we instruct an LLM to do this task like a human would?
- Me:
	- 