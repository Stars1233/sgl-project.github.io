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


    2026-03-31 06:09:50.874 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 06:09:50] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 06:09:50.874 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 06:09:50] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 06:09:50.874 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 06:09:50] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 06:09:50.874 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 06:09:50] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 06:09:50.874 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 06:09:50] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.99it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.98it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:10,  2.29s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:10,  2.29s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:10,  2.29s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:10,  2.29s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:24,  2.17it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:24,  2.17it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:24,  2.17it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:24,  2.17it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:24,  2.17it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:24,  2.17it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:24,  2.17it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.58it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.58it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.58it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.58it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.58it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.58it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.58it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.58it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.58it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:02, 13.75it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:02, 13.75it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:02, 13.75it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:02, 13.75it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:02, 13.75it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:02, 13.75it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:02, 13.75it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:02<00:02, 13.75it/s]

    Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 20.40it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 20.40it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 20.40it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 20.40it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 20.40it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:02<00:01, 20.40it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:02<00:01, 20.40it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:02<00:01, 26.13it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:02<00:01, 26.13it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:02<00:01, 26.13it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:02<00:01, 26.13it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:02<00:01, 26.13it/s]Compiling num tokens (num_tokens=288):  53%|█████▎    | 31/58 [00:02<00:01, 26.13it/s]Compiling num tokens (num_tokens=256):  53%|█████▎    | 31/58 [00:02<00:01, 26.13it/s]

    Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:02<00:00, 31.91it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:02<00:00, 31.91it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:02<00:00, 31.91it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:02<00:00, 31.91it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:02<00:00, 31.91it/s]Compiling num tokens (num_tokens=176):  64%|██████▍   | 37/58 [00:03<00:00, 31.91it/s]Compiling num tokens (num_tokens=160):  64%|██████▍   | 37/58 [00:03<00:00, 31.91it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:03<00:00, 36.37it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:03<00:00, 36.37it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:03<00:00, 36.37it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:03<00:00, 36.37it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:03<00:00, 36.37it/s] Compiling num tokens (num_tokens=80):  74%|███████▍  | 43/58 [00:03<00:00, 36.37it/s]

    Compiling num tokens (num_tokens=64):  74%|███████▍  | 43/58 [00:03<00:00, 36.37it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:03<00:00, 39.52it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:03<00:00, 39.52it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:03<00:00, 39.52it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:03<00:00, 39.52it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:03<00:00, 39.52it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:03<00:00, 39.52it/s]Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:03<00:00, 39.52it/s]Compiling num tokens (num_tokens=12):  84%|████████▍ | 49/58 [00:03<00:00, 39.52it/s]Compiling num tokens (num_tokens=8):  84%|████████▍ | 49/58 [00:03<00:00, 39.52it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:03<00:00, 48.06it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:03<00:00, 48.06it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.70it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=119.67 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=119.67 GB):   2%|▏         | 1/58 [00:00<00:20,  2.79it/s]Capturing num tokens (num_tokens=7680 avail_mem=119.64 GB):   2%|▏         | 1/58 [00:00<00:20,  2.79it/s]Capturing num tokens (num_tokens=7680 avail_mem=119.64 GB):   3%|▎         | 2/58 [00:00<00:13,  4.28it/s]Capturing num tokens (num_tokens=7168 avail_mem=119.62 GB):   3%|▎         | 2/58 [00:00<00:13,  4.28it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=119.05 GB):   3%|▎         | 2/58 [00:00<00:13,  4.28it/s]Capturing num tokens (num_tokens=6656 avail_mem=119.05 GB):   7%|▋         | 4/58 [00:00<00:06,  8.30it/s]Capturing num tokens (num_tokens=6144 avail_mem=119.05 GB):   7%|▋         | 4/58 [00:00<00:06,  8.30it/s]Capturing num tokens (num_tokens=5632 avail_mem=119.05 GB):   7%|▋         | 4/58 [00:00<00:06,  8.30it/s]Capturing num tokens (num_tokens=5120 avail_mem=119.05 GB):   7%|▋         | 4/58 [00:00<00:06,  8.30it/s]Capturing num tokens (num_tokens=5120 avail_mem=119.05 GB):  12%|█▏        | 7/58 [00:00<00:03, 13.82it/s]Capturing num tokens (num_tokens=4608 avail_mem=119.05 GB):  12%|█▏        | 7/58 [00:00<00:03, 13.82it/s]Capturing num tokens (num_tokens=4096 avail_mem=119.04 GB):  12%|█▏        | 7/58 [00:00<00:03, 13.82it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=119.04 GB):  12%|█▏        | 7/58 [00:00<00:03, 13.82it/s]Capturing num tokens (num_tokens=3584 avail_mem=119.03 GB):  12%|█▏        | 7/58 [00:00<00:03, 13.82it/s]Capturing num tokens (num_tokens=3584 avail_mem=119.03 GB):  19%|█▉        | 11/58 [00:00<00:02, 20.44it/s]Capturing num tokens (num_tokens=3328 avail_mem=119.03 GB):  19%|█▉        | 11/58 [00:00<00:02, 20.44it/s]Capturing num tokens (num_tokens=3072 avail_mem=119.03 GB):  19%|█▉        | 11/58 [00:00<00:02, 20.44it/s]Capturing num tokens (num_tokens=2816 avail_mem=119.03 GB):  19%|█▉        | 11/58 [00:00<00:02, 20.44it/s]Capturing num tokens (num_tokens=2560 avail_mem=119.02 GB):  19%|█▉        | 11/58 [00:00<00:02, 20.44it/s]Capturing num tokens (num_tokens=2304 avail_mem=119.02 GB):  19%|█▉        | 11/58 [00:00<00:02, 20.44it/s]Capturing num tokens (num_tokens=2304 avail_mem=119.02 GB):  28%|██▊       | 16/58 [00:00<00:01, 27.49it/s]Capturing num tokens (num_tokens=2048 avail_mem=119.02 GB):  28%|██▊       | 16/58 [00:00<00:01, 27.49it/s]Capturing num tokens (num_tokens=1792 avail_mem=119.01 GB):  28%|██▊       | 16/58 [00:00<00:01, 27.49it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=119.01 GB):  28%|██▊       | 16/58 [00:00<00:01, 27.49it/s]Capturing num tokens (num_tokens=1280 avail_mem=119.00 GB):  28%|██▊       | 16/58 [00:01<00:01, 27.49it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.98 GB):  28%|██▊       | 16/58 [00:01<00:01, 27.49it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.98 GB):  36%|███▌      | 21/58 [00:01<00:01, 32.73it/s]Capturing num tokens (num_tokens=960 avail_mem=119.00 GB):  36%|███▌      | 21/58 [00:01<00:01, 32.73it/s] Capturing num tokens (num_tokens=896 avail_mem=118.99 GB):  36%|███▌      | 21/58 [00:01<00:01, 32.73it/s]Capturing num tokens (num_tokens=832 avail_mem=118.99 GB):  36%|███▌      | 21/58 [00:01<00:01, 32.73it/s]Capturing num tokens (num_tokens=768 avail_mem=118.98 GB):  36%|███▌      | 21/58 [00:01<00:01, 32.73it/s]Capturing num tokens (num_tokens=704 avail_mem=118.98 GB):  36%|███▌      | 21/58 [00:01<00:01, 32.73it/s]Capturing num tokens (num_tokens=704 avail_mem=118.98 GB):  45%|████▍     | 26/58 [00:01<00:00, 36.34it/s]Capturing num tokens (num_tokens=640 avail_mem=118.95 GB):  45%|████▍     | 26/58 [00:01<00:00, 36.34it/s]Capturing num tokens (num_tokens=576 avail_mem=118.92 GB):  45%|████▍     | 26/58 [00:01<00:00, 36.34it/s]

    Capturing num tokens (num_tokens=512 avail_mem=118.91 GB):  45%|████▍     | 26/58 [00:01<00:00, 36.34it/s]Capturing num tokens (num_tokens=480 avail_mem=118.92 GB):  45%|████▍     | 26/58 [00:01<00:00, 36.34it/s]Capturing num tokens (num_tokens=448 avail_mem=118.92 GB):  45%|████▍     | 26/58 [00:01<00:00, 36.34it/s]Capturing num tokens (num_tokens=448 avail_mem=118.92 GB):  53%|█████▎    | 31/58 [00:01<00:00, 38.94it/s]Capturing num tokens (num_tokens=416 avail_mem=118.92 GB):  53%|█████▎    | 31/58 [00:01<00:00, 38.94it/s]Capturing num tokens (num_tokens=384 avail_mem=118.92 GB):  53%|█████▎    | 31/58 [00:01<00:00, 38.94it/s]Capturing num tokens (num_tokens=352 avail_mem=118.91 GB):  53%|█████▎    | 31/58 [00:01<00:00, 38.94it/s]Capturing num tokens (num_tokens=320 avail_mem=118.91 GB):  53%|█████▎    | 31/58 [00:01<00:00, 38.94it/s]Capturing num tokens (num_tokens=288 avail_mem=118.90 GB):  53%|█████▎    | 31/58 [00:01<00:00, 38.94it/s]Capturing num tokens (num_tokens=288 avail_mem=118.90 GB):  62%|██████▏   | 36/58 [00:01<00:00, 40.80it/s]Capturing num tokens (num_tokens=256 avail_mem=118.90 GB):  62%|██████▏   | 36/58 [00:01<00:00, 40.80it/s]Capturing num tokens (num_tokens=240 avail_mem=118.90 GB):  62%|██████▏   | 36/58 [00:01<00:00, 40.80it/s]

    Capturing num tokens (num_tokens=224 avail_mem=118.90 GB):  62%|██████▏   | 36/58 [00:01<00:00, 40.80it/s]Capturing num tokens (num_tokens=208 avail_mem=118.89 GB):  62%|██████▏   | 36/58 [00:01<00:00, 40.80it/s]Capturing num tokens (num_tokens=192 avail_mem=118.89 GB):  62%|██████▏   | 36/58 [00:01<00:00, 40.80it/s]Capturing num tokens (num_tokens=192 avail_mem=118.89 GB):  71%|███████   | 41/58 [00:01<00:00, 42.32it/s]Capturing num tokens (num_tokens=176 avail_mem=118.89 GB):  71%|███████   | 41/58 [00:01<00:00, 42.32it/s]Capturing num tokens (num_tokens=160 avail_mem=118.89 GB):  71%|███████   | 41/58 [00:01<00:00, 42.32it/s]Capturing num tokens (num_tokens=144 avail_mem=118.88 GB):  71%|███████   | 41/58 [00:01<00:00, 42.32it/s]Capturing num tokens (num_tokens=128 avail_mem=118.88 GB):  71%|███████   | 41/58 [00:01<00:00, 42.32it/s]Capturing num tokens (num_tokens=112 avail_mem=118.88 GB):  71%|███████   | 41/58 [00:01<00:00, 42.32it/s]Capturing num tokens (num_tokens=112 avail_mem=118.88 GB):  79%|███████▉  | 46/58 [00:01<00:00, 42.95it/s]Capturing num tokens (num_tokens=96 avail_mem=118.87 GB):  79%|███████▉  | 46/58 [00:01<00:00, 42.95it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=118.87 GB):  79%|███████▉  | 46/58 [00:01<00:00, 42.95it/s]Capturing num tokens (num_tokens=64 avail_mem=118.87 GB):  79%|███████▉  | 46/58 [00:01<00:00, 42.95it/s]Capturing num tokens (num_tokens=48 avail_mem=118.86 GB):  79%|███████▉  | 46/58 [00:01<00:00, 42.95it/s]Capturing num tokens (num_tokens=32 avail_mem=118.86 GB):  79%|███████▉  | 46/58 [00:01<00:00, 42.95it/s]Capturing num tokens (num_tokens=32 avail_mem=118.86 GB):  88%|████████▊ | 51/58 [00:01<00:00, 42.99it/s]Capturing num tokens (num_tokens=28 avail_mem=118.85 GB):  88%|████████▊ | 51/58 [00:01<00:00, 42.99it/s]Capturing num tokens (num_tokens=24 avail_mem=118.85 GB):  88%|████████▊ | 51/58 [00:01<00:00, 42.99it/s]Capturing num tokens (num_tokens=20 avail_mem=118.85 GB):  88%|████████▊ | 51/58 [00:01<00:00, 42.99it/s]Capturing num tokens (num_tokens=16 avail_mem=118.82 GB):  88%|████████▊ | 51/58 [00:01<00:00, 42.99it/s]Capturing num tokens (num_tokens=12 avail_mem=118.81 GB):  88%|████████▊ | 51/58 [00:01<00:00, 42.99it/s]

    Capturing num tokens (num_tokens=12 avail_mem=118.81 GB):  97%|█████████▋| 56/58 [00:01<00:00, 43.54it/s]Capturing num tokens (num_tokens=8 avail_mem=118.81 GB):  97%|█████████▋| 56/58 [00:01<00:00, 43.54it/s] Capturing num tokens (num_tokens=4 avail_mem=118.80 GB):  97%|█████████▋| 56/58 [00:01<00:00, 43.54it/s]Capturing num tokens (num_tokens=4 avail_mem=118.80 GB): 100%|██████████| 58/58 [00:01<00:00, 30.84it/s]


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
    Generated text:  John. I'm 27 years old. My favorite color is blue. I like to wear my favorite red shirt and I like to eat red and blue food. I don't like to use my phone because I want to spend more time with my family and friends. In the past, I had a car. I liked driving and it was my favorite hobby. Now I use a car only when I have to. I like going to the beach for the vacation. I like to play with my friends. My favorite movie is The Dark Knight. It's a scary movie. I enjoy watching it with my family. 
    
    Based on
    ===============================
    Prompt: The president of the United States is
    Generated text:  a ceremonial head of state. He or she serves a term of five years. However, in practice, there are only four out of the 50 states that have a president. These four states are Illinois, Nevada, Utah, and Wisconsin. But, as of now, the four states that are out of play for president are New York, New Jersey, and Massachusetts. 
    
    The top candidate for president to be able to succeed the current president is not confirmed yet. The two most recent top candidates are Mitt Romney and Donald Trump. 
    
    What is the probability that Mitt Romney is the top candidate to be the next president?
    
    To determine
    ===============================
    Prompt: The capital of France is
    Generated text:  ________. A. Rome B. Paris C. London D. Madrid
    Answer:
    
    B
    
    Which of the following statements about the property rights of real estate is incorrect?
    A. In real estate transactions, the land use rights of the land are the most valuable property right.
    B. The security interests in property rights are the most valuable property rights.
    C. The right to use the land is the most valuable property right.
    D. The property rights of real estate are the most valuable property rights.
    Answer:
    
    B
    
    Which of the following statements about property rights is incorrect?
    A. In real estate transactions, the land use rights of the
    ===============================
    Prompt: The future of AI is
    Generated text:  uncertain. However, an examination of recent events reveals a key underlying theme: AI will impact nearly every aspect of human life, and that will include education. According to researchers from the University of Pittsburgh and the National Institute for Standards and Technology (NIST), AI will create a new paradigm for education. The new paradigm, termed the 'deep learning' paradigm, is based on the use of large data sets and machine learning algorithms to create the next generation of digital learners. In their paper published in the Proceedings of the National Academy of Sciences, researchers from the University of Pittsburgh and the NIST describe their work on the new paradigm.
    The researchers


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a short description of your profession or role]. I enjoy [insert a short description of your hobbies or interests]. What brings you to [company name] and what do you do there? I'm always looking for new challenges and opportunities to grow and learn. What do you do at [company name] and what do you enjoy about your job? I'm always looking for new challenges and opportunities to grow and learn. What do you do
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris. It is the largest city in the European Union and the third-largest city in the world by population. Paris is known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. The city is also famous for its cuisine, fashion, and music. Paris is a cultural and economic hub that attracts millions of tourists each year. It is home to many world-renowned museums, theaters, and art galleries. The city is also known for its fashion industry, with many famous designers and boutiques. Paris is a vibrant and dynamic city that is a must-visit for anyone interested in French
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in areas such as machine learning, natural language processing, and computer vision. Some potential trends include:
    
    1. Increased use of AI in healthcare: AI is already being used to improve patient care, from diagnosing diseases to predicting patient outcomes. As AI technology continues to improve, we can expect to see even more sophisticated applications in healthcare.
    
    2. AI in manufacturing: AI is already being used to optimize production processes, reduce waste, and improve quality control. As AI technology continues to evolve, we can expect to see even more advanced applications in manufacturing.
    
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
    Generated text:  [Your Name], and I'm a/an [Your profession or title] with over [number] years of experience in [specific field or industry]. With a keen eye for detail and a natural ability to see the bigger picture, I'm always looking for opportunities to help others succeed. I love [subject matter] and am always eager to learn new skills and be a part of a community that inspires and supports others. I am always looking for new challenges and opportunities to grow and improve. I'm always willing to ask questions and seek advice to ensure my knowledge is current and relevant. Thanks for taking the time to meet me! [Your
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    Paris is the capital city of France, located in the south of the country on the island of France. It is the largest city in Europe and is known for its rich history, beautiful architecture, and vibrant culture. Paris is home to many famous landmarks such as the Eiffel Tower, the Louvre Museum, and the Notre Dame Cathedral, as well as numerous museums and museums dedicated to various art and cultural themes. The city is also known for its jazz and hip-hop scenes, and has become a popular tourist destination in recent years. Overall, Paris is a city of infinite possibility and endless creativity.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  a rapidly evolving field that is shaping up to be highly complex, and it's difficult to predict exactly what will happen. However, several trends are likely to continue to shape the field in the coming years:
    
    1. Continued improvement in processing power: AI will continue to get faster and more powerful, allowing it to process more data and perform more tasks at a faster pace. This will lead to more efficient decision-making and better problem-solving.
    
    2. Increased availability of AI: The cost of AI technologies will continue to drop, making them more accessible to businesses and individuals. This will allow more people to use AI for their own benefit, and push


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

     first

     name

    ],

     and

     I

    'm

     [

    insert

     character

    's

     profession

     and

     identity

    .

    ]"

     


    Feel

     free

     to

     add

     any

     personal

     anecdotes

     or

     background

     information

     about

     yourself

    ,

     if

     you

    'd

     like

     to

    .

     But

     make

     sure

     to

     keep

     your

     introduction

     neutral

     and

     factual

    .

     It

    's

     important

     to

     maintain

     a

     professional

     tone

     throughout

     the

     conversation

    .

     Remember

     to

     keep

     the

     length

     of

     your

     introduction

     short

     and

     sweet

    ,

     ideally

     around

     one

     to

     two

     sentences

    ,

     to

     avoid

     overwhelming

     your

     listener

    .

     Let

     me

     know

     if

     you

    'd

     like

     me

     to

     elaborate

     on

     any

     aspects

     of

     your

     identity

     or

     personal

     life

    ,

     and

     I

    'll

     do

     my

     best

     to

     provide

     helpful

     and

     informative

     responses

    .

     Good

     luck

    !

     

    🎓

    📚

    📚

    📚

    


    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     
    


    1

    .

     Paris

     is

     the

     largest

     city

     in

     France

    ,

     located

     in

     the

     heart

     of

     the

     south

    -central

     region

    .


    2

    .

     It

     is

     the

     seat

     of

     government

    ,

     administration

    ,

     culture

    ,

     education

    ,

     and

     commerce

     in

     France

    .


    3

    .

     The

     city

    's

     skyline

     is

     famous

     for

     its

     historic

     architecture

    ,

     including

     the

     Lou

    vre

    ,

     Notre

    -D

    ame

     Cathedral

    ,

     and

     E

    iff

    el

     Tower

    .


    4

    .

     Paris

     is

     known

     for

     its

     rich

     cultural

     scene

    ,

     including

     theaters

    ,

     museums

    ,

     and

     festivals

    .


    5

    .

     The

     French

     national

     anthem

    ,

     "

    La

     M

    arse

    ill

    aise

    ,"

     is

     performed

     in

     the

     city

    's

     streets

     and

     in

     public

     spaces

    .


    6

    .

     Paris

     is

     a

     popular

     tourist

     destination

    ,

     attracting

     millions

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     very

     exciting

     and

     will

     have

     a

     significant

     impact

     on

     our

     daily

     lives

    ,

     businesses

    ,

     and

     society

     as

     a

     whole

    .

     Here

     are

     some

     of

     the

     possible

     future

     trends

     in

     AI

    :
    


    1

    .

     Advanced

     AI

     techniques

    :

     AI

     techniques

     such

     as

     deep

     learning

    ,

     natural

     language

     processing

    ,

     and

     computer

     vision

     will

     continue

     to

     advance

     and

     become

     more

     advanced

    .

     These

     techniques

     will

     allow

     AI

     to

     perform

     tasks

     that

     are

     currently

     impossible

     with

     traditional

     AI

     systems

    ,

     such

     as

     generating

     art

    ,

     music

    ,

     and

     literature

    ,

     and

     understanding

     human

     emotions

     and

     experiences

    .
    


    2

    .

     Autonomous

     systems

    :

     Autonomous

     systems

    ,

     such

     as

     drones

    ,

     self

    -driving

     cars

    ,

     and

     robots

    ,

     are

     becoming

     more

     common

    .

     These

     systems

     will

     be

     able

     to

     navigate

     their

     environment

    



```python
llm.shutdown()
```
