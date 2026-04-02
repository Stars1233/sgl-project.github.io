# OpenAI APIs - Vision

SGLang provides OpenAI-compatible APIs to enable a smooth transition from OpenAI services to self-hosted local models.
A complete reference for the API is available in the [OpenAI API Reference](https://platform.openai.com/docs/guides/vision).
This tutorial covers the vision APIs for vision language models.

SGLang supports various vision language models such as Llama 3.2, LLaVA-OneVision, Qwen2.5-VL, Gemma3 and [more](../supported_models/text_generation/multimodal_language_models.md).

As an alternative to the OpenAI API, you can also use the [SGLang offline engine](https://github.com/sgl-project/sglang/blob/main/examples/runtime/engine/offline_batch_inference_vlm.py).

## Launch A Server

Launch the server in your terminal and wait for it to initialize.


```python
from sglang.test.doc_patch import launch_server_cmd
from sglang.utils import wait_for_server, print_highlight, terminate_process

example_image_url = "https://raw.githubusercontent.com/sgl-project/sglang/main/examples/assets/example_image.png"
logo_image_url = (
    "https://raw.githubusercontent.com/sgl-project/sglang/main/assets/logo.png"
)

vision_process, port = launch_server_cmd("""
python3 -m sglang.launch_server --model-path Qwen/Qwen2.5-VL-7B-Instruct --log-level warning
""")

wait_for_server(f"http://localhost:{port}", process=vision_process)
```

    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:172: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    2026-04-02 22:12:30.205 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 22:12:30] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 22:12:30.205 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 22:12:30] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 22:12:30.205 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 22:12:30] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 22:12:30.205 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 22:12:30] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 22:12:30.205 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 22:12:30] Persistent cache disabled, using in-memory JIT cache


    2026-04-02 22:12:40.488 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 22:12:40] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 22:12:40.488 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 22:12:40] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 22:12:40.488 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 22:12:40] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 22:12:40.488 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 22:12:40] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 22:12:40.489 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 22:12:40] Persistent cache disabled, using in-memory JIT cache


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Multi-thread loading shards:   0% Completed | 0/5 [00:00<?, ?it/s]

    Multi-thread loading shards:  20% Completed | 1/5 [00:00<00:02,  1.97it/s]

    Multi-thread loading shards:  40% Completed | 2/5 [00:00<00:01,  2.46it/s]

    Multi-thread loading shards:  60% Completed | 3/5 [00:01<00:00,  2.03it/s]

    Multi-thread loading shards:  80% Completed | 4/5 [00:02<00:00,  1.69it/s]

    Multi-thread loading shards: 100% Completed | 5/5 [00:02<00:00,  1.52it/s]Multi-thread loading shards: 100% Completed | 5/5 [00:02<00:00,  1.69it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)


    2026-04-02 22:12:52,808 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 22:12:52] Unexpected error during package walk: cutlass.cute.experimental



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>


## Using cURL

Once the server is up, you can send test requests using curl or requests.


```python
import subprocess

curl_command = f"""
curl -s http://localhost:{port}/v1/chat/completions \\
  -H "Content-Type: application/json" \\
  -d '{{
    "model": "Qwen/Qwen2.5-VL-7B-Instruct",
    "messages": [
      {{
        "role": "user",
        "content": [
          {{
            "type": "text",
            "text": "What’s in this image?"
          }},
          {{
            "type": "image_url",
            "image_url": {{
              "url": "{example_image_url}"
            }}
          }}
        ]
      }}
    ],
    "max_tokens": 300
  }}'
"""

response = subprocess.check_output(curl_command, shell=True).decode()
print_highlight(response)


response = subprocess.check_output(curl_command, shell=True).decode()
print_highlight(response)
```

    /actions-runner/_work/sglang/sglang/python/sglang/srt/utils/common.py:799: UserWarning: The given buffer is not writable, and PyTorch does not support non-writable tensors. This means you can write to the underlying (supposedly non-writable) buffer using the tensor. You may want to copy the buffer to protect its data or make it writable before converting it to a tensor. This type of warning will be suppressed for the rest of this program. (Triggered internally at /pytorch/torch/csrc/utils/tensor_new.cpp:1581.)
      encoded_image = torch.frombuffer(image_bytes, dtype=torch.uint8)



<strong style='color: #00008B;'>{"id":"bba982826c5d4180930adc03ebc7c9ff","object":"chat.completion","created":1775167978,"model":"Qwen/Qwen2.5-VL-7B-Instruct","choices":[{"index":0,"message":{"role":"assistant","content":"The image shows a man standing on the back of a yellow taxi, ironing a blue shirt. The taxi is parked on a city street with other vehicles and buildings in the background. The man appears to be balancing on the tailgate while performing this task. The scene seems to be set in an urban environment, possibly in New York City, given the style of the taxi and the architecture visible in the background.","reasoning_content":null,"tool_calls":null},"logprobs":null,"finish_reason":"stop","matched_stop":151645}],"usage":{"prompt_tokens":307,"total_tokens":391,"completion_tokens":84,"prompt_tokens_details":null,"reasoning_tokens":0},"metadata":{"weight_version":"default"}}</strong>



