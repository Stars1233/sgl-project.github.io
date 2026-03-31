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


    2026-03-31 21:12:31.565 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:12:31] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:12:31.565 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:12:31] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:12:31.565 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:12:31] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:12:31.565 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:12:31] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:12:31.565 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:12:31] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.47it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.47it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:25,  2.55s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:25,  2.55s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:25,  2.55s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:25,  2.55s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.00it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.00it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.00it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.00it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.00it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.00it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.00it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.00it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.00it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:03, 12.66it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:03, 12.66it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:03, 12.66it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:03, 12.66it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:03, 12.66it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:03, 12.66it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:03, 12.66it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:02<00:03, 12.66it/s]Compiling num tokens (num_tokens=704):  31%|███       | 18/58 [00:02<00:03, 12.66it/s]

    Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:02<00:01, 20.21it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:02<00:01, 20.21it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:02<00:01, 20.21it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 20.21it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 20.21it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 20.21it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 20.21it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 20.21it/s]Compiling num tokens (num_tokens=352):  45%|████▍     | 26/58 [00:03<00:01, 20.21it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:03<00:00, 28.20it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:03<00:00, 28.20it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:03<00:00, 28.20it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:03<00:00, 28.20it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:03<00:00, 28.20it/s]Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:03<00:00, 28.20it/s]Compiling num tokens (num_tokens=208):  59%|█████▊    | 34/58 [00:03<00:00, 28.20it/s]Compiling num tokens (num_tokens=192):  59%|█████▊    | 34/58 [00:03<00:00, 28.20it/s]

    Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:03<00:00, 34.84it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:03<00:00, 34.84it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:03<00:00, 34.84it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:03<00:00, 34.84it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:03<00:00, 34.84it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:03<00:00, 34.84it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:03<00:00, 34.84it/s] Compiling num tokens (num_tokens=80):  71%|███████   | 41/58 [00:03<00:00, 34.84it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:03<00:00, 41.17it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:03<00:00, 41.17it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:03<00:00, 41.17it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:03<00:00, 41.17it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:03<00:00, 41.17it/s]Compiling num tokens (num_tokens=24):  83%|████████▎ | 48/58 [00:03<00:00, 41.17it/s]Compiling num tokens (num_tokens=20):  83%|████████▎ | 48/58 [00:03<00:00, 41.17it/s]Compiling num tokens (num_tokens=16):  83%|████████▎ | 48/58 [00:03<00:00, 41.17it/s]Compiling num tokens (num_tokens=12):  83%|████████▎ | 48/58 [00:03<00:00, 41.17it/s]Compiling num tokens (num_tokens=8):  83%|████████▎ | 48/58 [00:03<00:00, 41.17it/s] 

    Compiling num tokens (num_tokens=4):  83%|████████▎ | 48/58 [00:03<00:00, 41.17it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 53.29it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.08it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=72.62 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=72.62 GB):   2%|▏         | 1/58 [00:00<00:07,  7.83it/s]Capturing num tokens (num_tokens=7680 avail_mem=72.59 GB):   2%|▏         | 1/58 [00:00<00:07,  7.83it/s]Capturing num tokens (num_tokens=7168 avail_mem=72.58 GB):   2%|▏         | 1/58 [00:00<00:07,  7.83it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=72.58 GB):   2%|▏         | 1/58 [00:00<00:07,  7.83it/s]Capturing num tokens (num_tokens=6656 avail_mem=72.58 GB):   7%|▋         | 4/58 [00:00<00:03, 16.86it/s]Capturing num tokens (num_tokens=6144 avail_mem=72.58 GB):   7%|▋         | 4/58 [00:00<00:03, 16.86it/s]Capturing num tokens (num_tokens=5632 avail_mem=72.58 GB):   7%|▋         | 4/58 [00:00<00:03, 16.86it/s]Capturing num tokens (num_tokens=5120 avail_mem=72.58 GB):   7%|▋         | 4/58 [00:00<00:03, 16.86it/s]Capturing num tokens (num_tokens=5120 avail_mem=72.58 GB):  12%|█▏        | 7/58 [00:00<00:02, 21.94it/s]Capturing num tokens (num_tokens=4608 avail_mem=72.58 GB):  12%|█▏        | 7/58 [00:00<00:02, 21.94it/s]Capturing num tokens (num_tokens=4096 avail_mem=72.58 GB):  12%|█▏        | 7/58 [00:00<00:02, 21.94it/s]Capturing num tokens (num_tokens=3840 avail_mem=72.57 GB):  12%|█▏        | 7/58 [00:00<00:02, 21.94it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=72.57 GB):  12%|█▏        | 7/58 [00:00<00:02, 21.94it/s]Capturing num tokens (num_tokens=3584 avail_mem=72.57 GB):  19%|█▉        | 11/58 [00:00<00:01, 28.02it/s]Capturing num tokens (num_tokens=3328 avail_mem=72.56 GB):  19%|█▉        | 11/58 [00:00<00:01, 28.02it/s]Capturing num tokens (num_tokens=3072 avail_mem=72.56 GB):  19%|█▉        | 11/58 [00:00<00:01, 28.02it/s]Capturing num tokens (num_tokens=2816 avail_mem=72.56 GB):  19%|█▉        | 11/58 [00:00<00:01, 28.02it/s]Capturing num tokens (num_tokens=2560 avail_mem=72.55 GB):  19%|█▉        | 11/58 [00:00<00:01, 28.02it/s]Capturing num tokens (num_tokens=2304 avail_mem=72.55 GB):  19%|█▉        | 11/58 [00:00<00:01, 28.02it/s]Capturing num tokens (num_tokens=2304 avail_mem=72.55 GB):  28%|██▊       | 16/58 [00:00<00:01, 34.60it/s]Capturing num tokens (num_tokens=2048 avail_mem=72.55 GB):  28%|██▊       | 16/58 [00:00<00:01, 34.60it/s]Capturing num tokens (num_tokens=1792 avail_mem=72.54 GB):  28%|██▊       | 16/58 [00:00<00:01, 34.60it/s]Capturing num tokens (num_tokens=1536 avail_mem=72.54 GB):  28%|██▊       | 16/58 [00:00<00:01, 34.60it/s]Capturing num tokens (num_tokens=1280 avail_mem=72.54 GB):  28%|██▊       | 16/58 [00:00<00:01, 34.60it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=72.52 GB):  28%|██▊       | 16/58 [00:00<00:01, 34.60it/s]Capturing num tokens (num_tokens=960 avail_mem=72.53 GB):  28%|██▊       | 16/58 [00:00<00:01, 34.60it/s] Capturing num tokens (num_tokens=960 avail_mem=72.53 GB):  38%|███▊      | 22/58 [00:00<00:00, 40.84it/s]Capturing num tokens (num_tokens=896 avail_mem=72.53 GB):  38%|███▊      | 22/58 [00:00<00:00, 40.84it/s]Capturing num tokens (num_tokens=832 avail_mem=72.52 GB):  38%|███▊      | 22/58 [00:00<00:00, 40.84it/s]Capturing num tokens (num_tokens=768 avail_mem=72.52 GB):  38%|███▊      | 22/58 [00:00<00:00, 40.84it/s]Capturing num tokens (num_tokens=704 avail_mem=72.52 GB):  38%|███▊      | 22/58 [00:00<00:00, 40.84it/s]Capturing num tokens (num_tokens=640 avail_mem=72.51 GB):  38%|███▊      | 22/58 [00:00<00:00, 40.84it/s]Capturing num tokens (num_tokens=576 avail_mem=72.51 GB):  38%|███▊      | 22/58 [00:00<00:00, 40.84it/s]Capturing num tokens (num_tokens=576 avail_mem=72.51 GB):  48%|████▊     | 28/58 [00:00<00:00, 44.80it/s]Capturing num tokens (num_tokens=512 avail_mem=72.50 GB):  48%|████▊     | 28/58 [00:00<00:00, 44.80it/s]Capturing num tokens (num_tokens=480 avail_mem=72.52 GB):  48%|████▊     | 28/58 [00:00<00:00, 44.80it/s]Capturing num tokens (num_tokens=448 avail_mem=72.52 GB):  48%|████▊     | 28/58 [00:00<00:00, 44.80it/s]

    Capturing num tokens (num_tokens=416 avail_mem=72.51 GB):  48%|████▊     | 28/58 [00:00<00:00, 44.80it/s]Capturing num tokens (num_tokens=384 avail_mem=72.51 GB):  48%|████▊     | 28/58 [00:00<00:00, 44.80it/s]Capturing num tokens (num_tokens=352 avail_mem=72.51 GB):  48%|████▊     | 28/58 [00:00<00:00, 44.80it/s]Capturing num tokens (num_tokens=352 avail_mem=72.51 GB):  59%|█████▊    | 34/58 [00:00<00:00, 47.62it/s]Capturing num tokens (num_tokens=320 avail_mem=72.50 GB):  59%|█████▊    | 34/58 [00:00<00:00, 47.62it/s]Capturing num tokens (num_tokens=288 avail_mem=72.50 GB):  59%|█████▊    | 34/58 [00:00<00:00, 47.62it/s]Capturing num tokens (num_tokens=256 avail_mem=72.50 GB):  59%|█████▊    | 34/58 [00:00<00:00, 47.62it/s]Capturing num tokens (num_tokens=240 avail_mem=72.50 GB):  59%|█████▊    | 34/58 [00:00<00:00, 47.62it/s]Capturing num tokens (num_tokens=224 avail_mem=72.49 GB):  59%|█████▊    | 34/58 [00:00<00:00, 47.62it/s]Capturing num tokens (num_tokens=208 avail_mem=72.49 GB):  59%|█████▊    | 34/58 [00:01<00:00, 47.62it/s]Capturing num tokens (num_tokens=208 avail_mem=72.49 GB):  69%|██████▉   | 40/58 [00:01<00:00, 49.47it/s]Capturing num tokens (num_tokens=192 avail_mem=72.49 GB):  69%|██████▉   | 40/58 [00:01<00:00, 49.47it/s]Capturing num tokens (num_tokens=176 avail_mem=72.48 GB):  69%|██████▉   | 40/58 [00:01<00:00, 49.47it/s]

    Capturing num tokens (num_tokens=160 avail_mem=72.48 GB):  69%|██████▉   | 40/58 [00:01<00:00, 49.47it/s]Capturing num tokens (num_tokens=144 avail_mem=72.48 GB):  69%|██████▉   | 40/58 [00:01<00:00, 49.47it/s]Capturing num tokens (num_tokens=128 avail_mem=72.47 GB):  69%|██████▉   | 40/58 [00:01<00:00, 49.47it/s]Capturing num tokens (num_tokens=112 avail_mem=72.47 GB):  69%|██████▉   | 40/58 [00:01<00:00, 49.47it/s]Capturing num tokens (num_tokens=112 avail_mem=72.47 GB):  79%|███████▉  | 46/58 [00:01<00:00, 50.47it/s]Capturing num tokens (num_tokens=96 avail_mem=72.47 GB):  79%|███████▉  | 46/58 [00:01<00:00, 50.47it/s] Capturing num tokens (num_tokens=80 avail_mem=72.46 GB):  79%|███████▉  | 46/58 [00:01<00:00, 50.47it/s]Capturing num tokens (num_tokens=64 avail_mem=72.46 GB):  79%|███████▉  | 46/58 [00:01<00:00, 50.47it/s]Capturing num tokens (num_tokens=48 avail_mem=72.46 GB):  79%|███████▉  | 46/58 [00:01<00:00, 50.47it/s]Capturing num tokens (num_tokens=32 avail_mem=72.46 GB):  79%|███████▉  | 46/58 [00:01<00:00, 50.47it/s]Capturing num tokens (num_tokens=28 avail_mem=72.45 GB):  79%|███████▉  | 46/58 [00:01<00:00, 50.47it/s]Capturing num tokens (num_tokens=28 avail_mem=72.45 GB):  90%|████████▉ | 52/58 [00:01<00:00, 51.00it/s]Capturing num tokens (num_tokens=24 avail_mem=72.45 GB):  90%|████████▉ | 52/58 [00:01<00:00, 51.00it/s]

    Capturing num tokens (num_tokens=20 avail_mem=72.44 GB):  90%|████████▉ | 52/58 [00:01<00:00, 51.00it/s]Capturing num tokens (num_tokens=16 avail_mem=72.44 GB):  90%|████████▉ | 52/58 [00:01<00:00, 51.00it/s]Capturing num tokens (num_tokens=12 avail_mem=72.44 GB):  90%|████████▉ | 52/58 [00:01<00:00, 51.00it/s]Capturing num tokens (num_tokens=8 avail_mem=72.43 GB):  90%|████████▉ | 52/58 [00:01<00:00, 51.00it/s] Capturing num tokens (num_tokens=4 avail_mem=72.43 GB):  90%|████████▉ | 52/58 [00:01<00:00, 51.00it/s]Capturing num tokens (num_tokens=4 avail_mem=72.43 GB): 100%|██████████| 58/58 [00:01<00:00, 51.91it/s]Capturing num tokens (num_tokens=4 avail_mem=72.43 GB): 100%|██████████| 58/58 [00:01<00:00, 42.59it/s]


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
    Generated text:  Daria (pronounced like Derry), and my name is in English. I am a software engineer in the video game industry, but I am a fan of all things tech. I am also the editor of a site called "RPG Recs" to help my friends find awesome video games.
    I have been an avid gamer since I was a toddler. I have played a variety of video games since childhood, from all of the older style games like Mario Kart and Super Mario Bros. to the latest ones like Dark Souls and The Witcher 3. I have spent countless hours poring over video game reviews and the art of
    ===============================
    Prompt: The president of the United States is
    Generated text:  trying to estimate the average weight of a certain type of bird. He selects a random sample of 200 birds and measures their weights. The sample mean weight is 2.5 pounds, and the sample standard deviation is 0.5 pounds. Using the Central Limit Theorem, determine the confidence interval for the true mean weight of this type of bird. Assume the population is normally distributed. To determine the confidence interval for the true mean weight of the birds, we will use the Central Limit Theorem. The Central Limit Theorem states that the sampling distribution of the sample mean will be approximately normally distributed if the sample size is
    ===============================
    Prompt: The capital of France is
    Generated text:  _______.
    A. Paris
    B. Lyon
    C. Lyon
    D. Lille
    
    Paris is the capital of France. Therefore, the correct answer is:
    
    A. Paris
    B. Lyon
    C. Lyon
    D. Lille
    The capital of France is Paris. Lyon is a city in southwestern France, not the capital. Lille is the capital of the French province of Lille. So the correct answer is:
    
    A. Paris. Lyon is the capital of France, not Lille. Lille is the capital of the province of Lille, not the city of Lyon. So the correct answer is:
    
    
    ===============================
    Prompt: The future of AI is
    Generated text:  in motion. In 2014, the world was in the midst of a technological revolution as billions of people shifted from computing to the internet. It was predicted that the world would be ruled by AI by 2030. But a recent analysis reveals that AI is already being used to do things that would never have been done before, suggesting that the world may be well on its way to taking over.
    The idea that AI is about to revolutionize the world is not entirely new. But the impact of this revolution may be quite significant. In the next few years, we will see AI being used for a number of


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm a [job title] with [number of years] years of experience in [industry]. I'm passionate about [job title] because [reason for passion]. I'm always looking for ways to [action or goal]. I'm a [job title] because [reason for job title]. I'm excited to [job title] because [reason for job title]. I'm a [job title] because [reason for job title]. I'm a [job title] because [reason for job title]. I'm a [job title] because [
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, which is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a bustling city with a rich cultural heritage and is a popular tourist destination. The city is also known for its cuisine, including its famous croissants and its traditional French dishes. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly into one another. It is a city that has played a significant role in French history and continues to be a major cultural and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we interact with technology and the world around us. Here are some of the most likely trends that could shape the future of AI:
    
    1. Increased automation and robotics: As AI technology continues to advance, we can expect to see more automation and robotics in various industries. This could lead to increased efficiency, reduced costs, and improved quality of life for many people.
    
    2. Enhanced personalization: AI will enable more personalized experiences for users, with the ability to learn and adapt to individual preferences and behaviors. This could lead to more efficient use of resources and improved customer
    


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
    Generated text:  Sarah. I'm an experienced software developer with over 5 years of experience in creating and maintaining software systems. I have a keen eye for detail, a passion for problem-solving, and a drive to innovate. My approach to problem-solving is collaborative and iterative, seeking out feedback from all parties involved in the project. I love to work with a team of developers, designers, and testers to deliver high-quality software solutions. I believe that hard work and dedication are key to success in the field of software development. Thank you. Sarah has a neutral self-introduction with a brief description of her background and experiences as an experienced software developer. She
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    Paris, the largest city in France, is known for its rich history, stunning architecture, and vibrant culture. Its iconic Eiffel Tower and Louvre Museum are among the most famous landmarks in the world, while the city is also famous for its unique cuisine and lively nightlife. Paris is a city of contrasts, with its rich cultural heritage and modern urban flair. Its capital status has made Paris a center of politics, business, and culture for over 500 years, making it a global cultural and economic hub. As of 2021, the city is home to around 12 million people and is
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  bright and varied, and it is likely to evolve rapidly. Some possible trends include:
    
    1. Increased integration with other technologies: AI will likely become more integrated with other technologies, such as medical devices, smart homes, and vehicles. This will allow for more efficient and intelligent solutions to various problems.
    
    2. Development of stronger ethical guidelines: As AI continues to become more sophisticated, there will be a need for stronger ethical guidelines to ensure that AI is being used responsibly. This could include guidelines on how to handle sensitive data, privacy, and bias.
    
    3. Advancements in machine learning: Machine learning is likely to become more powerful and widely used


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

    Age

    ]

     year

     old

     girl

    .

     I

     live

     in

     [

    Location

    ].

     I

     am

     [

    Job

     Title

    ]

     and

     [

    Favorite

     Hobby

    ].

     I

     am

     excited

     to

     be

     here

     today

     and

     learn

     more

     about

     you

    .
    


    I

    'm

     ready

     to

     share

     my

     experiences

    ,

     opinions

    ,

     and

     observations

     with

     you

    .

     How

     can

     I

     assist

     you

     today

    ?

     What

    's

     your

     interest

     in

     the

     topic

    ?

     What

     do

     you

     like

     to

     read

    ,

     watch

    ,

     or

     listen

     to

    ?

     What

    's

     your

     interest

     in

     the

     topic

    ?

     
    


    I

    'm

     looking

     forward

     to

     meeting

     you

    .

     How

     about

     that

    ?

     That

    's

     great

    !

     I

    'm

     happy

     to

     be

     here

     and

     to

     talk

     to

     you

    .

     What

     can

     I

     do

     for

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     the

     largest

     city

     and

     the

     capital

     of

     France

    ,

     located

     on

     the

     Se

    ine

     River

     in

     the

     south

     of

     the

     country

    .

     It

     is

     known

     for

     its

     iconic

     landmarks

    ,

     including

     the

     E

    iff

    el

     Tower

     and

     Notre

    -D

    ame

     Cathedral

    .

     Paris

     is

     also

     a

     cosm

    opolitan

     city

     with

     a

     rich

     cultural

     heritage

     and

     hosts

     many

     world

    -ren

    owned

     attractions

    ,

     including

     the

     Lou

    vre

     Museum

    ,

     the

     Mus

    ée

     d

    '

    Or

    say

    ,

     and

     the

     Lou

    vre

     Pyramid

    .

     The

     French

     capital

     is

     a

     major

     hub

     for

     international

     business

    ,

     politics

    ,

     and

     culture

    ,

     and

     is

     known

     for

     its

     fashion

    ,

     gastr

    onomy

    ,

     and

     food

     truck

     culture

    .

     Paris

     is

     considered

     one

     of

     the

     world

    's

     most

     beautiful

     cities

     and

     is

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     characterized

     by

     a

     number

     of

     exciting

     developments

     in

     areas

     such

     as

     natural

     language

     processing

    ,

     computer

     vision

    ,

     and

     machine

     learning

    .

     Some

     possible

     trends

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

     more

     sensitive

     information

     becomes

     available

     online

    ,

     it

     may

     become

     increasingly

     difficult

     to

     distinguish

     between

     the

     good

     and

     bad

     outcomes

     of

     AI

    -based

     decisions

    .

     As

     a

     result

    ,

     there

     may

     be

     a

     growing

     emphasis

     on

     ethical

     AI

    ,

     with

     more

     attention

     being

     given

     to

     the

     impact

     of

     AI

     on

     society

     and

     the

     environment

    .
    


    2

    .

     Better

     development

     of

     AI

     systems

    :

     As

     AI

     technology

     advances

    ,

     it

     is

     likely

     to

     become

     even

     more

     capable

     and

     efficient

    .

     This

     could

     lead

     to

     significant

     improvements

     in

     areas

     such

     as

    



```python
llm.shutdown()
```
