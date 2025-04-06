```python
generator = pipeline("text-generation", model="distilgpt2")
generator("In this course, we will teach you how to",
          max_length=15,
          num_return_sequences=2)
"""
[{'generated_text': 'In this course, we will teach you how to understand and use '
                    'data flow and data interchange when handling user data. We '
                    'will be working with one or more of the most commonly used '
                    'data flows — data flows of various types, as seen by the '
                    'HTTP'}]
"""
```

