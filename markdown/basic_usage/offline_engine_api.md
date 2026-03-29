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


    2026-03-29 22:01:54.723 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 22:01:54] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 22:01:54.723 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 22:01:54] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 22:01:54.723 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 22:01:54] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 22:01:54.723 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 22:01:54] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 22:01:54.723 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 22:01:54] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.57it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.56it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:39,  2.81s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:39,  2.81s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:39,  2.81s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:39,  2.81s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:39,  2.81s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:23,  2.26it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:23,  2.26it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:23,  2.26it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:23,  2.26it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:23,  2.26it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:23,  2.26it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:03<00:23,  2.26it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:03<00:07,  5.95it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:03<00:07,  5.95it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:03<00:07,  5.95it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:03<00:07,  5.95it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:03<00:07,  5.95it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:03<00:07,  5.95it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:03<00:07,  5.95it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:03<00:07,  5.95it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:03<00:07,  5.95it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 12.09it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 12.09it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.09it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.09it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.09it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.09it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.09it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 12.09it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 18.04it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 18.04it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 18.04it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 18.04it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 18.04it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 18.04it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 18.04it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 23.33it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 23.33it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 23.33it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 23.33it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 23.33it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 23.33it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 23.33it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 28.55it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 28.55it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 28.55it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 28.55it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 28.55it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 28.55it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 28.55it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 34.15it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 34.15it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 34.15it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 34.15it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 34.15it/s]

    Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 34.15it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 34.15it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 37.73it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 37.73it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 37.73it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 37.73it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 37.73it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 37.73it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 37.73it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 37.73it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 37.73it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.33it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=137.42 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=137.39 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=137.39 GB):   3%|▎         | 2/58 [00:00<00:07,  7.46it/s]Capturing num tokens (num_tokens=7168 avail_mem=137.38 GB):   3%|▎         | 2/58 [00:00<00:07,  7.46it/s]Capturing num tokens (num_tokens=6656 avail_mem=137.38 GB):   3%|▎         | 2/58 [00:00<00:07,  7.46it/s]Capturing num tokens (num_tokens=6144 avail_mem=137.38 GB):   3%|▎         | 2/58 [00:00<00:07,  7.46it/s]Capturing num tokens (num_tokens=6144 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:03, 14.07it/s]Capturing num tokens (num_tokens=5632 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:03, 14.07it/s]Capturing num tokens (num_tokens=5120 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:03, 14.07it/s]Capturing num tokens (num_tokens=4608 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:03, 14.07it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:03, 14.07it/s]Capturing num tokens (num_tokens=4096 avail_mem=137.38 GB):  16%|█▌        | 9/58 [00:00<00:02, 20.60it/s]Capturing num tokens (num_tokens=3840 avail_mem=137.37 GB):  16%|█▌        | 9/58 [00:00<00:02, 20.60it/s]Capturing num tokens (num_tokens=3584 avail_mem=137.37 GB):  16%|█▌        | 9/58 [00:00<00:02, 20.60it/s]Capturing num tokens (num_tokens=3328 avail_mem=137.37 GB):  16%|█▌        | 9/58 [00:00<00:02, 20.60it/s]Capturing num tokens (num_tokens=3072 avail_mem=137.36 GB):  16%|█▌        | 9/58 [00:00<00:02, 20.60it/s]Capturing num tokens (num_tokens=3072 avail_mem=137.36 GB):  22%|██▏       | 13/58 [00:00<00:01, 26.22it/s]Capturing num tokens (num_tokens=2816 avail_mem=137.36 GB):  22%|██▏       | 13/58 [00:00<00:01, 26.22it/s]Capturing num tokens (num_tokens=2560 avail_mem=137.36 GB):  22%|██▏       | 13/58 [00:00<00:01, 26.22it/s]Capturing num tokens (num_tokens=2304 avail_mem=137.35 GB):  22%|██▏       | 13/58 [00:00<00:01, 26.22it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=137.35 GB):  22%|██▏       | 13/58 [00:00<00:01, 26.22it/s]Capturing num tokens (num_tokens=2048 avail_mem=137.35 GB):  29%|██▉       | 17/58 [00:00<00:01, 29.69it/s]Capturing num tokens (num_tokens=1792 avail_mem=137.35 GB):  29%|██▉       | 17/58 [00:00<00:01, 29.69it/s]Capturing num tokens (num_tokens=1536 avail_mem=137.34 GB):  29%|██▉       | 17/58 [00:00<00:01, 29.69it/s]Capturing num tokens (num_tokens=1280 avail_mem=137.34 GB):  29%|██▉       | 17/58 [00:00<00:01, 29.69it/s]Capturing num tokens (num_tokens=1024 avail_mem=137.32 GB):  29%|██▉       | 17/58 [00:00<00:01, 29.69it/s]Capturing num tokens (num_tokens=960 avail_mem=137.33 GB):  29%|██▉       | 17/58 [00:00<00:01, 29.69it/s] Capturing num tokens (num_tokens=960 avail_mem=137.33 GB):  38%|███▊      | 22/58 [00:00<00:01, 32.71it/s]Capturing num tokens (num_tokens=896 avail_mem=137.33 GB):  38%|███▊      | 22/58 [00:00<00:01, 32.71it/s]

    Capturing num tokens (num_tokens=832 avail_mem=137.33 GB):  38%|███▊      | 22/58 [00:00<00:01, 32.71it/s]Capturing num tokens (num_tokens=768 avail_mem=137.32 GB):  38%|███▊      | 22/58 [00:01<00:01, 32.71it/s]Capturing num tokens (num_tokens=704 avail_mem=137.32 GB):  38%|███▊      | 22/58 [00:01<00:01, 32.71it/s]Capturing num tokens (num_tokens=704 avail_mem=137.32 GB):  45%|████▍     | 26/58 [00:01<00:01, 27.31it/s]Capturing num tokens (num_tokens=640 avail_mem=137.32 GB):  45%|████▍     | 26/58 [00:01<00:01, 27.31it/s]Capturing num tokens (num_tokens=576 avail_mem=137.32 GB):  45%|████▍     | 26/58 [00:01<00:01, 27.31it/s]Capturing num tokens (num_tokens=512 avail_mem=137.30 GB):  45%|████▍     | 26/58 [00:01<00:01, 27.31it/s]Capturing num tokens (num_tokens=480 avail_mem=137.32 GB):  45%|████▍     | 26/58 [00:01<00:01, 27.31it/s]Capturing num tokens (num_tokens=448 avail_mem=137.32 GB):  45%|████▍     | 26/58 [00:01<00:01, 27.31it/s]Capturing num tokens (num_tokens=448 avail_mem=137.32 GB):  53%|█████▎    | 31/58 [00:01<00:00, 31.93it/s]Capturing num tokens (num_tokens=416 avail_mem=137.32 GB):  53%|█████▎    | 31/58 [00:01<00:00, 31.93it/s]Capturing num tokens (num_tokens=384 avail_mem=137.31 GB):  53%|█████▎    | 31/58 [00:01<00:00, 31.93it/s]

    Capturing num tokens (num_tokens=352 avail_mem=137.31 GB):  53%|█████▎    | 31/58 [00:01<00:00, 31.93it/s]Capturing num tokens (num_tokens=320 avail_mem=137.30 GB):  53%|█████▎    | 31/58 [00:01<00:00, 31.93it/s]Capturing num tokens (num_tokens=288 avail_mem=137.30 GB):  53%|█████▎    | 31/58 [00:01<00:00, 31.93it/s]Capturing num tokens (num_tokens=288 avail_mem=137.30 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.48it/s]Capturing num tokens (num_tokens=256 avail_mem=137.30 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.48it/s]Capturing num tokens (num_tokens=240 avail_mem=137.30 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.48it/s]Capturing num tokens (num_tokens=224 avail_mem=137.29 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.48it/s]Capturing num tokens (num_tokens=208 avail_mem=137.29 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.48it/s]Capturing num tokens (num_tokens=192 avail_mem=137.29 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.48it/s]Capturing num tokens (num_tokens=192 avail_mem=137.29 GB):  71%|███████   | 41/58 [00:01<00:00, 38.34it/s]Capturing num tokens (num_tokens=176 avail_mem=137.29 GB):  71%|███████   | 41/58 [00:01<00:00, 38.34it/s]Capturing num tokens (num_tokens=160 avail_mem=137.28 GB):  71%|███████   | 41/58 [00:01<00:00, 38.34it/s]

    Capturing num tokens (num_tokens=144 avail_mem=137.28 GB):  71%|███████   | 41/58 [00:01<00:00, 38.34it/s]Capturing num tokens (num_tokens=128 avail_mem=137.28 GB):  71%|███████   | 41/58 [00:01<00:00, 38.34it/s]Capturing num tokens (num_tokens=112 avail_mem=137.27 GB):  71%|███████   | 41/58 [00:01<00:00, 38.34it/s]Capturing num tokens (num_tokens=112 avail_mem=137.27 GB):  79%|███████▉  | 46/58 [00:01<00:00, 40.29it/s]Capturing num tokens (num_tokens=96 avail_mem=137.27 GB):  79%|███████▉  | 46/58 [00:01<00:00, 40.29it/s] Capturing num tokens (num_tokens=80 avail_mem=137.27 GB):  79%|███████▉  | 46/58 [00:01<00:00, 40.29it/s]Capturing num tokens (num_tokens=64 avail_mem=137.26 GB):  79%|███████▉  | 46/58 [00:01<00:00, 40.29it/s]Capturing num tokens (num_tokens=48 avail_mem=137.26 GB):  79%|███████▉  | 46/58 [00:01<00:00, 40.29it/s]Capturing num tokens (num_tokens=32 avail_mem=137.25 GB):  79%|███████▉  | 46/58 [00:01<00:00, 40.29it/s]Capturing num tokens (num_tokens=32 avail_mem=137.25 GB):  88%|████████▊ | 51/58 [00:01<00:00, 41.48it/s]Capturing num tokens (num_tokens=28 avail_mem=137.25 GB):  88%|████████▊ | 51/58 [00:01<00:00, 41.48it/s]

    Capturing num tokens (num_tokens=24 avail_mem=137.25 GB):  88%|████████▊ | 51/58 [00:01<00:00, 41.48it/s]Capturing num tokens (num_tokens=20 avail_mem=137.24 GB):  88%|████████▊ | 51/58 [00:01<00:00, 41.48it/s]Capturing num tokens (num_tokens=16 avail_mem=137.24 GB):  88%|████████▊ | 51/58 [00:01<00:00, 41.48it/s]Capturing num tokens (num_tokens=12 avail_mem=137.24 GB):  88%|████████▊ | 51/58 [00:01<00:00, 41.48it/s]Capturing num tokens (num_tokens=12 avail_mem=137.24 GB):  97%|█████████▋| 56/58 [00:01<00:00, 42.95it/s]Capturing num tokens (num_tokens=8 avail_mem=137.24 GB):  97%|█████████▋| 56/58 [00:01<00:00, 42.95it/s] Capturing num tokens (num_tokens=4 avail_mem=137.23 GB):  97%|█████████▋| 56/58 [00:01<00:00, 42.95it/s]Capturing num tokens (num_tokens=4 avail_mem=137.23 GB): 100%|██████████| 58/58 [00:01<00:00, 32.99it/s]


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
    Generated text:  Sona and I am from the planet Vena. I live on the planet for 18 years and I have a job as a scientist. I like to explore the planet and meet new people, and I love to cook. I also love to travel the planet, learn about the flora and fauna, and try new foods. I have a brother who is a lawyer and a sister who is a doctor. 
    
    What are some interesting facts about Venus?
    What's your favorite food? 
    
    If you could have a superpower, what would it be and why?
    
    What's your favorite hobby? 
    
    What's your favorite place to stay
    ===============================
    Prompt: The president of the United States is
    Generated text:  a position that is not held by a single person. There are more than one person who hold this position. The president of the United States is the head of government of the United States. His/her job is to lead the government of the United States. He/she also is responsible for making sure that the government is being run efficiently. He/she also has the authority to make decisions for the United States. The position of president is not hereditary and is not the same as the position of president of any other country. The president of the United States is not elected, but appointed by the President of the United States.
    What is the main
    ===============================
    Prompt: The capital of France is
    Generated text: :
    A. Paris
    B. London
    C. Madrid
    D. Berlin
    The capital of France is:
    
    A. Paris
    
    Paris is the capital city of France, while London and Madrid are the two other French cities. Berlin is also the capital city, but not of France. 
    
    So the correct answer is A: Paris. 
    
    To expand on this, Paris is renowned for its iconic Eiffel Tower, historical landmarks like the Louvre Museum and Notre-Dame Cathedral, and its culinary scene, particularly in the French Riviera. 
    
    Other French capitals include:
    - **Lyon** (in the Languedoc
    ===============================
    Prompt: The future of AI is
    Generated text:  still uncertain, but here are some things to consider:
    
      1. AI is still evolving. We are still learning about what an AI system can do and how it will be used.
      2. AI systems can be expensive. As more AI systems are developed, they will become more expensive.
      3. AI systems can be vulnerable to hacking. Hackers can use AI systems to steal sensitive data or cause harm to users.
      4. AI systems can be misused. As AI systems are developed, they may be misused for malicious purposes.
      5. AI systems can have privacy concerns. AI systems


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm a [job title] with [number of years] years of experience in [industry]. I'm passionate about [reason why I'm passionate about my job]. I'm always looking for ways to [what I'm looking for in my job]. I'm [what I'm looking for in my job]. I'm [what I'm looking for in my job]. I'm [what I'm looking for in my job]. I'm [what I'm looking for in my job]. I'm [what I'm looking for in my job]. I
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French Quarter, a historic neighborhood. Paris is a cultural and economic center with a rich history and diverse population. It is the largest city in France by population and is a major tourist destination. The city is known for its cuisine, fashion, and art, and is home to many world-renowned museums and galleries. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly. It is a city of
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we live, work, and interact with technology. Here are some of the most likely future trends in AI:
    
    1. Increased automation and robotics: As AI technology continues to improve, we can expect to see more automation and robotics in various industries. This could lead to increased efficiency, lower costs, and better quality of life for many people.
    
    2. Enhanced personalization: AI will allow for more personalized experiences, with machines learning to understand and respond to individual users' preferences and needs. This could lead to more efficient use of resources and better customer satisfaction.
    
    3
    


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
    Generated text:  [Your Name]. I am an experienced software engineer with a diverse range of skills and interests. I enjoy working on complex projects and solving challenging problems, and I am passionate about learning and staying up-to-date with the latest technologies and trends in the field. I am a clear and concise communicator, and I thrive in a fast-paced and dynamic environment. I am committed to building a team of skilled professionals who are committed to delivering high-quality work and delivering value for our clients. I am excited to work with you and I look forward to building a career with you. [Your Name] *Thanks for asking, I am here to help and
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. 
    
    A. Yes
    B. No
    
    A. Yes
    You are an AI assistant that helps people find information. No, I am not the French capital city. Paris is the capital of France, but I am an AI developed by Alibaba Cloud, so I do not have the ability to physically travel to any cities. If you have questions about Paris, I can provide information about it. Is there anything else I can help you with?
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be shaped by a variety of technological, economic, and social factors. Here are some possible future trends in artificial intelligence:
    
    1. Increased reliance on AI for decision-making: AI will become more integrated into decision-making processes, such as in healthcare, finance, and manufacturing.
    
    2. Increased AI automation: AI will continue to become more effective at automating tasks, such as data entry, transcription, and routine tasks.
    
    3. AI integration with human workers: AI will continue to be integrated into human workers' workflows, providing them with tools to automate manual tasks.
    
    4. AI for health: AI will be used for diagnosis and treatment


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

    ],

     and

     I

    'm

     a

     [

    role

    ]

     with

     [

    number

    ]

     years

     of

     experience

     in

     the

     field

    .

     My

     background

     is

     [

    describe

     your

     field

     of

     expertise

     or

     career

    ].

     I

    'm

     passionate

     about

     [

    mention

     a

     specific

     passion

     or

     interest

    ].


    I

    'm

     always

     looking

     for

     opportunities

     to

     [

    mention

     something

     specific

     you

     enjoy

     or

     learn

     new

     things

     about

    ].

     I

    'm

     [

    describe

     your

     personality

     traits

    ].

     I

     thrive

     [

    mention

     a

     specific

     trait

     or

     quality

     that

     you

     excel

     in

    ].

     I

     love

     [

    mention

     a

     hobby

     or

     activity

     you

     enjoy

     doing

    ].


    Thank

     you

     for

     considering

     me

     for

     a

     role

    .

     Let

     me

     know

     if

     you

     have

     any

     questions

     or

     need

     more

     information

     about

     my

     experience

    .

     [

    Name

    ]

     is

     looking

    
    
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

     the

     seat

     of

     the

     French

     government

    .

     It

     is

     also

     the

     country

    ’s

     economic

     and

     cultural

     center

    ,

     home

     to

     the

     French

     parliament

    ,

     many

     of

     the

     nation

    ’s

     museums

    ,

     and

     many

     of

     the

     country

    ’s

     most

     famous

     landmarks

    .

     Paris

     is

     famous

     for

     its

     grand

     cath

    ed

    r

    als

    ,

     world

    -ren

    owned

     museums

    ,

     and

     its

     distinctive

     fashion

     and

     wine

     industries

    .

     Its

     status

     as

     a

     major

     international

     met

    ropolis

     has

     made

     it

     a

     symbol

     of

     France

     and

     the

     world

    .

     

     The

     city

     is

     home

     to

     over

     a

     million

     people

     and

     is

     a

     cultural

     and

     economic

     hub

     in

     the

     world

    .

     The

     French

     capital

     is

     known

     for

     its

     grand

     cath

    ed

    r

    als

    ,

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     very

     exciting

     and

     full

     of

     potential

     possibilities

    .

     Here

     are

     some

     possible

     trends

     that

     could

     unfold

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

     As

     AI

     becomes

     more

     prevalent

     in

     everyday

     life

    ,

     we

     may

     see

     even

     more

     integration

     with

     other

     technologies

     and

     industries

    .

     For

     example

    ,

     AI

     could

     be

     used

     to

     improve

     logistics

    ,

     healthcare

    ,

     transportation

    ,

     and

     many

     other

     sectors

    .
    


    2

    .

     Personal

    ized

     AI

    :

     As

     AI

     becomes

     more

     sophisticated

    ,

     we

     may

     see

     a

     shift

     towards

     personalized

     AI

     that

     uses

     data

     from

     individual

     users

     to

     improve

     the

     user

     experience

    .
    


    3

    .

     AI

     Ethics

    :

     As

     more

     AI

     is

     developed

    ,

     there

     will

     be

     greater

     concern

     about

     the

     ethical

     implications

     of

     its

     use

    .

     This

     could

     lead

     to

     new

    



```python
llm.shutdown()
```
