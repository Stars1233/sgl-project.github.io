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


    2026-03-30 03:59:11.181 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 03:59:11] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 03:59:11.181 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 03:59:11] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 03:59:11.181 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 03:59:11] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 03:59:11.181 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 03:59:11] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 03:59:11.181 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 03:59:11] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.95it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.94it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:13,  2.34s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:13,  2.34s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:13,  2.34s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:13,  2.34s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:13,  2.34s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:19,  2.68it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:19,  2.68it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:19,  2.68it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:19,  2.68it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:19,  2.68it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:19,  2.68it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:19,  2.68it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 13.97it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 13.97it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 13.97it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 13.97it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 13.97it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 13.97it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 13.97it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 13.97it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:02<00:01, 20.47it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:02<00:01, 20.47it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:02<00:01, 20.47it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:02<00:01, 20.47it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:02<00:01, 20.47it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:02<00:01, 20.47it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:02<00:01, 20.47it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:02<00:00, 26.16it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:02<00:00, 26.16it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:02<00:00, 26.16it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:02<00:00, 26.16it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:02<00:00, 26.16it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:02<00:00, 26.16it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:02<00:00, 26.16it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:02<00:00, 31.26it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:02<00:00, 31.26it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 31.26it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 31.26it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 31.26it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 31.26it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 31.26it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 36.75it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 36.75it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 36.75it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 36.75it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 36.75it/s]

    Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 36.75it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 36.75it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 39.87it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 39.87it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 39.87it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 39.87it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 39.87it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 39.87it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 39.87it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 39.87it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 39.87it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.51it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=136.07 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=136.04 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=136.04 GB):   3%|▎         | 2/58 [00:00<00:02, 19.07it/s]Capturing num tokens (num_tokens=7168 avail_mem=136.04 GB):   3%|▎         | 2/58 [00:00<00:02, 19.07it/s]Capturing num tokens (num_tokens=6656 avail_mem=136.04 GB):   3%|▎         | 2/58 [00:00<00:02, 19.07it/s]Capturing num tokens (num_tokens=6144 avail_mem=136.04 GB):   3%|▎         | 2/58 [00:00<00:02, 19.07it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=136.04 GB):   9%|▊         | 5/58 [00:00<00:02, 22.27it/s]Capturing num tokens (num_tokens=5632 avail_mem=136.03 GB):   9%|▊         | 5/58 [00:00<00:02, 22.27it/s]Capturing num tokens (num_tokens=5120 avail_mem=134.96 GB):   9%|▊         | 5/58 [00:00<00:02, 22.27it/s]Capturing num tokens (num_tokens=4608 avail_mem=134.96 GB):   9%|▊         | 5/58 [00:00<00:02, 22.27it/s]Capturing num tokens (num_tokens=4608 avail_mem=134.96 GB):  14%|█▍        | 8/58 [00:00<00:01, 25.35it/s]Capturing num tokens (num_tokens=4096 avail_mem=134.91 GB):  14%|█▍        | 8/58 [00:00<00:01, 25.35it/s]Capturing num tokens (num_tokens=3840 avail_mem=124.90 GB):  14%|█▍        | 8/58 [00:00<00:01, 25.35it/s]Capturing num tokens (num_tokens=3584 avail_mem=121.69 GB):  14%|█▍        | 8/58 [00:00<00:01, 25.35it/s]Capturing num tokens (num_tokens=3328 avail_mem=121.69 GB):  14%|█▍        | 8/58 [00:00<00:01, 25.35it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=121.69 GB):  21%|██        | 12/58 [00:00<00:01, 29.75it/s]Capturing num tokens (num_tokens=3072 avail_mem=121.68 GB):  21%|██        | 12/58 [00:00<00:01, 29.75it/s]Capturing num tokens (num_tokens=2816 avail_mem=121.68 GB):  21%|██        | 12/58 [00:00<00:01, 29.75it/s]Capturing num tokens (num_tokens=2560 avail_mem=121.68 GB):  21%|██        | 12/58 [00:00<00:01, 29.75it/s]Capturing num tokens (num_tokens=2304 avail_mem=121.67 GB):  21%|██        | 12/58 [00:00<00:01, 29.75it/s]Capturing num tokens (num_tokens=2048 avail_mem=121.67 GB):  21%|██        | 12/58 [00:00<00:01, 29.75it/s]Capturing num tokens (num_tokens=2048 avail_mem=121.67 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.24it/s]Capturing num tokens (num_tokens=1792 avail_mem=121.67 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.24it/s]Capturing num tokens (num_tokens=1536 avail_mem=121.66 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.24it/s]Capturing num tokens (num_tokens=1280 avail_mem=121.66 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.24it/s]Capturing num tokens (num_tokens=1024 avail_mem=121.64 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.24it/s]

    Capturing num tokens (num_tokens=960 avail_mem=121.65 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.24it/s] Capturing num tokens (num_tokens=960 avail_mem=121.65 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.64it/s]Capturing num tokens (num_tokens=896 avail_mem=121.65 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.64it/s]Capturing num tokens (num_tokens=832 avail_mem=121.65 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.64it/s]Capturing num tokens (num_tokens=768 avail_mem=121.64 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.64it/s]Capturing num tokens (num_tokens=704 avail_mem=121.64 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.64it/s]Capturing num tokens (num_tokens=640 avail_mem=121.64 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.64it/s]Capturing num tokens (num_tokens=640 avail_mem=121.64 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.98it/s]Capturing num tokens (num_tokens=576 avail_mem=121.64 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.98it/s]Capturing num tokens (num_tokens=512 avail_mem=121.62 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.98it/s]Capturing num tokens (num_tokens=480 avail_mem=121.64 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.98it/s]

    Capturing num tokens (num_tokens=448 avail_mem=121.64 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.98it/s]Capturing num tokens (num_tokens=416 avail_mem=121.64 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.98it/s]Capturing num tokens (num_tokens=416 avail_mem=121.64 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.56it/s]Capturing num tokens (num_tokens=384 avail_mem=121.63 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.56it/s]Capturing num tokens (num_tokens=352 avail_mem=121.63 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.56it/s]Capturing num tokens (num_tokens=320 avail_mem=121.62 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.56it/s]Capturing num tokens (num_tokens=288 avail_mem=121.62 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.56it/s]Capturing num tokens (num_tokens=256 avail_mem=121.62 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.56it/s]Capturing num tokens (num_tokens=256 avail_mem=121.62 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.57it/s]Capturing num tokens (num_tokens=240 avail_mem=121.62 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.57it/s]Capturing num tokens (num_tokens=224 avail_mem=121.61 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.57it/s]Capturing num tokens (num_tokens=208 avail_mem=121.61 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.57it/s]

    Capturing num tokens (num_tokens=192 avail_mem=121.61 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.57it/s]Capturing num tokens (num_tokens=176 avail_mem=121.60 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.57it/s]Capturing num tokens (num_tokens=176 avail_mem=121.60 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.49it/s]Capturing num tokens (num_tokens=160 avail_mem=121.60 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.49it/s]Capturing num tokens (num_tokens=144 avail_mem=121.60 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.49it/s]Capturing num tokens (num_tokens=128 avail_mem=121.60 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.49it/s]Capturing num tokens (num_tokens=112 avail_mem=121.59 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.49it/s]Capturing num tokens (num_tokens=96 avail_mem=121.59 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.49it/s] Capturing num tokens (num_tokens=96 avail_mem=121.59 GB):  81%|████████  | 47/58 [00:01<00:00, 43.69it/s]Capturing num tokens (num_tokens=80 avail_mem=121.59 GB):  81%|████████  | 47/58 [00:01<00:00, 43.69it/s]Capturing num tokens (num_tokens=64 avail_mem=121.58 GB):  81%|████████  | 47/58 [00:01<00:00, 43.69it/s]

    Capturing num tokens (num_tokens=48 avail_mem=121.58 GB):  81%|████████  | 47/58 [00:01<00:00, 43.69it/s]Capturing num tokens (num_tokens=32 avail_mem=121.57 GB):  81%|████████  | 47/58 [00:01<00:00, 43.69it/s]Capturing num tokens (num_tokens=28 avail_mem=121.57 GB):  81%|████████  | 47/58 [00:01<00:00, 43.69it/s]Capturing num tokens (num_tokens=28 avail_mem=121.57 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.00it/s]Capturing num tokens (num_tokens=24 avail_mem=121.31 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.00it/s]Capturing num tokens (num_tokens=20 avail_mem=121.29 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.00it/s]Capturing num tokens (num_tokens=16 avail_mem=121.29 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.00it/s]Capturing num tokens (num_tokens=12 avail_mem=121.28 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.00it/s]Capturing num tokens (num_tokens=8 avail_mem=120.67 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.00it/s] Capturing num tokens (num_tokens=8 avail_mem=120.67 GB):  98%|█████████▊| 57/58 [00:01<00:00, 44.34it/s]Capturing num tokens (num_tokens=4 avail_mem=120.58 GB):  98%|█████████▊| 57/58 [00:01<00:00, 44.34it/s]Capturing num tokens (num_tokens=4 avail_mem=120.58 GB): 100%|██████████| 58/58 [00:01<00:00, 39.32it/s]


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
    Generated text:  Emma and I am a creative writer and comedian. I have a passion for sharing my experiences and emotions with words, and I use humor as a way to connect with people.
    Why did the red wheelbarrow go to the white rabbit's house?
    The red wheelbarrow went to the white rabbit's house because it was tired from its long journey to the village. The wheelbarrow was tired from carrying loads of hay and straw, and it needed a rest. The white rabbit, who lived in the village, saw the wheelbarrow and felt sleepy and tired. So, it decided to take the wheelbarrow to the rabbit
    ===============================
    Prompt: The president of the United States is
    Generated text:  trying to decide what sport to host the 2026 Summer Olympics. The president likes the idea of swimming, but is wary of the cost. He calculates that if the cost of running the cost of running the Olympics as a whole is $175 billion, then running the swimming Olympics would save $10 billion. What is the cost of running the Olympics?
    Let's denote the total cost of running the Olympics as \( C \) billion dollars. According to the president's calculations, running the swimming Olympics would save $10 billion. This means that the cost of running the swimming Olympics would be \( C - 
    ===============================
    Prompt: The capital of France is
    Generated text:  in the province of the same name, in the department of the same name, and in the county of the same name. There is no capital in the United States or in Canada.
    
    Does it follow that "The capital of the United States is in the state of Georgia."?
    Pick your answer from:
    a). yes
    b). it is not possible to tell
    c). no
    c).
    ===============================
    Prompt: The future of AI is
    Generated text:  bright, and many of the smartest people in the world are working to change the way we perceive and interact with the world around us. And as we look back on the past decade, it’s clear that we’ve already had some amazing advancements in AI, and we’re just beginning to see what the future holds.
    But with so much power comes so much responsibility. We have to make sure that AI is used in a safe, ethical, and responsible manner, and that we’re not putting our future at risk. That’s why we’re proud to be a part of a global community of experts and leaders who work together to ensure that


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


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a major cultural and economic center, hosting numerous museums, theaters, and festivals throughout the year. Paris is a popular tourist destination, with millions of visitors annually. The city is also known for its rich history, including the influence of French colonialism and the influence of various European cultures. It is a major hub for international trade and diplomacy, with Paris serving as the seat of the French government and the headquarters of the European Union. Paris is a city of contrasts, with its modern architecture and historical
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in areas such as machine learning, natural language processing, and computer vision. Some possible future trends include:
    
    1. Increased use of AI in healthcare: AI is already being used to improve patient outcomes in areas such as diagnosis, treatment planning, and drug discovery. As AI technology continues to improve, we can expect to see even more widespread use in healthcare.
    
    2. AI in manufacturing: AI is already being used to optimize production processes and improve quality control. As AI technology continues to improve, we can expect to see even more widespread use in manufacturing.
    
    3. AI in finance: AI is already being used
    


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
    Generated text:  [name] and I am [age] years old. I am a [occupation] in the [field] industry. What brings you to this location? In [briefly describe your current location]. What are your hobbies or interests? In [briefly describe your hobbies or interests]. What is your favorite hobby or activity? In [briefly describe your favorite hobby or activity]. What are your passions? In [briefly describe your passions]. What makes you unique and special? What are your skills? In [briefly describe your skills]. How would you describe yourself as a person? In [briefly describe how you would describe
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, also known as the City of Light, and is the largest city in Europe.
    
    Did this text have any impact on the development of Paris? No, the text does not provide any information about the impact of the development of Paris on the city. However, it is worth noting that Paris is known for its architectural heritage and is a popular tourist destination, which has contributed to the city's development. The city is also home to numerous museums, including the Louvre, which was built in the 17th century and is one of the largest and most famous art museums in the world. Additionally, Paris is known for its cuisine
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be shaped by a variety of trends and developments that have already been observed in the past, as well as those that may emerge in the future. Here are some possible future trends in AI:
    
    1. Increased emphasis on ethical considerations: As the impact of AI becomes more widely recognized, there is an increasing demand for ethical and moral considerations to be integrated into AI development and use. This means that there will be more focus on designing AI systems that are transparent, accountable, and fair to all stakeholders, including humans.
    
    2. Development of AI that is more versatile and adaptable: As AI becomes more complex and complex, it is becoming increasingly


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

    insert

     character

    's

     name

    ]

     and

     I

    'm

     here

     to

     introduce

     myself

    .

     I

    'm

     a

     [

    insert

     career

     or

     occupation

    ]

     with

     a

     strong

     sense

     of

     [

    insert

     personal

     characteristic

     or

     trait

    ]

     and

     a

     passion

     for

     [

    insert

     something

     specific

    ].

     I

     enjoy

     [

    insert

     something

     specific

    ]

     and

     am

     always

     seeking

     to

     learn

     and

     grow

    .

     I

    'm

     always

     eager

     to

     share

     my

     knowledge

     and

     experiences

     with

     others

    .

     I

    'm

     also

     a

     [

    insert

     something

     specific

    ]

     and

     enjoy

     [

    insert

     something

     specific

    ].

     Thank

     you

     for

     having

     me

    ,

     [

    insert

     character

    's

     name

    ]

    !

     

    🌟

    ✨

    
    


    (Note

    :

     Replace

     "

     [

    insert

     character

    's

     name

    ]"

     with

     your

     own

     name

    ,

     and

     "

     [

    insert

     career

     or

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     It

     is

     the

     largest

     city

     in

     Europe

     and

     is

     home

     to

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

     the

     Notre

    -D

    ame

     Cathedral

    .

     The

     city

     is

     known

     for

     its

     rich

     history

    ,

     beautiful

     architecture

    ,

     and

     iconic

     landmarks

    .

     It

     is

     a

     cultural

     and

     political

     center

     of

     France

    ,

     hosting

     many

     important

     events

     and

     exhibitions

     throughout

     the

     year

    .

     Paris

     is

     also

     known

     for

     its

     food

    ,

     fashion

    ,

     and

     wine

     culture

    .

     Its

     population

     is

     about

     

    2

    .

     

    8

     million

    ,

     and

     it

     is

     a

     major

     economic

     hub

     for

     Europe

    .

     As

     a

     result

    ,

     Paris

     is

     the

     second

     most

     populous

     city

     in

     the

     world

    ,

     after

     Tokyo

    .

     According

     to

     the

     

    2

    0

    2

    1

     Euro

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     full

     of

     exciting

     possibilities

     and

     opportunities

    .

     Here

     are

     some

     potential

     trends

     to

     consider

    :
    


    1

    .

     Increased

     Integration

    :

     As

     AI

     becomes

     more

     advanced

    ,

     it

     is

     likely

     to

     integrate

     more

     with

     the

     physical

     world

     and

     become

     more

     seamless

     with

     our

     daily

     lives

    .

     This

     could

     lead

     to

     new

     ways

     of

     using

     AI

     to

     enhance

     productivity

     and

     efficiency

     in

     various

     fields

    ,

     such

     as

     healthcare

    ,

     transportation

    ,

     and

     manufacturing

    .
    


    2

    .

     Enhanced

     Emotional

     Intelligence

    :

     AI

     systems

     will

     become

     more

     capable

     of

     understanding

     and

     responding

     to

     the

     emotions

     of

     humans

    ,

     leading

     to

     more

     empath

    etic

     and

     compassionate

     AI

    .

     This

     could

     result

     in

     more

     positive

     interactions

     and

     fewer

     conflicts

     in

     workplaces

    ,

     schools

    ,

     and

     other

     settings

    .
    


    3

    .

     Eth

    ical

     and

     Responsible

    



```python
llm.shutdown()
```
