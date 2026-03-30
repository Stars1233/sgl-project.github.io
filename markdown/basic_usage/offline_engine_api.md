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


    2026-03-30 17:31:35.848 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 17:31:35] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 17:31:35.848 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 17:31:35] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 17:31:35.848 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 17:31:35] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 17:31:35.848 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 17:31:35] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 17:31:35.848 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 17:31:35] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.64it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.64it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:03,  2.16s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:03,  2.16s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:03,  2.16s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:03,  2.16s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:23,  2.29it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:23,  2.29it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:23,  2.29it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:23,  2.29it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:23,  2.29it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:23,  2.29it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:23,  2.29it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:06,  6.93it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:06,  6.93it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:06,  6.93it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:06,  6.93it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:06,  6.93it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:06,  6.93it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:06,  6.93it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:06,  6.93it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:06,  6.93it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:02<00:06,  6.93it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 15.34it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 15.34it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 15.34it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 15.34it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 15.34it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 15.34it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 15.34it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 15.34it/s]

    Compiling num tokens (num_tokens=640):  33%|███▎      | 19/58 [00:02<00:02, 15.34it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:02<00:01, 23.34it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:02<00:01, 23.34it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:02<00:01, 23.34it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:02<00:01, 23.34it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:02<00:01, 23.34it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:02<00:01, 23.34it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:02<00:01, 23.34it/s]Compiling num tokens (num_tokens=352):  47%|████▋     | 27/58 [00:02<00:01, 23.34it/s]Compiling num tokens (num_tokens=320):  47%|████▋     | 27/58 [00:02<00:01, 23.34it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:02<00:00, 31.64it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:02<00:00, 31.64it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:02<00:00, 31.64it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:02<00:00, 31.64it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:02<00:00, 31.64it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:02<00:00, 31.64it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:02<00:00, 31.64it/s]

    Compiling num tokens (num_tokens=176):  60%|██████    | 35/58 [00:02<00:00, 31.64it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:02<00:00, 38.33it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:02<00:00, 38.33it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:02<00:00, 38.33it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:02<00:00, 38.33it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:02<00:00, 38.33it/s]Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:02<00:00, 38.33it/s] Compiling num tokens (num_tokens=80):  72%|███████▏  | 42/58 [00:02<00:00, 38.33it/s]Compiling num tokens (num_tokens=64):  72%|███████▏  | 42/58 [00:02<00:00, 38.33it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:02<00:00, 44.31it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:02<00:00, 44.31it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:02<00:00, 44.31it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:02<00:00, 44.31it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:02<00:00, 44.31it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:02<00:00, 44.31it/s]Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:02<00:00, 44.31it/s]Compiling num tokens (num_tokens=12):  84%|████████▍ | 49/58 [00:02<00:00, 44.31it/s]Compiling num tokens (num_tokens=8):  84%|████████▍ | 49/58 [00:02<00:00, 44.31it/s] Compiling num tokens (num_tokens=4):  84%|████████▍ | 49/58 [00:02<00:00, 44.31it/s]

    Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:02<00:00, 19.37it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=76.79 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.76 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:02, 19.73it/s]Capturing num tokens (num_tokens=7168 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:02, 19.73it/s]Capturing num tokens (num_tokens=6656 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:02, 19.73it/s]Capturing num tokens (num_tokens=6144 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:02, 19.73it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 23.06it/s]Capturing num tokens (num_tokens=5632 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 23.06it/s]Capturing num tokens (num_tokens=5120 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 23.06it/s]Capturing num tokens (num_tokens=4608 avail_mem=76.75 GB):   9%|▊         | 5/58 [00:00<00:02, 23.06it/s]Capturing num tokens (num_tokens=4096 avail_mem=76.75 GB):   9%|▊         | 5/58 [00:00<00:02, 23.06it/s]Capturing num tokens (num_tokens=4096 avail_mem=76.75 GB):  16%|█▌        | 9/58 [00:00<00:01, 28.27it/s]Capturing num tokens (num_tokens=3840 avail_mem=76.75 GB):  16%|█▌        | 9/58 [00:00<00:01, 28.27it/s]Capturing num tokens (num_tokens=3584 avail_mem=76.74 GB):  16%|█▌        | 9/58 [00:00<00:01, 28.27it/s]Capturing num tokens (num_tokens=3328 avail_mem=76.74 GB):  16%|█▌        | 9/58 [00:00<00:01, 28.27it/s]Capturing num tokens (num_tokens=3072 avail_mem=76.74 GB):  16%|█▌        | 9/58 [00:00<00:01, 28.27it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=76.74 GB):  16%|█▌        | 9/58 [00:00<00:01, 28.27it/s]Capturing num tokens (num_tokens=2816 avail_mem=76.74 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.56it/s]Capturing num tokens (num_tokens=2560 avail_mem=76.73 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.56it/s]Capturing num tokens (num_tokens=2304 avail_mem=76.73 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.56it/s]Capturing num tokens (num_tokens=2048 avail_mem=76.73 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.56it/s]Capturing num tokens (num_tokens=1792 avail_mem=76.70 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.56it/s]Capturing num tokens (num_tokens=1792 avail_mem=76.70 GB):  31%|███       | 18/58 [00:00<00:01, 30.64it/s]Capturing num tokens (num_tokens=1536 avail_mem=76.70 GB):  31%|███       | 18/58 [00:00<00:01, 30.64it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=76.69 GB):  31%|███       | 18/58 [00:00<00:01, 30.64it/s]Capturing num tokens (num_tokens=1024 avail_mem=76.67 GB):  31%|███       | 18/58 [00:00<00:01, 30.64it/s]Capturing num tokens (num_tokens=960 avail_mem=76.67 GB):  31%|███       | 18/58 [00:00<00:01, 30.64it/s] Capturing num tokens (num_tokens=960 avail_mem=76.67 GB):  38%|███▊      | 22/58 [00:00<00:01, 31.83it/s]Capturing num tokens (num_tokens=896 avail_mem=76.19 GB):  38%|███▊      | 22/58 [00:00<00:01, 31.83it/s]Capturing num tokens (num_tokens=832 avail_mem=76.10 GB):  38%|███▊      | 22/58 [00:00<00:01, 31.83it/s]Capturing num tokens (num_tokens=768 avail_mem=76.03 GB):  38%|███▊      | 22/58 [00:00<00:01, 31.83it/s]Capturing num tokens (num_tokens=704 avail_mem=76.02 GB):  38%|███▊      | 22/58 [00:00<00:01, 31.83it/s]Capturing num tokens (num_tokens=640 avail_mem=76.02 GB):  38%|███▊      | 22/58 [00:00<00:01, 31.83it/s]Capturing num tokens (num_tokens=576 avail_mem=76.02 GB):  38%|███▊      | 22/58 [00:00<00:01, 31.83it/s]Capturing num tokens (num_tokens=576 avail_mem=76.02 GB):  48%|████▊     | 28/58 [00:00<00:00, 38.23it/s]Capturing num tokens (num_tokens=512 avail_mem=76.01 GB):  48%|████▊     | 28/58 [00:00<00:00, 38.23it/s]

    Capturing num tokens (num_tokens=480 avail_mem=76.02 GB):  48%|████▊     | 28/58 [00:00<00:00, 38.23it/s]Capturing num tokens (num_tokens=448 avail_mem=76.02 GB):  48%|████▊     | 28/58 [00:00<00:00, 38.23it/s]Capturing num tokens (num_tokens=416 avail_mem=76.02 GB):  48%|████▊     | 28/58 [00:00<00:00, 38.23it/s]Capturing num tokens (num_tokens=384 avail_mem=76.02 GB):  48%|████▊     | 28/58 [00:00<00:00, 38.23it/s]Capturing num tokens (num_tokens=352 avail_mem=76.01 GB):  48%|████▊     | 28/58 [00:00<00:00, 38.23it/s]Capturing num tokens (num_tokens=352 avail_mem=76.01 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.90it/s]Capturing num tokens (num_tokens=320 avail_mem=76.01 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.90it/s]Capturing num tokens (num_tokens=288 avail_mem=76.00 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.90it/s]Capturing num tokens (num_tokens=256 avail_mem=76.00 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.90it/s]Capturing num tokens (num_tokens=240 avail_mem=76.00 GB):  59%|█████▊    | 34/58 [00:01<00:00, 42.90it/s]Capturing num tokens (num_tokens=224 avail_mem=76.00 GB):  59%|█████▊    | 34/58 [00:01<00:00, 42.90it/s]Capturing num tokens (num_tokens=208 avail_mem=75.99 GB):  59%|█████▊    | 34/58 [00:01<00:00, 42.90it/s]

    Capturing num tokens (num_tokens=208 avail_mem=75.99 GB):  69%|██████▉   | 40/58 [00:01<00:00, 46.34it/s]Capturing num tokens (num_tokens=192 avail_mem=75.99 GB):  69%|██████▉   | 40/58 [00:01<00:00, 46.34it/s]Capturing num tokens (num_tokens=176 avail_mem=75.99 GB):  69%|██████▉   | 40/58 [00:01<00:00, 46.34it/s]Capturing num tokens (num_tokens=160 avail_mem=75.99 GB):  69%|██████▉   | 40/58 [00:01<00:00, 46.34it/s]Capturing num tokens (num_tokens=144 avail_mem=75.98 GB):  69%|██████▉   | 40/58 [00:01<00:00, 46.34it/s]Capturing num tokens (num_tokens=128 avail_mem=75.98 GB):  69%|██████▉   | 40/58 [00:01<00:00, 46.34it/s]Capturing num tokens (num_tokens=112 avail_mem=75.98 GB):  69%|██████▉   | 40/58 [00:01<00:00, 46.34it/s]Capturing num tokens (num_tokens=112 avail_mem=75.98 GB):  79%|███████▉  | 46/58 [00:01<00:00, 48.56it/s]Capturing num tokens (num_tokens=96 avail_mem=75.97 GB):  79%|███████▉  | 46/58 [00:01<00:00, 48.56it/s] Capturing num tokens (num_tokens=80 avail_mem=75.97 GB):  79%|███████▉  | 46/58 [00:01<00:00, 48.56it/s]Capturing num tokens (num_tokens=64 avail_mem=75.97 GB):  79%|███████▉  | 46/58 [00:01<00:00, 48.56it/s]Capturing num tokens (num_tokens=48 avail_mem=75.96 GB):  79%|███████▉  | 46/58 [00:01<00:00, 48.56it/s]Capturing num tokens (num_tokens=32 avail_mem=75.96 GB):  79%|███████▉  | 46/58 [00:01<00:00, 48.56it/s]

    Capturing num tokens (num_tokens=28 avail_mem=75.95 GB):  79%|███████▉  | 46/58 [00:01<00:00, 48.56it/s]Capturing num tokens (num_tokens=28 avail_mem=75.95 GB):  90%|████████▉ | 52/58 [00:01<00:00, 49.93it/s]Capturing num tokens (num_tokens=24 avail_mem=75.95 GB):  90%|████████▉ | 52/58 [00:01<00:00, 49.93it/s]Capturing num tokens (num_tokens=20 avail_mem=75.95 GB):  90%|████████▉ | 52/58 [00:01<00:00, 49.93it/s]Capturing num tokens (num_tokens=16 avail_mem=75.95 GB):  90%|████████▉ | 52/58 [00:01<00:00, 49.93it/s]Capturing num tokens (num_tokens=12 avail_mem=75.94 GB):  90%|████████▉ | 52/58 [00:01<00:00, 49.93it/s]Capturing num tokens (num_tokens=8 avail_mem=75.94 GB):  90%|████████▉ | 52/58 [00:01<00:00, 49.93it/s] Capturing num tokens (num_tokens=4 avail_mem=75.94 GB):  90%|████████▉ | 52/58 [00:01<00:00, 49.93it/s]Capturing num tokens (num_tokens=4 avail_mem=75.94 GB): 100%|██████████| 58/58 [00:01<00:00, 51.35it/s]Capturing num tokens (num_tokens=4 avail_mem=75.94 GB): 100%|██████████| 58/58 [00:01<00:00, 41.61it/s]


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
    Generated text:  Aisha from the United States. I am a Senior at the University of Alabama. I have been a professional basketball player for the Birmingham Aztecs since the 2011-2012 season. Before college, I played in several minor league basketball.
    I have participated in a number of events, including the Women’s National Basketball Association Women’s All-Star Game and the Alabama Tournament. I play volleyball, playing for the University of Alabama Volleyball team and am in the Associated Women’s Volleyball League. I also play soccer, playing for the Birmingham Aztecs under the direction of my father, Tony.
    My father
    ===============================
    Prompt: The president of the United States is
    Generated text:  a person. Which of the following statements about the president of the United States is correct?
    
    A: He can be the head of state.
    B: He is a person who represents the country in various matters.
    C: He is an elected official.
    D: He holds office for life. To determine the correct statement about the president of the United States, let's analyze each option step by step:
    
    A: He can be the head of state.
    - This statement is incorrect. The president is indeed the head of state, but not the only head of state. The other heads of state are the Vice President and the Congress. So,
    ===============================
    Prompt: The capital of France is
    Generated text:  _______.
    A. Paris
    B. Lyon
    C. Marseille
    D. Geneva
    答案：A。巴黎（Pâis)，又称凡尔赛（Fontaine),是法国的首都，位于法国中部的东南部，是法国最大的城市，也是欧洲最美丽的城市之一。巴黎是法国的首都、文化和经济中心，也是世界上最受旅游者的欢迎的城市之一，每年吸引着数以百万计的游客。巴黎以巴黎圣母院、卢浮宫、凯旋门等著名建筑而闻名。
    
    答案 C. Marseille 是法国的第三大城市，位于法国东南部，但不是法国
    ===============================
    Prompt: The future of AI is
    Generated text:  important, and there are many different types of AI. This is especially true in the realm of healthcare, where the potential impact of AI in improving patient outcomes and reducing costs is widely discussed.
    There are several different types of AI that can be used in healthcare, including natural language processing, computer vision, and machine learning. These AI technologies can be used to analyze medical data, diagnose diseases, and predict patient outcomes.
    One of the most promising areas of AI in healthcare is in the field of machine learning. Machine learning algorithms can be used to identify patterns in medical data that can be used to improve diagnosis and treatment decisions. For example, machine


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light. It is a bustling metropolis with a rich history and a diverse population of over 10 million people. The city is home to iconic landmarks such as the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral, as well as a vibrant arts scene and a thriving food and wine industry. Paris is a cultural and political center of France, and its influence can be seen in its architecture, cuisine, and fashion. The city is also known for its annual festivals and events, including the World Cup of Football and the Eiffel Tower Festival. Overall, Paris
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction. Here are some of the most likely trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn and adapt to human behavior and preferences. This could lead to more personalized and effective AI systems.
    
    2. Greater emphasis on ethical considerations: As AI becomes more integrated with human intelligence, there will be a greater emphasis on ethical considerations. This could lead to more robust AI systems that are designed to be transparent, accountable, and responsible.
    
    3. Increased use of AI in healthcare: AI is
    


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
    Generated text:  [Name]. I'm a [Occupation] who has been practicing self-improvement for over [X] years now. I'm a [Job Title] who has always been passionate about [Motto or Cause]. I'm always up for new challenges and have a deep-seated [Qualification] in [Field/Industry]. I'm dedicated to [Mission/Goal], [Reason why], and I'm always looking for ways to [Relevant Skill/Adaptability]. What brings you here today? I'm excited to learn from you and contribute to your journey. Let's dive in! [What the character does for
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    Paris is the largest city in France and is the capital of France. It is located in the center of the country and is home to the country's largest city, Lyon, as well as many smaller cities and towns. Paris is known for its art, culture, and history, and it is a popular tourist destination. Paris has a rich history dating back to the ancient Romans and has undergone many changes over the centuries, including the construction of the Eiffel Tower in the late 19th century. Today, Paris is a bustling metropolis with a diverse population and a vibrant culture that draws people from all over the world
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  rapidly evolving, and there are many potential trends and developments that could shape the industry in the years to come. Here are some possible trends that could impact AI in the coming years:
    
    1. Increased Use of AI in Healthcare: AI has already been used in healthcare to diagnose diseases, predict patient outcomes, and optimize treatment plans. In the future, we can expect to see more AI-driven medical devices and algorithms that can help doctors make more informed decisions and develop more effective treatments.
    
    2. More Realistic AI: The ability to create realistic AI models that can mimic human behavior and emotions is becoming more advanced. We could see AI systems that can


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

    Your

     Name

    ]

     and

     I

     am

     a

     [

    What

     is

     your

     profession

    /

    occupation

    ?

     e

    .g

    .

     Software

     Developer

    ,

     Writer

    ,

     etc

    .]

    .
    


    I

     am

     passionate

     about

     [

    What

     is

     your

     area

     of

     interest

     or

     expertise

    ?

     e

    .g

    .

     technology

    ,

     music

    ,

     etc

    .]

     and

     I

     enjoy

     [

    What

     is

     your

     favorite

     hobby

    ?

     e

    .g

    .

     hiking

    ,

     reading

    ,

     cooking

    ,

     etc

    .]

    .
    


    I

     am

     always

     looking

     for

     new

     challenges

     and

     learning

     opportunities

     to

     grow

     and

     develop

     my

     skills

    .

     I

     am

     always

     eager

     to

     learn

     and

     improve

    ,

     and

     I

     am

     always

     willing

     to

     share

     my

     knowledge

     and

     experience

     with

     others

    .
    


    I

     am

     [

    What

     is

     your

     favorite

     book

    /

    any

     other

     book

     you

     enjoy

     reading

    ?

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    That

    's

     right

    !

     I

     know

     that

    .

     Do

     you

     have

     any

     other

     questions

     about

     France

    ?

     Let

     me

     know

    !

     You

    're

     welcome

     to

     ask

     more

     questions

     about

     France

    .

     I

    'll

     do

     my

     best

     to

     answer

     them

    .
    


    Did

     you

     know

     that

     Paris

     is

     home

     to

     some

     of

     the

     world

    's

     most

     famous

     landmarks

    ,

     including

     the

     E

    iff

    el

     Tower

    ,

     the

     Lou

    vre

     Museum

    ,

     and

     the

     Notre

    -D

    ame

     Cathedral

    ?

     And

     it

    's

     also

     a

     bustling

     city

     with

     a

     vibrant

     culture

    ,

     with

     many

     cultural

     events

     taking

     place

     throughout

     the

     year

    .

     Paris

     is

     a

     city

     that

     is

     both

     a

     historical

     and

     modern

     met

    ropolis

    ,

     and

     it

    's

     a

     city

     that

     I

    'm

     really

     interested

     in

     exploring

    .

     What

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     driven

     by

     the

     increasing

     complexity

     of

     tasks

     and

     the

     need

     to

     create

     systems

     that

     can

     operate

     in

     real

    -world

     environments

    .

     As

     AI

     technologies

     continue

     to

     advance

    ,

     we

     are

     likely

     to

     see

    :
    


    1

    .

     The

     development

     of

     more

     sophisticated

     and

     intelligent

     algorithms

     that

     can

     handle

     a

     wider

     range

     of

     inputs

     and

     tasks

    .

     This

     could

     include

     developing

     more

     advanced

     natural

     language

     processing

     and

     machine

     learning

     systems

     that

     can

     understand

     and

     reason

     about

     human

     language

    ,

     as

     well

     as

     more

     complex

     and

     sophisticated

     image

     recognition

     and

     perception

     systems

    .
    


    2

    .

     The

     emergence

     of

     more

     powerful

     hardware

     and

     software

     platforms

     that

     can

     handle

     larger

     and

     more

     complex

     AI

     tasks

    ,

     such

     as

     speech

     recognition

     and

     natural

     language

     generation

    .

     This

     could

     include

     advancements

     in

    



```python
llm.shutdown()
```
