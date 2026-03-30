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


    2026-03-30 19:56:32.938 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 19:56:32] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 19:56:32.938 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 19:56:32] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 19:56:32.938 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 19:56:32] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 19:56:32.938 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 19:56:32] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 19:56:32.938 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 19:56:32] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.10it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.09it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:05,  2.20s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:05,  2.20s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:05,  2.20s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:05,  2.20s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:24,  2.24it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:24,  2.24it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:24,  2.24it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:24,  2.24it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:24,  2.24it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:24,  2.24it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:24,  2.24it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.79it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.79it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.79it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.79it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.79it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.79it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.79it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.79it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.79it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:02<00:07,  6.79it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 15.05it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 15.05it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 15.05it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 15.05it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 15.05it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 15.05it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 15.05it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 15.05it/s]Compiling num tokens (num_tokens=640):  33%|███▎      | 19/58 [00:02<00:02, 15.05it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:02<00:01, 22.95it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:02<00:01, 22.95it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:02<00:01, 22.95it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:02<00:01, 22.95it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:02<00:01, 22.95it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:02<00:01, 22.95it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:02<00:01, 22.95it/s]Compiling num tokens (num_tokens=352):  47%|████▋     | 27/58 [00:02<00:01, 22.95it/s]Compiling num tokens (num_tokens=320):  47%|████▋     | 27/58 [00:02<00:01, 22.95it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:02<00:00, 31.09it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:02<00:00, 31.09it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:02<00:00, 31.09it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:02<00:00, 31.09it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:02<00:00, 31.09it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:02<00:00, 31.09it/s]

    Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:02<00:00, 31.09it/s]Compiling num tokens (num_tokens=176):  60%|██████    | 35/58 [00:02<00:00, 31.09it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:02<00:00, 37.81it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:02<00:00, 37.81it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:02<00:00, 37.81it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:02<00:00, 37.81it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:02<00:00, 37.81it/s]Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:02<00:00, 37.81it/s] Compiling num tokens (num_tokens=80):  72%|███████▏  | 42/58 [00:02<00:00, 37.81it/s]Compiling num tokens (num_tokens=64):  72%|███████▏  | 42/58 [00:02<00:00, 37.81it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:02<00:00, 43.17it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:02<00:00, 43.17it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:02<00:00, 43.17it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:03<00:00, 43.17it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:03<00:00, 43.17it/s]

    Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:03<00:00, 43.17it/s]Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:03<00:00, 43.17it/s]Compiling num tokens (num_tokens=12):  84%|████████▍ | 49/58 [00:03<00:00, 43.17it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:03<00:00, 48.18it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:03<00:00, 48.18it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:03<00:00, 48.18it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 18.81it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=76.12 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.09 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.09 GB):   3%|▎         | 2/58 [00:00<00:02, 18.91it/s]Capturing num tokens (num_tokens=7168 avail_mem=76.09 GB):   3%|▎         | 2/58 [00:00<00:02, 18.91it/s]Capturing num tokens (num_tokens=6656 avail_mem=76.09 GB):   3%|▎         | 2/58 [00:00<00:02, 18.91it/s]Capturing num tokens (num_tokens=6144 avail_mem=76.09 GB):   3%|▎         | 2/58 [00:00<00:02, 18.91it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=76.09 GB):   9%|▊         | 5/58 [00:00<00:02, 22.42it/s]Capturing num tokens (num_tokens=5632 avail_mem=76.08 GB):   9%|▊         | 5/58 [00:00<00:02, 22.42it/s]Capturing num tokens (num_tokens=5120 avail_mem=76.09 GB):   9%|▊         | 5/58 [00:00<00:02, 22.42it/s]Capturing num tokens (num_tokens=4608 avail_mem=76.08 GB):   9%|▊         | 5/58 [00:00<00:02, 22.42it/s]Capturing num tokens (num_tokens=4096 avail_mem=76.08 GB):   9%|▊         | 5/58 [00:00<00:02, 22.42it/s]Capturing num tokens (num_tokens=4096 avail_mem=76.08 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.55it/s]Capturing num tokens (num_tokens=3840 avail_mem=76.08 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.55it/s]Capturing num tokens (num_tokens=3584 avail_mem=76.07 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.55it/s]Capturing num tokens (num_tokens=3328 avail_mem=76.07 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.55it/s]Capturing num tokens (num_tokens=3072 avail_mem=76.07 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.55it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=76.07 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.55it/s]Capturing num tokens (num_tokens=2816 avail_mem=76.07 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.72it/s]Capturing num tokens (num_tokens=2560 avail_mem=76.06 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.72it/s]Capturing num tokens (num_tokens=2304 avail_mem=76.06 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.72it/s]Capturing num tokens (num_tokens=2048 avail_mem=76.06 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.72it/s]Capturing num tokens (num_tokens=1792 avail_mem=76.05 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.72it/s]Capturing num tokens (num_tokens=1536 avail_mem=76.05 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.72it/s]Capturing num tokens (num_tokens=1280 avail_mem=76.04 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.72it/s]Capturing num tokens (num_tokens=1280 avail_mem=76.04 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.04it/s]Capturing num tokens (num_tokens=1024 avail_mem=76.02 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.04it/s]Capturing num tokens (num_tokens=960 avail_mem=76.04 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.04it/s] Capturing num tokens (num_tokens=896 avail_mem=76.04 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.04it/s]Capturing num tokens (num_tokens=832 avail_mem=76.03 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.04it/s]

    Capturing num tokens (num_tokens=768 avail_mem=76.03 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.04it/s]Capturing num tokens (num_tokens=704 avail_mem=76.03 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.04it/s]Capturing num tokens (num_tokens=704 avail_mem=76.03 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.40it/s]Capturing num tokens (num_tokens=640 avail_mem=76.02 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.40it/s]Capturing num tokens (num_tokens=576 avail_mem=76.02 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.40it/s]Capturing num tokens (num_tokens=512 avail_mem=76.01 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.40it/s]Capturing num tokens (num_tokens=480 avail_mem=76.03 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.40it/s]Capturing num tokens (num_tokens=448 avail_mem=76.02 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.40it/s]Capturing num tokens (num_tokens=416 avail_mem=76.02 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.40it/s]Capturing num tokens (num_tokens=416 avail_mem=76.02 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.38it/s]Capturing num tokens (num_tokens=384 avail_mem=76.02 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.38it/s]Capturing num tokens (num_tokens=352 avail_mem=76.01 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.38it/s]Capturing num tokens (num_tokens=320 avail_mem=76.01 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.38it/s]

    Capturing num tokens (num_tokens=288 avail_mem=76.01 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.38it/s]Capturing num tokens (num_tokens=256 avail_mem=76.00 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.38it/s]Capturing num tokens (num_tokens=240 avail_mem=76.00 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.38it/s]Capturing num tokens (num_tokens=240 avail_mem=76.00 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.37it/s]Capturing num tokens (num_tokens=224 avail_mem=76.00 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.37it/s]Capturing num tokens (num_tokens=208 avail_mem=76.00 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.37it/s]Capturing num tokens (num_tokens=192 avail_mem=76.00 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.37it/s]Capturing num tokens (num_tokens=176 avail_mem=75.99 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.37it/s]Capturing num tokens (num_tokens=160 avail_mem=75.99 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.37it/s]Capturing num tokens (num_tokens=144 avail_mem=75.98 GB):  66%|██████▌   | 38/58 [00:01<00:00, 49.37it/s]Capturing num tokens (num_tokens=144 avail_mem=75.98 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.73it/s]Capturing num tokens (num_tokens=128 avail_mem=75.98 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.73it/s]Capturing num tokens (num_tokens=112 avail_mem=75.98 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.73it/s]

    Capturing num tokens (num_tokens=96 avail_mem=75.98 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.73it/s] Capturing num tokens (num_tokens=80 avail_mem=75.97 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.73it/s]Capturing num tokens (num_tokens=64 avail_mem=75.97 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.73it/s]Capturing num tokens (num_tokens=48 avail_mem=75.97 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.73it/s]Capturing num tokens (num_tokens=48 avail_mem=75.97 GB):  86%|████████▌ | 50/58 [00:01<00:00, 51.36it/s]Capturing num tokens (num_tokens=32 avail_mem=75.96 GB):  86%|████████▌ | 50/58 [00:01<00:00, 51.36it/s]Capturing num tokens (num_tokens=28 avail_mem=75.96 GB):  86%|████████▌ | 50/58 [00:01<00:00, 51.36it/s]Capturing num tokens (num_tokens=24 avail_mem=75.95 GB):  86%|████████▌ | 50/58 [00:01<00:00, 51.36it/s]Capturing num tokens (num_tokens=20 avail_mem=75.95 GB):  86%|████████▌ | 50/58 [00:01<00:00, 51.36it/s]Capturing num tokens (num_tokens=16 avail_mem=75.95 GB):  86%|████████▌ | 50/58 [00:01<00:00, 51.36it/s]Capturing num tokens (num_tokens=12 avail_mem=75.94 GB):  86%|████████▌ | 50/58 [00:01<00:00, 51.36it/s]Capturing num tokens (num_tokens=12 avail_mem=75.94 GB):  97%|█████████▋| 56/58 [00:01<00:00, 52.07it/s]Capturing num tokens (num_tokens=8 avail_mem=75.94 GB):  97%|█████████▋| 56/58 [00:01<00:00, 52.07it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=75.94 GB):  97%|█████████▋| 56/58 [00:01<00:00, 52.07it/s]Capturing num tokens (num_tokens=4 avail_mem=75.94 GB): 100%|██████████| 58/58 [00:01<00:00, 44.93it/s]


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
    Generated text:  Marjorie K. Johnson and I was 45 years old when I was diagnosed with cancer. The battle for me was a long and difficult one. My first diagnosis was in 1984, and I was told that I would never live to see the day that I could be cured. I was so sad that I even got depressed.
    When I was diagnosed with cancer in 1984, I was a young 45 year old and my husband, Arthur, was on his way to being diagnosed with lung cancer. The only cancer in the family that I was aware of was my mother. I had
    ===============================
    Prompt: The president of the United States is
    Generated text:  a very important person, as he or she represents the country. As the leader of the country, the president has the power to veto legislation. The veto power is a powerful tool in a democracy. As the president of the United States, the first thing he or she should do is to get the vote from the people, and if they say they do not want a veto, the president should make a decision on whether or not to veto a bill. The president can veto a bill passed by Congress, but he or she cannot veto a bill passed by the Senate. The president can veto a bill passed by the House of Representatives, but
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, located in the heart of the city center. It is also the main political, cultural, financial, and economic center of the country.
    The city has a population of about 2.4 million, 40% of whom live in the city center. The city has a population of 3.2 million. The main urban area, the city center, is at the heart of the city, with many of the country’s major industries, a diverse shopping and entertainment area, and the nation’s capital. The city’s wealth and culture are reflected in the city’s buildings and streets, which are built of stone, brick
    ===============================
    Prompt: The future of AI is
    Generated text:  not bright
    
    A blueprint for a better world
    Originally published on The Atlantic August 14, 2019
    
    One of the most common ways to think about AI is in terms of the future, a future where humanity would be in control. As a result, most AI research is focused on creating things that can make the world a better place.
    
    But the reality is far different.
    
    In fact, by concentrating on these things that can make the world a better place, we run the risk of creating the very thing that makes things better — a world of unprecedented complexity.
    
    The future of AI is not bright. It is the


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French National Museum, and the French Quarter, a historic neighborhood. Paris is a cultural and historical center with a rich history dating back to the Roman Empire and the French Revolution. It is a major economic and financial center, with many international companies and institutions headquartered there. The city is also known for its fashion industry, with Paris Fashion Week being one of the largest in the world. Paris is a popular tourist destination, with millions of visitors each year.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn from and adapt to human behavior and decision-making processes.
    
    2. Enhanced natural language processing: AI will continue to improve its ability to understand and interpret human language, leading to more natural and intuitive interactions with machines.
    
    3. Greater emphasis on ethical considerations: As AI becomes more integrated into our daily lives, there will be a greater emphasis on ethical considerations and responsible development of AI systems.
    
    4. Increased use of AI in healthcare: AI will be used to improve the accuracy and efficiency of
    


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
    Generated text:  [Your Name]. I am a [Your Profession or Degree] who has been working in the [Your Industry or Field] for [Your Career Stage]. My [Your Key Skill or Contribution] has been [Your Achievement or Significant Contribution]. If you would like to know more about my background, please let me know. You have [Your Age], [Your Gender], and [Your Nationality]. How would you like to be introduced to you? [Your Name]. [Your Age], [Your Gender], and [Your Nationality]. How would you like to be introduced to you? [Your Name]. [Your Age], [Your
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. 
    
    France's capital, Paris, is renowned for its historical landmarks, including the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. The city is known for its vibrant music scene, food culture, and fashion industry, as well as its role in the French Revolution and the Bastille threat. Paris is also home to numerous museums and art galleries, including the Louvre, Musée de la Science et des Arts et les Musées de l'Écologie, and the Musée d'Orsay. 
    
    In addition, Paris is a major hub for the global French-speaking community, hosting major international
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to be characterized by significant progress and rapid development, driven by advances in computing power, big data analytics, and machine learning. Some possible future trends in AI include:
    
    1. Increased use of AI in healthcare: AI is being used to improve patient outcomes, reduce costs, and enhance the quality of care. The use of AI in healthcare can include the development of more accurate medical diagnoses, personalized treatment plans, and the use of AI to analyze patient data for early detection of disease.
    
    2. Advancements in natural language processing: AI is being used to improve the accuracy of language processing and to develop language understanding technologies that can help automate language


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

    'm

     an

     AI

     language

     model

    .

     I

    'm

     here

     to

     assist

     you

     with

     any

     questions

     or

     tasks

     you

     may

     have

    .

     How

     can

     I

     help

     you

     today

    ?

     Remember

    ,

     I

    'm

     here

     for

     you

    ,

     and

     you

     can

     always

     count

     on

     me

     to

     be

     there

     for

     you

    .

     Let

     me

     know

     if

     you

     have

     any

     specific

     questions

     or

     if

     you

     need

     help

     with

     anything

    .

     I

    'm

     always

     here

     to

     help

    !

     

    🖼

    ️

    📚

    🔍

    


    Your

     Name

    :

     [

    Your

     Name

    ]


    Your

     Contact

     Information

    :

     [

    Insert

     Contact

     Information

    ]


    Your

     Website

     URL

    :

     [

    Insert

     Website

     URL

    ]


    Your

     Profile

     Link

    :

     [

    Insert

     Profile

     Link

    ]


    Your

     Social

     Media

     Handles

    :

     [

    Insert

     Social

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     
    


    I

     apologize

    ,

     but

     I

     can

    't

     provide

     a

     factual

     statement

     about

     France

    's

     capital

     city

     because

     Paris

     is

     not

     its

     capital

    .

     The

     capital

     of

     France

     is

     Paris

    ,

     and

     it

     is

     located

     in

     the

     region

     of

     Haut

    s

    -de

    -F

    rance

    .

     However

    ,

     I

     can

     provide

     some

     information

     about

     Paris

    :
    


    1

    .

     Known

     as

     "

    City

     of

     Love

    "

     due

     to

     its

     romantic

     atmosphere

     and

     beautiful

     gardens

    .


    2

    .

     Home

     to

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


    3

    .

     Famous

     for

     its

     iconic

     E

    iff

    el

     Tower

    ,

     which

     stands

     

    3

    2

    4

     feet

     tall

    .


    4

    .

     Home

     to

     many

     museums

     and

     art

     galleries

    ,

     including

     the

     Lou

    vre

     and

     Mus

    ée

     d

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     characterized

     by

     rapid

     progress

     in

     areas

     such

     as

     machine

     learning

    ,

     natural

     language

     processing

    ,

     computer

     vision

    ,

     and

     robotics

    .

     AI

     systems

     are

     likely

     to

     become

     increasingly

     capable

     of

     performing

     a

     wide

     range

     of

     tasks

    ,

     from

     autonomous

     driving

     to

     personalized

     customer

     service

     to

     the

     design

     of

     new

     materials

    .

     AI

     will

     also

     be

     increasingly

     integrated

     into

     everyday

     life

    ,

     from

     smart

     home

     devices

     to

     virtual

     assistants

     to

     virtual

     reality

     experiences

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

     increasing

     importance

     of

     AI

     ethics

     and

     safety

    .

     As

     AI

     systems

     become

     more

     sophisticated

     and

     involved

     in

     decision

    -making

    ,

     they

     will

     be

     required

     to

     navigate

     complex

     ethical

     dile

    mmas

    ,

     such

     as

     the

     deployment

     of

     AI

     in

     vulnerable

     populations

    .

     This

     will

    



```python
llm.shutdown()
```
