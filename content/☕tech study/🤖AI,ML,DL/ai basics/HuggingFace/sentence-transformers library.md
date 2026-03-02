- A specialized library built on top of Hugging Face's `transformers` to specifically handle tasks that involve sentence embeddings, such as semantic search, clustering, and sentence similarity.
- The [[Transformers library]] Provides a broad set of models for various NLP tasks, including language generation, classification, and more; The `sentence-transformers` focuses on models that can efficiently generate sentence embeddings.

- Useful for sentence similarity
	- useful in sentence retrieval, clustering, or grouping

- Sentence similarity models convert input text into vectors ([[Embeddings]])

Code
```python
pip install sentence-transformers

from sentence_transformers import SentenceTransformer
from sentence_transformers import util

model = SentenceTransformer("all-MiniLm-L6-v2")

sentences1 = [
	'The cat sits outside',
    'A man is playing guitar',
    'The movies are awesome'
]

embeddings1 = model.encode(sentences1, convert_to_tensor=True)
print(embeddings1)
"""
tensor([[ 0.1392,  0.0030,  0.0470,  ...,  0.0641, -0.0163,  0.0636],
        [ 0.0227, -0.0014, -0.0056,  ..., -0.0225,  0.0846, -0.0283],
        [-0.1043, -0.0628,  0.0093,  ...,  0.0020,  0.0653, -0.0150]])
"""

sentences2 = [
	'The dog plays in the garden',
    'A woman watches TV',
    'The new movie is so great'
]
embeddings2 = model.encode(sentences2, convert_to_tensor=True)

cosine_scores = util.cos_sim(embeddings1,embeddings2)
print(cosine_scores)
""" cosine similary for each pair of sentences1 and sentences2
tensor([[ 0.2838,  0.1310, -0.0029],
        [ 0.2277, -0.0327, -0.0136],
        [-0.0124, -0.0465,  0.6571]])
"""
```


### Related
- [[Embeddings]]