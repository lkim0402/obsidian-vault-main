- Splitting raw text into words by spaces and/or punctuation
- Each word has a number of ID
	- The number is higher if the word contains lots of contextual and semantic information in a sentence
- Limitations
	- Very similar words have entirely different meanings (entirely different IDs)
	- Too many words in English dictionary (leading to heavy model). We can ignore certain words we don't really need, like taking only 10000 most frequent words in the text.
		- Any other word will be "unknown"