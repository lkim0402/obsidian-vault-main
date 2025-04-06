> The ability of a model to classify audio samples into categories it has never explicitly been trained on

```bash
pip install transformers
pip install datasets
pip install soundfile
pip install librosa
```

```python
from datasets import load_dataset, load_from_disk

# This dataset is a collection of different sounds of 5 seconds
# dataset = load_dataset("ashraq/esc50",
#                       split="train[0:10]")
# the `split` argument specifies which portion of the dataset you want to load

dataset = load_from_disk("./models/ashraq/esc50/train")

audio_sample = dataset[0]
"""
{'filename': '1-100032-A-0.wav', 
'fold': 1, 
'target': 0, 
'category': 'dog', 
'esc10': True, 
'src_file': 100032, 
'take': 'A', 
'audio': {'path': None, 'array': array([0., 0., 0., ..., 0., 0., 0.]), 
'sampling_rate': 44100}}
"""

# this code lets you listen to that sample
from IPython.display import Audio as IPythonAudio
IPythonAudio(audio_sample["audio"]["array"],
             rate=audio_sample["audio"]["sampling_rate"])  
```

```python
# Building the pipeline
from transformers import pipeline

classifier = pipeline(
	task="zero-shot-audio-classification",
    model="./models/laion/clap-htsat-unfused"
)
```
- The pretrained CLAP model
- To classify your audio example you'll only need the array of audio data. However, the example has to have a sampling rate that the model expects.
- So we check the sampling rate of the model and the dataset
	- How long does 1 second of high resolution audio (192,000 Hz) appear to the Whisper model (which is trained to expect audio files at 16,000 Hz)? 
		- $\frac{(1 * 192000)}{16000} = 12.0$
		- The 1 second of high resolution audio appears to the model as it is 12s of audio
	- How about 5 seconds of audio?
		- $\frac{(5 * 192000)}{16000} = 60.0$
```python
zero_shot_classifier.feature_extractor.sampling_rate
# 48000

audio_sample["audio"]["sampling_rate"]
#44100
```
- It's not a big difference so it's okay

```python
from datasets import Audio

dataset = dataset.cast_column(
    "audio",
     Audio(sampling_rate=48_000)
 )

audio_sample = dataset[0]

candidate_labels = [
	"Sound of a dog",
	"Sound of vacuum cleaner"
]

zero_shot_classifier(
	 audio_sample["audio"]["array"],                candidate_labels=candidate_labels
 )

"""
[{'score': 0.9985589385032654, 'label': 'Sound of a dog'},
 {'score': 0.0014411123702302575, 'label': 'Sound of vacuum cleaner'}]
"""
```
