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


    2026-03-27 15:56:23.209 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 15:56:23] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 15:56:23.209 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 15:56:23] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 15:56:23.209 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 15:56:23] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 15:56:23.209 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 15:56:23] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 15:56:23.209 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 15:56:23] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.31it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.30it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:14,  2.37s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:14,  2.37s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:14,  2.37s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:14,  2.37s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:14,  2.37s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:20,  2.65it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:20,  2.65it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:20,  2.65it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:20,  2.65it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:20,  2.65it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:20,  2.65it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:20,  2.65it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 13.82it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 13.82it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 13.82it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 13.82it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 13.82it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 13.82it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 13.82it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 13.82it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:02<00:01, 20.23it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:02<00:01, 20.23it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:02<00:01, 20.23it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:02<00:01, 20.23it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:02<00:01, 20.23it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:02<00:01, 20.23it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:02<00:01, 20.23it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:02<00:01, 20.23it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:02<00:00, 26.97it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:02<00:00, 26.97it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:02<00:00, 26.97it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:02<00:00, 26.97it/s]

    Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:02<00:00, 26.97it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:00, 26.97it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:00, 26.97it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 31.32it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 31.32it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 31.32it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 31.32it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 31.32it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 31.32it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 31.32it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 36.06it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 36.06it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 36.06it/s] 

    Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 36.06it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 36.06it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 36.06it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 36.06it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 39.91it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 39.91it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 39.91it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 39.91it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 39.91it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 39.91it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 39.91it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 39.91it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.31it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=121.74 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=121.44 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=121.44 GB):   3%|▎         | 2/58 [00:00<00:03, 17.45it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 17.45it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 17.45it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 17.45it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.73 GB):   9%|▊         | 5/58 [00:00<00:02, 21.30it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 21.30it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 21.30it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 21.30it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.72 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.20it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.72 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.20it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.72 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.20it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=120.71 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.20it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.71 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.20it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.71 GB):  21%|██        | 12/58 [00:00<00:01, 24.55it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.70 GB):  21%|██        | 12/58 [00:00<00:01, 24.55it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.70 GB):  21%|██        | 12/58 [00:00<00:01, 24.55it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.70 GB):  21%|██        | 12/58 [00:00<00:01, 24.55it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=120.70 GB):  26%|██▌       | 15/58 [00:00<00:01, 25.81it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.37 GB):  26%|██▌       | 15/58 [00:00<00:01, 25.81it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.27 GB):  26%|██▌       | 15/58 [00:00<00:01, 25.81it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  26%|██▌       | 15/58 [00:00<00:01, 25.81it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  26%|██▌       | 15/58 [00:00<00:01, 25.81it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.26 GB):  26%|██▌       | 15/58 [00:00<00:01, 25.81it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.26 GB):  34%|███▍      | 20/58 [00:00<00:01, 31.58it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.24 GB):  34%|███▍      | 20/58 [00:00<00:01, 31.58it/s]Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  34%|███▍      | 20/58 [00:00<00:01, 31.58it/s] Capturing num tokens (num_tokens=896 avail_mem=120.25 GB):  34%|███▍      | 20/58 [00:00<00:01, 31.58it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  34%|███▍      | 20/58 [00:00<00:01, 31.58it/s]

    Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  34%|███▍      | 20/58 [00:00<00:01, 31.58it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  43%|████▎     | 25/58 [00:00<00:00, 35.47it/s]Capturing num tokens (num_tokens=704 avail_mem=120.24 GB):  43%|████▎     | 25/58 [00:00<00:00, 35.47it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  43%|████▎     | 25/58 [00:00<00:00, 35.47it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  43%|████▎     | 25/58 [00:00<00:00, 35.47it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  43%|████▎     | 25/58 [00:00<00:00, 35.47it/s]Capturing num tokens (num_tokens=480 avail_mem=120.23 GB):  43%|████▎     | 25/58 [00:00<00:00, 35.47it/s]Capturing num tokens (num_tokens=480 avail_mem=120.23 GB):  52%|█████▏    | 30/58 [00:00<00:00, 38.17it/s]Capturing num tokens (num_tokens=448 avail_mem=120.23 GB):  52%|█████▏    | 30/58 [00:00<00:00, 38.17it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  52%|█████▏    | 30/58 [00:00<00:00, 38.17it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  52%|█████▏    | 30/58 [00:00<00:00, 38.17it/s]

    Capturing num tokens (num_tokens=352 avail_mem=120.22 GB):  52%|█████▏    | 30/58 [00:01<00:00, 38.17it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  52%|█████▏    | 30/58 [00:01<00:00, 38.17it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  60%|██████    | 35/58 [00:01<00:00, 40.21it/s]Capturing num tokens (num_tokens=288 avail_mem=120.21 GB):  60%|██████    | 35/58 [00:01<00:00, 40.21it/s]Capturing num tokens (num_tokens=256 avail_mem=120.21 GB):  60%|██████    | 35/58 [00:01<00:00, 40.21it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  60%|██████    | 35/58 [00:01<00:00, 40.21it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  60%|██████    | 35/58 [00:01<00:00, 40.21it/s]Capturing num tokens (num_tokens=208 avail_mem=120.20 GB):  60%|██████    | 35/58 [00:01<00:00, 40.21it/s]Capturing num tokens (num_tokens=208 avail_mem=120.20 GB):  69%|██████▉   | 40/58 [00:01<00:00, 41.81it/s]Capturing num tokens (num_tokens=192 avail_mem=120.20 GB):  69%|██████▉   | 40/58 [00:01<00:00, 41.81it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  69%|██████▉   | 40/58 [00:01<00:00, 41.81it/s]Capturing num tokens (num_tokens=160 avail_mem=120.19 GB):  69%|██████▉   | 40/58 [00:01<00:00, 41.81it/s]

    Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  69%|██████▉   | 40/58 [00:01<00:00, 41.81it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  69%|██████▉   | 40/58 [00:01<00:00, 41.81it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  78%|███████▊  | 45/58 [00:01<00:00, 42.75it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  78%|███████▊  | 45/58 [00:01<00:00, 42.75it/s]Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  78%|███████▊  | 45/58 [00:01<00:00, 42.75it/s] Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  78%|███████▊  | 45/58 [00:01<00:00, 42.75it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  78%|███████▊  | 45/58 [00:01<00:00, 42.75it/s]Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  78%|███████▊  | 45/58 [00:01<00:00, 42.75it/s]Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  86%|████████▌ | 50/58 [00:01<00:00, 43.13it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  86%|████████▌ | 50/58 [00:01<00:00, 43.13it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  86%|████████▌ | 50/58 [00:01<00:00, 43.13it/s]

    Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  86%|████████▌ | 50/58 [00:01<00:00, 43.13it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  86%|████████▌ | 50/58 [00:01<00:00, 43.13it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  86%|████████▌ | 50/58 [00:01<00:00, 43.13it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  95%|█████████▍| 55/58 [00:01<00:00, 43.96it/s]Capturing num tokens (num_tokens=12 avail_mem=120.15 GB):  95%|█████████▍| 55/58 [00:01<00:00, 43.96it/s]Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  95%|█████████▍| 55/58 [00:01<00:00, 43.96it/s] Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  95%|█████████▍| 55/58 [00:01<00:00, 43.96it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 36.85it/s]


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
    Generated text:  Aya Saim. I’m a fan of the latest generation of cars, and I’m a tech enthusiast with a passion for how technology can shape our lives. I have a love for the Japanese culture and my parents are both Japanese, so I’m intrigued by how technology can play into Japanese culture.
    
    What is your experience with technology? 
    
    As an AI language model, I don't have personal experiences. However, I am programmed to understand and respond to queries related to technology, including its impact on society, the latest developments, and the role of technology in daily life.
    
    Is there a particular technology or area of focus that excites
    ===============================
    Prompt: The president of the United States is
    Generated text:  now considering whether to send a new generation of Starfleet officers. The president has identified four potential candidates for leadership roles: Captain James Kirk, Captain Sarah Patel, Captain Charles Starzak, and Captain Tuvan Starzak. The president wants to select a leader who, when acting alone, can always predict the actions of all other officers in the fleet with a probability greater than 0.5. What is the probability that Captain Tuvan Starzak will be the selected leader?
    
    To determine the probability that Captain Tuvan Starzak will be the selected leader, we need to understand that the probability that Captain T
    ===============================
    Prompt: The capital of France is
    Generated text:  [ ]
    A. Paris
    B. New York
    C. Rome
    D. London
    Answer: A
    
    When measuring the content of salt in a solution, if the previously prepared standard solution contains 0.01000 mol/L of NaCl, and it is necessary to measure the content of 0.002500 mol/L of salt in the solution, how many moles of NaCl are required for the measurement?
    A. 0.01000
    B. 0.02000
    C. 0.03000
    D.
    ===============================
    Prompt: The future of AI is
    Generated text:  here and it’s transforming industries like never before. AI is a rapidly evolving technology that is driving innovation and change across various sectors.
    Here are some examples of how AI is impacting various industries:
    1. Healthcare: AI is revolutionizing the healthcare industry by providing more accurate diagnoses, faster and more personalized treatment, and better patient care. AI-powered systems can analyze medical data to identify patterns and make predictions, which can help doctors make more informed decisions.
    2. Finance: AI is helping banks and financial institutions to better understand customers, improve customer service, and manage risk. AI-powered chatbots can provide 24/7 customer service, while


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm a [job title] with [number of years] years of experience in [industry]. I'm passionate about [reason for interest] and I'm always looking for ways to [action or goal]. I'm a [reason for interest] and I'm always looking for ways to [action or goal]. I'm a [reason for interest] and I'm always looking for ways to [action or goal]. I'm a [reason for interest] and I'm always looking for ways to [action or goal]. I'm a [reason for interest
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a major cultural and economic center, hosting numerous world-renowned museums, theaters, and art galleries. Paris is known for its rich history, including the influence of the French Revolution and the influence of the French Revolution on art and literature. It is also home to many famous landmarks, including the Arc de Triomphe and the Champs-Élysées. Paris is a vibrant and dynamic city that is known for its diverse culture, food, and fashion. It is also a popular
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way that AI is used and developed. Here are some of the most likely trends that are expected to shape the future of AI:
    
    1. Increased Use of AI in Healthcare: AI is already being used in healthcare to help diagnose and treat diseases, and it has the potential to be even more effective in the future. AI-powered diagnostic tools, such as machine learning algorithms, can help doctors make more accurate diagnoses and provide more personalized treatment plans.
    
    2. Increased Use of AI in Manufacturing: AI is already being used in manufacturing to help automate processes, improve efficiency, and
    


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
    Generated text:  [First Name] and I am a self-proclaimed "quiet thinker" who enjoys reading, reading about books, and sometimes even crafting. I'm always looking for new ways to challenge my perspective and expand my knowledge. I enjoy spending time with my family and friends, and I'm a big fan of sports, especially football. I'm also a lover of gourmet food and wine, and I love going to the local wine bar to sample new and exciting wines. I'm always up for learning something new and discovering new recipes, so if I have the time and the inclination, I'll happily spend some time trying out new recipes and learning
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, known for its iconic Eiffel Tower, Louvre Museum, and the Notre-Dame Cathedral. According to the latest statistics, Paris has a population of about 2.2 million people. The city is also known for its vibrant nightlife, annual festivals, and rich culture. The French are a highly skilled and passionate community, contributing significantly to the country's economic and social life. Paris is a city that is constantly evolving, with new developments and cultural events taking place throughout the year. In terms of cuisine, French food is considered a global culinary treasure and enjoys worldwide popularity. The city is also home to numerous museums, art
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  rapidly evolving and there are many potential trends that could shape the field. Here are some possible future trends in AI:
    
    1. More personalized and accurate AI: With the increasing amount of data and human interaction, it is likely that AI will become more personalized and accurate. Machine learning algorithms will be able to learn from and understand the patterns in large amounts of data, leading to more accurate predictions and decision-making.
    
    2. AI will become more ethical and transparent: As AI is becoming more integrated into our daily lives, it will become increasingly important to ensure that it is ethical and transparent. This means that AI will need to be developed in ways that


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

     ____

     and

     I

    'm

     a

    /an

     ____

    .


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

    /an

     [

    Occup

    ation

    /

    Role

    ].

     I

    've

     always

     been

     curious

     about

     the

     world

     around

     me

     and

     always

     wanted

     to

     learn

     more

    .

     Whether

     it

    's

     reading

     books

    ,

     watching

     movies

    ,

     or

     attending

     workshops

    ,

     I

    've

     always

     been

     fascinated

     by

     the

     stories

     and

     ideas

     that

     are

     out

     there

    .

     I

     enjoy

     sharing

     my

     knowledge

     and

     passion

     for

     learning

     with

     others

     and

     I

    'm

     excited

     to

     be

     a

     part

     of

     this

     community

    .

     Thanks

     for

     having

     me

     here

     today

    .

     What

    's

     your

     name

    ,

     and

     what

     brings

     you

     to

     this

     table

     today

    ?

     I

    'm

     ready

     for

     a

     bit

     of

     conversation

    .

     How

     about

     you

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     also

     known

     as

     the

     City

     of

     Light

    ,

     and

     is

     the

     largest

     city

     in

     the

     European

     Union

    .

     It

     is

     home

     to

     the

     country

    's

     government

    ,

     institutions

    ,

     and

     cultural

     centers

    ,

     and

     has

     been

     the

     seat

     of

     French

     administration

     since

     

    1

    6

    8

    2

    .

     Paris

     is

     renowned

     for

     its

     architecture

    ,

     including

     iconic

     landmarks

     such

     as

     the

     E

    iff

    el

     Tower

    ,

     Lou

    vre

     Museum

    ,

     and

     Notre

    -D

    ame

     Cathedral

    .

     The

     city

     is

     also

     known

     for

     its

     cuisine

    ,

     fashion

     industry

    ,

     and

     annual

     summer

     festivals

    .

     Paris

     is

     a

     global

     cultural

     and

     political

     center

    ,

     hosting

     many

     of

     the

     world

    's

     leading

     events

     and

     institutions

    .

     The

     city

     is

     also

     the

     birth

    place

     of

     the

     French

     Revolution

     and

     remains

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     rapidly

     evolving

    ,

     with

     a

     number

     of

     potential

     trends

     that

     could

     shape

     the

     industry

    .

     Here

     are

     some

     of

     the

     most

     likely

     trends

     we

     can

     expect

     to

     see

     in

     the

     coming

     years

    :
    


    1

    .

     Increased

     Integration

    :

     AI

     will

     continue

     to

     be

     integrated

     into

     various

     industries

    ,

     from

     healthcare

     and

     finance

     to

     manufacturing

     and

     transportation

    .

     This

     integration

     will

     likely

     become

     more

     seamless

    ,

     with

     more

     companies

     adopting

     AI

     technologies

     and

     providing

     their

     employees

     with

     access

     to

     these

     tools

    .
    


    2

    .

     Personal

    ization

    :

     AI

     will

     become

     more

     personalized

    ,

     allowing

     machines

     to

     understand

     and

     respond

     to

     individual

     users

    '

     needs

     and

     preferences

    .

     This

     will

     enable

     more

     accurate

     and

     efficient

     customer

     service

     and

     personalized

     marketing

    .
    


    3

    .

     Autonomous

     Agents

    :

     Autonomous

     agents

     will

     become

    



```python
llm.shutdown()
```
