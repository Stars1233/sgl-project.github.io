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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.36it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.36it/s]


    2026-04-05 09:32:05,619 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-05 09:32:05] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:26,  2.56s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:26,  2.56s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:26,  2.56s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:26,  2.56s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:27,  1.95it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:27,  1.95it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:27,  1.95it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:27,  1.95it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:27,  1.95it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:27,  1.95it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:27,  1.95it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.97it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.97it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.97it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.97it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.97it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.97it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.97it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.97it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:08,  5.97it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:02<00:08,  5.97it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 13.42it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 13.42it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 13.42it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 13.42it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 13.42it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 13.42it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 13.42it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 13.42it/s]

    Compiling num tokens (num_tokens=640):  33%|███▎      | 19/58 [00:02<00:02, 13.42it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:02<00:01, 20.70it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:02<00:01, 20.70it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 20.70it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 20.70it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 20.70it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 20.70it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:03<00:01, 20.70it/s]Compiling num tokens (num_tokens=352):  47%|████▋     | 27/58 [00:03<00:01, 20.70it/s]Compiling num tokens (num_tokens=320):  47%|████▋     | 27/58 [00:03<00:01, 20.70it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:03<00:00, 28.42it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:03<00:00, 28.42it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:03<00:00, 28.42it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:03<00:00, 28.42it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:03<00:00, 28.42it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:03<00:00, 28.42it/s]

    Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:03<00:00, 28.42it/s]Compiling num tokens (num_tokens=176):  60%|██████    | 35/58 [00:03<00:00, 28.42it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:03<00:00, 34.88it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:03<00:00, 34.88it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:03<00:00, 34.88it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:03<00:00, 34.88it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:03<00:00, 34.88it/s]Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:03<00:00, 34.88it/s] Compiling num tokens (num_tokens=80):  72%|███████▏  | 42/58 [00:03<00:00, 34.88it/s]Compiling num tokens (num_tokens=64):  72%|███████▏  | 42/58 [00:03<00:00, 34.88it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:03<00:00, 40.91it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:03<00:00, 40.91it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:03<00:00, 40.91it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:03<00:00, 40.91it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:03<00:00, 40.91it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:03<00:00, 40.91it/s]Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:03<00:00, 40.91it/s]

    Compiling num tokens (num_tokens=12):  84%|████████▍ | 49/58 [00:03<00:00, 40.91it/s]Compiling num tokens (num_tokens=8):  84%|████████▍ | 49/58 [00:03<00:00, 40.91it/s] Compiling num tokens (num_tokens=4):  84%|████████▍ | 49/58 [00:03<00:00, 40.91it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.99it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=74.18 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=74.16 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=74.16 GB):   3%|▎         | 2/58 [00:00<00:02, 19.61it/s]Capturing num tokens (num_tokens=7168 avail_mem=74.15 GB):   3%|▎         | 2/58 [00:00<00:02, 19.61it/s]Capturing num tokens (num_tokens=6656 avail_mem=74.15 GB):   3%|▎         | 2/58 [00:00<00:02, 19.61it/s]Capturing num tokens (num_tokens=6144 avail_mem=74.15 GB):   3%|▎         | 2/58 [00:00<00:02, 19.61it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=74.15 GB):   9%|▊         | 5/58 [00:00<00:02, 22.89it/s]Capturing num tokens (num_tokens=5632 avail_mem=74.15 GB):   9%|▊         | 5/58 [00:00<00:02, 22.89it/s]Capturing num tokens (num_tokens=5120 avail_mem=74.15 GB):   9%|▊         | 5/58 [00:00<00:02, 22.89it/s]Capturing num tokens (num_tokens=4608 avail_mem=74.14 GB):   9%|▊         | 5/58 [00:00<00:02, 22.89it/s]Capturing num tokens (num_tokens=4096 avail_mem=74.15 GB):   9%|▊         | 5/58 [00:00<00:02, 22.89it/s]Capturing num tokens (num_tokens=4096 avail_mem=74.15 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.96it/s]Capturing num tokens (num_tokens=3840 avail_mem=74.14 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.96it/s]Capturing num tokens (num_tokens=3584 avail_mem=74.14 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.96it/s]Capturing num tokens (num_tokens=3328 avail_mem=74.13 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.96it/s]Capturing num tokens (num_tokens=3072 avail_mem=74.13 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.96it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=74.13 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.96it/s]Capturing num tokens (num_tokens=2816 avail_mem=74.13 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.24it/s]Capturing num tokens (num_tokens=2560 avail_mem=74.12 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.24it/s]Capturing num tokens (num_tokens=2304 avail_mem=74.12 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.24it/s]Capturing num tokens (num_tokens=2048 avail_mem=74.12 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.24it/s]Capturing num tokens (num_tokens=1792 avail_mem=74.11 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.24it/s]Capturing num tokens (num_tokens=1536 avail_mem=74.11 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.24it/s]Capturing num tokens (num_tokens=1536 avail_mem=74.11 GB):  33%|███▎      | 19/58 [00:00<00:00, 39.10it/s]Capturing num tokens (num_tokens=1280 avail_mem=74.11 GB):  33%|███▎      | 19/58 [00:00<00:00, 39.10it/s]Capturing num tokens (num_tokens=1024 avail_mem=74.09 GB):  33%|███▎      | 19/58 [00:00<00:00, 39.10it/s]Capturing num tokens (num_tokens=960 avail_mem=74.10 GB):  33%|███▎      | 19/58 [00:00<00:00, 39.10it/s] Capturing num tokens (num_tokens=896 avail_mem=74.10 GB):  33%|███▎      | 19/58 [00:00<00:00, 39.10it/s]

    Capturing num tokens (num_tokens=832 avail_mem=74.09 GB):  33%|███▎      | 19/58 [00:00<00:00, 39.10it/s]Capturing num tokens (num_tokens=768 avail_mem=74.09 GB):  33%|███▎      | 19/58 [00:00<00:00, 39.10it/s]Capturing num tokens (num_tokens=768 avail_mem=74.09 GB):  43%|████▎     | 25/58 [00:00<00:00, 43.75it/s]Capturing num tokens (num_tokens=704 avail_mem=74.09 GB):  43%|████▎     | 25/58 [00:00<00:00, 43.75it/s]Capturing num tokens (num_tokens=640 avail_mem=74.08 GB):  43%|████▎     | 25/58 [00:00<00:00, 43.75it/s]Capturing num tokens (num_tokens=576 avail_mem=74.08 GB):  43%|████▎     | 25/58 [00:00<00:00, 43.75it/s]Capturing num tokens (num_tokens=512 avail_mem=74.07 GB):  43%|████▎     | 25/58 [00:00<00:00, 43.75it/s]Capturing num tokens (num_tokens=480 avail_mem=74.09 GB):  43%|████▎     | 25/58 [00:00<00:00, 43.75it/s]Capturing num tokens (num_tokens=448 avail_mem=74.09 GB):  43%|████▎     | 25/58 [00:00<00:00, 43.75it/s]Capturing num tokens (num_tokens=448 avail_mem=74.09 GB):  53%|█████▎    | 31/58 [00:00<00:00, 46.12it/s]Capturing num tokens (num_tokens=416 avail_mem=74.08 GB):  53%|█████▎    | 31/58 [00:00<00:00, 46.12it/s]Capturing num tokens (num_tokens=384 avail_mem=74.08 GB):  53%|█████▎    | 31/58 [00:00<00:00, 46.12it/s]

    Capturing num tokens (num_tokens=352 avail_mem=74.08 GB):  53%|█████▎    | 31/58 [00:00<00:00, 46.12it/s]Capturing num tokens (num_tokens=320 avail_mem=74.07 GB):  53%|█████▎    | 31/58 [00:00<00:00, 46.12it/s]Capturing num tokens (num_tokens=288 avail_mem=74.07 GB):  53%|█████▎    | 31/58 [00:00<00:00, 46.12it/s]Capturing num tokens (num_tokens=288 avail_mem=74.07 GB):  62%|██████▏   | 36/58 [00:00<00:00, 41.08it/s]Capturing num tokens (num_tokens=256 avail_mem=74.07 GB):  62%|██████▏   | 36/58 [00:00<00:00, 41.08it/s]Capturing num tokens (num_tokens=240 avail_mem=74.07 GB):  62%|██████▏   | 36/58 [00:00<00:00, 41.08it/s]Capturing num tokens (num_tokens=224 avail_mem=74.06 GB):  62%|██████▏   | 36/58 [00:00<00:00, 41.08it/s]

    Capturing num tokens (num_tokens=208 avail_mem=76.66 GB):  62%|██████▏   | 36/58 [00:01<00:00, 41.08it/s]Capturing num tokens (num_tokens=192 avail_mem=76.66 GB):  62%|██████▏   | 36/58 [00:01<00:00, 41.08it/s]Capturing num tokens (num_tokens=192 avail_mem=76.66 GB):  71%|███████   | 41/58 [00:01<00:00, 34.78it/s]Capturing num tokens (num_tokens=176 avail_mem=76.66 GB):  71%|███████   | 41/58 [00:01<00:00, 34.78it/s]Capturing num tokens (num_tokens=160 avail_mem=76.65 GB):  71%|███████   | 41/58 [00:01<00:00, 34.78it/s]Capturing num tokens (num_tokens=144 avail_mem=76.65 GB):  71%|███████   | 41/58 [00:01<00:00, 34.78it/s]Capturing num tokens (num_tokens=128 avail_mem=76.65 GB):  71%|███████   | 41/58 [00:01<00:00, 34.78it/s]Capturing num tokens (num_tokens=112 avail_mem=76.65 GB):  71%|███████   | 41/58 [00:01<00:00, 34.78it/s]Capturing num tokens (num_tokens=96 avail_mem=76.64 GB):  71%|███████   | 41/58 [00:01<00:00, 34.78it/s] Capturing num tokens (num_tokens=96 avail_mem=76.64 GB):  81%|████████  | 47/58 [00:01<00:00, 39.49it/s]Capturing num tokens (num_tokens=80 avail_mem=76.64 GB):  81%|████████  | 47/58 [00:01<00:00, 39.49it/s]Capturing num tokens (num_tokens=64 avail_mem=76.63 GB):  81%|████████  | 47/58 [00:01<00:00, 39.49it/s]Capturing num tokens (num_tokens=48 avail_mem=76.63 GB):  81%|████████  | 47/58 [00:01<00:00, 39.49it/s]

    Capturing num tokens (num_tokens=32 avail_mem=76.63 GB):  81%|████████  | 47/58 [00:01<00:00, 39.49it/s]Capturing num tokens (num_tokens=28 avail_mem=76.62 GB):  81%|████████  | 47/58 [00:01<00:00, 39.49it/s]Capturing num tokens (num_tokens=24 avail_mem=76.62 GB):  81%|████████  | 47/58 [00:01<00:00, 39.49it/s]Capturing num tokens (num_tokens=24 avail_mem=76.62 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.13it/s]Capturing num tokens (num_tokens=20 avail_mem=76.62 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.13it/s]Capturing num tokens (num_tokens=16 avail_mem=76.62 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.13it/s]Capturing num tokens (num_tokens=12 avail_mem=76.61 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.13it/s]Capturing num tokens (num_tokens=8 avail_mem=76.61 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.13it/s] Capturing num tokens (num_tokens=4 avail_mem=76.60 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.13it/s]Capturing num tokens (num_tokens=4 avail_mem=76.60 GB): 100%|██████████| 58/58 [00:01<00:00, 39.87it/s]


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
    Generated text:  Janice. I’m a digital marketer, but I’m not just any marketer. I’m also a tech consultant and marketer who specializes in the digital marketing and technology of businesses. My journey began at the beginning of 2010, when I worked as a Webmaster for the Cleveland Clinic. I also was the vice president of technology for the Cleveland Clinic and led the technology team. The technology team included a team of medical technologists, information technology staff, and medical billing staff. My role was to oversee the use of technology to improve efficiency, as well as to help solve technology issues. By the time I started working at
    ===============================
    Prompt: The president of the United States is
    Generated text:  3 feet tall. If a person on the moon is 1/3 as tall as the president, how tall is the person on the moon?
    To determine the height of the person on the moon, we start by understanding the given information and the relationship between the heights of the president and the person on the moon.
    
    1. The president of the United States is 3 feet tall.
    2. The person on the moon is \(\frac{1}{3}\) as tall as the president.
    
    We can express the height of the person on the moon mathematically as:
    \[
    \text{Height of the person on
    ===============================
    Prompt: The capital of France is
    Generated text:  ______.
    A. Paris
    B. Rome
    C. Berlin
    D. Amsterdam
    Answer: A
    
    Which of the following statements about the Northern Hemisphere and the Southern Hemisphere is incorrect?
    A. The Northern Hemisphere is entirely located in the Northern Hemisphere.
    B. The Southern Hemisphere is entirely located in the Southern Hemisphere.
    C. The boundary between the Northern and Southern Hemispheres is the 0° meridian.
    D. The boundary between the Northern and Southern Hemispheres is the meridian of the 20°E meridian.
    Answer: B
    
    Which of the following statements about the geographical features of the southern hemisphere is
    ===============================
    Prompt: The future of AI is
    Generated text:  going to be very bright, but you have to be patient. Here is a little introduction to the topic:
    
    - *What is AI?*
    - *What is the future of AI?*
    - *Common misconceptions about AI*
    
    *Note: Do not feel obligated to use the same definition for each topic. Feel free to adapt the information in a way that suits your audience and makes it relevant to them.*
    
    What is AI?
    
    AI stands for Artificial Intelligence, a field of study that aims to develop machines that can perform tasks that usually require human intelligence, such as language comprehension, decision-making, problem-solving, and creative thinking. AI


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about your career. What can you tell me about yourself? I'm a [insert a brief description of your personality or skills]. I enjoy [insert a short description of your hobbies or interests]. I'm always looking for new challenges and opportunities to grow and learn. What's your favorite hobby or activity? I'm always looking for new ways to challenge myself and expand my knowledge. What's your favorite book or movie? I'm always looking for new ways to challenge myself and expand my knowledge. What's your favorite
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris. It is the largest city in France and the second-largest city in the European Union. It is also the oldest capital city in Europe, having been founded in 789 AD. Paris is known for its rich history, beautiful architecture, and vibrant culture. It is home to many famous landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. Paris is also a major transportation hub, with many major highways and rail lines connecting the city to other parts of France and the world. The city is known for its fashion industry, art scene, and food culture, and is a popular tourist
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the development of the technology in the coming years. Here are some of the most likely trends that are expected to shape the development of AI in the coming years:
    
    1. Increased Use of AI in Healthcare: AI is expected to play a significant role in healthcare in the coming years. AI-powered diagnostic tools, personalized medicine, and predictive analytics are expected to revolutionize the way we diagnose and treat diseases.
    
    2. Increased Use of AI in Transportation: AI is expected to play a significant role in transportation in the coming years. Autonomous vehicles, self-driving cars, and intelligent transportation
    


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
    Generated text:  [Your Name], and I am a [role]! As a [role] expert, my expertise lies in [mention the skill or expertise related to the role]. I am passionate about [mention a personal or professional interest or hobby], and I enjoy [mention a hobby or activity related to the role]. I am always eager to learn and improve, and I am always looking for ways to contribute to the community. I am confident in my abilities and would be delighted to help anyone in need! [Your Name] [Your personal or professional background] [Your skills and experiences] [Your hobbies or activities] [Your passions and interests
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    Paris is the largest city in France and the third-largest city in the European Union. It is home to many of France's most iconic landmarks, such as the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral. Paris is also known for its French cuisine, art, and culture. The city is a hub for global business and diplomacy and plays an important role in French society and government. While the French government has been working to improve Paris's quality of life over the past few decades, the city remains one of the most popular tourist destinations in the world. Paris has a strong economy, with services
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to be a highly dynamic and unpredictable one. Here are some possible future trends that could impact AI:
    
    1. Integration of AI into new industries: As AI becomes more prevalent in daily life, we can expect to see more integration of AI into new industries, such as healthcare, finance, and transportation. For example, AI-powered self-driving cars could revolutionize transportation and increase efficiency, while AI-powered healthcare applications could improve patient care and outcomes.
    
    2. Increased use of AI in education: AI could be used to personalize learning experiences for students, improving academic performance and reducing costs. For example, AI-powered tutoring platforms could provide more targeted feedback


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

     [

    Age

    ]

     year

     old

     aspiring

     [

    Occup

    ation

     or

     Hobby

    ].

     I

     have

     always

     been

     fascinated

     by

     the

     world

     of

     sports

    ,

     and

     I

     believe

     that

     I

     have

     the

     talent

     to

     be

     the

     best

     player

     in

     the

     league

    .

     I

     love

     to

     spend

     my

     free

     time

     playing

     basketball

     and

     hiking

    ,

     and

     I

     am

     always

     looking

     to

     improve

     myself

     and

     learn

     new

     things

    .

     What

    's

     your

     favorite

     sport

     and

     why

     do

     you

     enjoy

     playing

     it

    ?

     As

     an

     AI

     language

     model

    ,

     I

     don

    't

     have

     personal

     preferences

     or

     emotions

     like

     humans

     do

    ,

     but

     I

     can

     tell

     you

     that

     sports

     have

     always

     been

     a

     popular

     hobby

     for

     many

     people

     around

     the

     world

    ,

     and

     I

     think

     it

    's

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    The

     French

     capital

    ,

     Paris

    ,

     is

     known

     for

     its

     rich

     history

    ,

     art

    ,

     culture

    ,

     and

     cuisine

    .

     It

     is

     also

     a

     world

    -ren

    owned

     tourist

     destination

     and

     a

     major

     center

     for

     finance

    ,

     media

    ,

     and

     politics

    .

     With

     its

     beautiful

     bou

    lev

    ards

    ,

     iconic

     landmarks

    ,

     and

     charming

     neighborhoods

    ,

     Paris

     is

     a

     favorite

     destination

     for

     many

     people

     around

     the

     globe

    .

     Its

     majestic

     E

    iff

    el

     Tower

     is

     a

     symbol

     of

     Paris

    ian

     spirit

     and

     innovation

    .

     In

     addition

     to

     its

     historical

     significance

    ,

     Paris

     is

     also

     a

     major

     hub

     for

     international

     diplomacy

    ,

     fashion

    ,

     and

     music

    .

     Despite

     its

     fame

    ,

     Paris

     remains

     a

     vibrant

     and

     dynamic

     city

    ,

     known

     for

     its

     diverse

     and

     multicultural

     population

    .

     The

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     shaped

     by

     a

     number

     of

     factors

    ,

     including

     advances

     in

     computing

     power

     and

     big

     data

    ,

     changes

     in

     societal

     values

     and

     attitudes

     towards

     AI

    ,

     and

     developments

     in

     the

     fields

     of

     neuroscience

     and

     ethics

    .

     Some

     potential

     trends

     that

     could

     emerge

     in

     the

     future

     include

    :
    


    1

    .

     Increased

     integration

     of

     AI

     into

     all

     aspects

     of

     daily

     life

    :

     As

     AI

     becomes

     more

     sophisticated

     and

     integrated

     into

     our

     daily

     lives

    ,

     we

     may

     see

     even

     more

     widespread

     use

     of

     AI

     in

     healthcare

    ,

     transportation

    ,

     and

     other

     sectors

    .

     This

     could

     lead

     to

     a

     more

     personalized

     and

     efficient

     approach

     to

     these

     sectors

    ,

     as

     well

     as

     the

     ability

     to

     deliver

     more

     targeted

     and

     personalized

     services

     to

     consumers

    .
    


    2

    .

     Greater

     use

     of

     AI

     in

    



```python
llm.shutdown()
```
