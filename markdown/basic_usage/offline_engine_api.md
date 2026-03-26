# Offline Engine API

SGLang provides a direct inference engine without the need for an HTTP server, especially for use cases where additional HTTP server adds unnecessary complexity or overhead. Here are two general use cases:

- Offline Batch Inference
- Custom Server on Top of the Engine

This document focuses on the offline batch inference, demonstrating four different inference modes:

- Non-streaming synchronous generation
- Streaming synchronous generation
- Non-streaming asynchronous generation
- Streaming asynchronous generation

Additionally, you can easily build a custom server on top of the SGLang offline engine. A detailed example working in a python script can be found in [custom_server](https://github.com/sgl-project/sglang/blob/main/examples/runtime/engine/custom_server.py).



## Nest Asyncio
Note that if you want to use **Offline Engine** in ipython or some other nested loop code, you need to add the following code:
```python
import nest_asyncio

nest_asyncio.apply()

```

## Advanced Usage

The engine supports [vlm inference](https://github.com/sgl-project/sglang/blob/main/examples/runtime/engine/offline_batch_inference_vlm.py) as well as [extracting hidden states](https://github.com/sgl-project/sglang/blob/main/examples/runtime/hidden_states). 

Please see [the examples](https://github.com/sgl-project/sglang/tree/main/examples/runtime/engine) for further use cases.

## Offline Batch Inference

SGLang offline engine supports batch inference with efficient scheduling.


```python
# launch the offline engine
import asyncio

import sglang as sgl
import sglang.test.doc_patch  # noqa: F401
from sglang.utils import async_stream_and_merge, stream_and_merge

llm = sgl.Engine(model_path="qwen/qwen2.5-0.5b-instruct")
```

    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-26 13:27:04.216 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 13:27:04] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 13:27:04.216 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 13:27:04] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 13:27:04.216 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 13:27:04] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 13:27:04.216 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 13:27:04] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 13:27:04.216 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 13:27:04] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.12it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.11it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 14.27it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 14.27it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 14.27it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 14.27it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 14.27it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 14.27it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 14.27it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 14.27it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:02<00:01, 20.90it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:02<00:01, 20.90it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:02<00:01, 20.90it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:02<00:01, 20.90it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:02<00:01, 20.90it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:02<00:01, 20.90it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:02<00:01, 20.90it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:02<00:01, 20.90it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:02<00:00, 27.94it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:02<00:00, 27.94it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:02<00:00, 27.94it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:02<00:00, 27.94it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:02<00:00, 27.94it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:02<00:00, 27.94it/s]

    Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:02<00:00, 27.94it/s]Compiling num tokens (num_tokens=208):  57%|█████▋    | 33/58 [00:02<00:00, 27.94it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:02<00:00, 34.52it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:02<00:00, 34.52it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:03<00:00, 34.52it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:03<00:00, 34.52it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:03<00:00, 34.52it/s]Compiling num tokens (num_tokens=128):  69%|██████▉   | 40/58 [00:03<00:00, 34.52it/s]Compiling num tokens (num_tokens=112):  69%|██████▉   | 40/58 [00:03<00:00, 34.52it/s]Compiling num tokens (num_tokens=96):  69%|██████▉   | 40/58 [00:03<00:00, 34.52it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:03<00:00, 40.51it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:03<00:00, 40.51it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:03<00:00, 40.51it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:03<00:00, 40.51it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:03<00:00, 40.51it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:03<00:00, 40.51it/s]

    Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:03<00:00, 40.51it/s]Compiling num tokens (num_tokens=20):  81%|████████  | 47/58 [00:03<00:00, 40.51it/s]Compiling num tokens (num_tokens=16):  81%|████████  | 47/58 [00:03<00:00, 40.51it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:03<00:00, 48.58it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:03<00:00, 48.58it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:03<00:00, 48.58it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:03<00:00, 48.58it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.93it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=76.80 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.77 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.77 GB):   3%|▎         | 2/58 [00:00<00:03, 18.24it/s]Capturing num tokens (num_tokens=7168 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:03, 18.24it/s]Capturing num tokens (num_tokens=6656 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:03, 18.24it/s]Capturing num tokens (num_tokens=6144 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:03, 18.24it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 21.65it/s]Capturing num tokens (num_tokens=5632 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 21.65it/s]Capturing num tokens (num_tokens=5120 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 21.65it/s]Capturing num tokens (num_tokens=4608 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 21.65it/s]Capturing num tokens (num_tokens=4096 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 21.65it/s]Capturing num tokens (num_tokens=4096 avail_mem=76.76 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.71it/s]Capturing num tokens (num_tokens=3840 avail_mem=76.75 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.71it/s]Capturing num tokens (num_tokens=3584 avail_mem=76.75 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.71it/s]Capturing num tokens (num_tokens=3328 avail_mem=76.74 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.71it/s]Capturing num tokens (num_tokens=3072 avail_mem=76.74 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.71it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=76.74 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.71it/s]Capturing num tokens (num_tokens=2816 avail_mem=76.74 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.93it/s]Capturing num tokens (num_tokens=2560 avail_mem=76.73 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.93it/s]Capturing num tokens (num_tokens=2304 avail_mem=76.73 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.93it/s]Capturing num tokens (num_tokens=2048 avail_mem=76.73 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.93it/s]Capturing num tokens (num_tokens=1792 avail_mem=76.72 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.93it/s]Capturing num tokens (num_tokens=1536 avail_mem=76.72 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.93it/s]Capturing num tokens (num_tokens=1536 avail_mem=76.72 GB):  33%|███▎      | 19/58 [00:00<00:01, 38.35it/s]Capturing num tokens (num_tokens=1280 avail_mem=76.72 GB):  33%|███▎      | 19/58 [00:00<00:01, 38.35it/s]Capturing num tokens (num_tokens=1024 avail_mem=76.70 GB):  33%|███▎      | 19/58 [00:00<00:01, 38.35it/s]Capturing num tokens (num_tokens=960 avail_mem=76.71 GB):  33%|███▎      | 19/58 [00:00<00:01, 38.35it/s] Capturing num tokens (num_tokens=896 avail_mem=76.71 GB):  33%|███▎      | 19/58 [00:00<00:01, 38.35it/s]Capturing num tokens (num_tokens=832 avail_mem=76.70 GB):  33%|███▎      | 19/58 [00:00<00:01, 38.35it/s]

    Capturing num tokens (num_tokens=768 avail_mem=76.70 GB):  33%|███▎      | 19/58 [00:00<00:01, 38.35it/s]Capturing num tokens (num_tokens=768 avail_mem=76.70 GB):  43%|████▎     | 25/58 [00:00<00:00, 42.86it/s]Capturing num tokens (num_tokens=704 avail_mem=76.70 GB):  43%|████▎     | 25/58 [00:00<00:00, 42.86it/s]Capturing num tokens (num_tokens=640 avail_mem=76.69 GB):  43%|████▎     | 25/58 [00:00<00:00, 42.86it/s]Capturing num tokens (num_tokens=576 avail_mem=76.69 GB):  43%|████▎     | 25/58 [00:00<00:00, 42.86it/s]Capturing num tokens (num_tokens=512 avail_mem=76.68 GB):  43%|████▎     | 25/58 [00:00<00:00, 42.86it/s]Capturing num tokens (num_tokens=480 avail_mem=76.70 GB):  43%|████▎     | 25/58 [00:00<00:00, 42.86it/s]Capturing num tokens (num_tokens=448 avail_mem=76.70 GB):  43%|████▎     | 25/58 [00:00<00:00, 42.86it/s]Capturing num tokens (num_tokens=448 avail_mem=76.70 GB):  53%|█████▎    | 31/58 [00:00<00:00, 45.50it/s]Capturing num tokens (num_tokens=416 avail_mem=76.69 GB):  53%|█████▎    | 31/58 [00:00<00:00, 45.50it/s]Capturing num tokens (num_tokens=384 avail_mem=76.69 GB):  53%|█████▎    | 31/58 [00:00<00:00, 45.50it/s]Capturing num tokens (num_tokens=352 avail_mem=76.69 GB):  53%|█████▎    | 31/58 [00:00<00:00, 45.50it/s]Capturing num tokens (num_tokens=320 avail_mem=76.68 GB):  53%|█████▎    | 31/58 [00:00<00:00, 45.50it/s]

    Capturing num tokens (num_tokens=288 avail_mem=76.68 GB):  53%|█████▎    | 31/58 [00:00<00:00, 45.50it/s]Capturing num tokens (num_tokens=256 avail_mem=76.68 GB):  53%|█████▎    | 31/58 [00:00<00:00, 45.50it/s]Capturing num tokens (num_tokens=256 avail_mem=76.68 GB):  64%|██████▍   | 37/58 [00:00<00:00, 47.34it/s]Capturing num tokens (num_tokens=240 avail_mem=76.68 GB):  64%|██████▍   | 37/58 [00:00<00:00, 47.34it/s]Capturing num tokens (num_tokens=224 avail_mem=76.67 GB):  64%|██████▍   | 37/58 [00:00<00:00, 47.34it/s]Capturing num tokens (num_tokens=208 avail_mem=76.67 GB):  64%|██████▍   | 37/58 [00:00<00:00, 47.34it/s]Capturing num tokens (num_tokens=192 avail_mem=76.67 GB):  64%|██████▍   | 37/58 [00:00<00:00, 47.34it/s]Capturing num tokens (num_tokens=176 avail_mem=76.66 GB):  64%|██████▍   | 37/58 [00:01<00:00, 47.34it/s]Capturing num tokens (num_tokens=160 avail_mem=76.66 GB):  64%|██████▍   | 37/58 [00:01<00:00, 47.34it/s]Capturing num tokens (num_tokens=160 avail_mem=76.66 GB):  74%|███████▍  | 43/58 [00:01<00:00, 48.50it/s]Capturing num tokens (num_tokens=144 avail_mem=76.66 GB):  74%|███████▍  | 43/58 [00:01<00:00, 48.50it/s]Capturing num tokens (num_tokens=128 avail_mem=76.65 GB):  74%|███████▍  | 43/58 [00:01<00:00, 48.50it/s]Capturing num tokens (num_tokens=112 avail_mem=76.65 GB):  74%|███████▍  | 43/58 [00:01<00:00, 48.50it/s]

    Capturing num tokens (num_tokens=96 avail_mem=76.65 GB):  74%|███████▍  | 43/58 [00:01<00:00, 48.50it/s] Capturing num tokens (num_tokens=80 avail_mem=76.64 GB):  74%|███████▍  | 43/58 [00:01<00:00, 48.50it/s]Capturing num tokens (num_tokens=64 avail_mem=76.64 GB):  74%|███████▍  | 43/58 [00:01<00:00, 48.50it/s]Capturing num tokens (num_tokens=64 avail_mem=76.64 GB):  84%|████████▍ | 49/58 [00:01<00:00, 49.18it/s]Capturing num tokens (num_tokens=48 avail_mem=76.64 GB):  84%|████████▍ | 49/58 [00:01<00:00, 49.18it/s]Capturing num tokens (num_tokens=32 avail_mem=76.64 GB):  84%|████████▍ | 49/58 [00:01<00:00, 49.18it/s]Capturing num tokens (num_tokens=28 avail_mem=76.63 GB):  84%|████████▍ | 49/58 [00:01<00:00, 49.18it/s]Capturing num tokens (num_tokens=24 avail_mem=76.63 GB):  84%|████████▍ | 49/58 [00:01<00:00, 49.18it/s]Capturing num tokens (num_tokens=20 avail_mem=76.62 GB):  84%|████████▍ | 49/58 [00:01<00:00, 49.18it/s]Capturing num tokens (num_tokens=20 avail_mem=76.62 GB):  93%|█████████▎| 54/58 [00:01<00:00, 48.39it/s]Capturing num tokens (num_tokens=16 avail_mem=76.62 GB):  93%|█████████▎| 54/58 [00:01<00:00, 48.39it/s]Capturing num tokens (num_tokens=12 avail_mem=76.62 GB):  93%|█████████▎| 54/58 [00:01<00:00, 48.39it/s]

    Capturing num tokens (num_tokens=8 avail_mem=76.61 GB):  93%|█████████▎| 54/58 [00:01<00:00, 48.39it/s] Capturing num tokens (num_tokens=4 avail_mem=76.61 GB):  93%|█████████▎| 54/58 [00:01<00:00, 48.39it/s]Capturing num tokens (num_tokens=4 avail_mem=76.61 GB): 100%|██████████| 58/58 [00:01<00:00, 42.80it/s]


