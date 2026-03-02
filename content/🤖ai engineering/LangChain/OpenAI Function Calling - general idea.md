# setup
```python
import os
import openai

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']

import json
```
# calling openai
```python
# Example dummy function hard coded to return the same weather
# In production, this could be your backend API or an external API
def get_current_weather(location, unit="fahrenheit"):
    """Get the current weather in a given location"""
    weather_info = {
        "location": location,
        "temperature": "72",
        "unit": unit,
        "forecast": ["sunny", "windy"],
    }
    return json.dumps(weather_info)
```

defining a function + message
- will use [[pydantic]] later - [[📌OpenAI Function Calling with LCEL (+pydantic)]]
```python
# define a function
functions = [
    {
        "name": "get_current_weather",
        "description": "Get the current weather in a given location",
        "parameters": {
            "type": "object",
            "properties": {
                "location": {
                    "type": "string",
                    "description": "The city and state, e.g. San Francisco, CA",
                },
                "unit": {"type": "string", "enum": ["celsius", "fahrenheit"]},
            },
            "required": ["location"],
        },
    }
]
messages = [
    {
        "role": "user",
        "content": "What's the weather like in Boston?"
    }
]
```
- `functions`
	- you can pass a list of function definitions that will be used in the LLM
	- the `description` part is rlly important as the LLM will read that and decide if it should be used or not

```python
import openai
# Call the ChatCompletion endpoint
response = openai.ChatCompletion.create(
    # OpenAI Updates: As of June 2024, we are now using the GPT-3.5-Turbo model
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions
)
print(response)
```

```
{
  "id": "chatcmpl-AVCsmqFCt4FILgYpy7wDxpjahOZvC",
  "object": "chat.completion",
  "created": 1732001052,
  "model": "gpt-3.5-turbo-0125",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": null,
        "function_call": { # <<<<<<<<<<<<<<<<<<<<<
          "name": "get_current_weather",
          "arguments": "{\"location\":\"Boston\",\"unit\":\"celsius\"}"
        },
        "refusal": null
      },
      "logprobs": null,
      "finish_reason": "function_call"
    }
  ],
  "usage": {
    "prompt_tokens": 82,
    "completion_tokens": 20,
    "total_tokens": 102,
    "prompt_tokens_details": {
      "cached_tokens": 0,
      "audio_tokens": 0
    },
    "completion_tokens_details": {
      "reasoning_tokens": 0,
      "audio_tokens": 0,
      "accepted_prediction_tokens": 0,
      "rejected_prediction_tokens": 0
    }
  },
  "system_fingerprint": null
}
```

this unfortunately doesn't do the actual function calling
- rather, it tells us **what function to call** + **what the arguments should be**!
- `json.loads` (like below) might give error because the LLM might not return a JSON, so we might need some guardrails

```python
response_message = response["choices"][0]["message"]
args = json.loads(response_message["function_call"]["arguments"])
# {'location': 'Boston', 'unit': 'celsius'}

# actually calling the method
get_current_weather(args)
```

what if we pass a msg that not's related to any function?
```python
messages = [
    {
        "role": "user",
        "content": "hi!",
    }
]
response = openai.ChatCompletion.create(
    # OpenAI Updates: As of June 2024, we are now using the GPT-3.5-Turbo model
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions,
)
```

```
{
  "id": "chatcmpl-BshNO9H4Q8wTwKPzKoMxxzxEh381R",
  "object": "chat.completion",
  "created": 1752376150,
  "model": "gpt-3.5-turbo-0125",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "Hello! How can I assist you today?",
        "refusal": null,
        "annotations": []
      },
      "logprobs": null,
      "finish_reason": "stop"
    }
  ],
```
- there is NO function call here!
- by default, the setting is `function_call:"auto"`, which we have rn (1st we see `function_call` and now we don't)

# function call options
- default is `function_call: "auto"`
```python
messages = [
    {
        "role": "user",
        "content": "hi!",
    }
]
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions,
    function_call="auto",
)
print(response)
```

- `none` mode for `0` function calls
```python
messages = [
    {
        "role": "user",
        "content": "hi!",
    }
]
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions,
    function_call="none",
)
print(response)
```

- force call a function
```python
messages = [
    {
        "role": "user",
        "content": "hi!",
    }
]
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions,
    function_call={"name": "get_current_weather"},
)
print(response)
```

>[!Note]
>- `function_call` and `functions` count for token rates (cost!)
# using output to LLM for final res
```python
messages = [
    {
        "role": "user",
        "content": "What's the weather like in Boston!",
    }
]
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions,
    function_call={"name": "get_current_weather"},
)
messages.append(response["choices"][0]["message"])
args = json.loads(response["choices"][0]["message"]['function_call']['arguments'])
observation = get_current_weather(args)
messages.append(
        {
            "role": "function",
            "name": "get_current_weather",
            "content": observation,
        }
)
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages,
)
print(response)
```

```
{
  "id": "chatcmpl-BFWKVL8edWMlnpwxIT0gAvcmOrNyq",
  "object": "chat.completion",
  "created": 1743038895,
  "model": "gpt-3.5-turbo-0125",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "The current weather in Boston is sunny and windy with a temperature of 72\u00b0F (22\u00b0C).",
        "refusal": null,
        "annotations": []
      },
      "logprobs": null,
      "finish_reason": "stop"
    }
  ],
  "usage": {
    "prompt_tokens": 84,
    "completion_tokens": 21,
    "total_tokens": 105,
    "prompt_tokens_details": {
      "cached_tokens": 0,
      "audio_tokens": 0
    },
    "completion_tokens_details": {
      "reasoning_tokens": 0,
      "audio_tokens": 0,
      "accepted_prediction_tokens": 0,
      "rejected_prediction_tokens": 0
    }
  },
  "service_tier": "default",
  "system_fingerprint": null
}
```