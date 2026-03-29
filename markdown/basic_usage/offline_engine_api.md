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


    2026-03-29 07:44:11.182 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 07:44:11] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 07:44:11.182 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 07:44:11] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 07:44:11.182 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 07:44:11] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 07:44:11.182 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 07:44:11] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 07:44:11.182 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 07:44:11] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.52it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.51it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:19,  2.67it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:19,  2.67it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:19,  2.67it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:19,  2.67it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:19,  2.67it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:19,  2.67it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:19,  2.67it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:06,  6.94it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:06,  6.94it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:06,  6.94it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:06,  6.94it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:06,  6.94it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:06,  6.94it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:06,  6.94it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:06,  6.94it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:02<00:06,  6.94it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 13.89it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 13.89it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 13.89it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 13.89it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 13.89it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 13.89it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 13.89it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 13.89it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:02<00:01, 20.33it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:02<00:01, 20.33it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:02<00:01, 20.33it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:02<00:01, 20.33it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:02<00:01, 20.33it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:02<00:01, 20.33it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:02<00:01, 20.33it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:02<00:01, 20.33it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:02<00:00, 27.16it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:02<00:00, 27.16it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:02<00:00, 27.16it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:02<00:00, 27.16it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:02<00:00, 27.16it/s]

    Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:02<00:00, 27.16it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:02<00:00, 27.16it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 32.08it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 32.08it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 32.08it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 32.08it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 32.08it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 32.08it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 32.08it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 36.45it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 36.45it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 36.45it/s] 

    Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 36.45it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 36.45it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 36.45it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 36.45it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 39.67it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 39.67it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 39.67it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 39.67it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 39.67it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 39.67it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 39.67it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 39.67it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.41it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=116.38 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=116.38 GB):   2%|▏         | 1/58 [00:00<00:10,  5.26it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.96 GB):   2%|▏         | 1/58 [00:00<00:10,  5.26it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=118.96 GB):   2%|▏         | 1/58 [00:00<00:10,  5.26it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.95 GB):   2%|▏         | 1/58 [00:00<00:10,  5.26it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.95 GB):   7%|▋         | 4/58 [00:00<00:03, 13.66it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.96 GB):   7%|▋         | 4/58 [00:00<00:03, 13.66it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.95 GB):   7%|▋         | 4/58 [00:00<00:03, 13.66it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.95 GB):   7%|▋         | 4/58 [00:00<00:03, 13.66it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.95 GB):  12%|█▏        | 7/58 [00:00<00:02, 18.82it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.95 GB):  12%|█▏        | 7/58 [00:00<00:02, 18.82it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=118.95 GB):  12%|█▏        | 7/58 [00:00<00:02, 18.82it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.94 GB):  12%|█▏        | 7/58 [00:00<00:02, 18.82it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.94 GB):  12%|█▏        | 7/58 [00:00<00:02, 18.82it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.94 GB):  19%|█▉        | 11/58 [00:00<00:01, 24.88it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.94 GB):  19%|█▉        | 11/58 [00:00<00:01, 24.88it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.93 GB):  19%|█▉        | 11/58 [00:00<00:01, 24.88it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.93 GB):  19%|█▉        | 11/58 [00:00<00:01, 24.88it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.93 GB):  19%|█▉        | 11/58 [00:00<00:01, 24.88it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.92 GB):  19%|█▉        | 11/58 [00:00<00:01, 24.88it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.92 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.99it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.92 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.99it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=118.92 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.99it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.91 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.99it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.91 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.99it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.89 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.99it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.89 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.20it/s]Capturing num tokens (num_tokens=960 avail_mem=118.90 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.20it/s] Capturing num tokens (num_tokens=896 avail_mem=118.90 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.20it/s]Capturing num tokens (num_tokens=832 avail_mem=118.90 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.20it/s]Capturing num tokens (num_tokens=768 avail_mem=118.89 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.20it/s]Capturing num tokens (num_tokens=704 avail_mem=118.89 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.20it/s]

    Capturing num tokens (num_tokens=704 avail_mem=118.89 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.14it/s]Capturing num tokens (num_tokens=640 avail_mem=118.89 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.14it/s]Capturing num tokens (num_tokens=576 avail_mem=118.89 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.14it/s]Capturing num tokens (num_tokens=512 avail_mem=118.88 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.14it/s]Capturing num tokens (num_tokens=480 avail_mem=118.89 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.14it/s]Capturing num tokens (num_tokens=448 avail_mem=118.89 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.14it/s]Capturing num tokens (num_tokens=448 avail_mem=118.89 GB):  53%|█████▎    | 31/58 [00:01<00:00, 40.13it/s]Capturing num tokens (num_tokens=416 avail_mem=118.85 GB):  53%|█████▎    | 31/58 [00:01<00:00, 40.13it/s]Capturing num tokens (num_tokens=384 avail_mem=118.84 GB):  53%|█████▎    | 31/58 [00:01<00:00, 40.13it/s]Capturing num tokens (num_tokens=352 avail_mem=118.80 GB):  53%|█████▎    | 31/58 [00:01<00:00, 40.13it/s]Capturing num tokens (num_tokens=320 avail_mem=118.80 GB):  53%|█████▎    | 31/58 [00:01<00:00, 40.13it/s]

    Capturing num tokens (num_tokens=288 avail_mem=118.79 GB):  53%|█████▎    | 31/58 [00:01<00:00, 40.13it/s]Capturing num tokens (num_tokens=288 avail_mem=118.79 GB):  62%|██████▏   | 36/58 [00:01<00:00, 41.30it/s]Capturing num tokens (num_tokens=256 avail_mem=118.79 GB):  62%|██████▏   | 36/58 [00:01<00:00, 41.30it/s]Capturing num tokens (num_tokens=240 avail_mem=118.79 GB):  62%|██████▏   | 36/58 [00:01<00:00, 41.30it/s]Capturing num tokens (num_tokens=224 avail_mem=118.78 GB):  62%|██████▏   | 36/58 [00:01<00:00, 41.30it/s]Capturing num tokens (num_tokens=208 avail_mem=118.78 GB):  62%|██████▏   | 36/58 [00:01<00:00, 41.30it/s]Capturing num tokens (num_tokens=192 avail_mem=118.78 GB):  62%|██████▏   | 36/58 [00:01<00:00, 41.30it/s]Capturing num tokens (num_tokens=192 avail_mem=118.78 GB):  71%|███████   | 41/58 [00:01<00:00, 42.09it/s]Capturing num tokens (num_tokens=176 avail_mem=118.78 GB):  71%|███████   | 41/58 [00:01<00:00, 42.09it/s]Capturing num tokens (num_tokens=160 avail_mem=118.77 GB):  71%|███████   | 41/58 [00:01<00:00, 42.09it/s]Capturing num tokens (num_tokens=144 avail_mem=118.77 GB):  71%|███████   | 41/58 [00:01<00:00, 42.09it/s]

    Capturing num tokens (num_tokens=128 avail_mem=118.77 GB):  71%|███████   | 41/58 [00:01<00:00, 42.09it/s]Capturing num tokens (num_tokens=112 avail_mem=118.75 GB):  71%|███████   | 41/58 [00:01<00:00, 42.09it/s]Capturing num tokens (num_tokens=112 avail_mem=118.75 GB):  79%|███████▉  | 46/58 [00:01<00:00, 42.00it/s]Capturing num tokens (num_tokens=96 avail_mem=118.72 GB):  79%|███████▉  | 46/58 [00:01<00:00, 42.00it/s] Capturing num tokens (num_tokens=80 avail_mem=118.72 GB):  79%|███████▉  | 46/58 [00:01<00:00, 42.00it/s]Capturing num tokens (num_tokens=64 avail_mem=118.72 GB):  79%|███████▉  | 46/58 [00:01<00:00, 42.00it/s]Capturing num tokens (num_tokens=48 avail_mem=118.71 GB):  79%|███████▉  | 46/58 [00:01<00:00, 42.00it/s]Capturing num tokens (num_tokens=32 avail_mem=118.71 GB):  79%|███████▉  | 46/58 [00:01<00:00, 42.00it/s]

    Capturing num tokens (num_tokens=32 avail_mem=118.71 GB):  88%|████████▊ | 51/58 [00:01<00:00, 32.22it/s]Capturing num tokens (num_tokens=28 avail_mem=118.70 GB):  88%|████████▊ | 51/58 [00:01<00:00, 32.22it/s]Capturing num tokens (num_tokens=24 avail_mem=118.70 GB):  88%|████████▊ | 51/58 [00:01<00:00, 32.22it/s]Capturing num tokens (num_tokens=20 avail_mem=118.70 GB):  88%|████████▊ | 51/58 [00:01<00:00, 32.22it/s]Capturing num tokens (num_tokens=16 avail_mem=118.70 GB):  88%|████████▊ | 51/58 [00:01<00:00, 32.22it/s]Capturing num tokens (num_tokens=16 avail_mem=118.70 GB):  95%|█████████▍| 55/58 [00:01<00:00, 31.61it/s]Capturing num tokens (num_tokens=12 avail_mem=118.69 GB):  95%|█████████▍| 55/58 [00:01<00:00, 31.61it/s]Capturing num tokens (num_tokens=8 avail_mem=118.69 GB):  95%|█████████▍| 55/58 [00:01<00:00, 31.61it/s] Capturing num tokens (num_tokens=4 avail_mem=118.69 GB):  95%|█████████▍| 55/58 [00:01<00:00, 31.61it/s]

    Capturing num tokens (num_tokens=4 avail_mem=118.69 GB): 100%|██████████| 58/58 [00:01<00:00, 30.42it/s]


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
    Generated text:  Catherine and I am a volunteer at the Safe Place shelter. I have been working with the Shelter since 2013, and I have been a volunteer in the shelter since 2015. I love helping out in the shelter because it is a place where people can get shelter, food, clothes, and medical care. I also enjoy meeting new people, having fun in a quiet place, and getting to know people from all walks of life. I have been volunteering in the shelter for 7 years now, and I have loved it all. I am passionate about helping people who have problems in their lives. As a
    ===============================
    Prompt: The president of the United States is
    Generated text:  a monarch, in the sense of the word "monarch," and she has never, as of today, resigned as president. Despite this, the United States is currently led by a woman, and she is the current president. What is this woman?
    The woman in question is not a monarch; she is the current president of the United States, which is a democratic republic. As such, she has not resigned as president. The United States is currently led by a woman named Hillary Rodham Clinton, who served as the 42nd president from 2009 to 2017. She was the first woman
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. Paris is the capital of France, the capital of France is Paris, and Paris is the capital of France, and the capital of France is Paris. Based on this statement, the sentence that does not follow the logical sequence of these statements is (　　)
    A: The capital of France is Paris
    B: Paris is the capital of France
    C: Paris is the capital of France
    D: The capital of France is Paris
    To determine which sentence does not follow the logical sequence of the given statements, we need to carefully analyze the structure and meaning of each statement.
    
    The statements are:
    1. The capital of France
    ===============================
    Prompt: The future of AI is
    Generated text:  expanding beyond the realms of traditional computing to include a wide range of applications that are revolutionizing the way we live and work. Here are some exciting developments in AI that are set to shape the future of technology and society:
    1. Autonomous vehicles: As AI technology advances, autonomous vehicles have become more common in everyday life. Autonomous vehicles rely on sensors and AI algorithms to navigate roads and interact with other vehicles, pedestrians, and objects.
    2. Healthcare: AI is being used in the healthcare industry to improve patient outcomes and reduce errors in diagnosis and treatment. AI can analyze large amounts of medical data to identify patterns and predict disease development, leading to


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


    Generated text:  [Name] and I am a [occupation] who has been [number of years] in the industry. I am passionate about [reason for passion], and I am always looking for ways to [action or goal]. I am always eager to learn and grow, and I am always willing to help others. I am a [character trait or quality] and I am always ready to [action or goal]. I am a [character type] and I am always [character trait or quality]. I am a [character type] and I am always [character trait or quality]. I am a [character type] and I am always [
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light. It is a historic city with a rich history dating back to the Roman Empire and the Middle Ages. Paris is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. The city is also famous for its cuisine, fashion, and art, and is a major tourist destination. Paris is a city of contrasts, with its modern architecture and vibrant culture blending with its traditional French heritage. It is a city that has played a significant role in French history and continues to be a major cultural and economic center in the world. Paris is the
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we live, work, and interact with technology. Here are some possible future trends in AI:
    
    1. Increased automation: As AI becomes more advanced, it is likely to automate more and more tasks, freeing up human workers to focus on more complex and creative work. This could lead to a shift in the job market, with many jobs becoming automated and replaced by AI.
    
    2. Improved privacy and security: As AI becomes more sophisticated, it is likely to require more data to function effectively. This could lead to increased privacy and security concerns, as AI systems may
    


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
    Generated text:  [Your Name], and I am a [character type]!
    
    I am a loyal friend, a supportive partner, and a helpful ally. I am always here for you when you need me, and I always strive to be the best friend, partner, and ally to those who share my life. What kind of character are you? How can I best connect with you? 🤔
    
    ---
    
    **Friendly intro:** Hi there! How’s it going? I’m [Your Name], a loyal friend, supportive partner, and helpful ally. I’m here to help you feel better when you need it, to stay connected with you,
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, often referred to as the "City of Light." 
    
    To provide additional context, Paris is the largest city in France by area and population, and it is known for its rich history, art, and cuisine. The city is home to the Eiffel Tower, the Louvre Museum, and many other iconic landmarks, as well as its vibrant nightlife and fashion scene. Paris is also home to numerous museums and cultural institutions, including the Louvre, the Museum of Modern Art, and the Musée d'Orsay. Additionally, the city is a major financial and trade center, with many world-renowned companies and institutions located
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  full of potential, but here are some possible trends that could shape its direction:
    
    1. Autonomous and self-driving vehicles: This trend is already starting to see a significant increase in the use of autonomous and self-driving vehicles, with many companies and governments investing heavily in developing and implementing these technologies.
    
    2. Increased reliance on AI in healthcare: AI has already been used to improve healthcare outcomes in fields such as diagnosis and treatment planning. As AI becomes more sophisticated, it is likely to become more integrated into healthcare systems, with more doctors and healthcare professionals using AI to improve patient outcomes.
    
    3. AI in finance: AI has already been used to improve


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

     Profession

    ].

     I

    'm

     excited

     to

     meet

     you

     at

     the

     [

    Your

     Location

    ].

     Let

    's

     do

     this

    !

     What

    's

     your

     name

    ?

     And

    ,

     what

    's

     your

     profession

    ?

     [

    Your

     Name

    ]

     is

     a

     [

    Your

     Profession

    ]

     from

     [

    Your

     Location

    ].

     I

    'm

     an

     enthusiastic

     [

    Your

     Passion

    ],

     [

    Your

     N

    iche

    ],

     and

     I

     love

     to

     [

    Your

     Hobby

    ,

     Interest

    ,

     or

     Skill

    ].

     My

     favorite

     hobby

     is

     [

    Your

     Hobby

    ,

     Interest

    ,

     or

     Skill

    ].

     I

    'm

     always

     up

     for

     [

    Your

     Goal

    ,

     Challenge

    ,

     or

     Adventure

    ],

     and

     I

    'm

     committed

     to

     [

    Your

     Mission

    ,

     Vision

    ,

     or

     Cause

    ].

     I

    'm

     always

     ready

     to

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     a

     historical

     city

     known

     for

     its

     vibrant

     culture

    ,

     fine

     arts

    ,

     and

     historical

     landmarks

    .

     The

     city

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

     Notre

    -D

    ame

     Cathedral

    ,

     among

     other

     notable

     sites

    .

     Paris

     is

     also

     known

     for

     its

     rich

     culinary

     scene

     and

     fashion

     industry

    .

     It

     is

     the

     seat

     of

     the

     French

     government

     and

     one

     of

     the

     world

    's

     most

     visited

     cities

    ,

     hosting

     the

     World

     Cup

     and

     other

     major

     events

    .

     With

     its

     stunning

     architecture

     and

     cultural

     importance

    ,

     Paris

     continues

     to

     be

     a

     major

     hub

     of

     global

     interest

    .

     Paris

    ,

     the

     capital

     city

     of

     France

    ,

     is

     a

     city

     of

     beauty

    ,

     history

    ,

     and

     vibrant

     culture

    .

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     shaped

     by

     a

     combination

     of

     technological

     advancements

    ,

     regulatory

     developments

    ,

     and

     societal

     shifts

    .

     Here

     are

     some

     possible

     trends

     in

     AI

     in

     the

     next

     few

     years

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

     data

     is

     collected

     and

     analyzed

    ,

     there

     will

     be

     more

     opportunities

     for

     ethical

     AI

     that

     priorit

    izes

     fairness

    ,

     transparency

    ,

     and

     accountability

    .

     This

     means

     that

     AI

     systems

     will

     need

     to

     be

     designed

     and

     tested

     with

     care

     to

     ensure

     they

     don

    't

     perpet

    uate

     or

     exacerb

    ate

     existing

     biases

    .
    


    2

    .

     Automation

     of

     mundane

     tasks

    :

     One

     potential

     future

     trend

     is

     the

     increasing

     automation

     of

     mundane

     tasks

     that

     humans

     can

    't

     do

     well

    .

     This

     could

     include

     tasks

     like

     data

     entry

    ,

     administrative

     tasks

    ,

    



```python
llm.shutdown()
```
