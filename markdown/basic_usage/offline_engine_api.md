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


    2026-03-31 13:59:06.466 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 13:59:06] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 13:59:06.466 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 13:59:06] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 13:59:06.466 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 13:59:06] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 13:59:06.466 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 13:59:06] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 13:59:06.466 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 13:59:06] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.45it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.45it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:25,  2.55s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:25,  2.55s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:25,  2.55s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:25,  2.55s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.99it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.99it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.99it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.99it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.99it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.99it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.99it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.99it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:08,  5.99it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:02<00:08,  5.99it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 13.44it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 13.44it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 13.44it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 13.44it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 13.44it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 13.44it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 13.44it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 13.44it/s]

    Compiling num tokens (num_tokens=640):  33%|███▎      | 19/58 [00:02<00:02, 13.44it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:02<00:01, 20.75it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:02<00:01, 20.75it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 20.75it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 20.75it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 20.75it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 20.75it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:03<00:01, 20.75it/s]Compiling num tokens (num_tokens=352):  47%|████▋     | 27/58 [00:03<00:01, 20.75it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:03<00:00, 27.22it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:03<00:00, 27.22it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:03<00:00, 27.22it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:03<00:00, 27.22it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:03<00:00, 27.22it/s]

    Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:03<00:00, 27.22it/s]Compiling num tokens (num_tokens=208):  59%|█████▊    | 34/58 [00:03<00:00, 27.22it/s]Compiling num tokens (num_tokens=192):  59%|█████▊    | 34/58 [00:03<00:00, 27.22it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:03<00:00, 33.05it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:03<00:00, 33.05it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:03<00:00, 33.05it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:03<00:00, 33.05it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:03<00:00, 33.05it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:03<00:00, 33.05it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:03<00:00, 33.05it/s] Compiling num tokens (num_tokens=80):  71%|███████   | 41/58 [00:03<00:00, 33.05it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:03<00:00, 39.48it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:03<00:00, 39.48it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:03<00:00, 39.48it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:03<00:00, 39.48it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:03<00:00, 39.48it/s]Compiling num tokens (num_tokens=24):  83%|████████▎ | 48/58 [00:03<00:00, 39.48it/s]

    Compiling num tokens (num_tokens=20):  83%|████████▎ | 48/58 [00:03<00:00, 39.48it/s]Compiling num tokens (num_tokens=16):  83%|████████▎ | 48/58 [00:03<00:00, 39.48it/s]Compiling num tokens (num_tokens=12):  83%|████████▎ | 48/58 [00:03<00:00, 39.48it/s]Compiling num tokens (num_tokens=8):  83%|████████▎ | 48/58 [00:03<00:00, 39.48it/s] Compiling num tokens (num_tokens=4):  83%|████████▎ | 48/58 [00:03<00:00, 39.48it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 51.54it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.92it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=61.41 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=61.38 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=61.38 GB):   3%|▎         | 2/58 [00:00<00:03, 17.10it/s]Capturing num tokens (num_tokens=7168 avail_mem=61.38 GB):   3%|▎         | 2/58 [00:00<00:03, 17.10it/s]Capturing num tokens (num_tokens=6656 avail_mem=61.38 GB):   3%|▎         | 2/58 [00:00<00:03, 17.10it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=61.38 GB):   3%|▎         | 2/58 [00:00<00:03, 17.10it/s]Capturing num tokens (num_tokens=6144 avail_mem=61.38 GB):   9%|▊         | 5/58 [00:00<00:02, 21.56it/s]Capturing num tokens (num_tokens=5632 avail_mem=61.37 GB):   9%|▊         | 5/58 [00:00<00:02, 21.56it/s]Capturing num tokens (num_tokens=5120 avail_mem=61.37 GB):   9%|▊         | 5/58 [00:00<00:02, 21.56it/s]Capturing num tokens (num_tokens=4608 avail_mem=61.37 GB):   9%|▊         | 5/58 [00:00<00:02, 21.56it/s]Capturing num tokens (num_tokens=4096 avail_mem=58.68 GB):   9%|▊         | 5/58 [00:00<00:02, 21.56it/s]Capturing num tokens (num_tokens=4096 avail_mem=58.68 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.88it/s]Capturing num tokens (num_tokens=3840 avail_mem=58.31 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.88it/s]Capturing num tokens (num_tokens=3584 avail_mem=58.30 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.88it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=57.65 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.88it/s]Capturing num tokens (num_tokens=3072 avail_mem=57.59 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.88it/s]Capturing num tokens (num_tokens=3072 avail_mem=57.59 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.41it/s]Capturing num tokens (num_tokens=2816 avail_mem=57.59 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.41it/s]Capturing num tokens (num_tokens=2560 avail_mem=57.58 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.41it/s]Capturing num tokens (num_tokens=2304 avail_mem=57.58 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.41it/s]Capturing num tokens (num_tokens=2048 avail_mem=57.58 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.41it/s]Capturing num tokens (num_tokens=1792 avail_mem=57.57 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.41it/s]Capturing num tokens (num_tokens=1792 avail_mem=57.57 GB):  31%|███       | 18/58 [00:00<00:01, 37.10it/s]Capturing num tokens (num_tokens=1536 avail_mem=57.57 GB):  31%|███       | 18/58 [00:00<00:01, 37.10it/s]Capturing num tokens (num_tokens=1280 avail_mem=57.57 GB):  31%|███       | 18/58 [00:00<00:01, 37.10it/s]Capturing num tokens (num_tokens=1024 avail_mem=57.55 GB):  31%|███       | 18/58 [00:00<00:01, 37.10it/s]

    Capturing num tokens (num_tokens=960 avail_mem=57.56 GB):  31%|███       | 18/58 [00:00<00:01, 37.10it/s] Capturing num tokens (num_tokens=896 avail_mem=57.56 GB):  31%|███       | 18/58 [00:00<00:01, 37.10it/s]Capturing num tokens (num_tokens=896 avail_mem=57.56 GB):  40%|███▉      | 23/58 [00:00<00:00, 40.61it/s]Capturing num tokens (num_tokens=832 avail_mem=57.55 GB):  40%|███▉      | 23/58 [00:00<00:00, 40.61it/s]Capturing num tokens (num_tokens=768 avail_mem=57.55 GB):  40%|███▉      | 23/58 [00:00<00:00, 40.61it/s]Capturing num tokens (num_tokens=704 avail_mem=57.55 GB):  40%|███▉      | 23/58 [00:00<00:00, 40.61it/s]Capturing num tokens (num_tokens=640 avail_mem=57.54 GB):  40%|███▉      | 23/58 [00:00<00:00, 40.61it/s]Capturing num tokens (num_tokens=576 avail_mem=57.54 GB):  40%|███▉      | 23/58 [00:00<00:00, 40.61it/s]Capturing num tokens (num_tokens=512 avail_mem=57.53 GB):  40%|███▉      | 23/58 [00:00<00:00, 40.61it/s]Capturing num tokens (num_tokens=512 avail_mem=57.53 GB):  50%|█████     | 29/58 [00:00<00:00, 44.63it/s]Capturing num tokens (num_tokens=480 avail_mem=57.55 GB):  50%|█████     | 29/58 [00:00<00:00, 44.63it/s]Capturing num tokens (num_tokens=448 avail_mem=57.55 GB):  50%|█████     | 29/58 [00:00<00:00, 44.63it/s]Capturing num tokens (num_tokens=416 avail_mem=57.54 GB):  50%|█████     | 29/58 [00:00<00:00, 44.63it/s]

    Capturing num tokens (num_tokens=384 avail_mem=57.54 GB):  50%|█████     | 29/58 [00:00<00:00, 44.63it/s]Capturing num tokens (num_tokens=352 avail_mem=57.14 GB):  50%|█████     | 29/58 [00:00<00:00, 44.63it/s]Capturing num tokens (num_tokens=320 avail_mem=57.11 GB):  50%|█████     | 29/58 [00:00<00:00, 44.63it/s]Capturing num tokens (num_tokens=320 avail_mem=57.11 GB):  60%|██████    | 35/58 [00:00<00:00, 47.21it/s]Capturing num tokens (num_tokens=288 avail_mem=57.10 GB):  60%|██████    | 35/58 [00:00<00:00, 47.21it/s]Capturing num tokens (num_tokens=256 avail_mem=57.10 GB):  60%|██████    | 35/58 [00:00<00:00, 47.21it/s]Capturing num tokens (num_tokens=240 avail_mem=57.10 GB):  60%|██████    | 35/58 [00:00<00:00, 47.21it/s]Capturing num tokens (num_tokens=224 avail_mem=57.09 GB):  60%|██████    | 35/58 [00:00<00:00, 47.21it/s]Capturing num tokens (num_tokens=208 avail_mem=57.09 GB):  60%|██████    | 35/58 [00:00<00:00, 47.21it/s]Capturing num tokens (num_tokens=192 avail_mem=57.09 GB):  60%|██████    | 35/58 [00:00<00:00, 47.21it/s]Capturing num tokens (num_tokens=192 avail_mem=57.09 GB):  71%|███████   | 41/58 [00:01<00:00, 49.06it/s]Capturing num tokens (num_tokens=176 avail_mem=57.08 GB):  71%|███████   | 41/58 [00:01<00:00, 49.06it/s]Capturing num tokens (num_tokens=160 avail_mem=57.08 GB):  71%|███████   | 41/58 [00:01<00:00, 49.06it/s]

    Capturing num tokens (num_tokens=144 avail_mem=57.08 GB):  71%|███████   | 41/58 [00:01<00:00, 49.06it/s]Capturing num tokens (num_tokens=128 avail_mem=57.07 GB):  71%|███████   | 41/58 [00:01<00:00, 49.06it/s]Capturing num tokens (num_tokens=112 avail_mem=57.07 GB):  71%|███████   | 41/58 [00:01<00:00, 49.06it/s]Capturing num tokens (num_tokens=96 avail_mem=57.07 GB):  71%|███████   | 41/58 [00:01<00:00, 49.06it/s] Capturing num tokens (num_tokens=96 avail_mem=57.07 GB):  81%|████████  | 47/58 [00:01<00:00, 49.85it/s]Capturing num tokens (num_tokens=80 avail_mem=57.06 GB):  81%|████████  | 47/58 [00:01<00:00, 49.85it/s]Capturing num tokens (num_tokens=64 avail_mem=57.06 GB):  81%|████████  | 47/58 [00:01<00:00, 49.85it/s]Capturing num tokens (num_tokens=48 avail_mem=57.06 GB):  81%|████████  | 47/58 [00:01<00:00, 49.85it/s]Capturing num tokens (num_tokens=32 avail_mem=57.06 GB):  81%|████████  | 47/58 [00:01<00:00, 49.85it/s]Capturing num tokens (num_tokens=28 avail_mem=57.05 GB):  81%|████████  | 47/58 [00:01<00:00, 49.85it/s]Capturing num tokens (num_tokens=24 avail_mem=57.05 GB):  81%|████████  | 47/58 [00:01<00:00, 49.85it/s]Capturing num tokens (num_tokens=24 avail_mem=57.05 GB):  91%|█████████▏| 53/58 [00:01<00:00, 50.68it/s]Capturing num tokens (num_tokens=20 avail_mem=57.04 GB):  91%|█████████▏| 53/58 [00:01<00:00, 50.68it/s]

    Capturing num tokens (num_tokens=16 avail_mem=57.04 GB):  91%|█████████▏| 53/58 [00:01<00:00, 50.68it/s]Capturing num tokens (num_tokens=12 avail_mem=57.04 GB):  91%|█████████▏| 53/58 [00:01<00:00, 50.68it/s]Capturing num tokens (num_tokens=8 avail_mem=57.03 GB):  91%|█████████▏| 53/58 [00:01<00:00, 50.68it/s] Capturing num tokens (num_tokens=4 avail_mem=57.03 GB):  91%|█████████▏| 53/58 [00:01<00:00, 50.68it/s]Capturing num tokens (num_tokens=4 avail_mem=57.03 GB): 100%|██████████| 58/58 [00:01<00:00, 43.45it/s]


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
    Generated text:  Luna and I am a fan of the different types of trees and plants that grow in our region. For my birthday, I want to create a beautiful flower garden. Can you provide me with some resources or suggestions on how to achieve this goal? 
    
    I'm not sure where to start. I want to create a garden that is not only beautiful, but also attracts beneficial insects and helps maintain the soil. Can you provide me with some ideas for the different types of trees and plants that I can include in my garden? Additionally, what are some tips for successful planting and care of plants? 
    
    Please make sure to include a variety of flowers
    ===============================
    Prompt: The president of the United States is
    Generated text:  trying to decide which of two new candidates for president to vote for. He has 100 voters, half of whom are Democrats and the other half are Republicans. The first candidate has a 70% chance of winning and the second candidate has a 60% chance of winning. What is the probability that the president will vote for the candidate with the higher winning percentage?
    
    To determine the probability that the president will vote for the candidate with the higher winning percentage, we first need to understand the probabilities of each candidate winning. The probability that the first candidate will win is given as 70%, or 0.7
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, and it is located in the eastern part of the country. The capital of France is located on the northern shore of the Seine River. The capital of France is located at the point where the Seine River meets the right bank of the River.
    
    What is the capital of France? To determine the capital of France, we need to identify the point on the Seine River where it meets the right bank. Here's a step-by-step reasoning:
    
    1. Identify the point where the Seine River meets the right bank of the river.
    2. The capital of France is located at this point.
    
    Therefore, the capital of France
    ===============================
    Prompt: The future of AI is
    Generated text:  bright, but the consequences of AI will not be easy to predict. As the AI industry has advanced, so too has the debate on what is fair and just, and how to proceed. The need to balance the risks and rewards of AI has never been more clear. As AI has become a significant part of our lives, we have become even more aware of the risks and ethical concerns surrounding it. The next chapter of AI will be very different from what it has become today. It will be essential to have a proper plan and method of doing business in the future to ensure that the industry is sustainable and secure.
    AI has become an integral


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


    Generated text:  [Name] and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a brief description of your job or role]. I enjoy [insert a short description of your hobbies or interests]. I'm always looking for new challenges and opportunities to grow and learn. What's your favorite hobby or activity? I love [insert a short description of your favorite hobby or activity]. I'm always looking for new experiences and adventures. What's your favorite book or movie? I love [insert a short description of your favorite book
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a cultural and historical center with a rich history dating back to the Roman Empire and the French Revolution. It is a major transportation hub, with the Eiffel Tower serving as a symbol of France's international status. Paris is also known for its cuisine, including its famous croissants and its famous French fries. The city is also known for its fashion industry, with Paris Fashion Week being one of
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: As AI becomes more sophisticated, it is likely to become more integrated with human intelligence, allowing for more complex and nuanced decision-making. This could lead to a more human-like experience for users.
    
    2. Greater use of AI in healthcare: AI is already being used in healthcare to improve diagnosis, treatment, and patient care. As AI becomes more advanced, it is likely to be used in even more complex and personalized ways.
    
    3. Increased use of AI in manufacturing: AI is already being used in manufacturing to optimize production processes and improve quality control. As AI
    


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
    Generated text:  Emily, I'm a writer. What's your name, Emily? I'm a writer. Nice to meet you! What brings you to my attention? I'm writing about a young girl who becomes a famous actress. Her story is inspiring, and I'm eager to share it with others. What do you do for a living? I'm a writer. What's your favorite book or movie to read? I love reading stories with strong female characters. It makes me think about what it's like to be a strong woman. What are some of your favorite hobbies? I love reading, painting, and hiking. What are your thoughts on
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    Paris is a city located in the south of France, and it is the capital of France. The city is known for its rich history, art, and culture. It is home to the Louvre museum, the Eiffel Tower, and the Notre-Dame Cathedral. Paris is a popular tourist destination, and it has become a major economic hub in France. The city has a diverse population, and it is home to many ethnic groups, including French, French-speaking Americans, and immigrants from other countries. Paris is known for its cuisine, including French dishes such as croissants and patisseries, and it is also
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be shaped by several key trends, including:
    
    1. Increased automation: AI is becoming increasingly capable of performing complex tasks that were previously done by humans. As this capability increases, it will likely lead to a greater focus on automation in various industries, including manufacturing, transportation, and healthcare.
    
    2. Integration with the physical world: AI is already being used to power smart cities and self-driving cars, but there is potential for even more advanced integration with the physical world, such as self-driving planes and vehicles.
    
    3. Improved privacy and security: As AI systems become more capable of processing large amounts of data, there is an increased risk


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

    'm

     an

     [

    Age

    ]

     year

     old

     [

    Location

    ]

     living

     in

     [

    City

    ].

     I

    'm

     a

     [

    Occup

    ation

    ],

     [

    Job

     Title

    ],

     [

    Role

    ].

     I

     am

     a

    [

    Service

    ]

    person

    ,

     and

     I

    've

     been

     a

     part

     of

     the

     community

     for

     [

    Number

     of

     Years

    ].

     I

     have

     a

    [

    Relationship

    ]

     with

     [

    Name

     or

     Relationship

    ].

     I

    'm

     passionate

     about

     [

    Your

     Passion

    ,

     such

     as

     music

    ,

     sports

    ,

     or

     art

    ].

     I

     love

     [

    What

     you

     do

     for

     a

     living

    ].

     I

    'm

     always

     looking

     for

     [

    What

     you

     do

     for

     a

     living

    ],

     and

     I

    'm

     always

     eager

     to

     learn

     something

     new

    .

     What

    's

     something

     you

    're

     proud

     of

     about

     yourself

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     It

     is

     a

     city

     in

     the

     heart

     of

     the

     French

     countryside

     and

     is

     known

     for

     its

     historical

     landmarks

    ,

     vibrant

     art

     scene

    ,

     and

     diverse

     culinary

     scene

    .

     Paris

     is

     also

     a

     major

     hub

     of

     international

     trade

     and

     tourism

    ,

     with

     a

     rich

     history

     dating

     back

     to

     the

     Roman

     Empire

    .

     As

     of

     

    2

    0

    2

    1

    ,

     Paris

     has

     a

     population

     of

     over

     

    2

    .

     

    4

     million

     people

    ,

     making

     it

     the

     fifth

    -largest

     city

     in

     the

     world

     by

     population

    .

     It

     is

     home

     to

     iconic

     landmarks

     such

     as

     the

     E

    iff

    el

     Tower

    ,

     the

     Lou

    vre

     Museum

    ,

     Notre

    -D

    ame

     Cathedral

    ,

     and

     many

     other

     historic

     sites

    .

     Paris

     has

     also

     become

     a

     major

     player

     in

     the

     global

     business

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     incredibly

     bright

     with

     many

     exciting

     possibilities

    .

     Here

     are

     some

     potential

     trends

    :
    


    1

    .

     Increased

     Transparency

    :

     As

     AI

     becomes

     more

     advanced

    ,

     we

     expect

     to

     see

     more

     transparency

     in

     how

     it

     makes

     decisions

    .

     This

     means

     that

     we

     can

     trust

     that

     AI

     is

     being

     used

     to

     make

     decisions

     that

     are

     fair

     and

     just

    .
    


    2

    .

     Personal

    ized

     AI

    :

     AI

     is

     already

     becoming

     increasingly

     personal

    ,

     with

     AI

     that

     can

     understand

     and

     respond

     to

     human

     language

    .

     As

     technology

     advances

    ,

     we

     can

     expect

     to

     see

     even

     more

     personal

     and

     nuanced

     AI

     that

     understands

     and

     responds

     to

     the

     needs

     of

     individual

     users

    .
    


    3

    .

     Edge

     AI

    :

     Edge

     AI

     is

     a

     type

     of

     AI

     that

     is

     closer

     to

     the

     hardware

     and

     data

     that

     is

    



```python
llm.shutdown()
```
