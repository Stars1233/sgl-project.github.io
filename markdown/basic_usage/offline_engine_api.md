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


    2026-03-28 05:24:17.838 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 05:24:17] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 05:24:17.838 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 05:24:17] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 05:24:17.838 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 05:24:17] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 05:24:17.838 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 05:24:17] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 05:24:17.838 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 05:24:17] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  4.09it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  4.09it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:22,  2.50s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:22,  2.50s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:22,  2.50s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:22,  2.50s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:27,  1.99it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:27,  1.99it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:27,  1.99it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:27,  1.99it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:27,  1.99it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:27,  1.99it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:02<00:09,  5.40it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:02<00:09,  5.40it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:02<00:09,  5.40it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:02<00:09,  5.40it/s]Compiling num tokens (num_tokens=3072):  16%|█▌        | 9/58 [00:02<00:09,  5.40it/s]Compiling num tokens (num_tokens=2816):  16%|█▌        | 9/58 [00:02<00:09,  5.40it/s]Compiling num tokens (num_tokens=2560):  16%|█▌        | 9/58 [00:02<00:09,  5.40it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:02<00:04, 10.42it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:02<00:04, 10.42it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:02<00:04, 10.42it/s]Compiling num tokens (num_tokens=1792):  26%|██▌       | 15/58 [00:02<00:04, 10.42it/s]Compiling num tokens (num_tokens=1536):  26%|██▌       | 15/58 [00:02<00:04, 10.42it/s]Compiling num tokens (num_tokens=1280):  26%|██▌       | 15/58 [00:02<00:04, 10.42it/s]Compiling num tokens (num_tokens=1024):  26%|██▌       | 15/58 [00:02<00:04, 10.42it/s]Compiling num tokens (num_tokens=960):  26%|██▌       | 15/58 [00:02<00:04, 10.42it/s] 

    Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:02<00:02, 17.23it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:02<00:02, 17.23it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:02<00:02, 17.23it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:02<00:02, 17.23it/s]Compiling num tokens (num_tokens=704):  38%|███▊      | 22/58 [00:02<00:02, 17.23it/s]Compiling num tokens (num_tokens=640):  38%|███▊      | 22/58 [00:02<00:02, 17.23it/s]Compiling num tokens (num_tokens=576):  38%|███▊      | 22/58 [00:02<00:02, 17.23it/s]Compiling num tokens (num_tokens=512):  38%|███▊      | 22/58 [00:03<00:02, 17.23it/s]Compiling num tokens (num_tokens=480):  38%|███▊      | 22/58 [00:03<00:02, 17.23it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:03<00:01, 25.58it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:03<00:01, 25.58it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:03<00:01, 25.58it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:03<00:01, 25.58it/s]Compiling num tokens (num_tokens=352):  52%|█████▏    | 30/58 [00:03<00:01, 25.58it/s]Compiling num tokens (num_tokens=320):  52%|█████▏    | 30/58 [00:03<00:01, 25.58it/s]Compiling num tokens (num_tokens=288):  52%|█████▏    | 30/58 [00:03<00:01, 25.58it/s]Compiling num tokens (num_tokens=256):  52%|█████▏    | 30/58 [00:03<00:01, 25.58it/s]

    Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:03<00:00, 32.82it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:03<00:00, 32.82it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:03<00:00, 32.82it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:03<00:00, 32.82it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:03<00:00, 32.82it/s]Compiling num tokens (num_tokens=176):  64%|██████▍   | 37/58 [00:03<00:00, 32.82it/s]Compiling num tokens (num_tokens=160):  64%|██████▍   | 37/58 [00:03<00:00, 32.82it/s]Compiling num tokens (num_tokens=144):  64%|██████▍   | 37/58 [00:03<00:00, 32.82it/s]Compiling num tokens (num_tokens=128):  64%|██████▍   | 37/58 [00:03<00:00, 32.82it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 40.44it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 40.44it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 40.44it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 40.44it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 40.44it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 40.44it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 40.44it/s]

    Compiling num tokens (num_tokens=28):  78%|███████▊  | 45/58 [00:03<00:00, 40.44it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:03<00:00, 46.59it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:03<00:00, 46.59it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:03<00:00, 46.59it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:03<00:00, 46.59it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:03<00:00, 46.59it/s]Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:03<00:00, 46.59it/s] Compiling num tokens (num_tokens=4):  90%|████████▉ | 52/58 [00:03<00:00, 46.59it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.00it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=61.78 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=61.75 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=61.75 GB):   3%|▎         | 2/58 [00:00<00:02, 18.68it/s]Capturing num tokens (num_tokens=7168 avail_mem=61.75 GB):   3%|▎         | 2/58 [00:00<00:02, 18.68it/s]Capturing num tokens (num_tokens=6656 avail_mem=61.75 GB):   3%|▎         | 2/58 [00:00<00:02, 18.68it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=61.75 GB):   7%|▋         | 4/58 [00:00<00:03, 17.98it/s]Capturing num tokens (num_tokens=6144 avail_mem=61.75 GB):   7%|▋         | 4/58 [00:00<00:03, 17.98it/s]Capturing num tokens (num_tokens=5632 avail_mem=61.75 GB):   7%|▋         | 4/58 [00:00<00:03, 17.98it/s]Capturing num tokens (num_tokens=5632 avail_mem=61.75 GB):  10%|█         | 6/58 [00:00<00:03, 13.24it/s]Capturing num tokens (num_tokens=5120 avail_mem=61.75 GB):  10%|█         | 6/58 [00:00<00:03, 13.24it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=61.74 GB):  10%|█         | 6/58 [00:00<00:03, 13.24it/s]Capturing num tokens (num_tokens=4096 avail_mem=61.74 GB):  10%|█         | 6/58 [00:00<00:03, 13.24it/s]Capturing num tokens (num_tokens=3840 avail_mem=61.74 GB):  10%|█         | 6/58 [00:00<00:03, 13.24it/s]Capturing num tokens (num_tokens=3840 avail_mem=61.74 GB):  17%|█▋        | 10/58 [00:00<00:02, 20.19it/s]Capturing num tokens (num_tokens=3584 avail_mem=61.73 GB):  17%|█▋        | 10/58 [00:00<00:02, 20.19it/s]Capturing num tokens (num_tokens=3328 avail_mem=61.73 GB):  17%|█▋        | 10/58 [00:00<00:02, 20.19it/s]Capturing num tokens (num_tokens=3072 avail_mem=61.73 GB):  17%|█▋        | 10/58 [00:00<00:02, 20.19it/s]Capturing num tokens (num_tokens=2816 avail_mem=61.73 GB):  17%|█▋        | 10/58 [00:00<00:02, 20.19it/s]Capturing num tokens (num_tokens=2560 avail_mem=61.72 GB):  17%|█▋        | 10/58 [00:00<00:02, 20.19it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=61.72 GB):  26%|██▌       | 15/58 [00:00<00:01, 27.20it/s]Capturing num tokens (num_tokens=2304 avail_mem=61.72 GB):  26%|██▌       | 15/58 [00:00<00:01, 27.20it/s]Capturing num tokens (num_tokens=2048 avail_mem=61.72 GB):  26%|██▌       | 15/58 [00:00<00:01, 27.20it/s]Capturing num tokens (num_tokens=1792 avail_mem=61.71 GB):  26%|██▌       | 15/58 [00:00<00:01, 27.20it/s]Capturing num tokens (num_tokens=1536 avail_mem=61.71 GB):  26%|██▌       | 15/58 [00:00<00:01, 27.20it/s]Capturing num tokens (num_tokens=1280 avail_mem=61.70 GB):  26%|██▌       | 15/58 [00:00<00:01, 27.20it/s]Capturing num tokens (num_tokens=1280 avail_mem=61.70 GB):  34%|███▍      | 20/58 [00:00<00:01, 32.38it/s]Capturing num tokens (num_tokens=1024 avail_mem=61.68 GB):  34%|███▍      | 20/58 [00:00<00:01, 32.38it/s]Capturing num tokens (num_tokens=960 avail_mem=61.70 GB):  34%|███▍      | 20/58 [00:00<00:01, 32.38it/s] Capturing num tokens (num_tokens=896 avail_mem=61.70 GB):  34%|███▍      | 20/58 [00:00<00:01, 32.38it/s]Capturing num tokens (num_tokens=832 avail_mem=61.69 GB):  34%|███▍      | 20/58 [00:00<00:01, 32.38it/s]Capturing num tokens (num_tokens=768 avail_mem=61.69 GB):  34%|███▍      | 20/58 [00:00<00:01, 32.38it/s]

    Capturing num tokens (num_tokens=768 avail_mem=61.69 GB):  43%|████▎     | 25/58 [00:00<00:00, 36.94it/s]Capturing num tokens (num_tokens=704 avail_mem=61.69 GB):  43%|████▎     | 25/58 [00:00<00:00, 36.94it/s]Capturing num tokens (num_tokens=640 avail_mem=61.68 GB):  43%|████▎     | 25/58 [00:00<00:00, 36.94it/s]Capturing num tokens (num_tokens=576 avail_mem=61.68 GB):  43%|████▎     | 25/58 [00:00<00:00, 36.94it/s]Capturing num tokens (num_tokens=512 avail_mem=61.67 GB):  43%|████▎     | 25/58 [00:00<00:00, 36.94it/s]Capturing num tokens (num_tokens=480 avail_mem=61.69 GB):  43%|████▎     | 25/58 [00:00<00:00, 36.94it/s]Capturing num tokens (num_tokens=448 avail_mem=61.68 GB):  43%|████▎     | 25/58 [00:00<00:00, 36.94it/s]Capturing num tokens (num_tokens=448 avail_mem=61.68 GB):  53%|█████▎    | 31/58 [00:00<00:00, 41.39it/s]Capturing num tokens (num_tokens=416 avail_mem=61.68 GB):  53%|█████▎    | 31/58 [00:00<00:00, 41.39it/s]Capturing num tokens (num_tokens=384 avail_mem=61.68 GB):  53%|█████▎    | 31/58 [00:01<00:00, 41.39it/s]Capturing num tokens (num_tokens=352 avail_mem=61.67 GB):  53%|█████▎    | 31/58 [00:01<00:00, 41.39it/s]Capturing num tokens (num_tokens=320 avail_mem=61.67 GB):  53%|█████▎    | 31/58 [00:01<00:00, 41.39it/s]Capturing num tokens (num_tokens=288 avail_mem=61.67 GB):  53%|█████▎    | 31/58 [00:01<00:00, 41.39it/s]

    Capturing num tokens (num_tokens=256 avail_mem=61.66 GB):  53%|█████▎    | 31/58 [00:01<00:00, 41.39it/s]Capturing num tokens (num_tokens=256 avail_mem=61.66 GB):  64%|██████▍   | 37/58 [00:01<00:00, 44.73it/s]Capturing num tokens (num_tokens=240 avail_mem=61.66 GB):  64%|██████▍   | 37/58 [00:01<00:00, 44.73it/s]Capturing num tokens (num_tokens=224 avail_mem=61.66 GB):  64%|██████▍   | 37/58 [00:01<00:00, 44.73it/s]Capturing num tokens (num_tokens=208 avail_mem=61.66 GB):  64%|██████▍   | 37/58 [00:01<00:00, 44.73it/s]Capturing num tokens (num_tokens=192 avail_mem=61.66 GB):  64%|██████▍   | 37/58 [00:01<00:00, 44.73it/s]Capturing num tokens (num_tokens=176 avail_mem=61.65 GB):  64%|██████▍   | 37/58 [00:01<00:00, 44.73it/s]Capturing num tokens (num_tokens=160 avail_mem=61.65 GB):  64%|██████▍   | 37/58 [00:01<00:00, 44.73it/s]Capturing num tokens (num_tokens=160 avail_mem=61.65 GB):  74%|███████▍  | 43/58 [00:01<00:00, 47.20it/s]Capturing num tokens (num_tokens=144 avail_mem=61.64 GB):  74%|███████▍  | 43/58 [00:01<00:00, 47.20it/s]Capturing num tokens (num_tokens=128 avail_mem=61.64 GB):  74%|███████▍  | 43/58 [00:01<00:00, 47.20it/s]Capturing num tokens (num_tokens=112 avail_mem=61.64 GB):  74%|███████▍  | 43/58 [00:01<00:00, 47.20it/s]Capturing num tokens (num_tokens=96 avail_mem=61.64 GB):  74%|███████▍  | 43/58 [00:01<00:00, 47.20it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=61.63 GB):  74%|███████▍  | 43/58 [00:01<00:00, 47.20it/s]Capturing num tokens (num_tokens=64 avail_mem=61.63 GB):  74%|███████▍  | 43/58 [00:01<00:00, 47.20it/s]Capturing num tokens (num_tokens=64 avail_mem=61.63 GB):  84%|████████▍ | 49/58 [00:01<00:00, 48.43it/s]Capturing num tokens (num_tokens=48 avail_mem=61.63 GB):  84%|████████▍ | 49/58 [00:01<00:00, 48.43it/s]Capturing num tokens (num_tokens=32 avail_mem=61.62 GB):  84%|████████▍ | 49/58 [00:01<00:00, 48.43it/s]Capturing num tokens (num_tokens=28 avail_mem=61.62 GB):  84%|████████▍ | 49/58 [00:01<00:00, 48.43it/s]Capturing num tokens (num_tokens=24 avail_mem=61.61 GB):  84%|████████▍ | 49/58 [00:01<00:00, 48.43it/s]Capturing num tokens (num_tokens=20 avail_mem=61.61 GB):  84%|████████▍ | 49/58 [00:01<00:00, 48.43it/s]Capturing num tokens (num_tokens=20 avail_mem=61.61 GB):  93%|█████████▎| 54/58 [00:01<00:00, 48.61it/s]Capturing num tokens (num_tokens=16 avail_mem=61.61 GB):  93%|█████████▎| 54/58 [00:01<00:00, 48.61it/s]Capturing num tokens (num_tokens=12 avail_mem=61.60 GB):  93%|█████████▎| 54/58 [00:01<00:00, 48.61it/s]Capturing num tokens (num_tokens=8 avail_mem=61.60 GB):  93%|█████████▎| 54/58 [00:01<00:00, 48.61it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=61.60 GB):  93%|█████████▎| 54/58 [00:01<00:00, 48.61it/s]Capturing num tokens (num_tokens=4 avail_mem=61.60 GB): 100%|██████████| 58/58 [00:01<00:00, 38.11it/s]


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
    Generated text:  Rana. I am a software engineer and I have been working with TypeScript for the past three years. I have also been programming in JavaScript for a while, but I am a bit of a beginner. I have always had a strong interest in computer science, but I am still learning it. In the past, I have found that it can be challenging to learn JavaScript, but I have learned a lot from doing so. Can you recommend some books or online courses that can help me understand and improve my JavaScript skills? Additionally, I have noticed that there is a lot of information online that can be confusing and confusing, so I would like
    ===============================
    Prompt: The president of the United States is
    Generated text:  a member of a particular political party, but he is not necessarily a member of that party. Thus, the president cannot be a member of the Democratic Party. What can be concluded about the president's political affiliation?
    To determine the conclusion about the president's political affiliation, let's analyze the given statement step by step.
    
    1. The president is a member of a particular political party.
    2. The president is not necessarily a member of that party.
    
    The conclusion is that the president cannot be a member of the Democratic Party. From the statement, we know that the president is not a member of the Democratic Party. Therefore, the president must
    ===============================
    Prompt: The capital of France is
    Generated text:  _________. A: Paris B: Milan C: Moscow D: New Delhi
    
    To determine the capital of France, we need to review the options and identify which one correctly identifies Paris as the capital of France.
    
    Let's go through the options one by one:
    
    A: Paris - This is the correct capital of France.
    
    B: Milan - This is a city in Italy, not a capital of France.
    
    C: Moscow - This is a capital of Russia, not a capital of France.
    
    D: New Delhi - This is a capital of India, not a capital of France.
    
    Based on this reasoning, the correct answer is:
    
    A
    ===============================
    Prompt: The future of AI is
    Generated text:  looking very different from the future of today. To understand what we might be able to achieve with the power of AI in the future, we need to look at a number of different approaches.
    This article is part of a series on AI.
    The future of AI is definitely very different from today. Here we look at some of the approaches that the industry is likely to take to making AI more effective, and what that might look like for the future.
    To understand the future of AI, we need to look at several different approaches. They may not all be equally effective, and some will be even more disruptive than others. Let's look at


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


    Generated text:  [Name], and I'm a [Age] year old [Occupation]. I'm a [Type of Vehicle] with [Number] wheels. I have [Number] years of experience in [Field of Work]. I'm [Type of Vehicle] and I'm [Number] years old. I'm a [Type of Vehicle] with [Number] wheels. I have [Number] years of experience in [Field of Work]. I'm [Type of Vehicle] and I'm [Number] years old. I'm a [Type of Vehicle] with [Number] wheels. I have [Number] years of experience in [
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a major cultural and economic center, hosting numerous museums, theaters, and other attractions. Paris is a popular tourist destination and is known for its rich history, art, and cuisine. It is also home to many famous French artists and writers. The city is known for its fashion industry, with many famous fashion designers and boutiques. Paris is a vibrant and dynamic city with a rich cultural and historical heritage. The city is also known for its annual festivals and events, including the Eiff
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction. Here are some of the most likely trends:
    
    1. Increased focus on ethical considerations: As AI becomes more integrated into our daily lives, there will be a growing emphasis on ethical considerations. This will include issues such as bias, transparency, and accountability.
    
    2. Greater integration with human intelligence: AI will continue to become more integrated with human intelligence, allowing for more sophisticated and nuanced decision-making. This will involve the use of machine learning algorithms that can learn from and adapt to human behavior.
    
    3. Increased use of AI in healthcare: AI will continue
    


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
    Generated text:  [insert name] and I'm a [insert occupation] with an MBA in [insert field of study]. I've always been passionate about [insert personal interest], and I'm always eager to learn and grow in my field. I'm always looking for new challenges and opportunities to contribute to my field and help people. I love getting out of my comfort zone and trying new things. I enjoy working on projects that make a difference and I'm always looking for ways to improve my skills and knowledge. I'm excited to be a part of this team and contribute to its success. How can I get to know you better? Let me know
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. 
    
    (Note: As of 2023, Paris is the largest city in the European Union and the fifth-most populous city in the world.) 
    
    Paris has a rich cultural history and is known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, and the Palace of Versailles. The city is also home to the French Parliament, the World Trade Center, and numerous museums, including the Louvre and the Musée d'Orsay. In addition to its impressive architecture and art, Paris is known for its culinary culture, including its famous French cuisine and gastronomy.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to continue to expand rapidly, with new technologies and applications emerging at a rapid pace. Here are some possible future trends in artificial intelligence:
    
    1. Increased automation and integration: As AI technology continues to advance, we can expect more automation and integration into daily life. This could include more efficient use of resources, better healthcare outcomes, and more personalized experiences.
    
    2. Enhanced emotional intelligence: AI is expected to improve our ability to understand and respond to human emotions. This could lead to more empathetic and compassionate AI systems that can better understand the motivations of their users.
    
    3. AI-driven healthcare: AI is already being used in healthcare to diagnose


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

    Age

    ]

     year

     old

     [

    Gender

    ].

     I

    'm

     a

     [

    field

     of

     study

    ]

     with

     [

    related

     skills

    ],

     and

     I

     currently

     work

     [

    job

     title

    ].

     I

    've

     always

     been

     passionate

     about

     [

    personal

     interest

    /

    interest

    ],

     and

     I

     enjoy

     [

    reason

     why

     I

     like

     my

     current

     occupation

    ].

     I

     enjoy

     [

    reason

     why

     I

     like

     [

    job

     title

    /

    occupation

    ]],

     and

     I

    'm

     always

     willing

     to

     learn

     and

     grow

    .

     I

     believe

     in

     [

    why

     I

     believe

     in

     my

     own

     identity

    ].

     My

     [

    gl

    ue

    ]

     is

     my

     [

    personal

     values

    /

     beliefs

    ].

     I

    'm

     [

    any

     other

     relevant

     information

    ],

     and

     I

    'm

     always

     looking

     forward

     to

     [

    future

     goals

    ].

     Thank

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     It

     is

     a

     historical

     and

     cultural

     center

     known

     for

     its

     landmarks

     such

     as

     Notre

     Dame

     Cathedral

    ,

     the

     E

    iff

    el

     Tower

    ,

     and

     the

     Lou

    vre

     Museum

    .

     It

     is

     also

     an

     important

     economic

     and

     political

     hub

    .

     Paris

     is

     home

     to

     many

     important

     museums

    ,

     including

     the

     Lou

    vre

    ,

     the

     National

     Library

    ,

     and

     the

     Mus

    ée

     d

    '

    Or

    say

    .

     It

     is

     also

     known

     for

     its

     rich

     cuisine

     and

     French

     culture

    .

     Paris

     is

     a

     popular

     tourist

     destination

    ,

     with

     millions

     of

     visitors

     each

     year

    .

     The

     French

     people

     enjoy

     a

     friendly

     and

     welcoming

     atmosphere

     in

     the

     city

    ,

     and

     are

     known

     for

     their

     love

     of

     art

    ,

     music

    ,

     and

     literature

    .

     Paris

     is

     a

     city

     of

     contrasts

    ,

     with

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     expected

     to

     continue

     to

     evolve

     rapidly

    ,

     with

     several

     key

     trends

     set

     to

     shape

     the

     technology

    's

     direction

     in

     the

     years

     to

     come

    .

     Here

     are

     some

     potential

     future

     trends

    :
    


    1

    .

     Increased

     use

     of

     AI

     in

     healthcare

    :

     As

     AI

     becomes

     more

     accessible

    ,

     there

     is

     potential

     for

     it

     to

     have

     a

     significant

     impact

     on

     healthcare

    .

     AI

     can

     be

     used

     to

     diagnose

     diseases

    ,

     identify

     treatment

     options

    ,

     and

     improve

     patient

     outcomes

    .
    


    2

    .

     Integration

     with

     other

     technologies

    :

     AI

     will

     continue

     to

     integrate

     with

     other

     technologies

    ,

     such

     as

     machine

     learning

     and

     cloud

     computing

    ,

     to

     create

     more

     efficient

     and

     effective

     systems

    .
    


    3

    .

     Personal

    ization

    :

     AI

     will

     be

     used

     to

     provide

     more

     accurate

     and

     personalized

     recommendations

     to

     users

    ,

     such

    



```python
llm.shutdown()
```
