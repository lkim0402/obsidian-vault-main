```python
summarizer = pipeline("summarization")
summarizer("some text...", min_length=10, max_length-100)