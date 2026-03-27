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


    2026-03-27 15:10:22.449 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 15:10:22] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 15:10:22.449 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 15:10:22] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 15:10:22.449 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 15:10:22] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 15:10:22.449 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 15:10:22] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 15:10:22.449 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 15:10:22] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.13it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.12it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.50it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.50it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.50it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.50it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.50it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.50it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.50it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.50it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.50it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:02, 13.58it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:02, 13.58it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:02, 13.58it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:02, 13.58it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:02, 13.58it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:02, 13.58it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:02, 13.58it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:02<00:02, 13.58it/s]

    Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:02<00:00, 27.18it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:02<00:00, 27.18it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:02<00:00, 27.18it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:02<00:00, 27.18it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:02<00:00, 27.18it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:02<00:00, 27.18it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:02<00:00, 27.18it/s]

    Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:02<00:00, 32.16it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:02<00:00, 32.16it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:02<00:00, 32.16it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 32.16it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 32.16it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 32.16it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 32.16it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 37.09it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 37.09it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 37.09it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 37.09it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 37.09it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 37.09it/s]

    Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 37.09it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 40.08it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 40.08it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 40.08it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 40.08it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 40.08it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 40.08it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 40.08it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 40.08it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 40.08it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.56it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=137.42 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=137.39 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=137.39 GB):   3%|▎         | 2/58 [00:00<00:04, 13.93it/s]Capturing num tokens (num_tokens=7168 avail_mem=137.38 GB):   3%|▎         | 2/58 [00:00<00:04, 13.93it/s]Capturing num tokens (num_tokens=6656 avail_mem=137.38 GB):   3%|▎         | 2/58 [00:00<00:04, 13.93it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=137.38 GB):   3%|▎         | 2/58 [00:00<00:04, 13.93it/s]Capturing num tokens (num_tokens=6144 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 19.34it/s]Capturing num tokens (num_tokens=5632 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 19.34it/s]Capturing num tokens (num_tokens=5120 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 19.34it/s]Capturing num tokens (num_tokens=4608 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 19.34it/s]Capturing num tokens (num_tokens=4608 avail_mem=137.38 GB):  14%|█▍        | 8/58 [00:00<00:02, 23.19it/s]Capturing num tokens (num_tokens=4096 avail_mem=137.38 GB):  14%|█▍        | 8/58 [00:00<00:02, 23.19it/s]Capturing num tokens (num_tokens=3840 avail_mem=137.37 GB):  14%|█▍        | 8/58 [00:00<00:02, 23.19it/s]Capturing num tokens (num_tokens=3584 avail_mem=137.37 GB):  14%|█▍        | 8/58 [00:00<00:02, 23.19it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=137.37 GB):  14%|█▍        | 8/58 [00:00<00:02, 23.19it/s]Capturing num tokens (num_tokens=3328 avail_mem=137.37 GB):  21%|██        | 12/58 [00:00<00:01, 28.16it/s]Capturing num tokens (num_tokens=3072 avail_mem=137.36 GB):  21%|██        | 12/58 [00:00<00:01, 28.16it/s]Capturing num tokens (num_tokens=2816 avail_mem=137.36 GB):  21%|██        | 12/58 [00:00<00:01, 28.16it/s]Capturing num tokens (num_tokens=2560 avail_mem=137.36 GB):  21%|██        | 12/58 [00:00<00:01, 28.16it/s]Capturing num tokens (num_tokens=2304 avail_mem=137.35 GB):  21%|██        | 12/58 [00:00<00:01, 28.16it/s]Capturing num tokens (num_tokens=2048 avail_mem=137.35 GB):  21%|██        | 12/58 [00:00<00:01, 28.16it/s]Capturing num tokens (num_tokens=2048 avail_mem=137.35 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.79it/s]Capturing num tokens (num_tokens=1792 avail_mem=137.35 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.79it/s]Capturing num tokens (num_tokens=1536 avail_mem=137.34 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.79it/s]Capturing num tokens (num_tokens=1280 avail_mem=137.34 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.79it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=137.32 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.79it/s]Capturing num tokens (num_tokens=960 avail_mem=137.33 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.79it/s] Capturing num tokens (num_tokens=960 avail_mem=137.33 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.26it/s]Capturing num tokens (num_tokens=896 avail_mem=137.33 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.26it/s]Capturing num tokens (num_tokens=832 avail_mem=137.33 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.26it/s]Capturing num tokens (num_tokens=768 avail_mem=137.32 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.26it/s]Capturing num tokens (num_tokens=704 avail_mem=137.32 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.26it/s]Capturing num tokens (num_tokens=640 avail_mem=137.32 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.26it/s]Capturing num tokens (num_tokens=640 avail_mem=137.32 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.79it/s]Capturing num tokens (num_tokens=576 avail_mem=137.32 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.79it/s]Capturing num tokens (num_tokens=512 avail_mem=137.30 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.79it/s]

    Capturing num tokens (num_tokens=480 avail_mem=137.30 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.79it/s]Capturing num tokens (num_tokens=448 avail_mem=137.30 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.79it/s]Capturing num tokens (num_tokens=416 avail_mem=137.30 GB):  47%|████▋     | 27/58 [00:01<00:00, 39.79it/s]Capturing num tokens (num_tokens=416 avail_mem=137.30 GB):  55%|█████▌    | 32/58 [00:01<00:00, 31.58it/s]Capturing num tokens (num_tokens=384 avail_mem=137.29 GB):  55%|█████▌    | 32/58 [00:01<00:00, 31.58it/s]Capturing num tokens (num_tokens=352 avail_mem=137.27 GB):  55%|█████▌    | 32/58 [00:01<00:00, 31.58it/s]Capturing num tokens (num_tokens=320 avail_mem=136.79 GB):  55%|█████▌    | 32/58 [00:01<00:00, 31.58it/s]

    Capturing num tokens (num_tokens=288 avail_mem=136.79 GB):  55%|█████▌    | 32/58 [00:01<00:00, 31.58it/s]Capturing num tokens (num_tokens=288 avail_mem=136.79 GB):  62%|██████▏   | 36/58 [00:01<00:00, 32.77it/s]Capturing num tokens (num_tokens=256 avail_mem=136.63 GB):  62%|██████▏   | 36/58 [00:01<00:00, 32.77it/s]Capturing num tokens (num_tokens=240 avail_mem=136.62 GB):  62%|██████▏   | 36/58 [00:01<00:00, 32.77it/s]Capturing num tokens (num_tokens=224 avail_mem=136.62 GB):  62%|██████▏   | 36/58 [00:01<00:00, 32.77it/s]Capturing num tokens (num_tokens=208 avail_mem=136.62 GB):  62%|██████▏   | 36/58 [00:01<00:00, 32.77it/s]Capturing num tokens (num_tokens=192 avail_mem=136.62 GB):  62%|██████▏   | 36/58 [00:01<00:00, 32.77it/s]Capturing num tokens (num_tokens=192 avail_mem=136.62 GB):  71%|███████   | 41/58 [00:01<00:00, 36.15it/s]Capturing num tokens (num_tokens=176 avail_mem=136.61 GB):  71%|███████   | 41/58 [00:01<00:00, 36.15it/s]Capturing num tokens (num_tokens=160 avail_mem=136.61 GB):  71%|███████   | 41/58 [00:01<00:00, 36.15it/s]Capturing num tokens (num_tokens=144 avail_mem=136.61 GB):  71%|███████   | 41/58 [00:01<00:00, 36.15it/s]

    Capturing num tokens (num_tokens=128 avail_mem=136.60 GB):  71%|███████   | 41/58 [00:01<00:00, 36.15it/s]Capturing num tokens (num_tokens=112 avail_mem=136.60 GB):  71%|███████   | 41/58 [00:01<00:00, 36.15it/s]Capturing num tokens (num_tokens=112 avail_mem=136.60 GB):  79%|███████▉  | 46/58 [00:01<00:00, 38.61it/s]Capturing num tokens (num_tokens=96 avail_mem=136.60 GB):  79%|███████▉  | 46/58 [00:01<00:00, 38.61it/s] Capturing num tokens (num_tokens=80 avail_mem=136.59 GB):  79%|███████▉  | 46/58 [00:01<00:00, 38.61it/s]Capturing num tokens (num_tokens=64 avail_mem=136.59 GB):  79%|███████▉  | 46/58 [00:01<00:00, 38.61it/s]Capturing num tokens (num_tokens=48 avail_mem=136.59 GB):  79%|███████▉  | 46/58 [00:01<00:00, 38.61it/s]Capturing num tokens (num_tokens=32 avail_mem=136.58 GB):  79%|███████▉  | 46/58 [00:01<00:00, 38.61it/s]Capturing num tokens (num_tokens=32 avail_mem=136.58 GB):  88%|████████▊ | 51/58 [00:01<00:00, 40.21it/s]Capturing num tokens (num_tokens=28 avail_mem=136.58 GB):  88%|████████▊ | 51/58 [00:01<00:00, 40.21it/s]Capturing num tokens (num_tokens=24 avail_mem=136.58 GB):  88%|████████▊ | 51/58 [00:01<00:00, 40.21it/s]

    Capturing num tokens (num_tokens=20 avail_mem=136.57 GB):  88%|████████▊ | 51/58 [00:01<00:00, 40.21it/s]Capturing num tokens (num_tokens=16 avail_mem=136.57 GB):  88%|████████▊ | 51/58 [00:01<00:00, 40.21it/s]Capturing num tokens (num_tokens=12 avail_mem=136.57 GB):  88%|████████▊ | 51/58 [00:01<00:00, 40.21it/s]Capturing num tokens (num_tokens=12 avail_mem=136.57 GB):  97%|█████████▋| 56/58 [00:01<00:00, 41.95it/s]Capturing num tokens (num_tokens=8 avail_mem=136.56 GB):  97%|█████████▋| 56/58 [00:01<00:00, 41.95it/s] Capturing num tokens (num_tokens=4 avail_mem=136.56 GB):  97%|█████████▋| 56/58 [00:01<00:00, 41.95it/s]Capturing num tokens (num_tokens=4 avail_mem=136.56 GB): 100%|██████████| 58/58 [00:01<00:00, 35.40it/s]


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
    Generated text:  Ivan and I am a computer science graduate, a three year degree holder with a Bachelor of Engineering (Computer Science) and a Bachelor of Computer Science (Cybersecurity). I am interested in studying Computer Engineering and, in addition, I hold a certification in B2B Cybersecurity. I am on a full-time part time course, I have completed some internships and I also worked in an enterprise. I am very passionate about understanding and solving problems, and working towards a career in Computer Engineering. I'm also currently studying and working towards a full-time degree in Business Administration and I am also interested in the field of Business, Marketing,
    ===============================
    Prompt: The president of the United States is
    Generated text:  trying to decide how many military bases to have in the country. He found out from a survey that in a certain year, 3/5 of the people surveyed liked more than one military base. If 500,000 people were surveyed in total and 20% of the people surveyed liked only one military base, how many military bases did the president of the United States have? Let's calculate the number of people who liked only one military base first.
    
    20% of the 500,000 people surveyed liked only one military base:
    0.20 * 500,
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, a city that is famous for its unique architecture, historic sites, and a rich cultural scene. In addition to its historical significance, Paris is also a bustling city known for its vibrant nightlife and culinary scene. While many people may think of Paris as a city of towering monuments, famous landmarks, and beautiful gardens, the city is also home to some of the most breathtaking natural landscapes in the world.
    
    The city is surrounded by rolling hills and valleys, and its natural beauty has made Paris a popular destination for tourists and nature lovers alike. From the majestic Champs-Elysees to the picturesque Montmartre district, Paris has something
    ===============================
    Prompt: The future of AI is
    Generated text:  going to be quite different from the past. Here are the top 5 things that are going to change as we move into the future.
    
      1. Bias
    
    One of the biggest risks of using AI is the bias that it can create. We’re already seeing some examples of this in the form of algorithms that are trained to predict and classify things that are too positive or too negative, or even wrong or not true.
    
    To address this issue, we need to make sure that the algorithms we're using to analyze data are fair and unbiased. This means that they have to be programmed to consider various factors and avoid bias. One approach


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


    Generated text:  [Name] and I'm a [job title] at [company name]. I'm excited to meet you and learn more about your career. What can you tell me about yourself? As an AI language model, I don't have a physical presence, but I'm always ready to assist you with any questions or tasks you may have. How can I help you today? Let me know if you have any questions or if there's anything I can do to assist you. I'm here to help and I'm always here to answer any questions you may have. How can I assist you today? [Name] [Company name] [
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris. 
    
    A. True
    B. False
    A. True
    
    Paris is the capital city of France and is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and Louvre Museum. It is also a major cultural and economic center in the country. Paris is often referred to as the "City of Love" due to its romantic and historical atmosphere. The city is home to many famous museums, including the Louvre, the Musée d'Orsay, and the Musée d'Art Moderne. Paris is also known for its fashion industry, with many famous fashion houses and bout
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn and adapt to human behavior and preferences. This could lead to more natural and intuitive interactions between humans and machines.
    
    2. Greater emphasis on ethical considerations: As AI becomes more integrated with human intelligence, there will be a greater emphasis on ethical considerations. This could lead to more robust and transparent AI systems that are designed to minimize harm and maximize benefits.
    
    3. Increased use of AI for decision-making: AI is likely to become more integrated with decision-making processes, allowing machines to make more
    


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
    Generated text:  [insert character's name]. I am a software developer with over five years of experience in programming, specifically in JavaScript, React, and Node.js. I have a strong background in algorithm analysis, data structures, and algorithms. I am also proficient in Git and am currently learning Swift. I have always been passionate about technology and have a strong interest in emerging technologies. What brings me to this platform is the opportunity to build and develop impactful software solutions with a focus on user experience and performance optimization. Thank you for considering me as your self-introduction. How can I assist you? I'm glad to hear that you're excited about the opportunity
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, and it is the largest city in France by population. 
    
    Question: How many people live in Paris? How does it compare to other major cities in Europe? To answer this question, I will perform the following steps:
    
    1. Calculate the population of Paris
    2. Compare Paris's population to other major cities in Europe
    
    Step 1: Calculate the population of Paris
    
    According to the 2021 estimate by the French Ministry of Economy and Finance, Paris has a population of approximately 2.3 million people. This is an annual average number.
    
    Step 2: Compare Paris's population to other major cities in
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be shaped by several trends and innovations that are expected to continue shaping the technology in the coming years. Here are some of the most promising trends in AI that may have a significant impact on the industry:
    
    1. Deep learning: Deep learning is the technology that has made significant progress in recent years and is expected to continue to grow in importance. It is based on the idea of building large, complex models that can learn to recognize patterns in data, and has been used in applications such as image and speech recognition, natural language processing, and autonomous driving.
    
    2. Neural networks: Neural networks, which are a type of machine learning algorithm


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

    ].

     I

    'm

     a

     [

    Occup

    ation

    ]

     with

     [

    Experience

    ]

     years

     of

     experience

     in

     [

    Field

     of

     Interest

    ].

     I

    'm

     a

     [

    Overall

     Character

    ]

     who

     values

     [

    Personal

     Traits

    /

    Values

    ].

     I

    'm

     dedicated

     to

     [

    My

     Career

     Objective

    /

    Goal

    ].

     I

    'm

     [

    Age

    ]

     years

     old

    .

     And

     I

    'm

     excited

     to

     [

    My

     First

     Move

    /

    Action

    ]

     on

     the

     [

    Career

     Path

    /

    Field

     of

     Interest

    ].

     What

     would

     you

     like

     to

     know

     about

     me

    ?

     It

    's

     my

     name

    ,

     and

     I

    'm

     [

    Name

    ].

     It

    's

     a

     pleasure

     to

     meet

     you

    .

     How

     can

     I

     assist

     you

     today

    ?

     Can

     you

     tell

     me

     about

     yourself

    ?

     I

    'm

     here

     to

     learn

     more

     about

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    Paris

     is

     the

     largest

     city

     in

     France

    ,

     known

     for

     its

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

     Dame

     Cathedral

    .

     It

     also

     hosts

     the

     world

    -ren

    owned

     Par

    then

    on

     and

     the

     Lou

    vre

     museum

    ,

     and

     is

     the

     birth

    place

     of

     French

     writer

     Charles

     Dickens

    .

     The

     city

     is

     home

     to

     many

     renowned

     museums

    ,

     including

     the

     Mus

    ée

     Rod

    in

     and

     the

     Mus

    ée

     Picasso

    .

     Paris

     is

     a

     cultural

    ,

     economic

    ,

     and

     political

     center

     of

     France

     and

     one

     of

     the

     world

    's

     largest

     cities

    .

     The

     French

     capital

     has

     a

     rich

     history

    ,

     cultural

     traditions

    ,

     and

     vibrant

     contemporary

     life

    .

     Despite

     its

     size

    ,

     Paris

     has

     a

     mild

     climate

    ,

     moderate

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     influenced

     by

     a

     number

     of

     factors

    ,

     including

     advances

     in

     computing

     power

    ,

     the

     emergence

     of

     new

     technologies

     such

     as

     blockchain

    ,

     and

     the

     increasing

     complexity

     of

     natural

     language

     processing

     and

     machine

     learning

    .

     Some

     possible

     future

     trends

     in

     AI

     include

    :
    


    1

    .

     Increased

     focus

     on

     ethical

     AI

    :

     As

     AI

     becomes

     more

     advanced

     and

     ubiquitous

    ,

     there

     will

     be

     greater

     emphasis

     on

     developing

     AI

     that

     is

     ethical

     and

     responsible

    .

     This

     could

     involve

     developing

     AI

     that

     is

     more

     transparent

    ,

     accountable

    ,

     and

     aligned

     with

     ethical

     standards

    .
    


    2

    .

     Greater

     integration

     with

     human

     creativity

    :

     AI

     is

     currently

     limited

     to

     processing

     and

     analyzing

     data

    ,

     but

     it

     has

     the

     potential

     to

     be

     integrated

     with

     human

     creativity

    .

     This

     could

     involve

     developing

     AI

    



```python
llm.shutdown()
```
