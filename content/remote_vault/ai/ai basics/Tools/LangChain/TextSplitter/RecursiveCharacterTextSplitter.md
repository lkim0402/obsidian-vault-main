- Source
	- https://dev.to/eteimz/understanding-langchains-recursivecharactertextsplitter-2846

> What occurs when you present these models with a document that exceeds their context window? This is where a clever strategy known as "chunking" comes into play. 
> - Chunking involves dividing the document into smaller, more manageable sections that fit comfortably within the context window of the large language [[model]].
> - [Langchain](https://www.langchain.com/) provides users with a range of chunking techniques to choose from. However, among these options, the [RecursiveCharacterTextSplitter](https://api.python.langchain.com/en/latest/text_splitter/langchain.text_splitter.RecursiveCharacterTextSplitter.html#langchain.text_splitter.RecursiveCharacterTextSplitter) emerges as the favored and strongly recommended method.


#### What it does
- Takes a large text and splits it based on a specified chunk size. It does this by using a set of characters. The default characters provided to it are ["\n\n", "\n", " ", ""].
- Takes in the large text then tries to split it by the first character \n\n. If the first split by \n\n is still large then it moves to the next character which is \n and tries to split by it. 
- If it is still larger than our specified chunk size it moves to the next character in the set until we get a split that is less than our specified chunk size.



```python
from langchain.text_splitter import RecursiveCharacterTextSplitter

text = "(some text....'What I worked On' by Paul Graham)"

text_splitter = RecursiveCharacterTextSplitter(
    chunk_size = 100,
    chunk_overlap  = 0,
    length_function = len,
)

texts = text_splitter.split_text(text)
print(len(texts)) # 11
print(texts[0]) # 'What I Worked On\n\nFebruary 2021'


```