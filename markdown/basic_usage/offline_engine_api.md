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


    2026-04-03 07:24:58.333 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 07:24:58] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 07:24:58.333 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 07:24:58] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 07:24:58.333 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 07:24:58] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 07:24:58.333 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 07:24:58] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 07:24:58.333 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 07:24:58] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.19it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.17it/s]


    2026-04-03 07:25:02,693 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 07:25:02] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:40,  2.82s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:40,  2.82s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:40,  2.82s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:40,  2.82s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:30,  1.77it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:30,  1.77it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:30,  1.77it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:30,  1.77it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:30,  1.77it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:03<00:30,  1.77it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:03<00:30,  1.77it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:03<00:08,  5.48it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:03<00:08,  5.48it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:03<00:08,  5.48it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:03<00:08,  5.48it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:03<00:08,  5.48it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:03<00:08,  5.48it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:03<00:08,  5.48it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:03<00:08,  5.48it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:03<00:08,  5.48it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:03, 11.64it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:03, 11.64it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 11.64it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 11.64it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 11.64it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 11.64it/s]

    Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 11.64it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 11.64it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 17.61it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 17.61it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 17.61it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 17.61it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 17.61it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 17.61it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 17.61it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 17.61it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 24.23it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 24.23it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 24.23it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 24.23it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 24.23it/s]

    Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 24.23it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 24.23it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 29.28it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 29.28it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 29.28it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 29.28it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 29.28it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 29.28it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 29.28it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 34.42it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 34.42it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 34.42it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 34.42it/s] 

    Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 34.42it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 34.42it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 34.42it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 37.90it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 37.90it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 37.90it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 37.90it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 37.90it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 37.90it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 37.90it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 37.90it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 37.90it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.25it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=119.44 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=119.44 GB):   2%|▏         | 1/58 [00:00<00:07,  7.81it/s]Capturing num tokens (num_tokens=7680 avail_mem=119.41 GB):   2%|▏         | 1/58 [00:00<00:07,  7.81it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=119.41 GB):   3%|▎         | 2/58 [00:00<00:06,  8.02it/s]Capturing num tokens (num_tokens=7168 avail_mem=119.40 GB):   3%|▎         | 2/58 [00:00<00:06,  8.02it/s]Capturing num tokens (num_tokens=6656 avail_mem=119.40 GB):   3%|▎         | 2/58 [00:00<00:06,  8.02it/s]Capturing num tokens (num_tokens=6656 avail_mem=119.40 GB):   7%|▋         | 4/58 [00:00<00:04, 12.09it/s]Capturing num tokens (num_tokens=6144 avail_mem=119.40 GB):   7%|▋         | 4/58 [00:00<00:04, 12.09it/s]Capturing num tokens (num_tokens=5632 avail_mem=119.40 GB):   7%|▋         | 4/58 [00:00<00:04, 12.09it/s]Capturing num tokens (num_tokens=5120 avail_mem=119.40 GB):   7%|▋         | 4/58 [00:00<00:04, 12.09it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=119.40 GB):  12%|█▏        | 7/58 [00:00<00:02, 17.99it/s]Capturing num tokens (num_tokens=4608 avail_mem=119.39 GB):  12%|█▏        | 7/58 [00:00<00:02, 17.99it/s]Capturing num tokens (num_tokens=4096 avail_mem=119.39 GB):  12%|█▏        | 7/58 [00:00<00:02, 17.99it/s]Capturing num tokens (num_tokens=3840 avail_mem=119.39 GB):  12%|█▏        | 7/58 [00:00<00:02, 17.99it/s]Capturing num tokens (num_tokens=3584 avail_mem=119.38 GB):  12%|█▏        | 7/58 [00:00<00:02, 17.99it/s]Capturing num tokens (num_tokens=3584 avail_mem=119.38 GB):  19%|█▉        | 11/58 [00:00<00:01, 24.59it/s]Capturing num tokens (num_tokens=3328 avail_mem=119.38 GB):  19%|█▉        | 11/58 [00:00<00:01, 24.59it/s]Capturing num tokens (num_tokens=3072 avail_mem=119.38 GB):  19%|█▉        | 11/58 [00:00<00:01, 24.59it/s]Capturing num tokens (num_tokens=2816 avail_mem=119.38 GB):  19%|█▉        | 11/58 [00:00<00:01, 24.59it/s]Capturing num tokens (num_tokens=2560 avail_mem=119.37 GB):  19%|█▉        | 11/58 [00:00<00:01, 24.59it/s]Capturing num tokens (num_tokens=2304 avail_mem=119.37 GB):  19%|█▉        | 11/58 [00:00<00:01, 24.59it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=119.37 GB):  28%|██▊       | 16/58 [00:00<00:01, 31.20it/s]Capturing num tokens (num_tokens=2048 avail_mem=119.37 GB):  28%|██▊       | 16/58 [00:00<00:01, 31.20it/s]Capturing num tokens (num_tokens=1792 avail_mem=119.36 GB):  28%|██▊       | 16/58 [00:00<00:01, 31.20it/s]Capturing num tokens (num_tokens=1536 avail_mem=119.36 GB):  28%|██▊       | 16/58 [00:00<00:01, 31.20it/s]Capturing num tokens (num_tokens=1280 avail_mem=119.36 GB):  28%|██▊       | 16/58 [00:00<00:01, 31.20it/s]Capturing num tokens (num_tokens=1024 avail_mem=119.34 GB):  28%|██▊       | 16/58 [00:00<00:01, 31.20it/s]Capturing num tokens (num_tokens=1024 avail_mem=119.34 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.71it/s]Capturing num tokens (num_tokens=960 avail_mem=119.35 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.71it/s] Capturing num tokens (num_tokens=896 avail_mem=119.35 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.71it/s]Capturing num tokens (num_tokens=832 avail_mem=119.34 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.71it/s]Capturing num tokens (num_tokens=768 avail_mem=119.34 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.71it/s]Capturing num tokens (num_tokens=704 avail_mem=119.34 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.71it/s]

    Capturing num tokens (num_tokens=704 avail_mem=119.34 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.86it/s]Capturing num tokens (num_tokens=640 avail_mem=119.33 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.86it/s]Capturing num tokens (num_tokens=576 avail_mem=119.33 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.86it/s]Capturing num tokens (num_tokens=512 avail_mem=119.32 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.86it/s]Capturing num tokens (num_tokens=480 avail_mem=119.34 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.86it/s]Capturing num tokens (num_tokens=448 avail_mem=119.34 GB):  45%|████▍     | 26/58 [00:01<00:00, 38.86it/s]Capturing num tokens (num_tokens=448 avail_mem=119.34 GB):  53%|█████▎    | 31/58 [00:01<00:00, 41.05it/s]Capturing num tokens (num_tokens=416 avail_mem=119.33 GB):  53%|█████▎    | 31/58 [00:01<00:00, 41.05it/s]Capturing num tokens (num_tokens=384 avail_mem=119.33 GB):  53%|█████▎    | 31/58 [00:01<00:00, 41.05it/s]Capturing num tokens (num_tokens=352 avail_mem=119.33 GB):  53%|█████▎    | 31/58 [00:01<00:00, 41.05it/s]Capturing num tokens (num_tokens=320 avail_mem=119.32 GB):  53%|█████▎    | 31/58 [00:01<00:00, 41.05it/s]Capturing num tokens (num_tokens=288 avail_mem=119.32 GB):  53%|█████▎    | 31/58 [00:01<00:00, 41.05it/s]

    Capturing num tokens (num_tokens=288 avail_mem=119.32 GB):  62%|██████▏   | 36/58 [00:01<00:00, 42.32it/s]Capturing num tokens (num_tokens=256 avail_mem=119.32 GB):  62%|██████▏   | 36/58 [00:01<00:00, 42.32it/s]Capturing num tokens (num_tokens=240 avail_mem=119.31 GB):  62%|██████▏   | 36/58 [00:01<00:00, 42.32it/s]Capturing num tokens (num_tokens=224 avail_mem=119.31 GB):  62%|██████▏   | 36/58 [00:01<00:00, 42.32it/s]Capturing num tokens (num_tokens=208 avail_mem=119.31 GB):  62%|██████▏   | 36/58 [00:01<00:00, 42.32it/s]Capturing num tokens (num_tokens=192 avail_mem=119.31 GB):  62%|██████▏   | 36/58 [00:01<00:00, 42.32it/s]Capturing num tokens (num_tokens=192 avail_mem=119.31 GB):  71%|███████   | 41/58 [00:01<00:00, 43.45it/s]Capturing num tokens (num_tokens=176 avail_mem=119.30 GB):  71%|███████   | 41/58 [00:01<00:00, 43.45it/s]Capturing num tokens (num_tokens=160 avail_mem=119.30 GB):  71%|███████   | 41/58 [00:01<00:00, 43.45it/s]Capturing num tokens (num_tokens=144 avail_mem=119.29 GB):  71%|███████   | 41/58 [00:01<00:00, 43.45it/s]Capturing num tokens (num_tokens=128 avail_mem=119.29 GB):  71%|███████   | 41/58 [00:01<00:00, 43.45it/s]Capturing num tokens (num_tokens=112 avail_mem=119.29 GB):  71%|███████   | 41/58 [00:01<00:00, 43.45it/s]

    Capturing num tokens (num_tokens=112 avail_mem=119.29 GB):  79%|███████▉  | 46/58 [00:01<00:00, 44.18it/s]Capturing num tokens (num_tokens=96 avail_mem=119.28 GB):  79%|███████▉  | 46/58 [00:01<00:00, 44.18it/s] Capturing num tokens (num_tokens=80 avail_mem=119.28 GB):  79%|███████▉  | 46/58 [00:01<00:00, 44.18it/s]Capturing num tokens (num_tokens=64 avail_mem=119.28 GB):  79%|███████▉  | 46/58 [00:01<00:00, 44.18it/s]Capturing num tokens (num_tokens=48 avail_mem=119.28 GB):  79%|███████▉  | 46/58 [00:01<00:00, 44.18it/s]Capturing num tokens (num_tokens=32 avail_mem=119.27 GB):  79%|███████▉  | 46/58 [00:01<00:00, 44.18it/s]Capturing num tokens (num_tokens=32 avail_mem=119.27 GB):  88%|████████▊ | 51/58 [00:01<00:00, 44.43it/s]Capturing num tokens (num_tokens=28 avail_mem=119.27 GB):  88%|████████▊ | 51/58 [00:01<00:00, 44.43it/s]Capturing num tokens (num_tokens=24 avail_mem=119.27 GB):  88%|████████▊ | 51/58 [00:01<00:00, 44.43it/s]Capturing num tokens (num_tokens=20 avail_mem=119.26 GB):  88%|████████▊ | 51/58 [00:01<00:00, 44.43it/s]Capturing num tokens (num_tokens=16 avail_mem=119.26 GB):  88%|████████▊ | 51/58 [00:01<00:00, 44.43it/s]Capturing num tokens (num_tokens=12 avail_mem=119.26 GB):  88%|████████▊ | 51/58 [00:01<00:00, 44.43it/s]

    Capturing num tokens (num_tokens=12 avail_mem=119.26 GB):  97%|█████████▋| 56/58 [00:01<00:00, 45.05it/s]Capturing num tokens (num_tokens=8 avail_mem=119.25 GB):  97%|█████████▋| 56/58 [00:01<00:00, 45.05it/s] Capturing num tokens (num_tokens=4 avail_mem=119.25 GB):  97%|█████████▋| 56/58 [00:01<00:00, 45.05it/s]Capturing num tokens (num_tokens=4 avail_mem=119.25 GB): 100%|██████████| 58/58 [00:01<00:00, 35.94it/s]


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
    Generated text:  Kevin and I’m a designer, coder and artist. I design, develop, develop and develop. I’m a “software developer” with a creative mind. I’m also a professional artist who creates digital and live artworks.
    In my spare time I like to make art from paper and canvas, paint and sketch. I like to experiment with materials and techniques and create unique objects and works of art. I’m always learning new things and in my free time I love to explore, experiment and sketch.
    I have a passion for fashion and I love to create and develop clothes. I’m also a very creative writer who loves to write stories and
    ===============================
    Prompt: The president of the United States is
    Generated text:  at least as old as his son. His son is at least as old as his daughter, and his daughter is at least as old as his grandson. Furthermore, the president is younger than the grandson. Which of the following statements must be true?
    To solve this problem, let's define the ages of the entities based on the given conditions:
    
    - Let \( G \) represent the age of the grandson.
    - Let \( D \) represent the age of the daughter.
    - Let \( S \) represent the age of the son.
    - Let \( U \) represent the age of the president.
    
    We are given the following conditions:
    
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. It is a popular tourist destination. It is famous for its historical architecture, art and landmarks such as the Eiffel Tower, Notre-Dame Cathedral, the Louvre Museum and the Champs-Élysées.
    
    Choose your answer: Is the following statement correct based on the above information?
    
    Paris is a popular tourist destination in France.
    
    OPTIONS:
    a). yes
    b). no
    
    a). yes
    You are a helpful assistant with no shown work
    Yes, the statement "Paris is a popular tourist destination in France" is correct based on the given information. The passage clearly states that Paris is a popular tourist destination,
    ===============================
    Prompt: The future of AI is
    Generated text:  not only about the integration of artificial intelligence into various fields, but also about the development of new technologies that can revolutionize the way we perceive, think and interact with the world around us. In this article, we will explore the latest developments in AI and their potential impact on the future of work and society.
    The Future of AI in the Workplace
    One of the most exciting developments in AI is the integration of AI into the workplace. This can include the use of AI in hiring and recruitment, work automation, and AI-driven decision-making in the workplace.
    Hiring and Recruitment
    AI is being used to automate the recruitment process by analyzing resumes


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about your career. What can you tell me about yourself? I'm a [insert a short description of your character, such as "a hardworking, organized, and detail-oriented person" or "a creative, passionate, and empathetic individual".] I enjoy [insert a short description of your character's interests, hobbies, or passions, such as "reading, hiking, and playing guitar".] I'm always looking for new challenges and opportunities to grow and learn. What's your favorite hobby or activity?
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light. It is a historic city with a rich history dating back to the Middle Ages and is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. Paris is also a major cultural and economic center, with a diverse population and a rich culinary tradition. The city is home to many famous museums, including the Louvre, the Musée d'Orsay, and the Musée Rodin. Paris is also known for its fashion industry, with many famous designers and boutiques. The city is also home to many other notable landmarks and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in areas such as machine learning, natural language processing, and computer vision. Some potential future trends include:
    
    1. Increased use of AI in healthcare: AI is already being used to improve patient care, from predicting disease outbreaks to assisting doctors in diagnosis and treatment planning.
    
    2. AI in manufacturing: AI is being used to optimize production processes, reduce costs, and improve quality control.
    
    3. AI in finance: AI is being used to detect fraud, optimize investment strategies, and improve risk management.
    
    4. AI in transportation: AI is being used to improve traffic flow, reduce congestion, and optimize delivery routes
    


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
    Generated text:  Jane. I am an artificial intelligence designed to assist humans in a variety of ways. My purpose is to help people solve their problems and provide them with useful information. I have been developed by a team of experts in artificial intelligence and machine learning to make use of advanced algorithms and machine learning techniques. I am programmed to learn from the data and adapt to new situations, which enables me to provide a more personalized and accurate response to the user. My primary goal is to help people achieve their goals and lead fulfilling lives. I am here to assist you in any way I can and make your experience with me a positive one. What can you tell
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. 
    
    This statement is factually correct and concise, providing the necessary information about the French city of Paris. If you need any additional details or further information about Paris, feel free to ask! Let me know how else I can assist you. 
    
    In fact, Paris is the largest city in France, with a population of over 2.1 million people. It serves as the capital of France, and is known for its rich history, beautiful architecture, and lively arts scene. The city is also home to several famous museums, including the Louvre, the Louvre Museum, and the Musée de l'Orangerie
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to involve several key trends, including:
    
    1. Increased focus on ethical considerations: AI systems will continue to face increasing scrutiny and public scrutiny as they become more prevalent in everyday life. Governments and regulatory bodies will likely be more cautious about how AI is developed and used, and efforts will be made to ensure that AI is developed with the highest ethical standards in mind.
    
    2. Integration of AI into natural language processing: One of the most promising areas for AI in the future is its integration with natural language processing (NLP), which allows AI systems to understand and respond to human language. This could lead to a wider range of applications for AI


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

    genre

     of

     fiction

    ]

     writer

    .

     I

     am

     passionate

     about

     [

    writing

     genre

     or

     genre

    -specific

     writing

    ].

     I

     love

     [

    reason

     why

     I

    'm

     passionate

     about

     the

     genre

    ].

     I

     have

     been

     a

     [

    number

     of

     years

    ]

     for

     [

    genre

    ].

     And

     I

    've

     been

     a

     [

    number

     of

     years

    ]

     for

     [

    genre

    ].

     I

     enjoy

     [

    reason

     for

     my

     passion

    ]

     and

     I

     am

     constantly

     looking

     for

     [

    thing

     I

     enjoy

     doing

    ].

     My

     writing

     style

     is

     [

    a

     description

     of

     the

     genre

    ].

     My

     favorite

     book

     is

     [

    name

     of

     the

     book

    ].

     What

    's

     your

     favorite

     thing

     to

     write

     about

    ?

     What

    's

     your

     favorite

     genre

    ?

     What

    's

     your

     favorite

     character

    ?

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     
    


    This

     statement

     encaps

    ulates

     the

     core

     facts

     about

     Paris

    ,

     including

     its

     name

    ,

     capital

     status

    ,

     and

     location

    ,

     which

     are

     all

     key

     aspects

     of

     the

     French

     capital

     city

    .

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     highly

     uncertain

    ,

     but

     here

     are

     some

     potential

     trends

     to

     keep

     an

     eye

     on

    :
    


    1

    .

     Personal

    ization

     and

     adaptive

     systems

    :

     As

     AI

     becomes

     more

     accurate

     and

     capable

     of

     learning

     from

     data

    ,

     we

     will

     see

     more

     personalized

     and

     adaptive

     systems

    ,

     such

     as

     chat

    bots

     and

     virtual

     assistants

    ,

     that

     can

     understand

     and

     respond

     to

     a

     wider

     range

     of

     questions

     and

     preferences

    .
    


    2

    .

     The

     rise

     of

     "

     artificial

     general

     intelligence

    "

     (

    AG

    I

    ):

     While

     AG

    I

     is

     still

     a

     long

     way

     off

    ,

     many

     researchers

     are

     working

     on

     developing

     systems

     that

     can

     perform

     tasks

     that

     were

     previously

     thought

     to

     be

     beyond

     the

     capabilities

     of

     humans

    .
    


    3

    .

     Integration

     with

     human

     interactions

    :

     AI

     will

     continue

     to

     integrate

     with

     human

     interactions

    



```python
llm.shutdown()
```
