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


    2026-03-28 18:48:27.334 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 18:48:27] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 18:48:27.334 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 18:48:27] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 18:48:27.334 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 18:48:27] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 18:48:27.334 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 18:48:27] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 18:48:27.334 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 18:48:27] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.62it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.61it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:08,  2.26s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:08,  2.26s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:08,  2.26s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:08,  2.26s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:24,  2.19it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:24,  2.19it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:24,  2.19it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:24,  2.19it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:24,  2.19it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:24,  2.19it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:24,  2.19it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.62it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.62it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.62it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.62it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.62it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.62it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.62it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.62it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.62it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:02<00:07,  6.62it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 14.68it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 14.68it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 14.68it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 14.68it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 14.68it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 14.68it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 14.68it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 14.68it/s]Compiling num tokens (num_tokens=640):  33%|███▎      | 19/58 [00:02<00:02, 14.68it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:02<00:01, 22.39it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:02<00:01, 22.39it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:02<00:01, 22.39it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:02<00:01, 22.39it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:02<00:01, 22.39it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:02<00:01, 22.39it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:02<00:01, 22.39it/s]Compiling num tokens (num_tokens=352):  47%|████▋     | 27/58 [00:02<00:01, 22.39it/s]Compiling num tokens (num_tokens=320):  47%|████▋     | 27/58 [00:02<00:01, 22.39it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:02<00:00, 30.35it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:02<00:00, 30.35it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:02<00:00, 30.35it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:02<00:00, 30.35it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:02<00:00, 30.35it/s]

    Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:02<00:00, 30.35it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:02<00:00, 30.35it/s]Compiling num tokens (num_tokens=176):  60%|██████    | 35/58 [00:02<00:00, 30.35it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:02<00:00, 36.75it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:02<00:00, 36.75it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:02<00:00, 36.75it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:02<00:00, 36.75it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:02<00:00, 36.75it/s]Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:02<00:00, 36.75it/s] Compiling num tokens (num_tokens=80):  72%|███████▏  | 42/58 [00:03<00:00, 36.75it/s]Compiling num tokens (num_tokens=64):  72%|███████▏  | 42/58 [00:03<00:00, 36.75it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:03<00:00, 42.74it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:03<00:00, 42.74it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:03<00:00, 42.74it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:03<00:00, 42.74it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:03<00:00, 42.74it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:03<00:00, 42.74it/s]

    Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:03<00:00, 42.74it/s]Compiling num tokens (num_tokens=12):  84%|████████▍ | 49/58 [00:03<00:00, 42.74it/s]Compiling num tokens (num_tokens=8):  84%|████████▍ | 49/58 [00:03<00:00, 42.74it/s] Compiling num tokens (num_tokens=4):  84%|████████▍ | 49/58 [00:03<00:00, 42.74it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 18.56it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=76.12 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.09 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.09 GB):   3%|▎         | 2/58 [00:00<00:03, 18.62it/s]Capturing num tokens (num_tokens=7168 avail_mem=75.64 GB):   3%|▎         | 2/58 [00:00<00:03, 18.62it/s]Capturing num tokens (num_tokens=6656 avail_mem=72.58 GB):   3%|▎         | 2/58 [00:00<00:03, 18.62it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=72.58 GB):   3%|▎         | 2/58 [00:00<00:03, 18.62it/s]Capturing num tokens (num_tokens=6144 avail_mem=72.58 GB):   9%|▊         | 5/58 [00:00<00:02, 20.79it/s]Capturing num tokens (num_tokens=5632 avail_mem=72.58 GB):   9%|▊         | 5/58 [00:00<00:02, 20.79it/s]Capturing num tokens (num_tokens=5120 avail_mem=72.58 GB):   9%|▊         | 5/58 [00:00<00:02, 20.79it/s]Capturing num tokens (num_tokens=4608 avail_mem=72.58 GB):   9%|▊         | 5/58 [00:00<00:02, 20.79it/s]Capturing num tokens (num_tokens=4096 avail_mem=72.58 GB):   9%|▊         | 5/58 [00:00<00:02, 20.79it/s]Capturing num tokens (num_tokens=4096 avail_mem=72.58 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.15it/s]Capturing num tokens (num_tokens=3840 avail_mem=72.57 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.15it/s]Capturing num tokens (num_tokens=3584 avail_mem=72.57 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.15it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=72.56 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.15it/s]Capturing num tokens (num_tokens=3072 avail_mem=72.56 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.15it/s]Capturing num tokens (num_tokens=2816 avail_mem=72.56 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.15it/s]Capturing num tokens (num_tokens=2816 avail_mem=72.56 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.41it/s]Capturing num tokens (num_tokens=2560 avail_mem=72.55 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.41it/s]Capturing num tokens (num_tokens=2304 avail_mem=72.55 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.41it/s]Capturing num tokens (num_tokens=2048 avail_mem=72.55 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.41it/s]Capturing num tokens (num_tokens=1792 avail_mem=72.54 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.41it/s]Capturing num tokens (num_tokens=1536 avail_mem=72.54 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.41it/s]Capturing num tokens (num_tokens=1536 avail_mem=72.54 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.98it/s]Capturing num tokens (num_tokens=1280 avail_mem=72.54 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.98it/s]Capturing num tokens (num_tokens=1024 avail_mem=72.52 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.98it/s]

    Capturing num tokens (num_tokens=960 avail_mem=72.53 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.98it/s] Capturing num tokens (num_tokens=896 avail_mem=72.53 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.98it/s]Capturing num tokens (num_tokens=832 avail_mem=72.52 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.98it/s]Capturing num tokens (num_tokens=768 avail_mem=72.52 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.98it/s]Capturing num tokens (num_tokens=768 avail_mem=72.52 GB):  43%|████▎     | 25/58 [00:00<00:00, 42.21it/s]Capturing num tokens (num_tokens=704 avail_mem=72.52 GB):  43%|████▎     | 25/58 [00:00<00:00, 42.21it/s]Capturing num tokens (num_tokens=640 avail_mem=72.51 GB):  43%|████▎     | 25/58 [00:00<00:00, 42.21it/s]Capturing num tokens (num_tokens=576 avail_mem=72.51 GB):  43%|████▎     | 25/58 [00:00<00:00, 42.21it/s]Capturing num tokens (num_tokens=512 avail_mem=72.50 GB):  43%|████▎     | 25/58 [00:00<00:00, 42.21it/s]Capturing num tokens (num_tokens=480 avail_mem=72.52 GB):  43%|████▎     | 25/58 [00:00<00:00, 42.21it/s]Capturing num tokens (num_tokens=448 avail_mem=72.52 GB):  43%|████▎     | 25/58 [00:00<00:00, 42.21it/s]Capturing num tokens (num_tokens=448 avail_mem=72.52 GB):  53%|█████▎    | 31/58 [00:00<00:00, 44.94it/s]Capturing num tokens (num_tokens=416 avail_mem=72.51 GB):  53%|█████▎    | 31/58 [00:00<00:00, 44.94it/s]

    Capturing num tokens (num_tokens=384 avail_mem=72.51 GB):  53%|█████▎    | 31/58 [00:00<00:00, 44.94it/s]Capturing num tokens (num_tokens=352 avail_mem=72.51 GB):  53%|█████▎    | 31/58 [00:00<00:00, 44.94it/s]Capturing num tokens (num_tokens=320 avail_mem=72.50 GB):  53%|█████▎    | 31/58 [00:00<00:00, 44.94it/s]Capturing num tokens (num_tokens=288 avail_mem=72.50 GB):  53%|█████▎    | 31/58 [00:00<00:00, 44.94it/s]Capturing num tokens (num_tokens=256 avail_mem=72.50 GB):  53%|█████▎    | 31/58 [00:00<00:00, 44.94it/s]Capturing num tokens (num_tokens=256 avail_mem=72.50 GB):  64%|██████▍   | 37/58 [00:00<00:00, 47.25it/s]Capturing num tokens (num_tokens=240 avail_mem=72.50 GB):  64%|██████▍   | 37/58 [00:00<00:00, 47.25it/s]Capturing num tokens (num_tokens=224 avail_mem=72.49 GB):  64%|██████▍   | 37/58 [00:00<00:00, 47.25it/s]Capturing num tokens (num_tokens=208 avail_mem=72.49 GB):  64%|██████▍   | 37/58 [00:00<00:00, 47.25it/s]Capturing num tokens (num_tokens=192 avail_mem=72.49 GB):  64%|██████▍   | 37/58 [00:00<00:00, 47.25it/s]Capturing num tokens (num_tokens=176 avail_mem=72.48 GB):  64%|██████▍   | 37/58 [00:01<00:00, 47.25it/s]Capturing num tokens (num_tokens=160 avail_mem=72.48 GB):  64%|██████▍   | 37/58 [00:01<00:00, 47.25it/s]

    Capturing num tokens (num_tokens=160 avail_mem=72.48 GB):  74%|███████▍  | 43/58 [00:01<00:00, 48.92it/s]Capturing num tokens (num_tokens=144 avail_mem=72.48 GB):  74%|███████▍  | 43/58 [00:01<00:00, 48.92it/s]Capturing num tokens (num_tokens=128 avail_mem=72.47 GB):  74%|███████▍  | 43/58 [00:01<00:00, 48.92it/s]Capturing num tokens (num_tokens=112 avail_mem=72.47 GB):  74%|███████▍  | 43/58 [00:01<00:00, 48.92it/s]Capturing num tokens (num_tokens=96 avail_mem=72.47 GB):  74%|███████▍  | 43/58 [00:01<00:00, 48.92it/s] Capturing num tokens (num_tokens=80 avail_mem=72.46 GB):  74%|███████▍  | 43/58 [00:01<00:00, 48.92it/s]Capturing num tokens (num_tokens=64 avail_mem=72.46 GB):  74%|███████▍  | 43/58 [00:01<00:00, 48.92it/s]Capturing num tokens (num_tokens=64 avail_mem=72.46 GB):  84%|████████▍ | 49/58 [00:01<00:00, 49.83it/s]Capturing num tokens (num_tokens=48 avail_mem=72.46 GB):  84%|████████▍ | 49/58 [00:01<00:00, 49.83it/s]Capturing num tokens (num_tokens=32 avail_mem=72.46 GB):  84%|████████▍ | 49/58 [00:01<00:00, 49.83it/s]Capturing num tokens (num_tokens=28 avail_mem=72.45 GB):  84%|████████▍ | 49/58 [00:01<00:00, 49.83it/s]Capturing num tokens (num_tokens=24 avail_mem=72.45 GB):  84%|████████▍ | 49/58 [00:01<00:00, 49.83it/s]Capturing num tokens (num_tokens=20 avail_mem=72.44 GB):  84%|████████▍ | 49/58 [00:01<00:00, 49.83it/s]

    Capturing num tokens (num_tokens=16 avail_mem=72.44 GB):  84%|████████▍ | 49/58 [00:01<00:00, 49.83it/s]Capturing num tokens (num_tokens=16 avail_mem=72.44 GB):  95%|█████████▍| 55/58 [00:01<00:00, 50.14it/s]Capturing num tokens (num_tokens=12 avail_mem=72.44 GB):  95%|█████████▍| 55/58 [00:01<00:00, 50.14it/s]Capturing num tokens (num_tokens=8 avail_mem=72.43 GB):  95%|█████████▍| 55/58 [00:01<00:00, 50.14it/s] Capturing num tokens (num_tokens=4 avail_mem=72.43 GB):  95%|█████████▍| 55/58 [00:01<00:00, 50.14it/s]Capturing num tokens (num_tokens=4 avail_mem=72.43 GB): 100%|██████████| 58/58 [00:01<00:00, 43.16it/s]


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
    Generated text:  Tania and I am an 18 year old female. I was told that I have a brain tumour and need chemotherapy to remove a large mass from my brain. I am currently 19 and have started chemotherapy this week. I am very confused and don't understand what is going on. I don't want to kill myself but I also want to live, and I can't deal with the stress and lack of knowledge. 
    
    I understand that my cancer cells are eating away at my brain and it's not going to be good. I am scared of radiation. I am very interested in finding out what chemotherapy is and how
    ===============================
    Prompt: The president of the United States is
    Generated text:  running for a second term. He is the current president of a country that has a population of 300 million. The president will be inaugurated in 2 months. How many people will vote for him in the election?
    To determine the number of people who will vote for the president, we need to consider the following:
    
    1. The current population of the United States.
    2. The president's term in office.
    3. The time until the next election.
    
    The president will be inaugurated in 2 months, which is equivalent to 4 weeks. The current population of the United States is 300 million,
    ===============================
    Prompt: The capital of France is
    Generated text: :
    
    A) Paris
    B) Berlin
    C) Amsterdam
    D) London
    
    To determine the capital of France, let's review the options given:
    
    A) Paris
    B) Berlin
    C) Amsterdam
    D) London
    
    Paris is the capital of France, and among these cities, it is the most famous, with its historical landmarks, fashion industry, and cultural heritage.
    
    Therefore, the correct answer is:
    
    \boxed{A}
    ===============================
    Prompt: The future of AI is
    Generated text:  bright, and many are discovering the incredible power of artificial intelligence in their work. This article will highlight the importance of AI in technology, and how it is transforming the way we live and work.
    Artificial intelligence has revolutionized the way we live and work. With AI, we can automate tasks that were once done manually, such as data entry, billing, and customer service. This has increased efficiency and reduced costs. Additionally, AI is used in areas like healthcare, education, and manufacturing to improve productivity and accuracy. In the future, AI is expected to have even more transformative power and will continue to shape the way we live and work


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


    Generated text:  [Name] and I am a [Age] year old [Occupation]. I am a [Skill] who has always been [Positive Trait]. I am passionate about [What I love to do]. I am [What I enjoy doing]. I am [What I enjoy doing]. I am [What I enjoy doing]. I am [What I enjoy doing]. I am [What I enjoy doing]. I am [What I enjoy doing]. I am [What I enjoy doing]. I am [What I enjoy doing]. I am [What I enjoy doing]. I am [What I enjoy doing]. I am [What I enjoy doing
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light. It is a bustling metropolis with a rich history and culture, known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. Paris is also home to many world-renowned museums, including the Louvre, the Musée d'Orsay, and the Musée Rodin. The city is also known for its fashion industry, with Paris Fashion Week being one of the largest in the world. Paris is a popular tourist destination, with millions of visitors annually. It is also home to many important institutions of higher education, including the
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: As AI becomes more sophisticated, it is likely to become more integrated with human intelligence, allowing for more complex and nuanced decision-making. This could lead to a more human-like interaction with AI, as well as a greater ability to understand and respond to human emotions and behaviors.
    
    2. Enhanced privacy and security: As AI becomes more integrated with human intelligence, there will be a greater need for privacy and security measures to protect against potential misuse. This could lead to increased investment in privacy and security technologies, as well as greater regulation of AI development and deployment.
    
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
    Generated text:  [Name] and I'm a [Role] who specializes in [Skill/Experience]. I have [Number of Years] years of experience in [Skill/Experience]. I'm passionate about [A Personal Trait or Aspect]. I always aim to [Action or Goal], and I'm always looking for [Opportunities/Advantages]. I thrive on [What makes me stand out]. I'm [Personality Trait], and I'm constantly seeking to [Motivation or Drive]. I'm always ready to learn, grow, and improve. I enjoy [What I enjoy doing most]. I'm [Personal Trait or Aspect], and I
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, located in the northwestern part of the country.
    
    Does this next sentence follow, given the preceding text?
    
    Paris is the capital of the country of France. 
    
    Pick your answer from:
    (i) yes.
    (ii) no. To determine if the sentence "Paris is the capital of the country of France" follows from the given information, let's analyze the statement step by step.
    
    1. **Initial statement**: "The capital of France's capital city is Paris."
       - This statement correctly identifies the capital city of Paris as being in the center of the country, as it is the capital of the country of France.
    
    2.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  exciting and promising, with new technologies and applications emerging regularly. Here are some possible trends in AI that could shape the industry in the next decade:
    
    1. Self-driving cars: Self-driving cars are a rapidly developing technology that could revolutionize transportation. These cars are equipped with cameras, sensors, and other technologies that allow them to recognize and avoid obstacles, as well as detect pedestrians, cyclists, and other drivers.
    
    2. AI-powered healthcare: AI is already being used in healthcare to assist doctors and patients in diagnosing and treating diseases. In the future, we could see the development of AI that can assist healthcare professionals in predicting diseases and developing


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

    ]

     and

     I

     am

     a

    /an

     [

    Age

    ]

     year

     old

     [

    Occup

    ation

    ]

     from

     [

    Your

     Location

    ].

     I

    'm

     always

     looking

     for

     new

     experiences

     and

     I

     love

     [

    My

     Passion

    /

    Interest

    ].

     I

    'm

     here

     to

     tell

     you

     all

     about

     [

    My

     First

     Adventure

    ]

    !
    


    Hello

    ,

     my

     name

     is

     [

    Name

    ]

     and

     I

     am

     a

    /an

     [

    Age

    ]

     year

     old

     [

    Occup

    ation

    ]

     from

     [

    Your

     Location

    ].

     I

    'm

     always

     looking

     for

     new

     experiences

     and

     I

     love

     [

    My

     Passion

    /

    Interest

    ].

     I

    'm

     here

     to

     tell

     you

     all

     about

     [

    My

     First

     Adventure

    ]

    !

     
    


    Hello

    ,

     my

     name

     is

     [

    Name

    ]

     and

     I

     am

     a

    /an

     [

    Age

    ]

     year

     old

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     
    


    The

     statement

     summarizes

     that

     Paris

     is

     the

     capital

     city

     of

     France

    ,

     specifying

     that

     it

     is

     the

     name

     of

     the

     capital

     city

     and

     noting

     that

     the

     word

     "

    capital

    "

     is

     used

     in

     French

     to

     refer

     to

     the

     city

    's

     administrative

     center

     and

     the

     heart

     of

     the

     country

    's

     political

     and

     cultural

     life

    .

     Additionally

    ,

     the

     statement

     does

     not

     provide

     any

     additional

     context

     or

     information

     about

     the

     city

     of

     Paris

     or

     its

     significance

     in

     French

     history

     or

     culture

    .

     
    


    The

     Paris

     statement

     is

     presented

     in

     a

     concise

     manner

     that

     is

     easy

     to

     understand

     and

     remember

    .

     It

     is

     a

     fact

    ually

     accurate

     summary

     that

     provides

     the

     name

     and

     location

     of

     a

     specific

     city

     in

     France

    ,

     rather

     than

     providing

     more

     details

     about

     its

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     full

     of

     exciting

     possibilities

    ,

     and

     it

     is

     likely

     to

     continue

     to

     evolve

     in

     many

     different

     directions

    .

     Here

     are

     some

     potential

     trends

     that

     are

     likely

     to

     shape

     the

     future

     of

     AI

    :
    


    1

    .

     Increased

     transparency

     and

     accountability

    :

     AI

     systems

     are

     becoming

     more

     complex

     and

     nuanced

    ,

     and

     as

     such

    ,

     there

     is

     a

     growing

     need

     for

     transparency

     and

     accountability

     in

     how

     they

     are

     used

     and

     operated

    .

     This

     may

     lead

     to

     increased

     regulations

     and

     standards

    ,

     as

     well

     as

     greater

     transparency

     in

     how

     AI

     is

     developed

    ,

     deployed

    ,

     and

     used

    .
    


    2

    .

     AI

     for

     human

     autonomy

    :

     AI

     has

     the

     potential

     to

     greatly

     improve

     human

     life

     and

     work

    ,

     but

     it

     is

     also

     raising

     important

     ethical

     questions

    .

     As

     AI

     is

     used

     for

    



```python
llm.shutdown()
```
