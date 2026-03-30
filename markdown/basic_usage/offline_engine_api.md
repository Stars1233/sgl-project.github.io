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


    2026-03-30 17:13:01.717 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 17:13:01] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 17:13:01.717 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 17:13:01] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 17:13:01.717 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 17:13:01] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 17:13:01.717 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 17:13:01] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 17:13:01.717 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 17:13:01] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.04it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.03it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:19,  2.67it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:19,  2.67it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:19,  2.67it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:19,  2.67it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:19,  2.67it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:19,  2.67it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:19,  2.67it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 13.80it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 13.80it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 13.80it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 13.80it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 13.80it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 13.80it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 13.80it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 13.80it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:02<00:01, 20.22it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:02<00:01, 20.22it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:02<00:01, 20.22it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:02<00:01, 20.22it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:02<00:01, 20.22it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:02<00:01, 20.22it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:02<00:01, 20.22it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:02<00:01, 20.22it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:02<00:00, 27.05it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:02<00:00, 27.05it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:02<00:00, 27.05it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:02<00:00, 27.05it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:02<00:00, 27.05it/s]

    Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:02<00:00, 27.05it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:00, 27.05it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 31.82it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 31.82it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 31.82it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 31.82it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 31.82it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 31.82it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 31.82it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 36.52it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 36.52it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 36.52it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 36.52it/s]

    Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 36.52it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 36.52it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 36.52it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 40.34it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 40.34it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 40.34it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 40.34it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 40.34it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 40.34it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 40.34it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 40.34it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.41it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.33 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.30 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:02, 18.78it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:02, 18.78it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:02, 18.78it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:02, 18.78it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 21.76it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.76it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.76it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.76it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.76it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.29 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.46it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.29 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.46it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.28 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.46it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.46it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=120.27 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.46it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.16it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.16it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.16it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.16it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.26 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.16it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.16it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  31%|███       | 18/58 [00:00<00:01, 32.22it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  31%|███       | 18/58 [00:00<00:01, 32.22it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.25 GB):  31%|███       | 18/58 [00:00<00:01, 32.22it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=120.23 GB):  31%|███       | 18/58 [00:00<00:01, 32.22it/s]Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  31%|███       | 18/58 [00:00<00:01, 32.22it/s] Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  38%|███▊      | 22/58 [00:00<00:01, 33.72it/s]Capturing num tokens (num_tokens=896 avail_mem=120.24 GB):  38%|███▊      | 22/58 [00:00<00:01, 33.72it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  38%|███▊      | 22/58 [00:00<00:01, 33.72it/s]

    Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  38%|███▊      | 22/58 [00:00<00:01, 33.72it/s]Capturing num tokens (num_tokens=704 avail_mem=120.23 GB):  38%|███▊      | 22/58 [00:00<00:01, 33.72it/s]Capturing num tokens (num_tokens=704 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:00<00:01, 27.91it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:00<00:01, 27.91it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:00<00:01, 27.91it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  45%|████▍     | 26/58 [00:00<00:01, 27.91it/s]Capturing num tokens (num_tokens=480 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:00<00:01, 27.91it/s]Capturing num tokens (num_tokens=448 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:01<00:01, 27.91it/s]Capturing num tokens (num_tokens=448 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:01<00:00, 32.36it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:01<00:00, 32.36it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:01<00:00, 32.36it/s]

    Capturing num tokens (num_tokens=352 avail_mem=120.22 GB):  53%|█████▎    | 31/58 [00:01<00:00, 32.36it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  53%|█████▎    | 31/58 [00:01<00:00, 32.36it/s]Capturing num tokens (num_tokens=288 avail_mem=120.21 GB):  53%|█████▎    | 31/58 [00:01<00:00, 32.36it/s]Capturing num tokens (num_tokens=288 avail_mem=120.21 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.66it/s]Capturing num tokens (num_tokens=256 avail_mem=120.21 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.66it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.66it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.66it/s]Capturing num tokens (num_tokens=208 avail_mem=120.20 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.66it/s]Capturing num tokens (num_tokens=192 avail_mem=120.20 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.66it/s]Capturing num tokens (num_tokens=192 avail_mem=120.20 GB):  71%|███████   | 41/58 [00:01<00:00, 38.34it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  71%|███████   | 41/58 [00:01<00:00, 38.34it/s]

    Capturing num tokens (num_tokens=160 avail_mem=120.19 GB):  71%|███████   | 41/58 [00:01<00:00, 38.34it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  71%|███████   | 41/58 [00:01<00:00, 38.34it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  71%|███████   | 41/58 [00:01<00:00, 38.34it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  71%|███████   | 41/58 [00:01<00:00, 38.34it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  79%|███████▉  | 46/58 [00:01<00:00, 39.86it/s]Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  79%|███████▉  | 46/58 [00:01<00:00, 39.86it/s] Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  79%|███████▉  | 46/58 [00:01<00:00, 39.86it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  79%|███████▉  | 46/58 [00:01<00:00, 39.86it/s]Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  79%|███████▉  | 46/58 [00:01<00:00, 39.86it/s]Capturing num tokens (num_tokens=32 avail_mem=120.21 GB):  79%|███████▉  | 46/58 [00:01<00:00, 39.86it/s]

    Capturing num tokens (num_tokens=32 avail_mem=120.21 GB):  88%|████████▊ | 51/58 [00:01<00:00, 40.26it/s]Capturing num tokens (num_tokens=28 avail_mem=120.20 GB):  88%|████████▊ | 51/58 [00:01<00:00, 40.26it/s]Capturing num tokens (num_tokens=24 avail_mem=120.20 GB):  88%|████████▊ | 51/58 [00:01<00:00, 40.26it/s]Capturing num tokens (num_tokens=20 avail_mem=118.97 GB):  88%|████████▊ | 51/58 [00:01<00:00, 40.26it/s]Capturing num tokens (num_tokens=16 avail_mem=118.88 GB):  88%|████████▊ | 51/58 [00:01<00:00, 40.26it/s]Capturing num tokens (num_tokens=12 avail_mem=118.87 GB):  88%|████████▊ | 51/58 [00:01<00:00, 40.26it/s]Capturing num tokens (num_tokens=12 avail_mem=118.87 GB):  97%|█████████▋| 56/58 [00:01<00:00, 41.77it/s]Capturing num tokens (num_tokens=8 avail_mem=118.87 GB):  97%|█████████▋| 56/58 [00:01<00:00, 41.77it/s] Capturing num tokens (num_tokens=4 avail_mem=118.87 GB):  97%|█████████▋| 56/58 [00:01<00:00, 41.77it/s]Capturing num tokens (num_tokens=4 avail_mem=118.87 GB): 100%|██████████| 58/58 [00:01<00:00, 35.44it/s]


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
    Generated text:  Xiao Xiong, and I am 15 years old. I am an only child, and I love reading and writing. I have a strong interest in English literature, and I am very good at it. I like to listen to classical music, such as Beethoven's symphonies, Chopin's nocturnes, and Bach's Brandenburg Concertos. I often visit the library to read and borrow books. I have several books on Chinese literature, English literature, and history. I love to write stories, and I am planning to publish my first novel. As for the future, I want to be an English
    ===============================
    Prompt: The president of the United States is
    Generated text:  a man. The president of the United States is a ______ (指示事项）.
    A. government official
    B. role model
    C. leader
    D. leader figure
    Answer:
    C
    
    Which of the following is NOT a primary function of a newspaper?
    A. Reporting
    B. Soliciting opinion
    C. Advertising
    D. Public relations
    Answer:
    C
    
    According to the passage, what does the "C" in "Every C person thinks that" indicate?
    A. The "C" person is a child.
    B. The "C" person is a child.
    C. The "C" person is a
    ===============================
    Prompt: The capital of France is
    Generated text: :
    A. Paris
    B. Brussels
    C. London
    D. Moscow
    Answer:
    
    A
    
    When conducting a financial audit, what is the core of the audit process? The audit team needs to deeply understand the company's internal control and business operations, conduct necessary testing, and ensure the completeness and accuracy of the financial statements.
    A. Information System Audit
    B. Cash Flow Analysis
    C. Detailed Testing
    D. Financial Audit
    Answer:
    
    C
    
    When performing a simple regression analysis, which of the following options must be included?
    A. Regression coefficient
    B. Sample size
    C. Sample mean
    D. Total sum
    ===============================
    Prompt: The future of AI is
    Generated text:  the same as the future of humans. Why is this so?
    
    A) Humans have made advancements in AI that surpass the current state of AI.
    
    B) Artificial Intelligence has a much greater impact on society than humans do.
    
    C) Humans have the ability to learn and adapt to new technologies, while AI has limitations and is incapable of learning.
    
    D) The two have nothing in common.
    
    What is the correct answer?
    To determine the correct answer, let's analyze each option step by step:
    
    A) Humans have made advancements in AI that surpass the current state of AI.
    - This statement is incorrect. While humans have made significant advancements in AI


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm a [job title] with [number of years] years of experience in [industry]. I'm a [job title] with [number of years] years of experience in [industry]. I'm a [job title] with [number of years] years of experience in [industry]. I'm a [job title] with [number of years] years of experience in [industry]. I'm a [job title] with [number of years] years of experience in [industry]. I'm a [job title] with [number of years
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as "La Ville Flottante" (floating city). It is the largest city in Europe and the third-largest city in the world by population. Paris is home to many famous landmarks such as the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral. The city is also known for its rich history, including the French Revolution and the French Revolution Monument. Paris is a cultural and economic center of France and a major tourist destination. It is home to many famous museums, theaters, and restaurants. The city is also known for its food culture, with many famous French dishes and restaurants. Paris
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: As AI becomes more advanced, it is likely to become more integrated with human intelligence, allowing it to learn and adapt to new situations more effectively. This could lead to more sophisticated and personalized AI systems that can better understand and respond to human needs.
    
    2. Greater use of AI in healthcare: AI is already being used in healthcare to improve diagnosis, treatment, and patient care. As AI becomes more advanced, it is likely to be used in even more areas, including personalized medicine, drug discovery, and patient monitoring.
    
    3. Increased use of AI in finance
    


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
    Generated text:  [Name] and I'm a/an [Occupation/Field]. I come from [Country/Region]. I live in [Location]. I'm a/an [Generational/Current/Ally] person. I'm passionate about [Something]. I've [Some Achievements], and I've [Some Personal Traits]. I'm a/an [Incredible Person]. What's your experience, and what do you want to be known for in the future? [Tell me about yourself in 5-7 sentences. ] [Tell me about yourself in 5-7 sentences.] I’m a passionate, self-motivated individual who
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    Paris, also known as the City of Love, is the capital and largest city of France. It is situated on the River Seine in the center of France and is a UNESCO World Heritage site. Paris is renowned for its architecture, art, and culture, including the iconic Eiffel Tower and its numerous museums and theaters. The city is also a center of global politics, economic power, and its cultural richness. Paris is a significant part of France's identity and is a bustling metropolis that attracts millions of visitors each year. It is also home to numerous artists, writers, and musicians, as well as the home to
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be shaped by a combination of technological advancements, shifts in societal values, and the emergence of new applications. Here are some potential trends to look for:
    
    1. Increased reliance on AI: As AI becomes more sophisticated and widely adopted, there will be an increasing demand for its application in various industries. This may result in a shift away from traditional human labor and towards more automated and robotic solutions.
    
    2. Greater emphasis on ethical considerations: As AI becomes more integrated into our daily lives, there will be a growing awareness of its potential to perpetuate inequality and harm. As a result, there will be increased emphasis on ethical considerations and best


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

     am

     a

     [

    Role

     or

     title

    ]

     at

     [

    Company

    ].

     I

     am

     [

    Age

    ]

     years

     old

    ,

     and

     I

     have

     [

    Number

     of

     years

     of

     experience

    ]

     years

     of

     experience

     in

     [

    specific

     field

     or

     area

     of

     expertise

    ].

     Currently

    ,

     I

     am

     [

    Career

     stage

    ]

     at

     [

    Company

    ],

     and

     I

     am

     [

    Role

     or

     title

    ]

     at

     [

    Role

     and

     company

    ].

     I

     have

     always

     been

     [

    some

     characteristic

     or

     trait

    ]

     and

     I

     strive

     to

     be

     [

    character

     trait

    ].

     I

     am

     a

     [

    gender

    ]

     and

     I

     am

     [

    gender

     identity

    ]

     and

     I

     believe

     in

     [

    personal

     belief

     or

     belief

    ].

     I

     am

     [

    job

     title

     or

     role

    ]

     and

     I

     am

     always

     [

    character

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     a

     historic

     city

     located

     on

     the

     Se

    ine

     River

    ,

     known

     for

     its

     stunning

     architecture

    ,

     rich

     history

    ,

     and

     vibrant

     culture

    .

     It

     is

     also

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

     Paris

     has

     many

     world

    -class

     museums

    ,

     museums

    ,

     and

     theaters

    ,

     as

     well

     as

     famous

     landmarks

     such

     as

     Notre

    -D

    ame

     Cathedral

    ,

     Lou

    vre

     Museum

    ,

     and

     E

    iff

    el

     Tower

    .

     The

     city

     is

     also

     home

     to

     some

     of

     the

     world

    's

     most

     renowned

     art

     institutions

    ,

     including

     the

     Museum

     of

     Modern

     Art

     and

     the

     Centre

     Pom

    pid

    ou

    .

     With

     its

     rich

     history

     and

     unique

     culture

    ,

     Paris

     remains

     a

     fascinating

     and

     ever

    -ev

    olving

     destination

     for

     both

     tourists

     and

     locals

     alike

    .

     Can

     you

     provide

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     characterized

     by

     rapid

     technological

     progress

    ,

     increased

     integration

     with

     other

     fields

     of

     technology

    ,

     and

     growing

     attention

     to

     ethical

     and

     social

     implications

    .
    


    One

     of

     the

     primary

     trends

     in

     AI

     is

     the

     increasing

     integration

     of

     AI

     with

     other

     fields

     of

     technology

    ,

     such

     as

     machine

     learning

    ,

     natural

     language

     processing

    ,

     and

     robotics

    .

     As

     these

     fields

     continue

     to

     evolve

    ,

     they

     will

     become

     more

     integrated

     with

     AI

    ,

     leading

     to

     new

     applications

     and

     capabilities

     that

     can

     be

     lever

    aged

     across

     multiple

     industries

    .
    


    In

     addition

     to

     this

    ,

     the

     development

     of

     new

     AI

     technologies

     will

     continue

     to

     be

     driven

     by

     advances

     in

     quantum

     computing

     and

     other

     emerging

     technologies

    .

     This

     will

     likely

     lead

     to

     more

     powerful

     and

     efficient

     AI

     systems

    ,

     as

     well

     as

    



```python
llm.shutdown()
```
