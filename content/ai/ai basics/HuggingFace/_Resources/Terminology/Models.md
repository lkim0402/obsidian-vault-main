- You can upload your own models to HuggingFace
- You can use the models!

Things model can have
- checkpoints
	- Refers to the saved [[model]], including the [[Pretrained model|pretrained]] weights and all necessary configurations
	- Checkpoints can have varying number of parameters (we say that this type of model comes in different sizes)
	- We say we load a model, but technically we load a model's checkpoint
	- Depending on your hardware you may not be able to run the largest ones, so you *estimate* how much memory you will need for a model