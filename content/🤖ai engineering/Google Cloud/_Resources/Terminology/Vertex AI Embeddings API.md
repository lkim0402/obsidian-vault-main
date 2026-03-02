>Simplifies the process of generating [[Embeddings]] for various types of data without requiring users to train their own [[model]]s
- The API leverages pre-trained models that Google has already trained on large datasets. 
    - These models are fine-tuned and optimized for various tasks, ensuring that the [[Embeddings]] they generate are effective for a wide range of applications.
- Efficient due to vector search technology: [[Vertex AI Matching Engine]]
- Can use with text processing tasks:
	- LLM-enabled semantic search
	- LLM-enabled text classification
	- LLM-enabled recommendation
- It has a large embedding space with 768 dimensions

How companies use this
1. Companies provide their own data (e.g., text documents, images) as input to the API
2. The API processes this input data and converts it into embeddings. These embeddings are dense vector representations that capture the semantic meaning of the data.
3. The company might use text embeddings for tasks
	- **Document Similarity:** Embeddings can be used to find similar documents based on their content, which is useful for search engines and recommendation systems.
	- **Search:** Embeddings enable semantic search, where the search engine understands the intent behind the query and retrieves relevant results based on the meaning rather than just keywords.
	- **Sentiment Analysis:** Embeddings can help analyze the sentiment of text data, identifying whether the sentiment is positive, negative, or neutral.
	- **Clustering and Classification:** Embeddings can be used to cluster similar items together or classify data into predefined categories.