<strong style='color: #00008B;'>{"id":"f597478e7e61438ca174a254bb50a34a","object":"chat.completion","created":1775167979,"model":"Qwen/Qwen2.5-VL-7B-Instruct","choices":[{"index":0,"message":{"role":"assistant","content":"The image shows a man standing on the back of a yellow taxi, ironing a piece of clothing. The taxi is parked on a city street, and there are other taxis visible in the background. The man appears to be balancing on the tailgate while ironing, which is an unusual and humorous scene. The setting suggests an urban environment with buildings and flags in the background.","reasoning_content":null,"tool_calls":null},"logprobs":null,"finish_reason":"stop","matched_stop":151645}],"usage":{"prompt_tokens":307,"total_tokens":384,"completion_tokens":77,"prompt_tokens_details":null,"reasoning_tokens":0},"metadata":{"weight_version":"default"}}</strong>


## Using Python Requests


```python
import requests

url = f"http://localhost:{port}/v1/chat/completions"

data = {
    "model": "Qwen/Qwen2.5-VL-7B-Instruct",
    "messages": [
        {
            "role": "user",
            "content": [
                {"type": "text", "text": "What’s in this image?"},
                {
                    "type": "image_url",
                    "image_url": {"url": example_image_url},
                },
            ],
        }
    ],
    "max_tokens": 300,
}

response = requests.post(url, json=data)
print_highlight(response.text)
```


<strong style='color: #00008B;'>{"id":"da5d6c3c4393454b8f848cf4bd12691f","object":"chat.completion","created":1775167980,"model":"Qwen/Qwen2.5-VL-7B-Instruct","choices":[{"index":0,"message":{"role":"assistant","content":"The image shows a man standing on the back of a yellow taxi, ironing a blue shirt. The taxi is parked on a city street with other vehicles and buildings in the background. The man appears to be balancing on the tailgate while performing the task. The scene suggests an unusual or humorous situation, as it is not typical for someone to iron clothes from the back of a moving vehicle.","reasoning_content":null,"tool_calls":null},"logprobs":null,"finish_reason":"stop","matched_stop":151645}],"usage":{"prompt_tokens":307,"total_tokens":387,"completion_tokens":80,"prompt_tokens_details":null,"reasoning_tokens":0},"metadata":{"weight_version":"default"}}</strong>


## Using OpenAI Python Client


```python
from openai import OpenAI

client = OpenAI(base_url=f"http://localhost:{port}/v1", api_key="None")

response = client.chat.completions.create(
    model="Qwen/Qwen2.5-VL-7B-Instruct",
    messages=[
        {
            "role": "user",
            "content": [
                {
                    "type": "text",
                    "text": "What is in this image?",
                },
                {
                    "type": "image_url",
                    "image_url": {"url": example_image_url},
                },
            ],
        }
    ],
    max_tokens=300,
)

print_highlight(response.choices[0].message.content)
```


<strong style='color: #00008B;'>The image shows a man standing on the back of a yellow taxi, ironing a piece of clothing. The taxi is parked on a city street, and there are other taxis visible in the background. The man appears to be balancing on the tailgate while ironing, which is an unusual and humorous scene. The setting suggests it might be in a busy urban area with tall buildings and flags in the background.</strong>


## Multiple-Image Inputs

The server also supports multiple images and interleaved text and images if the model supports it.


```python
from openai import OpenAI

client = OpenAI(base_url=f"http://localhost:{port}/v1", api_key="None")

response = client.chat.completions.create(
    model="Qwen/Qwen2.5-VL-7B-Instruct",
    messages=[
        {
            "role": "user",
            "content": [
                {
                    "type": "image_url",
                    "image_url": {
                        "url": example_image_url,
                    },
                },
                {
                    "type": "image_url",
                    "image_url": {
                        "url": logo_image_url,
                    },
                },
                {
                    "type": "text",
                    "text": "I have two very different images. They are not related at all. "
                    "Please describe the first image in one sentence, and then describe the second image in another sentence.",
                },
            ],
        }
    ],
    temperature=0,
)

print_highlight(response.choices[0].message.content)
```


<strong style='color: #00008B;'>The first image shows a man ironing clothes on the back of a taxi in an urban setting. The second image is a stylized logo featuring the letters "SGL" with a book and a computer icon incorporated into the design.</strong>



```python
terminate_process(vision_process)
```
