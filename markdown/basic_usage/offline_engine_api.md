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


    2026-03-31 06:21:02.283 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 06:21:02] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 06:21:02.283 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 06:21:02] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 06:21:02.283 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 06:21:02] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 06:21:02.283 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 06:21:02] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 06:21:02.283 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 06:21:02] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.39it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.39it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:26,  2.57s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:26,  2.57s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:26,  2.57s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:26,  2.57s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:27,  1.94it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:27,  1.94it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:27,  1.94it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:27,  1.94it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:27,  1.94it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:27,  1.94it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:02<00:09,  5.29it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:02<00:09,  5.29it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:02<00:09,  5.29it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:02<00:09,  5.29it/s]Compiling num tokens (num_tokens=3072):  16%|█▌        | 9/58 [00:02<00:09,  5.29it/s]Compiling num tokens (num_tokens=2816):  16%|█▌        | 9/58 [00:02<00:09,  5.29it/s]Compiling num tokens (num_tokens=2560):  16%|█▌        | 9/58 [00:02<00:09,  5.29it/s]Compiling num tokens (num_tokens=2304):  16%|█▌        | 9/58 [00:02<00:09,  5.29it/s]Compiling num tokens (num_tokens=2048):  16%|█▌        | 9/58 [00:02<00:09,  5.29it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:02<00:03, 12.02it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:02<00:03, 12.02it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:02<00:03, 12.02it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:02<00:03, 12.02it/s]Compiling num tokens (num_tokens=1024):  29%|██▉       | 17/58 [00:02<00:03, 12.02it/s]Compiling num tokens (num_tokens=960):  29%|██▉       | 17/58 [00:02<00:03, 12.02it/s] Compiling num tokens (num_tokens=896):  29%|██▉       | 17/58 [00:02<00:03, 12.02it/s]Compiling num tokens (num_tokens=832):  29%|██▉       | 17/58 [00:02<00:03, 12.02it/s]

    Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:02<00:01, 18.59it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:02<00:01, 18.59it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:02<00:01, 18.59it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:03<00:01, 18.59it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:03<00:01, 18.59it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:03<00:01, 18.59it/s]Compiling num tokens (num_tokens=480):  41%|████▏     | 24/58 [00:03<00:01, 18.59it/s]Compiling num tokens (num_tokens=448):  41%|████▏     | 24/58 [00:03<00:01, 18.59it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:03<00:01, 25.48it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:03<00:01, 25.48it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:03<00:01, 25.48it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:03<00:01, 25.48it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:03<00:01, 25.48it/s]Compiling num tokens (num_tokens=288):  53%|█████▎    | 31/58 [00:03<00:01, 25.48it/s]Compiling num tokens (num_tokens=256):  53%|█████▎    | 31/58 [00:03<00:01, 25.48it/s]

    Compiling num tokens (num_tokens=240):  53%|█████▎    | 31/58 [00:03<00:01, 25.48it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 32.33it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 32.33it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 32.33it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 32.33it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 32.33it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 32.33it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 32.33it/s]Compiling num tokens (num_tokens=128):  66%|██████▌   | 38/58 [00:03<00:00, 32.33it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 39.00it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 39.00it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 39.00it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 39.00it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 39.00it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 39.00it/s]

    Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 39.00it/s]Compiling num tokens (num_tokens=28):  78%|███████▊  | 45/58 [00:03<00:00, 39.00it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:03<00:00, 45.39it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:03<00:00, 45.39it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:03<00:00, 45.39it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:03<00:00, 45.39it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:03<00:00, 45.39it/s]Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:03<00:00, 45.39it/s] Compiling num tokens (num_tokens=4):  90%|████████▉ | 52/58 [00:03<00:00, 45.39it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.75it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=76.80 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.77 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.77 GB):   3%|▎         | 2/58 [00:00<00:03, 18.65it/s]Capturing num tokens (num_tokens=7168 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:03, 18.65it/s]Capturing num tokens (num_tokens=6656 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:03, 18.65it/s]Capturing num tokens (num_tokens=6144 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:03, 18.65it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 21.66it/s]Capturing num tokens (num_tokens=5632 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 21.66it/s]Capturing num tokens (num_tokens=5120 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 21.66it/s]Capturing num tokens (num_tokens=4608 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 21.66it/s]Capturing num tokens (num_tokens=4096 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 21.66it/s]Capturing num tokens (num_tokens=4096 avail_mem=76.76 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.32it/s]Capturing num tokens (num_tokens=3840 avail_mem=76.75 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.32it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=76.73 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.32it/s]Capturing num tokens (num_tokens=3328 avail_mem=76.73 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.32it/s]Capturing num tokens (num_tokens=3328 avail_mem=76.73 GB):  21%|██        | 12/58 [00:00<00:02, 19.15it/s]Capturing num tokens (num_tokens=3072 avail_mem=76.72 GB):  21%|██        | 12/58 [00:00<00:02, 19.15it/s]Capturing num tokens (num_tokens=2816 avail_mem=76.72 GB):  21%|██        | 12/58 [00:00<00:02, 19.15it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=76.71 GB):  21%|██        | 12/58 [00:00<00:02, 19.15it/s]Capturing num tokens (num_tokens=2560 avail_mem=76.71 GB):  26%|██▌       | 15/58 [00:00<00:02, 16.66it/s]Capturing num tokens (num_tokens=2304 avail_mem=76.71 GB):  26%|██▌       | 15/58 [00:00<00:02, 16.66it/s]Capturing num tokens (num_tokens=2048 avail_mem=76.70 GB):  26%|██▌       | 15/58 [00:00<00:02, 16.66it/s]Capturing num tokens (num_tokens=1792 avail_mem=76.69 GB):  26%|██▌       | 15/58 [00:00<00:02, 16.66it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=76.69 GB):  31%|███       | 18/58 [00:00<00:02, 18.26it/s]Capturing num tokens (num_tokens=1536 avail_mem=76.21 GB):  31%|███       | 18/58 [00:00<00:02, 18.26it/s]Capturing num tokens (num_tokens=1280 avail_mem=76.08 GB):  31%|███       | 18/58 [00:01<00:02, 18.26it/s]Capturing num tokens (num_tokens=1280 avail_mem=76.08 GB):  34%|███▍      | 20/58 [00:01<00:02, 16.84it/s]Capturing num tokens (num_tokens=1024 avail_mem=76.02 GB):  34%|███▍      | 20/58 [00:01<00:02, 16.84it/s]

    Capturing num tokens (num_tokens=960 avail_mem=76.04 GB):  34%|███▍      | 20/58 [00:01<00:02, 16.84it/s] Capturing num tokens (num_tokens=960 avail_mem=76.04 GB):  38%|███▊      | 22/58 [00:01<00:02, 14.91it/s]Capturing num tokens (num_tokens=896 avail_mem=76.04 GB):  38%|███▊      | 22/58 [00:01<00:02, 14.91it/s]Capturing num tokens (num_tokens=832 avail_mem=76.03 GB):  38%|███▊      | 22/58 [00:01<00:02, 14.91it/s]Capturing num tokens (num_tokens=768 avail_mem=76.03 GB):  38%|███▊      | 22/58 [00:01<00:02, 14.91it/s]Capturing num tokens (num_tokens=768 avail_mem=76.03 GB):  43%|████▎     | 25/58 [00:01<00:01, 17.35it/s]Capturing num tokens (num_tokens=704 avail_mem=76.03 GB):  43%|████▎     | 25/58 [00:01<00:01, 17.35it/s]

    Capturing num tokens (num_tokens=640 avail_mem=76.02 GB):  43%|████▎     | 25/58 [00:01<00:01, 17.35it/s]Capturing num tokens (num_tokens=576 avail_mem=76.02 GB):  43%|████▎     | 25/58 [00:01<00:01, 17.35it/s]Capturing num tokens (num_tokens=576 avail_mem=76.02 GB):  48%|████▊     | 28/58 [00:01<00:01, 15.57it/s]Capturing num tokens (num_tokens=512 avail_mem=76.01 GB):  48%|████▊     | 28/58 [00:01<00:01, 15.57it/s]

    Capturing num tokens (num_tokens=480 avail_mem=76.03 GB):  48%|████▊     | 28/58 [00:01<00:01, 15.57it/s]Capturing num tokens (num_tokens=480 avail_mem=76.03 GB):  52%|█████▏    | 30/58 [00:01<00:01, 15.59it/s]Capturing num tokens (num_tokens=448 avail_mem=76.02 GB):  52%|█████▏    | 30/58 [00:01<00:01, 15.59it/s]Capturing num tokens (num_tokens=416 avail_mem=76.02 GB):  52%|█████▏    | 30/58 [00:01<00:01, 15.59it/s]Capturing num tokens (num_tokens=384 avail_mem=76.02 GB):  52%|█████▏    | 30/58 [00:01<00:01, 15.59it/s]Capturing num tokens (num_tokens=352 avail_mem=76.01 GB):  52%|█████▏    | 30/58 [00:01<00:01, 15.59it/s]Capturing num tokens (num_tokens=320 avail_mem=76.01 GB):  52%|█████▏    | 30/58 [00:01<00:01, 15.59it/s]Capturing num tokens (num_tokens=320 avail_mem=76.01 GB):  60%|██████    | 35/58 [00:01<00:01, 22.86it/s]Capturing num tokens (num_tokens=288 avail_mem=76.01 GB):  60%|██████    | 35/58 [00:01<00:01, 22.86it/s]Capturing num tokens (num_tokens=256 avail_mem=76.00 GB):  60%|██████    | 35/58 [00:01<00:01, 22.86it/s]Capturing num tokens (num_tokens=240 avail_mem=76.00 GB):  60%|██████    | 35/58 [00:01<00:01, 22.86it/s]

    Capturing num tokens (num_tokens=224 avail_mem=76.00 GB):  60%|██████    | 35/58 [00:01<00:01, 22.86it/s]Capturing num tokens (num_tokens=224 avail_mem=76.00 GB):  67%|██████▋   | 39/58 [00:01<00:00, 26.34it/s]Capturing num tokens (num_tokens=208 avail_mem=76.00 GB):  67%|██████▋   | 39/58 [00:01<00:00, 26.34it/s]Capturing num tokens (num_tokens=192 avail_mem=76.00 GB):  67%|██████▋   | 39/58 [00:02<00:00, 26.34it/s]Capturing num tokens (num_tokens=176 avail_mem=75.99 GB):  67%|██████▋   | 39/58 [00:02<00:00, 26.34it/s]

    Capturing num tokens (num_tokens=176 avail_mem=75.99 GB):  72%|███████▏  | 42/58 [00:02<00:00, 22.35it/s]Capturing num tokens (num_tokens=160 avail_mem=75.99 GB):  72%|███████▏  | 42/58 [00:02<00:00, 22.35it/s]Capturing num tokens (num_tokens=144 avail_mem=75.98 GB):  72%|███████▏  | 42/58 [00:02<00:00, 22.35it/s]Capturing num tokens (num_tokens=128 avail_mem=75.98 GB):  72%|███████▏  | 42/58 [00:02<00:00, 22.35it/s]Capturing num tokens (num_tokens=112 avail_mem=75.98 GB):  72%|███████▏  | 42/58 [00:02<00:00, 22.35it/s]Capturing num tokens (num_tokens=96 avail_mem=75.98 GB):  72%|███████▏  | 42/58 [00:02<00:00, 22.35it/s] Capturing num tokens (num_tokens=96 avail_mem=75.98 GB):  81%|████████  | 47/58 [00:02<00:00, 28.44it/s]Capturing num tokens (num_tokens=80 avail_mem=75.97 GB):  81%|████████  | 47/58 [00:02<00:00, 28.44it/s]Capturing num tokens (num_tokens=64 avail_mem=75.97 GB):  81%|████████  | 47/58 [00:02<00:00, 28.44it/s]Capturing num tokens (num_tokens=48 avail_mem=75.97 GB):  81%|████████  | 47/58 [00:02<00:00, 28.44it/s]Capturing num tokens (num_tokens=32 avail_mem=75.96 GB):  81%|████████  | 47/58 [00:02<00:00, 28.44it/s]Capturing num tokens (num_tokens=28 avail_mem=75.96 GB):  81%|████████  | 47/58 [00:02<00:00, 28.44it/s]

    Capturing num tokens (num_tokens=28 avail_mem=75.96 GB):  90%|████████▉ | 52/58 [00:02<00:00, 32.10it/s]Capturing num tokens (num_tokens=24 avail_mem=75.95 GB):  90%|████████▉ | 52/58 [00:02<00:00, 32.10it/s]Capturing num tokens (num_tokens=20 avail_mem=75.95 GB):  90%|████████▉ | 52/58 [00:02<00:00, 32.10it/s]Capturing num tokens (num_tokens=16 avail_mem=75.95 GB):  90%|████████▉ | 52/58 [00:02<00:00, 32.10it/s]Capturing num tokens (num_tokens=12 avail_mem=75.94 GB):  90%|████████▉ | 52/58 [00:02<00:00, 32.10it/s]Capturing num tokens (num_tokens=8 avail_mem=75.94 GB):  90%|████████▉ | 52/58 [00:02<00:00, 32.10it/s] Capturing num tokens (num_tokens=8 avail_mem=75.94 GB):  98%|█████████▊| 57/58 [00:02<00:00, 35.26it/s]Capturing num tokens (num_tokens=4 avail_mem=75.94 GB):  98%|█████████▊| 57/58 [00:02<00:00, 35.26it/s]Capturing num tokens (num_tokens=4 avail_mem=75.94 GB): 100%|██████████| 58/58 [00:02<00:00, 23.13it/s]


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
    Generated text:  John. I'm a 13 year old boy. I have a very happy family and I like to go to the beach. I often go there in my summer holidays. One day last summer, I went to the beach. It was very beautiful. But when I got back home, I found I had a little water in my shirt. I washed it off and then I put it in the shower to clean it up. The next day I put it in the bathtub. I didn't wash it. I left it in the bathtub for a long time. But I forgot to turn it off. It was all wet and dirty
    ===============================
    Prompt: The president of the United States is
    Generated text:  considering a new policy proposal. He has two possible options:
    
    1. Option A: The government will increase the minimum wage by 20%.
    2. Option B: The government will increase the minimum wage by 10%.
    
    If the minimum wage currently in effect is $15 per hour, and the government decides to implement Option A, how much would the government's additional budget be compared to implementing Option B?
    
    To determine the difference in the government's additional budget between implementing Option A and Option B, we need to calculate the additional budget for each option and then find the difference.
    
    First, let's calculate the additional budget for
    ===============================
    Prompt: The capital of France is
    Generated text: :
    A. Paris
    B. London
    C. Madrid
    D. New York
    
    A. Paris is the capital of France. Paris is the capital city of the French department of Île-de-France. It is also the capital of France, which is the most populous country in Europe. Paris is located on the banks of the Seine River in the Île de France, a small island in the center of the Seine estuary. The city is home to many famous landmarks such as Notre-Dame Cathedral, the Eiffel Tower, and the Louvre Museum. It is also the home of the French Revolution and
    ===============================
    Prompt: The future of AI is
    Generated text:  empowering
    
    It’s not yet clear how the future of AI will be shaped. However, given the current state of AI, there are two reasons for optimism about the future. The first is the speed at which research is being made. The second is the impact of the recent successful application of AI to many areas.
    
    However, the reality is that AI is not going to come to a complete stop. It will not only evolve, it will continue to evolve. It is already doing so. The next 10 years is going to be a period of AI adoption, migration and intensification, a period of great opportunity


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about your career and interests. What can you tell me about yourself? I'm a [insert a short, positive description of your personality or skills]. I enjoy [insert a short, positive description of your hobbies or interests]. I'm always looking for new challenges and opportunities to grow and learn. What's your favorite hobby or activity? I love [insert a short, positive description of your favorite hobby or activity]. I'm always looking for new ways to challenge myself and expand my horizons. What's your favorite
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French National Library, and the French Parliament building. Paris is a bustling metropolis with a rich cultural heritage and is a major tourist destination. It is also a major center for business and finance, with many of the world’s largest corporations and financial institutions headquartered in the city. Paris is a city of contrasts, with its historic architecture and modern fashion, and is a popular destination for tourists and locals alike. The city is known for its vibrant nightlife,
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: As AI becomes more advanced, it is likely to become more integrated with human intelligence, allowing it to learn from and adapt to human behavior and decision-making processes.
    
    2. Enhanced ethical considerations: As AI becomes more advanced, there will be increased scrutiny of its ethical implications, including issues such as bias, transparency, and accountability.
    
    3. Greater reliance on AI for decision-making: As AI becomes more integrated with human intelligence, it is likely to become a more significant factor in decision-making processes, particularly in areas such as healthcare, finance, and transportation.
    
    4
    


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
    Generated text:  [Name] and I am a [job title] in [industry]. I am a [job title] at [company name], and I'm passionate about [job title] because I love [reason why I'm passionate]. I enjoy [job title] because it makes me feel [job title] and because it gives me [job title] to do what I love. I love what I do because it gives me [reason why I love my job]. I have [job title] experience, and I am [job title] at [company name]. If you're looking for a job that I can help you with, I
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. It is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, Louvre Museum, and Montmartre. The city is also famous for its cuisine, music, and wine, and is home to numerous world-class universities and museums. Additionally, Paris is a popular tourist destination, and is the largest city in the European Union by population. Its rich history, cultural heritage, and architectural traditions make it a unique and fascinating destination for visitors. According to historical records, Paris was founded by the French King Charles I in the 13th century and has been the capital of France since 8
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be characterized by rapid progress and transformation, driven by the convergence of new technologies, advances in computation power, and a growing understanding of human behavior and mental health. Some potential trends include:
    
    1. Enhanced precision and efficiency: AI will continue to improve its ability to process and analyze large amounts of data at a rapid pace, leading to more precise and efficient solutions to complex problems.
    
    2. Autonomous systems: As AI becomes more integrated into our daily lives, we may see more autonomous systems like robots, drones, and vehicles that can make decisions on their own without human intervention.
    
    3. Personalized experiences: AI will continue to become more


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

     an

     exceptional

     [

    occup

    ational

     or

     personal

     trait

    ]

     who

     is

     passionate

     about

     [

    career

    -related

     activity

     or

     hobby

    ].

     I

    'm

     [

    Age

    ]

     years

     old

    ,

     and

     I

    'm

     [

    Background

    ]

     with

     an

     [

    Experience

    ]

     of

     [

    number

     of

     years

     in

     a

     similar

     occupation

    ].

     I

     love

     [

    occupation

    /person

    al

     goal

     or

     hobby

    ]

     because

     [

    reason

     for

     passion

     or

     love

    ].

     As

     an

     [

    occupation

    /person

    al

     goal

     or

     hobby

    ],

     I

     aim

     to

     [

    specific

     goal

     or

     achievement

    ].

     I

    'm

     always

     ready

     to

     help

     others

     with

     my

     knowledge

     and

     skills

    ,

     and

     I

     thrive

     in

     [

    occupation

    /person

    al

     goal

     or

     hobby

    ]

     because

     of

     [

    reason

     for

     passion

     or

     love

    ].

     If

     you

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    Paris

     is

     the

     capital

     city

     of

     France

    ,

     located

     in

     the

     south

     of

     the

     country

    .

     It

     is

     the

     largest

     city

     in

     Europe

     and

     the

     eighth

    -largest

     city

     in

     the

     world

    ,

     with

     a

     population

     of

     over

     

    2

     million

     people

    .

     Paris

     is

     known

     for

     its

     rich

     history

    ,

     arts

    ,

     and

     culture

    ,

     and

     is

     a

     significant

     cultural

     and

     economic

     hub

     in

     Europe

    .

     The

     city

    's

     skyline

     is

     dotted

     with

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

     It

     is

     also

     home

     to

     the

     world

    's

     largest

     library

    ,

     the

     Bibli

    oth

    è

    que

     nation

    ale

     de

     France

    ,

     and

     the

     Lou

    vre

     Museum

    .

     Paris

     is

     a

     major

     city

     that

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     highly

     uncertain

     and

     depends

     on

     a

     variety

     of

     factors

    ,

     including

     technological

     advancements

    ,

     policy

     changes

    ,

     and

     changes

     in

     human

     behavior

    .

     Here

     are

     some

     possible

     trends

     that

     may

     emerge

     in

     the

     coming

     years

    :
    


    1

    .

     Increased

     automation

    :

     As

     more

     tasks

     are

     automated

    ,

     AI

     will

     become

     more

     prevalent

     in

     various

     industries

    ,

     from

     manufacturing

     and

     healthcare

     to

     customer

     service

     and

     transportation

    .

     This

     will

     lead

     to

     the

     development

     of

     more

     efficient

     and

     cost

    -effective

     solutions

    ,

     as

     well

     as

     the

     creation

     of

     new

     professions

     in

     fields

     such

     as

     AI

     development

     and

     machine

     learning

    .
    


    2

    .

     Enhanced

     privacy

     and

     security

    :

     As

     AI

     becomes

     more

     advanced

    ,

     there

     will

     be

     increased

     concerns

     about

     the

     potential

     for

     AI

     systems

     to

     be

     hacked

     or

     manipulated

    .

    



```python
llm.shutdown()
```
