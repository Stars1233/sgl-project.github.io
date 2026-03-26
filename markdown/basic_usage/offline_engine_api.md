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


    2026-03-26 08:37:27.254 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 08:37:27] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 08:37:27.254 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 08:37:27] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 08:37:27.254 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 08:37:27] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 08:37:27.254 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 08:37:27] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 08:37:27.254 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 08:37:27] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.26it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.25it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:19,  2.44s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:19,  2.44s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:19,  2.44s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:19,  2.44s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:03, 13.03it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:03, 13.03it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:03, 13.03it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:03, 13.03it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:03, 13.03it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:03, 13.03it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:03, 13.03it/s]

    Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:02<00:03, 13.03it/s]Compiling num tokens (num_tokens=704):  31%|███       | 18/58 [00:02<00:03, 13.03it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:02<00:01, 20.66it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:02<00:01, 20.66it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:02<00:01, 20.66it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:02<00:01, 20.66it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:02<00:01, 20.66it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:02<00:01, 20.66it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:02<00:01, 20.66it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:02<00:00, 26.13it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:02<00:00, 26.13it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:02<00:00, 26.13it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:00, 26.13it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:00, 26.13it/s]

    Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:00, 26.13it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:00, 26.13it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 30.24it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 30.24it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 30.24it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 30.24it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 30.24it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 30.24it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 30.24it/s]Compiling num tokens (num_tokens=128):  66%|██████▌   | 38/58 [00:03<00:00, 30.24it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 37.63it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 37.63it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 37.63it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 37.63it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 37.63it/s]

    Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 37.63it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 37.63it/s]Compiling num tokens (num_tokens=28):  78%|███████▊  | 45/58 [00:03<00:00, 37.63it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:03<00:00, 43.67it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:03<00:00, 43.67it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:03<00:00, 43.67it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:03<00:00, 43.67it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:03<00:00, 43.67it/s]Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:03<00:00, 43.67it/s] Compiling num tokens (num_tokens=4):  90%|████████▉ | 52/58 [00:03<00:00, 43.67it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.20it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=71.05 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=71.05 GB):   2%|▏         | 1/58 [00:00<00:07,  7.47it/s]Capturing num tokens (num_tokens=7680 avail_mem=71.02 GB):   2%|▏         | 1/58 [00:00<00:07,  7.47it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=71.02 GB):   2%|▏         | 1/58 [00:00<00:07,  7.47it/s]Capturing num tokens (num_tokens=7168 avail_mem=71.02 GB):   5%|▌         | 3/58 [00:00<00:05, 10.46it/s]Capturing num tokens (num_tokens=6656 avail_mem=71.01 GB):   5%|▌         | 3/58 [00:00<00:05, 10.46it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=71.02 GB):   5%|▌         | 3/58 [00:00<00:05, 10.46it/s]Capturing num tokens (num_tokens=6144 avail_mem=71.02 GB):   9%|▊         | 5/58 [00:00<00:07,  7.46it/s]Capturing num tokens (num_tokens=5632 avail_mem=71.01 GB):   9%|▊         | 5/58 [00:00<00:07,  7.46it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=71.01 GB):   9%|▊         | 5/58 [00:00<00:07,  7.46it/s]Capturing num tokens (num_tokens=5120 avail_mem=71.01 GB):  12%|█▏        | 7/58 [00:00<00:05,  8.87it/s]Capturing num tokens (num_tokens=4608 avail_mem=71.01 GB):  12%|█▏        | 7/58 [00:00<00:05,  8.87it/s]Capturing num tokens (num_tokens=4096 avail_mem=71.01 GB):  12%|█▏        | 7/58 [00:00<00:05,  8.87it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=71.01 GB):  16%|█▌        | 9/58 [00:00<00:04, 10.59it/s]Capturing num tokens (num_tokens=3840 avail_mem=71.00 GB):  16%|█▌        | 9/58 [00:00<00:04, 10.59it/s]Capturing num tokens (num_tokens=3584 avail_mem=71.00 GB):  16%|█▌        | 9/58 [00:00<00:04, 10.59it/s]Capturing num tokens (num_tokens=3328 avail_mem=71.00 GB):  16%|█▌        | 9/58 [00:01<00:04, 10.59it/s]Capturing num tokens (num_tokens=3072 avail_mem=70.99 GB):  16%|█▌        | 9/58 [00:01<00:04, 10.59it/s]Capturing num tokens (num_tokens=3072 avail_mem=70.99 GB):  22%|██▏       | 13/58 [00:01<00:02, 16.44it/s]Capturing num tokens (num_tokens=2816 avail_mem=70.99 GB):  22%|██▏       | 13/58 [00:01<00:02, 16.44it/s]Capturing num tokens (num_tokens=2560 avail_mem=70.99 GB):  22%|██▏       | 13/58 [00:01<00:02, 16.44it/s]Capturing num tokens (num_tokens=2304 avail_mem=70.99 GB):  22%|██▏       | 13/58 [00:01<00:02, 16.44it/s]Capturing num tokens (num_tokens=2048 avail_mem=70.98 GB):  22%|██▏       | 13/58 [00:01<00:02, 16.44it/s]Capturing num tokens (num_tokens=1792 avail_mem=70.98 GB):  22%|██▏       | 13/58 [00:01<00:02, 16.44it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=70.98 GB):  31%|███       | 18/58 [00:01<00:01, 24.22it/s]Capturing num tokens (num_tokens=1536 avail_mem=70.98 GB):  31%|███       | 18/58 [00:01<00:01, 24.22it/s]Capturing num tokens (num_tokens=1280 avail_mem=70.97 GB):  31%|███       | 18/58 [00:01<00:01, 24.22it/s]Capturing num tokens (num_tokens=1024 avail_mem=70.95 GB):  31%|███       | 18/58 [00:01<00:01, 24.22it/s]Capturing num tokens (num_tokens=960 avail_mem=70.96 GB):  31%|███       | 18/58 [00:01<00:01, 24.22it/s] Capturing num tokens (num_tokens=896 avail_mem=70.96 GB):  31%|███       | 18/58 [00:01<00:01, 24.22it/s]Capturing num tokens (num_tokens=832 avail_mem=70.96 GB):  31%|███       | 18/58 [00:01<00:01, 24.22it/s]Capturing num tokens (num_tokens=832 avail_mem=70.96 GB):  41%|████▏     | 24/58 [00:01<00:01, 32.01it/s]Capturing num tokens (num_tokens=768 avail_mem=70.95 GB):  41%|████▏     | 24/58 [00:01<00:01, 32.01it/s]Capturing num tokens (num_tokens=704 avail_mem=70.95 GB):  41%|████▏     | 24/58 [00:01<00:01, 32.01it/s]Capturing num tokens (num_tokens=640 avail_mem=70.95 GB):  41%|████▏     | 24/58 [00:01<00:01, 32.01it/s]Capturing num tokens (num_tokens=576 avail_mem=70.95 GB):  41%|████▏     | 24/58 [00:01<00:01, 32.01it/s]

    Capturing num tokens (num_tokens=576 avail_mem=70.95 GB):  48%|████▊     | 28/58 [00:01<00:00, 33.96it/s]Capturing num tokens (num_tokens=512 avail_mem=70.94 GB):  48%|████▊     | 28/58 [00:01<00:00, 33.96it/s]Capturing num tokens (num_tokens=480 avail_mem=70.95 GB):  48%|████▊     | 28/58 [00:01<00:00, 33.96it/s]Capturing num tokens (num_tokens=448 avail_mem=70.95 GB):  48%|████▊     | 28/58 [00:01<00:00, 33.96it/s]

    Capturing num tokens (num_tokens=416 avail_mem=70.95 GB):  48%|████▊     | 28/58 [00:01<00:00, 33.96it/s]Capturing num tokens (num_tokens=416 avail_mem=70.95 GB):  55%|█████▌    | 32/58 [00:01<00:01, 24.65it/s]Capturing num tokens (num_tokens=384 avail_mem=70.95 GB):  55%|█████▌    | 32/58 [00:01<00:01, 24.65it/s]Capturing num tokens (num_tokens=352 avail_mem=70.94 GB):  55%|█████▌    | 32/58 [00:01<00:01, 24.65it/s]Capturing num tokens (num_tokens=320 avail_mem=70.94 GB):  55%|█████▌    | 32/58 [00:01<00:01, 24.65it/s]

    Capturing num tokens (num_tokens=288 avail_mem=76.68 GB):  55%|█████▌    | 32/58 [00:01<00:01, 24.65it/s]Capturing num tokens (num_tokens=288 avail_mem=76.68 GB):  62%|██████▏   | 36/58 [00:01<00:00, 22.39it/s]Capturing num tokens (num_tokens=256 avail_mem=76.68 GB):  62%|██████▏   | 36/58 [00:01<00:00, 22.39it/s]Capturing num tokens (num_tokens=240 avail_mem=76.68 GB):  62%|██████▏   | 36/58 [00:01<00:00, 22.39it/s]Capturing num tokens (num_tokens=224 avail_mem=76.67 GB):  62%|██████▏   | 36/58 [00:01<00:00, 22.39it/s]Capturing num tokens (num_tokens=208 avail_mem=76.67 GB):  62%|██████▏   | 36/58 [00:01<00:00, 22.39it/s]Capturing num tokens (num_tokens=192 avail_mem=76.67 GB):  62%|██████▏   | 36/58 [00:01<00:00, 22.39it/s]Capturing num tokens (num_tokens=176 avail_mem=76.66 GB):  62%|██████▏   | 36/58 [00:01<00:00, 22.39it/s]Capturing num tokens (num_tokens=176 avail_mem=76.66 GB):  72%|███████▏  | 42/58 [00:01<00:00, 28.63it/s]Capturing num tokens (num_tokens=160 avail_mem=76.66 GB):  72%|███████▏  | 42/58 [00:01<00:00, 28.63it/s]Capturing num tokens (num_tokens=144 avail_mem=76.66 GB):  72%|███████▏  | 42/58 [00:01<00:00, 28.63it/s]Capturing num tokens (num_tokens=128 avail_mem=76.65 GB):  72%|███████▏  | 42/58 [00:02<00:00, 28.63it/s]Capturing num tokens (num_tokens=112 avail_mem=76.65 GB):  72%|███████▏  | 42/58 [00:02<00:00, 28.63it/s]

    Capturing num tokens (num_tokens=96 avail_mem=76.65 GB):  72%|███████▏  | 42/58 [00:02<00:00, 28.63it/s] Capturing num tokens (num_tokens=96 avail_mem=76.65 GB):  81%|████████  | 47/58 [00:02<00:00, 33.12it/s]Capturing num tokens (num_tokens=80 avail_mem=76.64 GB):  81%|████████  | 47/58 [00:02<00:00, 33.12it/s]Capturing num tokens (num_tokens=64 avail_mem=76.64 GB):  81%|████████  | 47/58 [00:02<00:00, 33.12it/s]Capturing num tokens (num_tokens=48 avail_mem=76.64 GB):  81%|████████  | 47/58 [00:02<00:00, 33.12it/s]Capturing num tokens (num_tokens=32 avail_mem=76.64 GB):  81%|████████  | 47/58 [00:02<00:00, 33.12it/s]Capturing num tokens (num_tokens=28 avail_mem=76.63 GB):  81%|████████  | 47/58 [00:02<00:00, 33.12it/s]Capturing num tokens (num_tokens=24 avail_mem=76.63 GB):  81%|████████  | 47/58 [00:02<00:00, 33.12it/s]Capturing num tokens (num_tokens=24 avail_mem=76.63 GB):  91%|█████████▏| 53/58 [00:02<00:00, 37.69it/s]Capturing num tokens (num_tokens=20 avail_mem=76.62 GB):  91%|█████████▏| 53/58 [00:02<00:00, 37.69it/s]Capturing num tokens (num_tokens=16 avail_mem=76.62 GB):  91%|█████████▏| 53/58 [00:02<00:00, 37.69it/s]Capturing num tokens (num_tokens=12 avail_mem=76.62 GB):  91%|█████████▏| 53/58 [00:02<00:00, 37.69it/s]Capturing num tokens (num_tokens=8 avail_mem=76.61 GB):  91%|█████████▏| 53/58 [00:02<00:00, 37.69it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=76.61 GB):  91%|█████████▏| 53/58 [00:02<00:00, 37.69it/s]Capturing num tokens (num_tokens=4 avail_mem=76.61 GB): 100%|██████████| 58/58 [00:02<00:00, 25.38it/s]


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
    Generated text:  Yves. I’m a 25-year-old French journalist living in Lille. I like to share my knowledge, opinions and ideas with the world. I’ve written for the likes of The Observer and The Telegraph, and I’m currently working on a book on the history of the Internet.
    I’m a bit of a fan of politics, especially the way that politicians talk to the public. I’ve been a member of several political parties and I’m always on the lookout for new voices and ideas in politics. I’m currently a member of the Socialists and the Greens, and I am an advocate of the right of voice for
    ===============================
    Prompt: The president of the United States is
    Generated text:  a very important person in the country. He is the leader of the country and he is responsible for the policies of the country. His job is to make sure that everyone in the country follows the laws of the country. He also makes sure that the country is safe from any attacks.
    The president of the United States is elected by the people who vote in the presidential election. He represents the country and takes care of the country's affairs.
    What is the main idea of this passage?
    A) The president is responsible for the policies of the country.
    B) The president is the leader of the country.
    C) The president is the only
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. Which of the following statements is true? (　　)  
    A: The capital of Germany is Berlin  
    B: The capital of England is London  
    C: The capital of Italy is Rome  
    D: The capital of France is Madrid
    
    To determine the correct capital of France, let's analyze each statement one by one:
    
    A: The capital of Germany is Berlin.
    - Germany is a country, not a city. Berlin is the capital of Germany. Therefore, this statement is false.
    
    B: The capital of England is London.
    - England is a country, not a city. London is the capital of England. Therefore
    ===============================
    Prompt: The future of AI is
    Generated text:  incredibly exciting, with many applications in healthcare, finance, and beyond. But the way we interact with AI, or the role we take in creating it, will determine what kind of AI we will have in the future.
    The field of AI is constantly evolving, with new technologies and techniques being introduced every day. But one of the most important aspects of AI is the way we interact with it.
    In healthcare, AI has the potential to transform the way we diagnose and treat diseases. It can analyze medical records, image data, and other types of information to help doctors make more accurate diagnoses and identify potential health risks. This can lead to improved


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [age] year old, [gender] and I have [number] years of experience in [job title]. I'm a [job title] at [company name], and I'm always looking for ways to [describe a new skill or initiative]. I'm a [job title] at [company name], and I'm always looking for ways to [describe a new skill or initiative]. I'm a [job title] at [company
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic Eiffel Tower and the annual Eiffel Tower Festival. It is also the seat of the French government and the largest city in the European Union. Paris is a cultural and historical center with many museums, art galleries, and landmarks. The city is also known for its cuisine, fashion, and music scene. Paris is a popular tourist destination and a major economic center in France. It is also home to many international organizations and institutions. Paris is a vibrant and dynamic city with a rich history and a strong sense of community. The city is known for its diverse population, multiculturalism, and its
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction. Here are some of the most likely trends:
    
    1. Increased use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes, reduce costs, and increase efficiency. As AI technology continues to improve, we can expect to see even more widespread use of AI in healthcare, particularly in areas such as diagnosis, treatment planning, and patient care.
    
    2. AI in finance: AI is already being used in finance to improve fraud detection, risk management, and investment decision-making. As AI technology continues to improve, we can expect to
    


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
    Generated text:  [Name] and I'm a [Age] year old [Country]. I'm from [Country], but I'm currently studying in [University/College] and I've been [Degree/Position] for [Number of Years] years. I'm particularly interested in [Area of Interest] and I'm always looking to [My Goal] in my free time. What brings you here today? I hope you have a great day. [Name] [Address] [Phone Number] [E-Mail Address] [LinkedIn Profile] If you have any questions or if you need any help, please don't hesitate to contact me. [
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. It is a bustling metropolis with a rich cultural heritage, characterized by its iconic Eiffel Tower, Notre Dame Cathedral, and many other landmarks. The city is known for its fashion, art, cuisine, and gastronomy, and is also home to many international institutions and organizations. With a population of over 10 million people, Paris is a major cultural and economic hub in Europe and beyond. It is a popular tourist destination, attracting millions of visitors each year. Paris's economic power has played a crucial role in shaping the modern world, and its influence can be seen in many aspects of French culture, politics, and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  set to be one of rapid growth, innovation, and transformation. Here are some potential trends that are likely to shape the future of AI:
    
    1. Increased Use of AI for Business: AI is already being used to improve efficiency and productivity in various industries, from finance to healthcare to retail. As AI technologies continue to improve, we can expect to see more widespread use of AI in business, from customer service to marketing to manufacturing.
    
    2. Integration of AI into Everyday Life: AI will continue to be integrated into everyday life, from cars and homes to devices and appliances. In the coming years, we can expect to see AI become an integral


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

    ],

     and

     I

    'm

     a

     [

    Your

     occupation

    ].

     I

     have

     a

     [

    Number

    ]

     of

     years

     of

     experience

     in

     [

    Your

     field

    ]

     and

     [

    Your

     specialty

    ].

     I

     enjoy

     [

    Your

     hobbies

    ,

     interests

    ,

     or

     passions

    ],

     and

     I

     believe

     that

     [

    Your

     profession

    ]

     is

     the

     best

     way

     to

     achieve

     [

    Your

     goal

    (s

    )].

     I

     am

     a

     [

    Your

     characteristic

    ],

     and

     I

     am

     [

    Your

     personality

     type

    ].

     I

     am

     always

     eager

     to

     learn

     and

     have

     a

     [

    Your

     positive

     attribute

    ],

     and

     I

     am

     always

     striving

     to

     be

     better

     than

     I

     am

    .

     Thank

     you

     for

     having

     me

    !

     [

    Your

     Name

    ].

     [

    Your

     Age

    ]

     years

     of

     experience

     in

     [

    Your

     field

    ]

     and

     [

    Your

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     also

     known

     as

     "

    La

     Ville

    lore

    ,"

     which

     means

     "

    The

     City

     of

     Light

    ."


    The

     city

     was

     founded

     by

     the

     Vikings

     and

     is

     home

     to

     the

     famous

     Lou

    vre

     Museum

     and

     Notre

    -D

    ame

     Cathedral

    .

     Paris

     is

     known

     for

     its

     historic

     landmarks

    ,

     including

     the

     E

    iff

    el

     Tower

     and

     the

     Arc

     de

     Tri

    omp

    he

    ,

     and

     is

     one

     of

     the

     world

    's

     most

     popular

     tourist

     destinations

    .

     It

     is

     also

     a

     hub

     for

     the

     arts

     and

     a

     center

     of

     political

     and

     cultural

     life

    .

     Paris

     is

     a

     major

     center

     for

     film

     production

    ,

     and

     the

     city

     has

     hosted

     some

     of

     the

     world

    's

     most

     famous

     musical

    s

    .

     In

     addition

    ,

     it

     is

     a

     major

     center

     for

     global

     trade

     and

     diplomacy

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     very

     promising

     and

     has

     many

     potential

     trends

     that

     could

     shape

     the

     industry

     in

     the

     coming

     years

    .

     Some

     of

     the

     most

     likely

     future

     trends

     in

     AI

     include

    :
    


    1

    .

     Increased

     automation

    :

     AI

     is

     already

     increasingly

     being

     used

     in

     manufacturing

     and

     other

     industries

     to

     automate

     repetitive

     tasks

    ,

     which

     could

     lead

     to

     new

     job

     losses

    .

     However

    ,

     it

     is

     also

     possible

     that

     AI

     could

     be

     used

     to

     replace

     these

     jobs

    ,

     leading

     to

     a

     more

     automated

     and

     efficient

     future

    .
    


    2

    .

     Personal

    ization

     and

     customization

    :

     AI

     is

     increasingly

     being

     used

     to

     personalize

     and

     customize

     the

     experience

     of

     users

     with

     personalized

     recommendations

     and

     insights

    .

     This

     could

     lead

     to

     a

     more

     seamless

     and

     personalized

     experience

    ,

     but

     it

     could

     also

     lead

     to

     new

     types

     of

    



```python
llm.shutdown()
```
