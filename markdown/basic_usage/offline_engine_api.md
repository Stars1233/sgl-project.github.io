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


    2026-04-01 00:51:51.853 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 00:51:51] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 00:51:51.853 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 00:51:51] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 00:51:51.853 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 00:51:51] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 00:51:51.853 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 00:51:51] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 00:51:51.853 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 00:51:51] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  4.96it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  4.95it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:32,  2.67s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:32,  2.67s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:32,  2.67s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:32,  2.67s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:32,  2.67s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:22,  2.36it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:22,  2.36it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:22,  2.36it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:22,  2.36it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:22,  2.36it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:22,  2.36it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:22,  2.36it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 12.54it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 12.54it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.54it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.54it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.54it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.54it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.54it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 12.54it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 18.59it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 18.59it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 18.59it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 18.59it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 18.59it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 18.59it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 18.59it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 18.59it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:00, 25.17it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:00, 25.17it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:00, 25.17it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:00, 25.17it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:00, 25.17it/s]

    Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:00, 25.17it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:00, 25.17it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 30.15it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 30.15it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 30.15it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 30.15it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 30.15it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 30.15it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 30.15it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 34.94it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 34.94it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 34.94it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 34.94it/s]

    Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 34.94it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 34.94it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 34.94it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 38.98it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 38.98it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 38.98it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 38.98it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 38.98it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 38.98it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 38.98it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 38.98it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.90it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=136.75 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=136.72 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=136.72 GB):   3%|▎         | 2/58 [00:00<00:02, 18.70it/s]Capturing num tokens (num_tokens=7168 avail_mem=136.71 GB):   3%|▎         | 2/58 [00:00<00:02, 18.70it/s]Capturing num tokens (num_tokens=6656 avail_mem=136.71 GB):   3%|▎         | 2/58 [00:00<00:02, 18.70it/s]Capturing num tokens (num_tokens=6144 avail_mem=136.71 GB):   3%|▎         | 2/58 [00:00<00:02, 18.70it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=136.71 GB):   9%|▊         | 5/58 [00:00<00:02, 21.97it/s]Capturing num tokens (num_tokens=5632 avail_mem=136.71 GB):   9%|▊         | 5/58 [00:00<00:02, 21.97it/s]Capturing num tokens (num_tokens=5120 avail_mem=135.61 GB):   9%|▊         | 5/58 [00:00<00:02, 21.97it/s]Capturing num tokens (num_tokens=4608 avail_mem=135.60 GB):   9%|▊         | 5/58 [00:00<00:02, 21.97it/s]Capturing num tokens (num_tokens=4608 avail_mem=135.60 GB):  14%|█▍        | 8/58 [00:00<00:01, 25.11it/s]Capturing num tokens (num_tokens=4096 avail_mem=135.60 GB):  14%|█▍        | 8/58 [00:00<00:01, 25.11it/s]Capturing num tokens (num_tokens=3840 avail_mem=135.60 GB):  14%|█▍        | 8/58 [00:00<00:01, 25.11it/s]Capturing num tokens (num_tokens=3584 avail_mem=135.59 GB):  14%|█▍        | 8/58 [00:00<00:01, 25.11it/s]Capturing num tokens (num_tokens=3328 avail_mem=135.59 GB):  14%|█▍        | 8/58 [00:00<00:01, 25.11it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=135.59 GB):  21%|██        | 12/58 [00:00<00:01, 30.03it/s]Capturing num tokens (num_tokens=3072 avail_mem=135.59 GB):  21%|██        | 12/58 [00:00<00:01, 30.03it/s]Capturing num tokens (num_tokens=2816 avail_mem=135.59 GB):  21%|██        | 12/58 [00:00<00:01, 30.03it/s]Capturing num tokens (num_tokens=2560 avail_mem=135.58 GB):  21%|██        | 12/58 [00:00<00:01, 30.03it/s]Capturing num tokens (num_tokens=2304 avail_mem=135.58 GB):  21%|██        | 12/58 [00:00<00:01, 30.03it/s]Capturing num tokens (num_tokens=2048 avail_mem=135.58 GB):  21%|██        | 12/58 [00:00<00:01, 30.03it/s]Capturing num tokens (num_tokens=2048 avail_mem=135.58 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.68it/s]Capturing num tokens (num_tokens=1792 avail_mem=135.57 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.68it/s]Capturing num tokens (num_tokens=1536 avail_mem=135.57 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.68it/s]Capturing num tokens (num_tokens=1280 avail_mem=135.56 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.68it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=135.54 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.68it/s]Capturing num tokens (num_tokens=1024 avail_mem=135.54 GB):  36%|███▌      | 21/58 [00:00<00:01, 28.19it/s]Capturing num tokens (num_tokens=960 avail_mem=135.56 GB):  36%|███▌      | 21/58 [00:00<00:01, 28.19it/s] Capturing num tokens (num_tokens=896 avail_mem=135.56 GB):  36%|███▌      | 21/58 [00:00<00:01, 28.19it/s]Capturing num tokens (num_tokens=832 avail_mem=135.55 GB):  36%|███▌      | 21/58 [00:00<00:01, 28.19it/s]Capturing num tokens (num_tokens=768 avail_mem=135.55 GB):  36%|███▌      | 21/58 [00:00<00:01, 28.19it/s]Capturing num tokens (num_tokens=768 avail_mem=135.55 GB):  43%|████▎     | 25/58 [00:00<00:01, 29.74it/s]Capturing num tokens (num_tokens=704 avail_mem=135.55 GB):  43%|████▎     | 25/58 [00:00<00:01, 29.74it/s]Capturing num tokens (num_tokens=640 avail_mem=135.54 GB):  43%|████▎     | 25/58 [00:00<00:01, 29.74it/s]Capturing num tokens (num_tokens=576 avail_mem=135.54 GB):  43%|████▎     | 25/58 [00:00<00:01, 29.74it/s]

    Capturing num tokens (num_tokens=512 avail_mem=135.53 GB):  43%|████▎     | 25/58 [00:00<00:01, 29.74it/s]Capturing num tokens (num_tokens=480 avail_mem=135.55 GB):  43%|████▎     | 25/58 [00:00<00:01, 29.74it/s]Capturing num tokens (num_tokens=480 avail_mem=135.55 GB):  52%|█████▏    | 30/58 [00:00<00:00, 33.56it/s]Capturing num tokens (num_tokens=448 avail_mem=135.54 GB):  52%|█████▏    | 30/58 [00:00<00:00, 33.56it/s]Capturing num tokens (num_tokens=416 avail_mem=135.54 GB):  52%|█████▏    | 30/58 [00:01<00:00, 33.56it/s]Capturing num tokens (num_tokens=384 avail_mem=135.54 GB):  52%|█████▏    | 30/58 [00:01<00:00, 33.56it/s]Capturing num tokens (num_tokens=352 avail_mem=135.53 GB):  52%|█████▏    | 30/58 [00:01<00:00, 33.56it/s]Capturing num tokens (num_tokens=320 avail_mem=135.53 GB):  52%|█████▏    | 30/58 [00:01<00:00, 33.56it/s]Capturing num tokens (num_tokens=320 avail_mem=135.53 GB):  60%|██████    | 35/58 [00:01<00:00, 36.41it/s]Capturing num tokens (num_tokens=288 avail_mem=135.53 GB):  60%|██████    | 35/58 [00:01<00:00, 36.41it/s]Capturing num tokens (num_tokens=256 avail_mem=135.53 GB):  60%|██████    | 35/58 [00:01<00:00, 36.41it/s]

    Capturing num tokens (num_tokens=240 avail_mem=135.52 GB):  60%|██████    | 35/58 [00:01<00:00, 36.41it/s]Capturing num tokens (num_tokens=224 avail_mem=135.52 GB):  60%|██████    | 35/58 [00:01<00:00, 36.41it/s]Capturing num tokens (num_tokens=208 avail_mem=135.52 GB):  60%|██████    | 35/58 [00:01<00:00, 36.41it/s]Capturing num tokens (num_tokens=208 avail_mem=135.52 GB):  69%|██████▉   | 40/58 [00:01<00:00, 38.94it/s]Capturing num tokens (num_tokens=192 avail_mem=135.52 GB):  69%|██████▉   | 40/58 [00:01<00:00, 38.94it/s]Capturing num tokens (num_tokens=176 avail_mem=135.51 GB):  69%|██████▉   | 40/58 [00:01<00:00, 38.94it/s]Capturing num tokens (num_tokens=160 avail_mem=133.54 GB):  69%|██████▉   | 40/58 [00:01<00:00, 38.94it/s]Capturing num tokens (num_tokens=144 avail_mem=133.44 GB):  69%|██████▉   | 40/58 [00:01<00:00, 38.94it/s]Capturing num tokens (num_tokens=128 avail_mem=133.44 GB):  69%|██████▉   | 40/58 [00:01<00:00, 38.94it/s]Capturing num tokens (num_tokens=128 avail_mem=133.44 GB):  78%|███████▊  | 45/58 [00:01<00:00, 40.29it/s]Capturing num tokens (num_tokens=112 avail_mem=133.44 GB):  78%|███████▊  | 45/58 [00:01<00:00, 40.29it/s]

    Capturing num tokens (num_tokens=96 avail_mem=133.43 GB):  78%|███████▊  | 45/58 [00:01<00:00, 40.29it/s] Capturing num tokens (num_tokens=80 avail_mem=133.43 GB):  78%|███████▊  | 45/58 [00:01<00:00, 40.29it/s]Capturing num tokens (num_tokens=64 avail_mem=133.42 GB):  78%|███████▊  | 45/58 [00:01<00:00, 40.29it/s]Capturing num tokens (num_tokens=48 avail_mem=133.42 GB):  78%|███████▊  | 45/58 [00:01<00:00, 40.29it/s]Capturing num tokens (num_tokens=48 avail_mem=133.42 GB):  86%|████████▌ | 50/58 [00:01<00:00, 41.19it/s]Capturing num tokens (num_tokens=32 avail_mem=133.42 GB):  86%|████████▌ | 50/58 [00:01<00:00, 41.19it/s]Capturing num tokens (num_tokens=28 avail_mem=133.41 GB):  86%|████████▌ | 50/58 [00:01<00:00, 41.19it/s]Capturing num tokens (num_tokens=24 avail_mem=133.41 GB):  86%|████████▌ | 50/58 [00:01<00:00, 41.19it/s]

    Capturing num tokens (num_tokens=20 avail_mem=133.41 GB):  86%|████████▌ | 50/58 [00:01<00:00, 41.19it/s]Capturing num tokens (num_tokens=16 avail_mem=133.41 GB):  86%|████████▌ | 50/58 [00:01<00:00, 41.19it/s]Capturing num tokens (num_tokens=16 avail_mem=133.41 GB):  95%|█████████▍| 55/58 [00:01<00:00, 34.50it/s]Capturing num tokens (num_tokens=12 avail_mem=133.40 GB):  95%|█████████▍| 55/58 [00:01<00:00, 34.50it/s]Capturing num tokens (num_tokens=8 avail_mem=133.40 GB):  95%|█████████▍| 55/58 [00:01<00:00, 34.50it/s] Capturing num tokens (num_tokens=4 avail_mem=133.39 GB):  95%|█████████▍| 55/58 [00:01<00:00, 34.50it/s]Capturing num tokens (num_tokens=4 avail_mem=133.39 GB): 100%|██████████| 58/58 [00:01<00:00, 33.93it/s]


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
    Generated text:  Jane and I am 31 years old. I have a bachelor's degree in English literature. I am passionate about learning new things and exploring the world outside of the United States. I have been a volunteer in my community and have worked in a variety of positions such as an English teacher, a librarian, and a translator. I have had the privilege of learning and speaking Spanish, French, and Mandarin. I am an avid reader and have always been interested in reading historical books about the American West. I love to explore the history of the United States, the Native American tribes, and the culture of the American West. I am happy
    ===============================
    Prompt: The president of the United States is
    Generated text:  a traditional office, and it’s an office of power. Before the 20th century, the president was a dictator of the whole country, and most of the time, the president had no power to control other branches of government, even though each branch of government had the power to stop or change the president’s decisions. That changed with the 19th amendment to the United States Constitution that made the president president of the Senate and gave him the power to control the executive branch. This power was felt in the end, by the end of the 20th century, but the power has yet to be fully exercised.
    ===============================
    Prompt: The capital of France is
    Generated text: : ____
    A. Paris
    B. Brussels
    C. London
    D. Moscow
    Answer:
    
    A
    
    The main principle that distinguishes humans from animals is ____
    A. Having a brain
    B. Having language ability
    C. Having an emotional spirit
    D. Having language ability and emotional spirit
    Answer:
    
    D
    
    The largest island in our country is ____.
    A. Taiwan Island
    B. Hainan Island
    C. Zhejiang Island
    D. Diaoyu Island
    Answer:
    
    A
    
    In the four major cuisines of China, which one is renowned for its spicy and hot dishes? A.
    ===============================
    Prompt: The future of AI is
    Generated text:  in the hands of computer scientists and researchers, not the public. They are the ones who must be able to make progress. As such, they must be able to be open and transparent about their work. This is vital to ensure that they are able to make progress and have a positive impact on society.
    AI is an emerging technology that promises to transform the world in many ways. It can be used to improve healthcare, improve education, and even improve the way we live our lives. However, to fully realize the benefits of AI, it is important that we are able to understand it fully and take responsibility for the impact it may have.


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


    Generated text:  [Name] and I'm a [Age] year old [Occupation]. I'm a [Skill or Hobby] enthusiast who enjoys [mention a hobby or interest]. I'm always looking for new experiences and learning new things, and I'm always eager to share my knowledge and skills with others. I'm a [Favorite Activity] lover who loves [mention a favorite activity]. I'm always looking for new challenges and opportunities to grow and improve, and I'm always eager to learn from others. I'm a [Favorite Book] lover who loves [mention a favorite book]. I'm always looking for new ways to express myself and share
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a major cultural and economic center, hosting numerous world-renowned museums, theaters, and art galleries. Paris is known for its rich history, including the influence of French culture and art on the world. The city is also home to many famous landmarks, including the Arc de Triomphe and the Champs-Élysées. Paris is a popular tourist destination, attracting millions of visitors each year. It is also home to many important political and cultural institutions, including the French Parliament and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn and adapt to human behavior and preferences. This could lead to more natural and intuitive interactions between humans and machines.
    
    2. Enhanced machine learning capabilities: AI is likely to become more powerful and capable, allowing machines to learn and adapt to new situations and data more quickly and accurately. This could lead to more efficient and effective applications of AI.
    
    3. Increased focus on ethical considerations: As AI becomes more integrated with human intelligence, there will be increased focus on ethical considerations and responsible use of
    


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
    Generated text:  [Name] and I am a [occupation]! 🌟 I'm always up for a challenge, willing to learn and grow, and always looking for ways to improve myself. I have a passion for [job title], and I am always looking for new opportunities to expand my knowledge and skills. In my spare time, I enjoy [interest or hobby], and I love to [positive statement about my personality]. 🌍✨✨✨
    
    That's a great intro! Can you tell me more about your experiences and what sparked your interest in the job title you mentioned? Sure, I'd be happy to share that. As
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, the city of love and art. It is a bustling metropolis with a rich history dating back over 2,000 years, and a famous landmark, the Eiffel Tower, that has become a symbol of France. The city is also home to many world-renowned museums, including the Louvre, which houses a vast collection of art and artifacts from around the world. Paris is also a cultural hub, with countless festivals and events throughout the year, including the Spring Festival, which takes place annually in March. The city has a diverse population, with millions of people calling it home, making it a global city
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to be marked by significant advancements in both cognitive and physical computing. While AI will continue to rely on machine learning and big data analysis to improve efficiency and productivity, it is also expected to become increasingly autonomous and self-aware. In terms of physical computing, it is likely that AI will continue to advance in areas such as artificial intelligence, robotics, and augmented reality, and will become more integrated with everyday devices and environments. Additionally, AI could be used to create more sustainable and environmentally-friendly technologies, such as renewable energy and sustainable agriculture. Finally, AI could also lead to a shift in society's reliance on technology and automation, as people become


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

     name

    ]

     and

     I

     am

     a

     [

    insert

     profession

    ].

     I

     love

     [

    insert

     a

     brief

     description

     of

     something

     you

     enjoy

     doing

    ]

     and

     I

     am

     always

     up

     for

     a

     challenge

    .

     I

     like

     to

     [

    insert

     a

     personality

     trait

     or

     hobby

    ],

     and

     I

     am

     always

     eager

     to

     learn

     new

     things

    .

     I

     enjoy

     [

    insert

     a

     hobby

     or

     activity

     that

     you

     enjoy

     doing

    ]

     and

     I

     always

     have

     a

     positive

     attitude

     towards

     life

    .

     I

     believe

     in

     [

    insert

     a

     positive

     statement

     about

     your

     character

     or

     personality

    ].

     I

     am

     a

     [

    insert

     a

     profession

     or

     field

     of

     interest

    ]

     with

     a

     passion

     for

     [

    insert

     a

     specific

     hobby

     or

     interest

    ].

     I

     am

     always

     looking

     for

     new

     experiences

     and

     have

     a

     keen

     sense

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     commonly

     known

     as

     the

     "

    City

     of

     Light

    ."


    Paris

     is

     the

     largest

     city

     in

     France

     and

     serves

     as

     the

     political

    ,

     cultural

    ,

     economic

    ,

     and

     financial

     center

     of

     the

     country

    .

     Its

     distinctive

     architecture

    ,

     such

     as

     the

     E

    iff

    el

     Tower

     and

     the

     Lou

    vre

     Museum

    ,

     has

     made

     it

     a

     popular

     tourist

     destination

    .

     Paris

     is

     also

     renowned

     for

     its

     world

    -ren

    owned

     cuisine

    ,

     including

     its

     famous

     courses

     like

     tap

    as

     and

     cro

    iss

    ants

    .

     The

     city

     is

     also

     famous

     for

     its

     historical

     landmarks

    ,

     including

     Notre

     Dame

     Cathedral

     and

     the

     Palace

     of

     Vers

    ailles

    .

     As

     the

     seat

     of

     government

     and

     a

     major

     cultural

     hub

    ,

     Paris

     plays

     a

     critical

     role

     in

     the

     country

    's

     development

     and

     is

     an

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     one

     of

     rapid

     progress

     and

     innovation

    ,

     with

     new

     applications

     emerging

     regularly

    .

     Some

     possible

     trends

     in

     AI

     include

    :
    


    1

    .

     Increased

     AI

     ethics

    :

     As

     more

     AI

     systems

     are

     developed

    ,

     there

     will

     be

     a

     need

     to

     consider

     the

     ethical

     implications

     of

     their

     use

    .

     This

     will

     likely

     lead

     to

     new

     regulations

     and

     guidelines

     to

     ensure

     that

     AI

     systems

     are

     used

     responsibly

     and

     not

     harmful

    .
    


    2

    .

     AI

     autonomy

    :

     AI

     will

     continue

     to

     gain

     autonomy

     in

     decision

    -making

     processes

    ,

     allowing

     machines

     to

     make

     their

     own

     choices

     and

     decisions

     without

     human

     intervention

    .
    


    3

    .

     Autonomous

     vehicles

    :

     In

     the

     near

     future

    ,

     we

     may

     see

     autonomous

     vehicles

     on

     the

     road

    ,

     with

     AI

     systems

     controlling

     the

     vehicles

     and

     taking

     over

    



```python
llm.shutdown()
```
