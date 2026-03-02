- https://python.langchain.com/v0.2/docs/tutorials/summarization/

### load_summarize_chain
- Three ways to summarize or otherwise combine documents.
    1. [Stuff](https://python.langchain.com/v0.2/docs/tutorials/summarization/#stuff) : simply concatenates documents into a prompt
	    - Usually the simplest method
	    - When documents fit in context window
    2. [Map-reduce](https://python.langchain.com/v0.2/docs/tutorials/summarization/#map-reduce) :  splits documents into batches, summarizes those, and then summarizes the summaries
	    - When documents does not fit in context window
    3. [Refine](https://python.langchain.com/v0.2/docs/tutorials/summarization/#refine): updates a rolling summary be iterating over the documents in a sequence.

```python
from langchain.chains.summarize import load_summarize_chain
```