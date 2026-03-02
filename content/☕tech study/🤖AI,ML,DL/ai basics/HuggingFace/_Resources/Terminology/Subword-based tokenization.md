- Lies in between [[Word-based tokenization]] and [[Character based tokenization]]
- The algorithm
	- Frequently used words should not be split into smaller subwords
	- Rare words should be decomposed into meaningful subwords
		![[Pasted image 20240703230049.png]]
		- Another example: tokenization -> 'token' and '##ization' where ## indicates it is used to complete a word (BERT)
	- Different algorithms you can use for subword-based tokenization
		- **Byte Pair Encoding (BPE)**: Used in models like GPT.
		- **WordPiece**: Used in BERT.
		- **Unigram**: Used in models like T5.