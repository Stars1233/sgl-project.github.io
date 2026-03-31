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


    2026-03-31 03:20:17.730 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 03:20:17] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 03:20:17.731 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 03:20:17] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 03:20:17.731 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 03:20:17] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 03:20:17.731 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 03:20:17] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 03:20:17.731 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 03:20:17] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.51it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.50it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:03<00:08,  5.69it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:03<00:08,  5.69it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 12.05it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]

    Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 18.17it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 18.17it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 18.17it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 18.17it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 18.17it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 18.17it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 18.17it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 18.17it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 24.89it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 24.89it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 24.89it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 24.89it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 24.89it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 24.89it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 24.89it/s]

    Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 29.93it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 29.93it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 29.93it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 29.93it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 29.93it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 29.93it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 29.93it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 35.36it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 35.36it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 35.36it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 35.36it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 35.36it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 35.36it/s]

    Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 35.36it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 38.66it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 38.66it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 38.66it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 38.66it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 38.66it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 38.66it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 38.66it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 38.66it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 38.66it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.76it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.76 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.73 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 18.56it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 18.56it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 18.56it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=120.40 GB):   3%|▎         | 2/58 [00:00<00:03, 18.56it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.40 GB):   9%|▊         | 5/58 [00:00<00:02, 21.02it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 21.02it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 21.02it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.02it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 20.43it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 20.43it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 20.43it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.28 GB):  14%|█▍        | 8/58 [00:00<00:02, 20.43it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.28 GB):  19%|█▉        | 11/58 [00:00<00:02, 16.20it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  19%|█▉        | 11/58 [00:00<00:02, 16.20it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.27 GB):  19%|█▉        | 11/58 [00:00<00:02, 16.20it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.27 GB):  19%|█▉        | 11/58 [00:00<00:02, 16.20it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.27 GB):  19%|█▉        | 11/58 [00:00<00:02, 16.20it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.27 GB):  19%|█▉        | 11/58 [00:00<00:02, 16.20it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=120.27 GB):  28%|██▊       | 16/58 [00:00<00:01, 23.44it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.26 GB):  28%|██▊       | 16/58 [00:00<00:01, 23.44it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  28%|██▊       | 16/58 [00:00<00:01, 23.44it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  28%|██▊       | 16/58 [00:00<00:01, 23.44it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.25 GB):  28%|██▊       | 16/58 [00:00<00:01, 23.44it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.23 GB):  28%|██▊       | 16/58 [00:00<00:01, 23.44it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.23 GB):  36%|███▌      | 21/58 [00:00<00:01, 29.21it/s]Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  36%|███▌      | 21/58 [00:00<00:01, 29.21it/s] Capturing num tokens (num_tokens=896 avail_mem=120.24 GB):  36%|███▌      | 21/58 [00:00<00:01, 29.21it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  36%|███▌      | 21/58 [00:00<00:01, 29.21it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  36%|███▌      | 21/58 [00:00<00:01, 29.21it/s]Capturing num tokens (num_tokens=704 avail_mem=120.23 GB):  36%|███▌      | 21/58 [00:00<00:01, 29.21it/s]

    Capturing num tokens (num_tokens=704 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:00<00:00, 33.72it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:00<00:00, 33.72it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:00<00:00, 33.72it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  45%|████▍     | 26/58 [00:01<00:00, 33.72it/s]Capturing num tokens (num_tokens=480 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:01<00:00, 33.72it/s]Capturing num tokens (num_tokens=448 avail_mem=120.27 GB):  45%|████▍     | 26/58 [00:01<00:00, 33.72it/s]Capturing num tokens (num_tokens=448 avail_mem=120.27 GB):  53%|█████▎    | 31/58 [00:01<00:00, 37.05it/s]Capturing num tokens (num_tokens=416 avail_mem=120.27 GB):  53%|█████▎    | 31/58 [00:01<00:00, 37.05it/s]Capturing num tokens (num_tokens=384 avail_mem=120.27 GB):  53%|█████▎    | 31/58 [00:01<00:00, 37.05it/s]Capturing num tokens (num_tokens=352 avail_mem=119.04 GB):  53%|█████▎    | 31/58 [00:01<00:00, 37.05it/s]Capturing num tokens (num_tokens=320 avail_mem=118.94 GB):  53%|█████▎    | 31/58 [00:01<00:00, 37.05it/s]

    Capturing num tokens (num_tokens=288 avail_mem=118.94 GB):  53%|█████▎    | 31/58 [00:01<00:00, 37.05it/s]Capturing num tokens (num_tokens=288 avail_mem=118.94 GB):  62%|██████▏   | 36/58 [00:01<00:00, 39.12it/s]Capturing num tokens (num_tokens=256 avail_mem=118.93 GB):  62%|██████▏   | 36/58 [00:01<00:00, 39.12it/s]Capturing num tokens (num_tokens=240 avail_mem=118.93 GB):  62%|██████▏   | 36/58 [00:01<00:00, 39.12it/s]Capturing num tokens (num_tokens=224 avail_mem=118.93 GB):  62%|██████▏   | 36/58 [00:01<00:00, 39.12it/s]Capturing num tokens (num_tokens=208 avail_mem=118.92 GB):  62%|██████▏   | 36/58 [00:01<00:00, 39.12it/s]Capturing num tokens (num_tokens=192 avail_mem=118.92 GB):  62%|██████▏   | 36/58 [00:01<00:00, 39.12it/s]Capturing num tokens (num_tokens=192 avail_mem=118.92 GB):  71%|███████   | 41/58 [00:01<00:00, 40.99it/s]Capturing num tokens (num_tokens=176 avail_mem=118.92 GB):  71%|███████   | 41/58 [00:01<00:00, 40.99it/s]Capturing num tokens (num_tokens=160 avail_mem=118.92 GB):  71%|███████   | 41/58 [00:01<00:00, 40.99it/s]Capturing num tokens (num_tokens=144 avail_mem=118.91 GB):  71%|███████   | 41/58 [00:01<00:00, 40.99it/s]Capturing num tokens (num_tokens=128 avail_mem=118.91 GB):  71%|███████   | 41/58 [00:01<00:00, 40.99it/s]

    Capturing num tokens (num_tokens=112 avail_mem=118.91 GB):  71%|███████   | 41/58 [00:01<00:00, 40.99it/s]Capturing num tokens (num_tokens=112 avail_mem=118.91 GB):  79%|███████▉  | 46/58 [00:01<00:00, 42.18it/s]Capturing num tokens (num_tokens=96 avail_mem=118.90 GB):  79%|███████▉  | 46/58 [00:01<00:00, 42.18it/s] Capturing num tokens (num_tokens=80 avail_mem=118.90 GB):  79%|███████▉  | 46/58 [00:01<00:00, 42.18it/s]Capturing num tokens (num_tokens=64 avail_mem=118.89 GB):  79%|███████▉  | 46/58 [00:01<00:00, 42.18it/s]Capturing num tokens (num_tokens=48 avail_mem=118.89 GB):  79%|███████▉  | 46/58 [00:01<00:00, 42.18it/s]Capturing num tokens (num_tokens=32 avail_mem=118.88 GB):  79%|███████▉  | 46/58 [00:01<00:00, 42.18it/s]Capturing num tokens (num_tokens=32 avail_mem=118.88 GB):  88%|████████▊ | 51/58 [00:01<00:00, 42.57it/s]Capturing num tokens (num_tokens=28 avail_mem=118.88 GB):  88%|████████▊ | 51/58 [00:01<00:00, 42.57it/s]Capturing num tokens (num_tokens=24 avail_mem=118.88 GB):  88%|████████▊ | 51/58 [00:01<00:00, 42.57it/s]Capturing num tokens (num_tokens=20 avail_mem=118.87 GB):  88%|████████▊ | 51/58 [00:01<00:00, 42.57it/s]

    Capturing num tokens (num_tokens=16 avail_mem=118.87 GB):  88%|████████▊ | 51/58 [00:01<00:00, 42.57it/s]Capturing num tokens (num_tokens=12 avail_mem=118.87 GB):  88%|████████▊ | 51/58 [00:01<00:00, 42.57it/s]Capturing num tokens (num_tokens=12 avail_mem=118.87 GB):  97%|█████████▋| 56/58 [00:01<00:00, 43.83it/s]Capturing num tokens (num_tokens=8 avail_mem=118.87 GB):  97%|█████████▋| 56/58 [00:01<00:00, 43.83it/s] Capturing num tokens (num_tokens=4 avail_mem=118.86 GB):  97%|█████████▋| 56/58 [00:01<00:00, 43.83it/s]Capturing num tokens (num_tokens=4 avail_mem=118.86 GB): 100%|██████████| 58/58 [00:01<00:00, 34.55it/s]


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
    Generated text:  Mary, and I am a high school senior in middle school. I have some problems with my math class. I don’t understand how to solve these problems. Could you please give me some advice on how to improve my math skills? 1. How do I learn better? 2. How can I improve my math skills? 3. Is there any online resource that can help me improve my math skills?
    Mary's problem with her math class has been causing her anxiety and frustration. Here are some suggestions to help Mary improve her math skills:
    
    1. Set a clear goal: Mary should set a specific goal in her mind.
    ===============================
    Prompt: The president of the United States is
    Generated text:  trying to decide what to eat for breakfast. He has a choice of three different options for the first candidate, two options for the second candidate, and three different options for the third candidate. However, he also has to consider the likelihood of the candidate winning the election. The likelihood of the first candidate winning is 30%, the second candidate's likelihood is 40%, and the third candidate's likelihood is 60%. 
    
    What is the probability that the president will choose the candidate with the lowest likelihood of winning? To determine the probability that the president will choose the candidate with the lowest likelihood of winning, we need to consider
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. 100 years ago, the Paris Metro was the world’s first underground system. Today, it is still the most popular metro network in the world. The first trains departed from the station at 4:00 in the morning and the last train departed at 11:00 at night. It took only 10 minutes to reach the last train, but it took much longer to reach the first train.
    
    The line has been expanding continuously and it is now about to arrive at the last train. The metro has 5 trains to reach it. If the trains have to be fully loaded and travel the
    ===============================
    Prompt: The future of AI is
    Generated text:  in the hands of the individuals responsible for it. You can’t control the technology, but you can control the decisions that it makes.
    How To Write an Effective AI Project Proposal
    With the growth of artificial intelligence, it is important to create a project proposal that will make the project successful. The proposal should clearly outline the goals, objectives, methodology, and timeline of the project. It should also provide a detailed analysis of the proposed technology, its potential impact, and the expected outcomes.
    Additionally, it should be written in a clear and concise manner, with an engaging and visually appealing design. It should be accessible to a broad audience, regardless


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm a [job title] with [number of years] years of experience in [industry]. I'm passionate about [job title] and I'm always looking for ways to [job title] my skills and knowledge. I'm a [job title] with [number of years] years of experience in [industry]. I'm a [job title] with [number of years] years of experience in [industry]. I'm a [job title] with [number of years] years of experience in [industry]. I'm a [job title]
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a cultural and economic center with a rich history dating back to the Roman Empire and the Renaissance. It is also a popular tourist destination, known for its romantic architecture, vibrant nightlife, and annual festivals. Paris is a city of contrasts, with its modern and historic aspects blending together to create a unique and fascinating place. The city is home to many notable French artists, writers, and musicians, and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends:
    
    1. Increased automation and artificial intelligence: As AI becomes more advanced, it is likely to become more integrated into our daily lives. This could lead to increased automation of tasks, such as manufacturing, transportation, and customer service, which could result in job losses for some workers.
    
    2. Enhanced privacy and security: As AI becomes more sophisticated, there is a risk that it could be used for malicious purposes. This could lead to increased privacy and security concerns, as well as potential misuse
    


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
    Generated text:  [insert name] and I am a [insert profession] with [insert current occupation] in [insert location]. I'm here to talk about [insert topic of conversation], but not to get into too much of [insert sensitive topic]. Is there anything on your mind I can talk about? I am here to listen, not to engage in a conversation about sensitive topics or issues that could be harmful. You can rest assured that I will not participate in any discussion that may be potentially harmful or disrespectful to others. Is there anything else you would like to discuss? [insert name] [insert profession] [insert current occupation] [insert
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. 
    
    The statement should be brief, highlighting the location of Paris in France and its significance in the country's history and culture. Include any additional information about the city's landmarks, population, or cuisine. However, avoid providing any personal or biased opinions. The concise statement should accurately represent the factual information provided.
    Paris, the vibrant capital city of France, is known for its stunning architecture, rich history, and delicious cuisine. The city is located in the north of the country, in the region of Burgundy. It has a population of over 2 million people and is a major hub for both trade and tourism. Paris is home
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to continue to evolve and expand as technology advances and new applications emerge. Here are some potential trends that could shape the future of AI:
    
    1. Increased integration with other fields: AI is already being integrated with other fields such as healthcare, finance, and marketing. As these fields continue to grow, AI could become more integrated with them, driving new applications and possibilities.
    
    2. Greater transparency and accountability: As AI becomes more sophisticated, there is a growing concern about its potential to be used for nefarious purposes. As a result, there is a growing emphasis on making AI systems more transparent and accountable, with greater regulation and oversight.
    
    3


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

     dedicated

     [

    industry

    ]

     professional

     with

     a

     passion

     for

     [

    key

     skill

    ].

     I

     enjoy

     [

    reason

     for

     being

     in

     the

     field

    ]

     and

     am

     constantly

     learning

     and

     growing

    .

     What

     can

     you

     tell

     me

     about

     yourself

    ?

     [

    Provide

     some

     personal

     information

     about

     yourself

    ,

     such

     as

     your

     age

    ,

     education

    ,

     work

     experience

    ,

     interests

    ,

     or

     any

     other

     relevant

     details

    .

    ]


    Your

     self

    -int

    roduction

     should

     be

     brief

    ,

     neutral

    ,

     and

     to

     the

     point

    ,

     but

     should

     provide

     a

     clear

     picture

     of

     who

     you

     are

    .

     The

     introduction

     should

     include

     your

     name

    ,

     industry

    ,

     key

     skills

    ,

     and

     any

     relevant

     experiences

     or

     achievements

     that

     you

     hope

     to

     share

     with

     others

    .

     The

     tone

     of

     the

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     It

     is

     the

     largest

     city

     and

     the

     economic

     and

     cultural

     center

     of

     the

     country

    .

     It

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

     scenic

     views

    ,

     and

     unique

     cuisine

    .

     Paris

     is

     also

     known

     for

     its

     romantic

     and

     historical

     attractions

     such

     as

     the

     E

    iff

    el

     Tower

    ,

     Notre

    -D

    ame

     Cathedral

    ,

     and

     the

     Lou

    vre

     Museum

    .

     The

     city

     also

     has

     a

     diverse

     population

     and

     a

     rich

     history

     dating

     back

     over

     

    3

    0

    0

    0

     years

    .

     Paris

     is

     home

     to

     many

     museums

    ,

     theaters

    ,

     and

     other

     cultural

     institutions

     that

     attract

     millions

     of

     visitors

     each

     year

    .

     Overall

    ,

     Paris

     is

     a

     major

     cultural

     and

     economic

     center

     of

     France

     and

     a

     world

    -ren

    owned

     capital

     city

    .

     
    


    It

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     expected

     to

     be

     a

     blend

     of

     artificial

     intelligence

     and

     advanced

     machine

     learning

    .

     In

     the

     near

     future

    ,

     we

     are

     likely

     to

     see

     more

     advanced

     and

     powerful

     AI

     systems

     being

     developed

    ,

     with

     applications

     ranging

     from

     healthcare

     and

     education

     to

     transportation

     and

     security

    .

     One

     of

     the

     key

     trends

     in

     AI

     is

     the

     development

     of

     more

     sophisticated

     algorithms

     that

     can

     analyze

     and

     understand

     complex

     data

    ,

     leading

     to

     more

     accurate

     predictions

     and

     insights

    .
    


    Another

     trend

     is

     the

     increasing

     use

     of

     AI

     in

     areas

     such

     as

     voice

     recognition

    ,

     chat

    bots

    ,

     and

     virtual

     assistants

    ,

     where

     AI

     is

     being

     integrated

     into

     everyday

     life

     to

     make

     our

     lives

     easier

     and

     more

     convenient

    .

     As

     more

     people

     become

     familiar

     with

     AI

    ,

     it

     is

     likely

     that

     we

     will

     see

    



```python
llm.shutdown()
```
