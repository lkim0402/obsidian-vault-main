> Bidirectional Encoder Representations from Transformers
- While BERT is an older model, it's still used as a baseline in NLP experiments
- model released in 2018 by Google researchers
- https://huggingface.co/google-bert
- The easy way to understand BERT
	- Take the [[Transformers|Transformer Architecture]] and just take the encoder, then stack them
	- It's Bi-directional, (not reading in a sequence like RNNs)
- It is pretrained on the following tasks
	- Masked language model (MLM)
		- provide input where tokens are masked
		- we ask BERT to fill in the blank for sentences
	- Next sentence prediction (NSP)
		- BERT is provided 2 sentences A and B
		- BERT has to predict if B would follow A
- Need to fine tune it further for specific tasks
	- Named Entity Recognition (NER)
	- Question Answering (QA)
	- Sentence pair tasks
	- summarization
	- feature extraction/embeddings
	- and more....
- BERT Variants
	- roBERTa
	- DistilBERT
	- ALBERT
	- ELCTRA
	- DeBERTa
- Multiple sizes
	- BERT Large 240M parameters
	- BERT Base 100M parameters
	- BERT Tiny 4M parameters
	- and ~24 other model sizes
- Today... ppl work with BERT for
	- classification (labels) and sentiment analysis
	- analyzing a text to reach a conclusion, not to generate something

### Example of using BERT
- sentiment analysis via the HuggingFace "sentiment analysis" pipeline
```python
from transformers import pipeline

# Creating a sentiment analysis pipeline using a pretrained BERT model
classifier = pipeline(
	"sentiment-analysis", 
	model="bert-bert-uncased",
	tokenizer="bert-base-uncased"
)

# Test sentences
sentences = [
	"I love using BERT for NLP tasks!",
	"I am not a fan of ice cream."
]

results = classifier(sentences)

for sentence, result in zip(sentences, results):
	print(f"sentence: {sentence}")
	print(f"prediction: {result['label']} | Score: {result['score']:.4f}")
	print()

```
- HuggingFace is downloading a BERT Base Uncased model that has been fine-tuned for Sentiment ANalysis
- BERT needs to be fine-tuned for specific tasks beyond pretraining
- related
	- [[zip|python zip function]]
	- [[HuggingFace]]

### BERT Lab
- https://colab.research.google.com/drive/1n5A4cRjakOIo3Ne1_1bvY17qe_V1Wn9V?usp=sharing
```python
pip install torch transformers

import torch
from transformers import BertTokenizer, BertModel
```
- BertTokenizer:
    - It converts raw text into input tokens that the BERT model can understand.
    - It also handles padding, truncation, and conversion into input IDs and attention masks.
- BertModel:
    - It is the core BERT model that provides the hidden states (embeddings) for each token in the input text.
    - It outputs:
        - last_hidden_state: The embeddings for all tokens.
        - pooler_output: The embedding for the [CLS] token, representing the entire sentence.
```python
model_name = "bert-base-uncased"
tokenizer = BertTokenizer.from_pretrained(model_name)
model = BertModel.from_pretrained(model_name)

# converting the test into input IDs and attention masks
text = "I love AI!"
inputs = tokenizer(text, return_tensors="pt", padding=True, truncation=True)
input_ids = inputs["input_ids"] 
attention_mask = inputs["attention_mask"]
```
- The tokenizer might break the text`"I love AI!`" to ``['[CLS]', 'i', 'love', 'ai', '!', '[SEP]']``
	- `[CLS]`: Special token added at the start. BERT uses this to understand the whole sentence.
	- `[SEP]`: Special token added at the end. Useful when comparing two sentences.
- `input_ids`: Input IDs are the numbers that represent the tokens. BERT has a vocabulary where every word or subword has a unique number.
  - Ex. the text is converted to `[101, 1045, 2293, 9937, 999, 102]`
- `attention_mask`: Tells the BERT which tokens are real words and which are just padding (extra zeros to make all input the same length).
  -  `input_ids = [101, 1045, 2293, 9937, 999, 102, 0, 0]`
  - `attention_mask = [1, 1, 1, 1, 1, 1, 0, 0]`
```python
# getting BERT embeddings
with torch.no_grad():
  outputs = model(input_ids, attention_mask=attention_mask)

# the last hidden state (embeddings for each token)
last_hidden_state = outputs.last_hidden_state

# pooled output
pooled_output = outputs.pooler_output

print("Last Hidden State Shape:", last_hidden_state.shape)
print("Pooled Output Shape:", pooled_output.shape)
```
- `last_hidden_state`
	- This is the output of BERT for every token in the input.
	  - A big matrix containing numbers (embeddings) for each token.
	  - Each token gets a 768-dimensional vector (for bert-base-uncased).
- `pooled_output`
	- The embedding for the `[CLS]` token, representing the entire sentence.