### Non-streaming Synchronous Generation


```python
prompts = [
    "Hello, my name is",
    "The president of the United States is",
    "The capital of France is",
    "The future of AI is",
]

sampling_params = {"temperature": 0.8, "top_p": 0.95}

outputs = llm.generate(prompts, sampling_params)
for prompt, output in zip(prompts, outputs):
    print("===============================")
    print(f"Prompt: {prompt}\nGenerated text: {output['text']}")
```

    ===============================
    Prompt: Hello, my name is
    Generated text:  John. I am a 16 year old boy. I have 11 years of experience in a "teacher" job. I learned many things about teaching. I know about group teaching, and I know the importance of patience. I know how to use different teaching methods. I can even teach physics! I can also give good advice to students. My passion for teaching is important. I love teaching and I want to teach more students. What can you do for me?
    Why does John like teaching? (If the question is unanswerable, say "unanswerable") John likes teaching because he has a passion for teaching
    ===============================
    Prompt: The president of the United States is
    Generated text:  a public office. His job is to help the country govern. He makes decisions and takes action to improve the country. The president works with other important people. They help him decide on important things. The president is the leader of the country. Many people think the president should make decisions for all the people in the country. But others think it is better for the president to work with other important people. Some people think the president has too much power. Others think the president should be held accountable (答案应填in charge of) for the choices they make. The president is important and in charge of the country, but he should still
    ===============================
    Prompt: The capital of France is
    Generated text:  a big, colorful building. It is called the Louvre. It is the most famous museum in the world. It has a big glass roof and it is very special. The Louvre is in the city of Paris, the capital of France. It has been a museum for over 200 years. A few years ago, the Louvre started building a new building. This building will be called the Musée de l'Enseignement Supérieur. The new building will be the first big building for a university in France. The Louvre has a long history. It was the first museum in France. People started
    ===============================
    Prompt: The future of AI is
    Generated text:  very exciting, and the research and development of AI algorithms is constantly evolving. Many researchers are working on creating new algorithms and technologies to help solve complex problems in fields such as healthcare, finance, and natural language processing.
    The development of AI algorithms is driven by a combination of factors, including advances in hardware, improvements in data processing, and advancements in machine learning techniques. Researchers are constantly working to improve the performance of existing algorithms and to develop new ones that can perform better.
    There are several types of AI algorithms, including supervised learning, unsupervised learning, and reinforcement learning. Each type of algorithm has its own strengths and weaknesses, and researchers


