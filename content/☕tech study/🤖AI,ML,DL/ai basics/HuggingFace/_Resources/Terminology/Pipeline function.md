> Most valuable API & most basic object in the [[Transformers library]].
- Connects a model with necessary **preprocessing** and **postprocessing** steps
- Returns an end-to-end object that performs an NLP task on one or several tasks
	![[Pasted image 20240702221743.png]]

- Official docs: https://huggingface.co/docs/transformers/en/main_classes/pipelines
```python
from transformers import pipeline

# THE PIPELINE FUNCTION
pipe = pipeline("text-classification")
pipe("This restaurant is awesome")
#output: [{'label': 'POSITIVE', 'score': 0.9998743534088135}]

pipe = pipeline("text-classification")
>>> pipe(["This restaurant is awesome", "This restaurant is awful"])
#output: [{'label': 'POSITIVE', 'score': 0.9998743534088135}, {'label': 'NEGATIVE', 'score': 0.9996669292449951}]

#We can add the model like this
pipe = pipeline(
	task="text-classification",
	model="FacebookAI/roberta-large-mnli",
	torch_dtype=torch.bfloat16
	)
pipe("This restaurant is awesome")
#output: [{'label': 'NEUTRAL', 'score': 0.7313136458396912}]
```
- Three main steps
	1. Text is preprocessed for model to understand
	2. The preprocessed inputs are passed to the model
	3. The predictions of the model are post-processed, so you can understand
- `torch_dtype`
	- specify the data type (dtype) for the tensors that the model will use during computation
	- can impact the model's performance, memory usage, and compatibility with certain hardware
	- represent the precision level at which the model computations are performed
		- **`torch.float32` (or `torch.FloatTensor`)**: The default data type for PyTorch tensors, representing 32-bit floating-point numbers. It is the standard for most model computations, providing a balance between precision and memory usage.
		- **`torch.float16` (or `torch.HalfTensor`)**: A 16-bit floating-point number. It is commonly used in mixed-precision training, especially on GPUs, to reduce memory usage and increase computational speed. However, it might be less precise than `float32`.
		- **`torch.bfloat16` (or `torch.BFloat16Tensor`)**: A 16-bit floating-point number with a larger exponent range than `float16`, making it better suited for some machine learning tasks, especially on newer hardware like NVIDIA's Ampere GPUs and TPUs. It retains much of the computational efficiency of `float16` while minimizing the risk of underflow/overflow in some operations.
- By default, the pipeline selects a [[Pretrained model]]that has been fine-tuned for sentiment analysis in English, then the model is downloaded and cached when the `pipe` object is created. 
- List of tasks
	- https://huggingface.co/docs/transformers/en/task_summary
	- https://huggingface.co/docs/transformers/en/tasks_explained
	- zero-shot-classification
	- text-generation
	- mask filling
	- ner (named entity recognition)
	- question-answering
	- summarization
	- translation
	- token classification
	- text-classification

## Pipelines
- [[Zero-shot classification]]
- [[Text generation]]
- [[Conversation (deprecated)]]
- [[Mask Filling]]
- [[Named entity recognition]]
- [[Question answering]]
- [[Summarization]]
- [[Translation]]
