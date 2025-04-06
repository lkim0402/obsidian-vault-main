- You can specify which labels to use for classification so you don't rely on the labels on the pretrained model
```python
from transformers import pipeline

classifier = pipeline("zero-shot-classification)
classifier(
   "This is a HuggingFace Course.",
   candidate_labels=["education", "politics", "business"]
)

"""
{'sequence': 'This is a course about the Transformers library',
 'labels': ['education', 'business', 'politics'],
 'scores': [0.8445963859558105, 0.111976258456707, 0.043427448719739914]}
"""
```