### Streaming Synchronous Generation


```python
prompts = [
    "Write a short, neutral self-introduction for a fictional character. Hello, my name is",
    "Provide a concise factual statement about France’s capital city. The capital of France is",
    "Explain possible future trends in artificial intelligence. The future of AI is",
]

sampling_params = {
    "temperature": 0.2,
    "top_p": 0.9,
}

print("\n=== Testing synchronous streaming generation with overlap removal ===\n")

for prompt in prompts:
    print(f"Prompt: {prompt}")
    merged_output = stream_and_merge(llm, prompt, sampling_params)
    print("Generated text:", merged_output)
    print()
```

    
    === Testing synchronous streaming generation with overlap removal ===
    
    Prompt: Write a short, neutral self-introduction for a fictional character. Hello, my name is


    Generated text:  [Name] and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a short, positive, enthusiastic statement about your personality or skills]. I'm always looking for new challenges and opportunities to grow and learn. I'm always eager to learn more about the world around me and to share my knowledge with others. I'm always looking for ways to make a positive impact in the world and to help others. I'm always eager to learn more about the world around me and to share my knowledge with others. I
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light. It is a historic city with a rich history dating back to the Roman Empire and the Middle Ages. Paris is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. The city is also famous for its fashion industry, art scene, and cuisine. Paris is a vibrant and dynamic city with a diverse population and a rich cultural heritage. It is the largest city in France by population and is a major economic and political center in Europe. The city is home to many world-renowned museums, theaters, and art galleries, as well
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we live, work, and interact with technology. Here are some of the most likely trends that are expected to shape the future of AI:
    
    1. Increased automation: One of the most significant trends in AI is the increasing automation of tasks that are currently performed by humans. This could include tasks such as data analysis, decision-making, and routine maintenance. As AI becomes more capable of performing these tasks, it is likely to become more efficient and less prone to errors.
    
    2. Improved privacy and security: As AI becomes more integrated into our daily lives, there is
    


