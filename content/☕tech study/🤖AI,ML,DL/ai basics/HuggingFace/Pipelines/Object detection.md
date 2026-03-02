- Use case: Helping a visually impaired individual identify things in the picture

```bash
pip install transformers
pip install gradio
pip install timm
pip install inflect
pip install phonemizer
    
sudo apt-get update
sudo apt-get install espeak-ng
pip install py-espeak-ng
```

```python
from helper import load_image_from_url, render_results_in_image

from transformers import pipeline

od_pipe = pipeline(
   "object-detection", 
   "./models/facebook/detr-resnet-50"
)
```

Opening the image and using the pipeline
```python
from PIL import Image

raw_image = Image.open('path_to_image..')
raw_image.resize((569,491))

# raw output
pipeline_output = od_pipe(raw_image)

# processing and rendering the raw output
processed_image = render_results_in_image(
	  raw_image,
	  pipeline_output
)

print(processed_image)
# You will see the image with things classified inside
```
#### Using Gradio
```python
import os
import gradio as gr

def get_pipeline_prediction(pil_image):
    # just refactoring the code above into a function
    pipeline_output = od_pipe(pil_image)
    processed_image = render_results_in_image(
	    pil_image,                                     pipeline_output
	)
    return processed_image

# making gradio demo
demo = gr.Interface(
	fn=get_pipeline_prediction,
	inputs=gr.Image(label="Input image",
		type="pil"),
	outputs=gr.Image(label="Output image with predicted instances",
		type="pil")
)

# `share=True` will provide an online link to access to the demo

demo.launch(
	share=True,
	server_port=int(os.environ['PORT1'])
)

#close after accesing link to demo
demo.close()
```

#### AI powered Audio assistant
- Combine the object detector with a text-to-speech model that will help dictate what is inside the image
```python
from helper import summarize_predictions_natural_language

raw_image = Image.open(
	'huggingface_friends.jpg'
)
raw_image.resize(
	(284, 245)
)

text = summarize_predictions_natural_language(
	pipeline_output
)

print(text)
"""
'In this image, there are two forks three bottles two cups four persons one bowl and one dining table.'
"""
```

#### Generating Audio Narration
```python
tts_pipe = pipeline(
	"text-to-speech",
    model="./models/kakao-enterprise/vits-ljs"
)

narrated_text = tts_pipe(text)

# playing the generated audio
from IPython.display import Audio as IPythonAudio

IPythonAudio(
	narrated_text["audio"][0],
	rate=narrated_text["sampling_rate"]
)
```