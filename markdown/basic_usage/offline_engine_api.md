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


    2026-03-31 02:27:25.349 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 02:27:25] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 02:27:25.349 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 02:27:25] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 02:27:25.349 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 02:27:25] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 02:27:25.349 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 02:27:25] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 02:27:25.349 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 02:27:25] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.09it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.08it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:25,  2.54s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:25,  2.54s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:25,  2.54s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:25,  2.54s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.98it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.98it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.98it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.98it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.98it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.98it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.98it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.98it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:08,  5.98it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:02<00:08,  5.98it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 13.40it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 13.40it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 13.40it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 13.40it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 13.40it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 13.40it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 13.40it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 13.40it/s]Compiling num tokens (num_tokens=640):  33%|███▎      | 19/58 [00:02<00:02, 13.40it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:02<00:01, 20.63it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:02<00:01, 20.63it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 20.63it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 20.63it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 20.63it/s]

    Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 20.63it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:03<00:01, 20.63it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:01, 23.26it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:01, 23.26it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:01, 23.26it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:01, 23.26it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:01, 23.26it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:01, 23.26it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:01, 23.26it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 28.73it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 28.73it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 28.73it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 28.73it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 28.73it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 28.73it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 28.73it/s]

    Compiling num tokens (num_tokens=112):  67%|██████▋   | 39/58 [00:03<00:00, 28.73it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:03<00:00, 35.59it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:03<00:00, 35.59it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:03<00:00, 35.59it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:03<00:00, 35.59it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:03<00:00, 35.59it/s]Compiling num tokens (num_tokens=32):  79%|███████▉  | 46/58 [00:03<00:00, 35.59it/s]Compiling num tokens (num_tokens=28):  79%|███████▉  | 46/58 [00:03<00:00, 35.59it/s]Compiling num tokens (num_tokens=24):  79%|███████▉  | 46/58 [00:03<00:00, 35.59it/s]Compiling num tokens (num_tokens=20):  79%|███████▉  | 46/58 [00:03<00:00, 35.59it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:03<00:00, 44.07it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:03<00:00, 44.07it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:03<00:00, 44.07it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:03<00:00, 44.07it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:03<00:00, 44.07it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.45it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=72.27 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=72.24 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=72.24 GB):   3%|▎         | 2/58 [00:00<00:02, 18.91it/s]Capturing num tokens (num_tokens=7168 avail_mem=72.24 GB):   3%|▎         | 2/58 [00:00<00:02, 18.91it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=72.23 GB):   3%|▎         | 2/58 [00:00<00:02, 18.91it/s]Capturing num tokens (num_tokens=6656 avail_mem=72.23 GB):   7%|▋         | 4/58 [00:00<00:03, 15.27it/s]Capturing num tokens (num_tokens=6144 avail_mem=72.24 GB):   7%|▋         | 4/58 [00:00<00:03, 15.27it/s]Capturing num tokens (num_tokens=5632 avail_mem=72.23 GB):   7%|▋         | 4/58 [00:00<00:03, 15.27it/s]Capturing num tokens (num_tokens=5120 avail_mem=72.23 GB):   7%|▋         | 4/58 [00:00<00:03, 15.27it/s]Capturing num tokens (num_tokens=5120 avail_mem=72.23 GB):  12%|█▏        | 7/58 [00:00<00:02, 20.54it/s]Capturing num tokens (num_tokens=4608 avail_mem=72.23 GB):  12%|█▏        | 7/58 [00:00<00:02, 20.54it/s]Capturing num tokens (num_tokens=4096 avail_mem=72.23 GB):  12%|█▏        | 7/58 [00:00<00:02, 20.54it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=72.22 GB):  12%|█▏        | 7/58 [00:00<00:02, 20.54it/s]Capturing num tokens (num_tokens=3584 avail_mem=72.22 GB):  12%|█▏        | 7/58 [00:00<00:02, 20.54it/s]Capturing num tokens (num_tokens=3584 avail_mem=72.22 GB):  19%|█▉        | 11/58 [00:00<00:01, 26.78it/s]Capturing num tokens (num_tokens=3328 avail_mem=72.22 GB):  19%|█▉        | 11/58 [00:00<00:01, 26.78it/s]Capturing num tokens (num_tokens=3072 avail_mem=72.21 GB):  19%|█▉        | 11/58 [00:00<00:01, 26.78it/s]Capturing num tokens (num_tokens=2816 avail_mem=72.21 GB):  19%|█▉        | 11/58 [00:00<00:01, 26.78it/s]Capturing num tokens (num_tokens=2560 avail_mem=72.21 GB):  19%|█▉        | 11/58 [00:00<00:01, 26.78it/s]Capturing num tokens (num_tokens=2304 avail_mem=72.20 GB):  19%|█▉        | 11/58 [00:00<00:01, 26.78it/s]Capturing num tokens (num_tokens=2304 avail_mem=72.20 GB):  28%|██▊       | 16/58 [00:00<00:01, 33.48it/s]Capturing num tokens (num_tokens=2048 avail_mem=72.20 GB):  28%|██▊       | 16/58 [00:00<00:01, 33.48it/s]Capturing num tokens (num_tokens=1792 avail_mem=72.20 GB):  28%|██▊       | 16/58 [00:00<00:01, 33.48it/s]Capturing num tokens (num_tokens=1536 avail_mem=72.19 GB):  28%|██▊       | 16/58 [00:00<00:01, 33.48it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=72.19 GB):  28%|██▊       | 16/58 [00:00<00:01, 33.48it/s]Capturing num tokens (num_tokens=1024 avail_mem=72.17 GB):  28%|██▊       | 16/58 [00:00<00:01, 33.48it/s]Capturing num tokens (num_tokens=1024 avail_mem=72.17 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.43it/s]Capturing num tokens (num_tokens=960 avail_mem=72.18 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.43it/s] Capturing num tokens (num_tokens=896 avail_mem=72.18 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.43it/s]Capturing num tokens (num_tokens=832 avail_mem=72.18 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.43it/s]Capturing num tokens (num_tokens=768 avail_mem=72.17 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.43it/s]Capturing num tokens (num_tokens=768 avail_mem=72.17 GB):  43%|████▎     | 25/58 [00:00<00:00, 35.77it/s]Capturing num tokens (num_tokens=704 avail_mem=72.17 GB):  43%|████▎     | 25/58 [00:00<00:00, 35.77it/s]Capturing num tokens (num_tokens=640 avail_mem=72.17 GB):  43%|████▎     | 25/58 [00:00<00:00, 35.77it/s]

    Capturing num tokens (num_tokens=576 avail_mem=72.17 GB):  43%|████▎     | 25/58 [00:00<00:00, 35.77it/s]Capturing num tokens (num_tokens=512 avail_mem=72.16 GB):  43%|████▎     | 25/58 [00:00<00:00, 35.77it/s]Capturing num tokens (num_tokens=480 avail_mem=72.17 GB):  43%|████▎     | 25/58 [00:00<00:00, 35.77it/s]Capturing num tokens (num_tokens=480 avail_mem=72.17 GB):  52%|█████▏    | 30/58 [00:00<00:00, 39.31it/s]Capturing num tokens (num_tokens=448 avail_mem=72.17 GB):  52%|█████▏    | 30/58 [00:00<00:00, 39.31it/s]Capturing num tokens (num_tokens=416 avail_mem=72.17 GB):  52%|█████▏    | 30/58 [00:00<00:00, 39.31it/s]Capturing num tokens (num_tokens=384 avail_mem=72.16 GB):  52%|█████▏    | 30/58 [00:00<00:00, 39.31it/s]Capturing num tokens (num_tokens=352 avail_mem=72.16 GB):  52%|█████▏    | 30/58 [00:00<00:00, 39.31it/s]Capturing num tokens (num_tokens=320 avail_mem=72.16 GB):  52%|█████▏    | 30/58 [00:01<00:00, 39.31it/s]Capturing num tokens (num_tokens=320 avail_mem=72.16 GB):  60%|██████    | 35/58 [00:01<00:00, 41.87it/s]Capturing num tokens (num_tokens=288 avail_mem=72.15 GB):  60%|██████    | 35/58 [00:01<00:00, 41.87it/s]Capturing num tokens (num_tokens=256 avail_mem=72.15 GB):  60%|██████    | 35/58 [00:01<00:00, 41.87it/s]

    Capturing num tokens (num_tokens=240 avail_mem=72.15 GB):  60%|██████    | 35/58 [00:01<00:00, 41.87it/s]Capturing num tokens (num_tokens=224 avail_mem=72.14 GB):  60%|██████    | 35/58 [00:01<00:00, 41.87it/s]Capturing num tokens (num_tokens=208 avail_mem=72.14 GB):  60%|██████    | 35/58 [00:01<00:00, 41.87it/s]Capturing num tokens (num_tokens=208 avail_mem=72.14 GB):  69%|██████▉   | 40/58 [00:01<00:00, 43.88it/s]Capturing num tokens (num_tokens=192 avail_mem=72.14 GB):  69%|██████▉   | 40/58 [00:01<00:00, 43.88it/s]Capturing num tokens (num_tokens=176 avail_mem=72.14 GB):  69%|██████▉   | 40/58 [00:01<00:00, 43.88it/s]Capturing num tokens (num_tokens=160 avail_mem=72.13 GB):  69%|██████▉   | 40/58 [00:01<00:00, 43.88it/s]Capturing num tokens (num_tokens=144 avail_mem=72.13 GB):  69%|██████▉   | 40/58 [00:01<00:00, 43.88it/s]Capturing num tokens (num_tokens=128 avail_mem=72.13 GB):  69%|██████▉   | 40/58 [00:01<00:00, 43.88it/s]Capturing num tokens (num_tokens=128 avail_mem=72.13 GB):  78%|███████▊  | 45/58 [00:01<00:00, 45.24it/s]Capturing num tokens (num_tokens=112 avail_mem=72.12 GB):  78%|███████▊  | 45/58 [00:01<00:00, 45.24it/s]Capturing num tokens (num_tokens=96 avail_mem=72.12 GB):  78%|███████▊  | 45/58 [00:01<00:00, 45.24it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=72.12 GB):  78%|███████▊  | 45/58 [00:01<00:00, 45.24it/s]Capturing num tokens (num_tokens=64 avail_mem=72.11 GB):  78%|███████▊  | 45/58 [00:01<00:00, 45.24it/s]Capturing num tokens (num_tokens=48 avail_mem=72.11 GB):  78%|███████▊  | 45/58 [00:01<00:00, 45.24it/s]Capturing num tokens (num_tokens=48 avail_mem=72.11 GB):  86%|████████▌ | 50/58 [00:01<00:00, 46.57it/s]Capturing num tokens (num_tokens=32 avail_mem=72.11 GB):  86%|████████▌ | 50/58 [00:01<00:00, 46.57it/s]Capturing num tokens (num_tokens=28 avail_mem=72.10 GB):  86%|████████▌ | 50/58 [00:01<00:00, 46.57it/s]Capturing num tokens (num_tokens=24 avail_mem=72.10 GB):  86%|████████▌ | 50/58 [00:01<00:00, 46.57it/s]Capturing num tokens (num_tokens=20 avail_mem=72.09 GB):  86%|████████▌ | 50/58 [00:01<00:00, 46.57it/s]Capturing num tokens (num_tokens=16 avail_mem=72.09 GB):  86%|████████▌ | 50/58 [00:01<00:00, 46.57it/s]Capturing num tokens (num_tokens=16 avail_mem=72.09 GB):  95%|█████████▍| 55/58 [00:01<00:00, 44.63it/s]Capturing num tokens (num_tokens=12 avail_mem=72.09 GB):  95%|█████████▍| 55/58 [00:01<00:00, 44.63it/s]

    Capturing num tokens (num_tokens=8 avail_mem=72.09 GB):  95%|█████████▍| 55/58 [00:01<00:00, 44.63it/s] Capturing num tokens (num_tokens=4 avail_mem=72.08 GB):  95%|█████████▍| 55/58 [00:01<00:00, 44.63it/s]Capturing num tokens (num_tokens=4 avail_mem=72.08 GB): 100%|██████████| 58/58 [00:01<00:00, 38.30it/s]


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
    Generated text:  Frank and I'm 21 years old. I was born in the USA but now I'm a college student in China. I'm going to study Chinese. I'm good at drawing and like to learn about the Chinese culture. My job is to help me get ready to live and study in China. I want to go to China by summer vacation in the fall. I'm looking for a summer school to help me prepare for studying Chinese. I'm also interested in the traditional Chinese culture. I would like to learn about the Chinese history, the social customs, and the music. I would like to learn about the nature and the
    ===============================
    Prompt: The president of the United States is
    Generated text:  32 years older than 2 times the age of a certain age. If the president is 46 years old now, what is the age of the certain person?
    
    Let's denote the age of the certain person as \( x \).
    
    According to the information given, the president is 32 years older than \( 2 \times \text{the age of the certain person} \).
    
    So we can write the equation:
    
    \[ 32 = 2 \times x \]
    
    Now we can solve for \( x \) by dividing both sides of the equation by 2:
    
    \[ x = \frac{3
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, and the capital of the United Kingdom is London. The capital of the United States is Washington, D.C. Which of the following statements is true?
    A) Washington, D. C. is the capital of France.
    B) Paris is the capital of the United Kingdom.
    C) Washington, D. C. is the capital of the United States.
    D) None of the statements are true.
    The capital of France is actually Paris, not London.
    B) Paris is the capital of the United Kingdom.
    This statement is false because the capital of the United Kingdom is London, not Paris.
    C) Washington, D. C
    ===============================
    Prompt: The future of AI is
    Generated text:  here, and it's already changing the way we live and work.
    Wired’s Science of the Future project looks at how new technologies are transforming the world.
    The future of AI is here, and it's already changing the way we live and work.
    Wired’s Science of the Future project looks at how new technologies are transforming the world.
    From Supercomputers to Artificial Intelligence to Machine Learning, this project takes a look at how new technologies are transforming the world and has already begun to transform the way we live and work.
    From supercomputers to artificial intelligence to machine learning, the Science of the Future project looks at how new technologies


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


    Generated text:  [Name] and I'm a [Age] year old [Gender] [Occupation]. I'm a [Skill] with [Number] years of experience in [Field]. I'm passionate about [What I Love to Do], and I'm always looking for new challenges and opportunities to grow and learn. I'm always eager to learn and improve, and I'm always willing to take risks and try new things. I'm a [Personality] person, and I'm always ready to help others and make a positive impact in the world. I'm a [Motivation] person, and I'm always motivated to achieve my goals
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a bustling metropolis with a rich cultural heritage and is a major economic and political center in Europe. It is also known for its fashion industry and its role in the French Revolution. The city is home to many famous French artists, writers, and musicians, and is a popular tourist destination. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly into one another
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: As AI becomes more sophisticated, it is likely to become more integrated with human intelligence, allowing for more complex and nuanced decision-making.
    
    2. Greater emphasis on ethical considerations: As AI becomes more advanced, there will be a greater emphasis on ethical considerations, including issues such as bias, transparency, and accountability.
    
    3. Greater use of AI in healthcare: AI is already being used in healthcare to improve diagnosis, treatment, and patient care. As AI becomes more advanced, it is likely to be used in even more complex and personalized ways.
    
    4. Greater use
    


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
    Generated text:  Sarah, and I'm a friendly and helpful personality who enjoys helping people find information and solve problems. I'm always available to lend a listening ear or a word of wisdom, and I'm always trying to keep my mind focused on finding solutions to the problems I encounter. If you have any questions or need any advice, don't hesitate to reach out to me! I look forward to working with you. How are you today? (yourself) How are you? (yourself) I hope you're doing well. I'm currently working on a new project and I'm very excited to see where it takes me. Is there anything
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    Paris, the iconic city of love and fashion, stands as a testament to the historical, cultural, and technological achievements of the French people. Located on the Île de la Cité, in the heart of the French Republic, it is the seat of the French government and a bustling metropolis with a rich history. Known for its opulent architecture, world-renowned museums, and diverse culinary scene, Paris has become an international hub of culture, commerce, and entertainment. Whether you're exploring the city’s historic landmarks, indulging in fine dining, or simply enjoying a glass of wine, Paris offers a unique and unforgettable experience
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be shaped by a variety of factors, including advances in computing power, the growth of data and data privacy, and shifts in societal values and expectations. Some possible future trends in AI include:
    
    1. Increased automation and artificial intelligence in jobs: As AI continues to evolve, it will likely replace some jobs that are currently done by humans, particularly in areas like manufacturing, healthcare, and customer service. However, this will also create new opportunities for workers in fields like data science, machine learning, and software engineering.
    
    2. Deep learning and machine learning: As AI continues to develop, the ability to learn from and make decisions on complex


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

    ],

     and

     I

    'm

     a

     [

    职业

    ]

     who

     enjoys

     [

    爱好

    /

    兴趣

    ]

     and

     [

    延伸

    职业

    /

    兴趣

    /

    特长

    ].

     [

    Your

     Name

    ]

     is

     a

     calm

    ,

     laid

    -back

     individual

     who

     values

     [

    character

     trait

    ]

     and

     [

    additional

     trait

    ].

     What

     is

     your

     background

    ,

     and

     what

     makes

     you

     unique

    ?

     And

     what

     brings

     you

     to

     this

     job

    ?

     I

     believe

     in

     [

    reason

     for

     being

    ],

     and

     I

     strive

     to

     [

    why

     you

     do

     what

     you

     do

    ].

     Looking

     forward

     to

     [

    what

     you

     hope

     to

     achieve

    ].

     It

    's

     a

     pleasure

     to

     meet

     you

    ,

     and

     I

     look

     forward

     to

     our

     conversation

    .

     [

    Your

     Name

    ]

     [

    Background

    ,

     achievements

    ,

     or

     personal

     qualities

    ]


    As

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     known

     for

     its

     rich

     history

    ,

     culture

    ,

     and

     iconic

     landmarks

     such

     as

     Notre

    -D

    ame

     Cathedral

    ,

     E

    iff

    el

     Tower

    ,

     and

     the

     Lou

    vre

     Museum

    .

     France

    's

     capital

     is

     also

     one

     of

     the

     world

    's

     largest

     cities

     and

     a

     major

     economic

     and

     cultural

     center

    .

     It

     is

     home

     to

     numerous

     international

     festivals

    ,

     fashion

     shows

    ,

     and

     cultural

     events

     throughout

     the

     year

    .

     Despite

     its

     size

    ,

     Paris

     is

     a

     city

     of

     simplicity

     and

     authenticity

    ,

     with

     its

     cob

    ble

    stone

     streets

     and

     narrow

    ,

     winding

     can

    als

     serving

     as

     its

     most

     distinctive

     features

    .

     The

     city

     is

     known

     for

     its

     beauty

     and

     its

     ability

     to

     adapt

     to

     changing

     times

     and

     trends

    ,

     making

     it

     a

     true

     master

     of

     urban

     design

    .

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     shaped

     by

     several

     key

     trends

    :
    


    1

    .

     Artificial

     Intelligence

     (

    AI

    )

     will

     continue

     to

     advance

     in

     speed

     and

     complexity

    .

     We

     will

     see

     breakthrough

    s

     in

     deep

     learning

    ,

     which

     allows

     machines

     to

     learn

     from

     large

     amounts

     of

     data

    .

     Additionally

    ,

     we

     will

     see

     improvements

     in

     natural

     language

     processing

    ,

     image

     recognition

    ,

     and

     autonomous

     systems

    .
    


    2

    .

     AI

     will

     be

     integrated

     into

     more

     and

     more

     sectors

    .

     AI

     will

     be

     integrated

     into

     healthcare

    ,

     finance

    ,

     transportation

    ,

     manufacturing

    ,

     and

     entertainment

    .

     We

     will

     also

     see

     the

     development

     of

     AI

    -powered

     robots

     that

     can

     perform

     tasks

     that

     humans

     can

    't

    .
    


    3

    .

     AI

     will

     become

     more

     personalized

    .

     AI

     will

     be

     able

     to

     learn

     from

     the

     interactions

     of

    



```python
llm.shutdown()
```