### Non-streaming Asynchronous Generation


```python
prompts = [
    "Write a short, neutral self-introduction for a fictional character. Hello, my name is",
    "Provide a concise factual statement about France’s capital city. The capital of France is",
    "Explain possible future trends in artificial intelligence. The future of AI is",
]

sampling_params = {"temperature": 0.8, "top_p": 0.95}

print("\n=== Testing asynchronous batch generation ===")


async def main():
    outputs = await llm.async_generate(prompts, sampling_params)

    for prompt, output in zip(prompts, outputs):
        print(f"\nPrompt: {prompt}")
        print(f"Generated text: {output['text']}")


asyncio.run(main())
```

    
    === Testing asynchronous batch generation ===


    
    Prompt: Write a short, neutral self-introduction for a fictional character. Hello, my name is
    Generated text:  [Name] and I'm a/an [occupation]!
    
    I'm [Age] years old. I'm [Gender] and I live in [Location]. I've always been [insert one or two things about you that make you unique] and I'm always [insert one or two things about you that make you stand out].
    
    I've always had a passion for [insert something you're passionate about] and I love [insert one or two things about it]. I've been studying [insert one or two things about it] for years now, and I'm [insert what you think you've gained from it]. I've been
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, often referred to as "the city of a thousand gardens". The city is home to many world-renowned museums, gardens, and historical landmarks, and is known for its vibrant culture, wine, and cuisine. It is the third most populous city in the world and has been an important center of European politics, culture, and art since the 11th century. Paris has a rich and diverse history dating back to the ancient Greeks, Romans, and French nobility, and has been the locus of important political, artistic, and scientific developments throughout history. Today, it remains a major cultural and economic hub in Europe. 
    
    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  one of rapid evolution, with new technologies and applications emerging all the time. Here are some possible future trends in AI:
    
    1. Smart homes: With the increase in AI technology, we can expect more advanced AI-powered devices in our homes. These devices can learn our preferences, routines, and habits to make our lives more comfortable and efficient. In the future, smart homes may also be able to detect potential safety hazards and alert us in real-time.
    
    2. Autonomous vehicles: As the technology improves, we can expect more advanced AI systems in autonomous vehicles. These vehicles can learn from our behavior, road conditions, and traffic patterns to make decisions


### Streaming Asynchronous Generation


```python
prompts = [
    "Write a short, neutral self-introduction for a fictional character. Hello, my name is",
    "Provide a concise factual statement about France’s capital city. The capital of France is",
    "Explain possible future trends in artificial intelligence. The future of AI is",
]

sampling_params = {"temperature": 0.8, "top_p": 0.95}

print("\n=== Testing asynchronous streaming generation (no repeats) ===")


async def main():
    for prompt in prompts:
        print(f"\nPrompt: {prompt}")
        print("Generated text: ", end="", flush=True)

        # Replace direct calls to async_generate with our custom overlap-aware version
        async for cleaned_chunk in async_stream_and_merge(llm, prompt, sampling_params):
            print(cleaned_chunk, end="", flush=True)

        print()  # New line after each prompt


asyncio.run(main())
```

    
    === Testing asynchronous streaming generation (no repeats) ===
    
    Prompt: Write a short, neutral self-introduction for a fictional character. Hello, my name is
    Generated text: 

     [

    Name

    ]

     and

     I

    'm

     a

     [

    short

     answer

     for

     your

     job

     or

     current

     position

    ].

     I

     have

     [

    number

     of

     years

     of

     experience

    ].

     I

     have

     always

     been

     passionate

     about

     [

    why

     you

     are

     passionate

     about

     what

     you

     do

    ].

     I

     enjoy

     [

    the

     things

     you

     enjoy

     doing

     at

     work

    ],

     [

    what

     you

     enjoy

     doing

     outside

     of

     work

    ].

     I

    'm

     always

     looking

     for

     ways

     to

     [

    what

     you

     like

     to

     learn

     or

     grow

    ].

     I

    'm

     a

     [

    what

     kind

     of

     person

     you

     are

     as

     a

     person

    ].

     If

     you

     want

     to

     talk

     to

     me

    ,

     please

     ask

     me

     [

    what

     you

    'd

     like

     to

     know

     about

     me

    ].

     Hello

    ,

     my

     name

     is

     [

    Name

    ]

     and

     I

    'm

     a

     [

    short

     answer

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     a

     city

     renowned

     for

     its

     rich

     cultural

     heritage

     and

     stunning

     architecture

    .

     It

     is

     home

     to

     numerous

     museums

    ,

     museums

    ,

     and

     galleries

    ,

     as

     well

     as

     the

     Notre

    -D

    ame

     Cathedral

    ,

     a

     stunning

     Gothic

     structure

     that

     stands

     as

     a

     symbol

     of

     French

     Catholic

    ism

    .

     Visitors

     to

     Paris

     can

     also

     explore

     the

     historic

     Quarter

    ,

     known

     for

     its

     historic

     French

     Quarter

    ,

     bo

    hem

    ian

     atmosphere

    ,

     and

     diverse

     food

     offerings

    .

     The

     city

     is

     also

     known

     for

     its

     French

     cuisine

    ,

     including

     haute

     cuisine

     and

     its

     famous

     Paris

     steak

    ,

     as

     well

     as

     its

     cultural

     and

     historical

     significance

    .

     Overall

    ,

     Paris

     is

     a

     city

     that

     is

     a

     true

     symbol

     of

     France

     and

     offers

     visitors

     a

     unique

     blend

     of

     history

    ,

     culture

    ,

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     highly

     exciting

     and

     uncertain

    ,

     as

     it

     continues

     to

     evolve

     rapidly

    .

     Some

     possible

     trends

     that

     are

     currently

     gaining

     attention

     include

    :
    


    1

    .

     Increased

     automation

     and

     artificial

     intelligence

     in

     manufacturing

    :

     One

     of

     the

     most

     significant

     areas

     of

     AI

     development

     is

     in

     the

     field

     of

     manufacturing

    ,

     where

     machines

     and

     automation

     are

     becoming

     more

     and

     more

     sophisticated

    .

     We

     are

     seeing

     a

     shift

     towards

     more

     robotic

     and

     automated

     production

     lines

    ,

     which

     will

     likely

     lead

     to

     increased

     efficiency

    ,

     lower

     costs

    ,

     and

     reduced

     labor

     requirements

    .
    


    2

    .

     Improved

     natural

     language

     processing

    :

     With

     the

     increasing

     use

     of

     AI

     in

     areas

     such

     as

     healthcare

     and

     customer

     service

    ,

     natural

     language

     processing

     has

     become

     increasingly

     important

    .

     AI

    -powered

     chat

    bots

     and

     virtual

     assistants

     are

     becoming

     more

    



```python
llm.shutdown()
```
