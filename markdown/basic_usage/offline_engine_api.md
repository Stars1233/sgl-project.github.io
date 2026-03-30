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


    2026-03-30 03:40:28.322 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 03:40:28] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 03:40:28.322 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 03:40:28] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 03:40:28.322 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 03:40:28] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 03:40:28.322 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 03:40:28] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 03:40:28.322 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 03:40:28] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.86it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.85it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:15,  2.38s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:15,  2.38s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:15,  2.38s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:15,  2.38s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.38it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.38it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.38it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.38it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.38it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.38it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.38it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.38it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.38it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:03, 13.31it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:03, 13.31it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:03, 13.31it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:03, 13.31it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:03, 13.31it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:03, 13.31it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:03, 13.31it/s]

    Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:02<00:03, 13.31it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 19.78it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 19.78it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 19.78it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 19.78it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 19.78it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:02<00:01, 19.78it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:02<00:01, 19.78it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:02<00:01, 19.78it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:02<00:00, 26.73it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:02<00:00, 26.73it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:02<00:00, 26.73it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:02<00:00, 26.73it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:02<00:00, 26.73it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:02<00:00, 26.73it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:00, 26.73it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 31.70it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 31.70it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 31.70it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 31.70it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 31.70it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 31.70it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 31.70it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 36.96it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 36.96it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 36.96it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 36.96it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 36.96it/s]

    Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 36.96it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 36.96it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 39.88it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 39.88it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 39.88it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 39.88it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 39.88it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 39.88it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 39.88it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 39.88it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 39.88it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 48.45it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.25it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=121.74 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=121.71 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=121.71 GB):   3%|▎         | 2/58 [00:00<00:03, 14.08it/s]Capturing num tokens (num_tokens=7168 avail_mem=121.70 GB):   3%|▎         | 2/58 [00:00<00:03, 14.08it/s]Capturing num tokens (num_tokens=6656 avail_mem=121.70 GB):   3%|▎         | 2/58 [00:00<00:03, 14.08it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=121.70 GB):   3%|▎         | 2/58 [00:00<00:03, 14.08it/s]Capturing num tokens (num_tokens=6144 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 19.04it/s]Capturing num tokens (num_tokens=5632 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 19.04it/s]Capturing num tokens (num_tokens=5120 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 19.04it/s]Capturing num tokens (num_tokens=4608 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 19.04it/s]Capturing num tokens (num_tokens=4096 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 19.04it/s]Capturing num tokens (num_tokens=4096 avail_mem=121.70 GB):  16%|█▌        | 9/58 [00:00<00:01, 24.50it/s]Capturing num tokens (num_tokens=3840 avail_mem=121.69 GB):  16%|█▌        | 9/58 [00:00<00:01, 24.50it/s]Capturing num tokens (num_tokens=3584 avail_mem=121.69 GB):  16%|█▌        | 9/58 [00:00<00:01, 24.50it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=121.69 GB):  16%|█▌        | 9/58 [00:00<00:01, 24.50it/s]Capturing num tokens (num_tokens=3072 avail_mem=121.68 GB):  16%|█▌        | 9/58 [00:00<00:01, 24.50it/s]Capturing num tokens (num_tokens=2816 avail_mem=121.68 GB):  16%|█▌        | 9/58 [00:00<00:01, 24.50it/s]Capturing num tokens (num_tokens=2816 avail_mem=121.68 GB):  24%|██▍       | 14/58 [00:00<00:01, 30.65it/s]Capturing num tokens (num_tokens=2560 avail_mem=121.68 GB):  24%|██▍       | 14/58 [00:00<00:01, 30.65it/s]Capturing num tokens (num_tokens=2304 avail_mem=121.67 GB):  24%|██▍       | 14/58 [00:00<00:01, 30.65it/s]Capturing num tokens (num_tokens=2048 avail_mem=121.67 GB):  24%|██▍       | 14/58 [00:00<00:01, 30.65it/s]Capturing num tokens (num_tokens=1792 avail_mem=121.67 GB):  24%|██▍       | 14/58 [00:00<00:01, 30.65it/s]Capturing num tokens (num_tokens=1536 avail_mem=121.66 GB):  24%|██▍       | 14/58 [00:00<00:01, 30.65it/s]Capturing num tokens (num_tokens=1536 avail_mem=121.66 GB):  33%|███▎      | 19/58 [00:00<00:01, 35.07it/s]Capturing num tokens (num_tokens=1280 avail_mem=121.66 GB):  33%|███▎      | 19/58 [00:00<00:01, 35.07it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=121.64 GB):  33%|███▎      | 19/58 [00:00<00:01, 35.07it/s]Capturing num tokens (num_tokens=960 avail_mem=121.38 GB):  33%|███▎      | 19/58 [00:00<00:01, 35.07it/s] Capturing num tokens (num_tokens=896 avail_mem=121.38 GB):  33%|███▎      | 19/58 [00:00<00:01, 35.07it/s]Capturing num tokens (num_tokens=832 avail_mem=121.37 GB):  33%|███▎      | 19/58 [00:00<00:01, 35.07it/s]Capturing num tokens (num_tokens=832 avail_mem=121.37 GB):  41%|████▏     | 24/58 [00:00<00:00, 37.24it/s]Capturing num tokens (num_tokens=768 avail_mem=120.81 GB):  41%|████▏     | 24/58 [00:00<00:00, 37.24it/s]Capturing num tokens (num_tokens=704 avail_mem=120.66 GB):  41%|████▏     | 24/58 [00:00<00:00, 37.24it/s]Capturing num tokens (num_tokens=640 avail_mem=120.66 GB):  41%|████▏     | 24/58 [00:00<00:00, 37.24it/s]Capturing num tokens (num_tokens=576 avail_mem=120.66 GB):  41%|████▏     | 24/58 [00:00<00:00, 37.24it/s]Capturing num tokens (num_tokens=512 avail_mem=120.65 GB):  41%|████▏     | 24/58 [00:00<00:00, 37.24it/s]

    Capturing num tokens (num_tokens=512 avail_mem=120.65 GB):  50%|█████     | 29/58 [00:00<00:00, 39.49it/s]Capturing num tokens (num_tokens=480 avail_mem=120.66 GB):  50%|█████     | 29/58 [00:00<00:00, 39.49it/s]Capturing num tokens (num_tokens=448 avail_mem=120.66 GB):  50%|█████     | 29/58 [00:00<00:00, 39.49it/s]Capturing num tokens (num_tokens=416 avail_mem=120.66 GB):  50%|█████     | 29/58 [00:00<00:00, 39.49it/s]Capturing num tokens (num_tokens=384 avail_mem=120.66 GB):  50%|█████     | 29/58 [00:00<00:00, 39.49it/s]Capturing num tokens (num_tokens=352 avail_mem=120.65 GB):  50%|█████     | 29/58 [00:00<00:00, 39.49it/s]Capturing num tokens (num_tokens=352 avail_mem=120.65 GB):  59%|█████▊    | 34/58 [00:00<00:00, 40.89it/s]Capturing num tokens (num_tokens=320 avail_mem=120.65 GB):  59%|█████▊    | 34/58 [00:00<00:00, 40.89it/s]Capturing num tokens (num_tokens=288 avail_mem=120.64 GB):  59%|█████▊    | 34/58 [00:01<00:00, 40.89it/s]Capturing num tokens (num_tokens=256 avail_mem=120.64 GB):  59%|█████▊    | 34/58 [00:01<00:00, 40.89it/s]Capturing num tokens (num_tokens=240 avail_mem=120.64 GB):  59%|█████▊    | 34/58 [00:01<00:00, 40.89it/s]

    Capturing num tokens (num_tokens=224 avail_mem=120.64 GB):  59%|█████▊    | 34/58 [00:01<00:00, 40.89it/s]Capturing num tokens (num_tokens=224 avail_mem=120.64 GB):  67%|██████▋   | 39/58 [00:01<00:00, 40.81it/s]Capturing num tokens (num_tokens=208 avail_mem=120.63 GB):  67%|██████▋   | 39/58 [00:01<00:00, 40.81it/s]Capturing num tokens (num_tokens=192 avail_mem=120.63 GB):  67%|██████▋   | 39/58 [00:01<00:00, 40.81it/s]Capturing num tokens (num_tokens=176 avail_mem=120.63 GB):  67%|██████▋   | 39/58 [00:01<00:00, 40.81it/s]Capturing num tokens (num_tokens=160 avail_mem=120.62 GB):  67%|██████▋   | 39/58 [00:01<00:00, 40.81it/s]Capturing num tokens (num_tokens=144 avail_mem=120.62 GB):  67%|██████▋   | 39/58 [00:01<00:00, 40.81it/s]Capturing num tokens (num_tokens=144 avail_mem=120.62 GB):  76%|███████▌  | 44/58 [00:01<00:00, 42.10it/s]Capturing num tokens (num_tokens=128 avail_mem=120.62 GB):  76%|███████▌  | 44/58 [00:01<00:00, 42.10it/s]Capturing num tokens (num_tokens=112 avail_mem=120.29 GB):  76%|███████▌  | 44/58 [00:01<00:00, 42.10it/s]Capturing num tokens (num_tokens=96 avail_mem=120.19 GB):  76%|███████▌  | 44/58 [00:01<00:00, 42.10it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  76%|███████▌  | 44/58 [00:01<00:00, 42.10it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  76%|███████▌  | 44/58 [00:01<00:00, 42.10it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  84%|████████▍ | 49/58 [00:01<00:00, 42.56it/s]Capturing num tokens (num_tokens=48 avail_mem=120.18 GB):  84%|████████▍ | 49/58 [00:01<00:00, 42.56it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  84%|████████▍ | 49/58 [00:01<00:00, 42.56it/s]Capturing num tokens (num_tokens=28 avail_mem=120.17 GB):  84%|████████▍ | 49/58 [00:01<00:00, 42.56it/s]Capturing num tokens (num_tokens=24 avail_mem=120.17 GB):  84%|████████▍ | 49/58 [00:01<00:00, 42.56it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  84%|████████▍ | 49/58 [00:01<00:00, 42.56it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  93%|█████████▎| 54/58 [00:01<00:00, 43.29it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  93%|█████████▎| 54/58 [00:01<00:00, 43.29it/s]Capturing num tokens (num_tokens=12 avail_mem=120.16 GB):  93%|█████████▎| 54/58 [00:01<00:00, 43.29it/s]Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  93%|█████████▎| 54/58 [00:01<00:00, 43.29it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  93%|█████████▎| 54/58 [00:01<00:00, 43.29it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 37.71it/s]


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
    Generated text:  Li. My favorite color is blue. My favorite subject is English. I'm a student in Grade 12. I like playing computer games and listening to music. I often have fun. My dream is to be a teacher. Everyone in the world has his or her dream. I think you all must have your own dream too. What's your dream? I think some of you may dream about being a teacher. Teachers can teach us knowledge and help us with our studies. And they also help us be kinder to others. Some of you may dream about becoming a doctor. Doctors can help sick people and save their lives.
    ===============================
    Prompt: The president of the United States is
    Generated text:  an elected official who serves for four-year terms. What does this imply about the United States government system?
    
    a. It is a direct democracy
    b. It is a representative democracy
    c. It is an autocratic government
    d. It is a federal government with multiple branches
    
    b. It is a representative democracy
    
    The United States government system is characterized by the existence of a two-party system where the president is an elected official who serves for four-year terms. This implies that it is a representative democracy. A representative democracy is a system of government in which the people have a representative or elector to whom they directly give their vote
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. It has a population of about 2.4 million, 80% of which are in Paris. The capital of the United Kingdom is London. It has a population of about 1.3 million, 90% of which are in London. What is the ratio of the population of Paris to the population of London? To find the ratio of the population of Paris to the population of London, we need to divide the population of Paris by the population of London. Let's perform the division step-by-step.
    
    The population of Paris is 2,400,000 and the population of London
    ===============================
    Prompt: The future of AI is
    Generated text:  really fast-paced, and it’s important to know what the most recent trends are. The majority of the time, it’s hard to keep up with the rapid pace of change, but there are some basic things you can do to stay ahead of the curve. In this article, we will discuss the latest trends in AI and how to stay up-to-date with them.
    Here are some of the most significant trends in AI today:
    1. Deep Learning: Deep learning is a type of machine learning where neural networks are trained to recognize complex patterns and relationships between data. This type of learning is increasingly used in the field of computer vision and natural


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a short description of your character here]. I enjoy [insert a short description of your character's interests or hobbies]. I'm always looking for new experiences and learning new things. What's your favorite hobby or activity? I love [insert a short description of your favorite hobby or activity here]. I'm always looking for new ways to expand my knowledge and skills. What's your favorite book or movie? I love [insert a short description
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament and the French National Library. Paris is a bustling metropolis with a rich cultural heritage and is a major economic and political center in Europe. The city is known for its fashion industry, art scene, and its role in the French Revolution and the French Revolution. Paris is also home to many famous landmarks, including the Louvre, the Notre-Dame Cathedral, and the Champs-Élysées. The city is known for its cuisine, including its famous
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that are expected to shape the future of AI:
    
    1. Increased use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes and reduce costs. As AI technology continues to improve, we can expect to see even more widespread use of AI in healthcare, particularly in areas such as diagnosis, treatment planning, and patient care.
    
    2. Increased use of AI in finance: AI is already being used in finance to improve fraud detection, risk management, and investment decision-making
    


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
    Generated text:  [Name]. I am a [job title] with 10 years of experience in the tech industry. I am currently a [job title] at [company name]. My specialization is [specific skill or knowledge area]. I am passionate about [reason why I love my job and industry]. I enjoy [activities or hobbies]. I am a team player, always striving to make the best decisions for my team and company. Thank you for asking! I am a highly qualified [job title] with [job title] and have [number] years of experience in the [job title]. I have a [specific skill or knowledge area]
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. Its skyline is filled with historic landmarks such as Notre-Dame Cathedral, the Louvre Museum, and the Eiffel Tower. The city is home to the French Parliament, the Metropolitan Cathedral, and numerous museums, including the Musée d'Orsay and the Musée national d'art moderne. Paris is a vibrant and culturally rich city with a rich history dating back to the Roman Empire and a modern city that is known for its artistic and cultural achievements. The city is also home to many international landmarks and cultural institutions, making it a significant cultural hub in Europe. 
    
    Paris is a popular tourist destination, with millions of
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to involve a number of trends that will shape the technology's direction and impact. Some possible future trends in AI include:
    
    1. Increased depth and complexity: AI systems are becoming more capable of handling complex tasks and making decisions that require a high degree of reasoning and problem-solving. This means that AI will likely become more intelligent and capable of solving more complex problems than it is today.
    
    2. Autonomous and cyber-physical systems: AI systems are expected to play a more significant role in autonomous and cyber-physical systems. Autonomous systems will be able to operate without human intervention, making them safer and more reliable. In the future, AI will


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

    typeof

    ]

     [

    occupation

    ].

     I

     specialize

     in

     [

    special

    ization

    ].

     I

    'm

     [

    years

     old

    ],

     and

     I

    've

     been

     with

     [

    company

    ]

     for

     [

    number

     of

     years

    ].

     I

    'm

     always

     [

    description

     of

     me

    ],

     and

     I

    'm

     [

    degree

    ]

     [

    special

    ization

    ]

     and

     am

     a

     [

    professional

     experience

    ]

     in

     my

     field

    .

     I

    'm

     [

    description

     of

     me

    ].

     I

     love

     [

    what

     I

     do

     best

    ].

     I

    'm

     [

    what

     I

     hope

     to

     achieve

     in

     the

     future

    ].

     I

     like

     [

    why

     I

     love

     this

     job

    ].

     My

     name

     is

     [

    Name

    ],

     a

     [

    typeof

    ]

     [

    occupation

    ],

     who

     specializes

     in

     [

    special

    ization

    ]

     and

     is

     [

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     located

     on

     the

     Se

    ine

     River

     and

     surrounded

     by

     the

     city

     of

     Paris

    .

     It

     is

     the

     largest

     city

     in

     France

     and

     the

     

    1

    3

    th

     largest

     city

     in

     the

     world

    .

     France

    's

     capital

     city

     is

     home

     to

     the

     E

    iff

    el

     Tower

     and

     is

     a

     UNESCO

     World

     Heritage

     site

    .

     It

     is

     known

     for

     its

     architecture

    ,

     music

    ,

     and

     food

    .

     Paris

     is

     also

     a

     popular

     tourist

     destination

     for

     its

     rich

     history

    ,

     cultural

     landmarks

    ,

     and

     fashion

    .

     The

     city

     is

     also

     the

     financial

     hub

     of

     France

     and

     plays

     an

     important

     role

     in

     the

     country

    's

     economy

    .

     Paris

     is

     also

     known

     for

     its

     fashion

     and

     art

     scene

    ,

     as

     well

     as

     its

     French

     language

     and

     culture

    .

     France

    's

     capital

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     exciting

     and

     unpredictable

    ,

     with

     many

     potential

     areas

     of

     development

     and

     impact

    .

     Here

     are

     some

     possible

     trends

     in

     AI

    :
    


    1

    .

     Increased

     collaboration

     between

     AI

     and

     human

     experts

    :

     As

     AI

     becomes

     more

     integrated

     with

     human

     expertise

    ,

     we

     may

     see

     a

     growing

     emphasis

     on

     collaboration

     between

     the

     two

     fields

    .

     This

     could

     lead

     to

     more

     efficient

     and

     effective

     use

     of

     AI

     technology

     by

     humans

     in

     a

     wide

     range

     of

     applications

    .
    


    2

    .

     Improved

     accuracy

     and

     reliability

    :

     AI

     is

     becoming

     more

     accurate

     and

     reliable

    ,

     with

     advances

     in

     machine

     learning

     and

     other

     technologies

     allowing

     for

     better

     predictions

     and

     decisions

    .

     This

     could

     lead

     to

     new

     applications

     in

     fields

     such

     as

     finance

    ,

     healthcare

    ,

     and

     transportation

    .
    


    3

    .

     Greater

     transparency

     and

     accountability

    :

    



```python
llm.shutdown()
```
