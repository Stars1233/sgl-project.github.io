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


    2026-03-28 03:15:02.692 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 03:15:02] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 03:15:02.692 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 03:15:02] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 03:15:02.692 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 03:15:02] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 03:15:02.692 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 03:15:02] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 03:15:02.692 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 03:15:02] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.12it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.12it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:12,  2.32s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:12,  2.32s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:12,  2.32s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:12,  2.32s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.50it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.50it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.50it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.50it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.50it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.50it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.50it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.50it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.50it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:02<00:07,  6.50it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 14.45it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 14.45it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 14.45it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 14.45it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 14.45it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 14.45it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 14.45it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 14.45it/s]

    Compiling num tokens (num_tokens=640):  33%|███▎      | 19/58 [00:02<00:02, 14.45it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:02<00:01, 21.97it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:02<00:01, 21.97it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:02<00:01, 21.97it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:02<00:01, 21.97it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:02<00:01, 21.97it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:02<00:01, 21.97it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:02<00:01, 21.97it/s]Compiling num tokens (num_tokens=352):  47%|████▋     | 27/58 [00:02<00:01, 21.97it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:02<00:00, 28.90it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:02<00:00, 28.90it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:02<00:00, 28.90it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:02<00:00, 28.90it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:02<00:00, 28.90it/s]Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:02<00:00, 28.90it/s]Compiling num tokens (num_tokens=208):  59%|█████▊    | 34/58 [00:02<00:00, 28.90it/s]

    Compiling num tokens (num_tokens=192):  59%|█████▊    | 34/58 [00:02<00:00, 28.90it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:02<00:00, 35.29it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:02<00:00, 35.29it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:02<00:00, 35.29it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:03<00:00, 35.29it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:03<00:00, 35.29it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:03<00:00, 35.29it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:03<00:00, 35.29it/s] Compiling num tokens (num_tokens=80):  71%|███████   | 41/58 [00:03<00:00, 35.29it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:03<00:00, 41.43it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:03<00:00, 41.43it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:03<00:00, 41.43it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:03<00:00, 41.43it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:03<00:00, 41.43it/s]Compiling num tokens (num_tokens=24):  83%|████████▎ | 48/58 [00:03<00:00, 41.43it/s]Compiling num tokens (num_tokens=20):  83%|████████▎ | 48/58 [00:03<00:00, 41.43it/s]Compiling num tokens (num_tokens=16):  83%|████████▎ | 48/58 [00:03<00:00, 41.43it/s]

    Compiling num tokens (num_tokens=12):  83%|████████▎ | 48/58 [00:03<00:00, 41.43it/s]Compiling num tokens (num_tokens=8):  83%|████████▎ | 48/58 [00:03<00:00, 41.43it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:03<00:00, 51.82it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:03<00:00, 51.82it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 18.18it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=71.05 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=71.02 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=71.02 GB):   3%|▎         | 2/58 [00:00<00:02, 19.52it/s]Capturing num tokens (num_tokens=7168 avail_mem=71.02 GB):   3%|▎         | 2/58 [00:00<00:02, 19.52it/s]Capturing num tokens (num_tokens=6656 avail_mem=71.01 GB):   3%|▎         | 2/58 [00:00<00:02, 19.52it/s]Capturing num tokens (num_tokens=6144 avail_mem=71.02 GB):   3%|▎         | 2/58 [00:00<00:02, 19.52it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=71.02 GB):   9%|▊         | 5/58 [00:00<00:02, 22.67it/s]Capturing num tokens (num_tokens=5632 avail_mem=71.01 GB):   9%|▊         | 5/58 [00:00<00:02, 22.67it/s]Capturing num tokens (num_tokens=5120 avail_mem=71.01 GB):   9%|▊         | 5/58 [00:00<00:02, 22.67it/s]Capturing num tokens (num_tokens=4608 avail_mem=71.01 GB):   9%|▊         | 5/58 [00:00<00:02, 22.67it/s]Capturing num tokens (num_tokens=4096 avail_mem=71.01 GB):   9%|▊         | 5/58 [00:00<00:02, 22.67it/s]Capturing num tokens (num_tokens=4096 avail_mem=71.01 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.74it/s]Capturing num tokens (num_tokens=3840 avail_mem=71.00 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.74it/s]Capturing num tokens (num_tokens=3584 avail_mem=71.00 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.74it/s]Capturing num tokens (num_tokens=3328 avail_mem=71.00 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.74it/s]Capturing num tokens (num_tokens=3072 avail_mem=70.99 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.74it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=70.99 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.74it/s]Capturing num tokens (num_tokens=2816 avail_mem=70.99 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.09it/s]Capturing num tokens (num_tokens=2560 avail_mem=70.99 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.09it/s]Capturing num tokens (num_tokens=2304 avail_mem=70.99 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.09it/s]Capturing num tokens (num_tokens=2048 avail_mem=70.98 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.09it/s]Capturing num tokens (num_tokens=1792 avail_mem=70.98 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.09it/s]Capturing num tokens (num_tokens=1536 avail_mem=70.98 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.09it/s]Capturing num tokens (num_tokens=1280 avail_mem=70.97 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.09it/s]Capturing num tokens (num_tokens=1280 avail_mem=70.97 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.60it/s]Capturing num tokens (num_tokens=1024 avail_mem=70.95 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.60it/s]Capturing num tokens (num_tokens=960 avail_mem=70.96 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.60it/s] Capturing num tokens (num_tokens=896 avail_mem=70.96 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.60it/s]Capturing num tokens (num_tokens=832 avail_mem=70.96 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.60it/s]

    Capturing num tokens (num_tokens=768 avail_mem=70.95 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.60it/s]Capturing num tokens (num_tokens=704 avail_mem=70.95 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.60it/s]Capturing num tokens (num_tokens=704 avail_mem=70.95 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.85it/s]Capturing num tokens (num_tokens=640 avail_mem=70.95 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.85it/s]Capturing num tokens (num_tokens=576 avail_mem=70.95 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.85it/s]Capturing num tokens (num_tokens=512 avail_mem=70.94 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.85it/s]Capturing num tokens (num_tokens=480 avail_mem=70.95 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.85it/s]Capturing num tokens (num_tokens=448 avail_mem=70.95 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.85it/s]Capturing num tokens (num_tokens=416 avail_mem=70.95 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.85it/s]Capturing num tokens (num_tokens=416 avail_mem=70.95 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.89it/s]Capturing num tokens (num_tokens=384 avail_mem=70.95 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.89it/s]Capturing num tokens (num_tokens=352 avail_mem=70.94 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.89it/s]Capturing num tokens (num_tokens=320 avail_mem=70.94 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.89it/s]

    Capturing num tokens (num_tokens=288 avail_mem=70.93 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.89it/s]Capturing num tokens (num_tokens=256 avail_mem=70.93 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.89it/s]Capturing num tokens (num_tokens=240 avail_mem=70.93 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.89it/s]Capturing num tokens (num_tokens=240 avail_mem=70.93 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.77it/s]Capturing num tokens (num_tokens=224 avail_mem=70.92 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.77it/s]Capturing num tokens (num_tokens=208 avail_mem=70.92 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.77it/s]Capturing num tokens (num_tokens=192 avail_mem=70.92 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.77it/s]Capturing num tokens (num_tokens=176 avail_mem=70.92 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.77it/s]Capturing num tokens (num_tokens=160 avail_mem=70.91 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.77it/s]Capturing num tokens (num_tokens=144 avail_mem=70.91 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.77it/s]Capturing num tokens (num_tokens=144 avail_mem=70.91 GB):  76%|███████▌  | 44/58 [00:01<00:00, 51.07it/s]Capturing num tokens (num_tokens=128 avail_mem=70.91 GB):  76%|███████▌  | 44/58 [00:01<00:00, 51.07it/s]Capturing num tokens (num_tokens=112 avail_mem=70.91 GB):  76%|███████▌  | 44/58 [00:01<00:00, 51.07it/s]

    Capturing num tokens (num_tokens=96 avail_mem=70.90 GB):  76%|███████▌  | 44/58 [00:01<00:00, 51.07it/s] Capturing num tokens (num_tokens=80 avail_mem=70.90 GB):  76%|███████▌  | 44/58 [00:01<00:00, 51.07it/s]Capturing num tokens (num_tokens=64 avail_mem=70.89 GB):  76%|███████▌  | 44/58 [00:01<00:00, 51.07it/s]Capturing num tokens (num_tokens=48 avail_mem=70.89 GB):  76%|███████▌  | 44/58 [00:01<00:00, 51.07it/s]Capturing num tokens (num_tokens=48 avail_mem=70.89 GB):  86%|████████▌ | 50/58 [00:01<00:00, 41.49it/s]Capturing num tokens (num_tokens=32 avail_mem=70.89 GB):  86%|████████▌ | 50/58 [00:01<00:00, 41.49it/s]Capturing num tokens (num_tokens=28 avail_mem=70.88 GB):  86%|████████▌ | 50/58 [00:01<00:00, 41.49it/s]Capturing num tokens (num_tokens=24 avail_mem=70.88 GB):  86%|████████▌ | 50/58 [00:01<00:00, 41.49it/s]Capturing num tokens (num_tokens=20 avail_mem=70.88 GB):  86%|████████▌ | 50/58 [00:01<00:00, 41.49it/s]Capturing num tokens (num_tokens=16 avail_mem=70.88 GB):  86%|████████▌ | 50/58 [00:01<00:00, 41.49it/s]

    Capturing num tokens (num_tokens=12 avail_mem=70.87 GB):  86%|████████▌ | 50/58 [00:01<00:00, 41.49it/s]Capturing num tokens (num_tokens=12 avail_mem=70.87 GB):  97%|█████████▋| 56/58 [00:01<00:00, 44.23it/s]Capturing num tokens (num_tokens=8 avail_mem=70.87 GB):  97%|█████████▋| 56/58 [00:01<00:00, 44.23it/s] Capturing num tokens (num_tokens=4 avail_mem=70.86 GB):  97%|█████████▋| 56/58 [00:01<00:00, 44.23it/s]Capturing num tokens (num_tokens=4 avail_mem=70.86 GB): 100%|██████████| 58/58 [00:01<00:00, 42.17it/s]


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
    Generated text:  Carmen and I am a part-time student at the University of Nebraska-Lincoln. I am the parent of a freshman student. I like to relax and enjoy reading books. I am also involved in the Student Government Association and the Student Union. What activities do you and your family participate in? My family and I participate in many different activities. My mother has a passion for reading and she enjoys reading books to her children. She also enjoys going to the zoo and the aquarium. My father enjoys working as a mechanic. He enjoys his job and it is also fun to drive around in his car. He likes to go to the movie theater
    ===============================
    Prompt: The president of the United States is
    Generated text:  running for a second term and has 100 million votes to be nominated for a second term. The first term winner has a 50% chance of being nominated, the second term winner has a 60% chance, and each won a total of 60 million votes. If the president wants to be a runner-up in the second term, what is the probability that he will not be nominated? To determine the probability that the president will not be nominated for a second term, we need to consider the probabilities of being nominated or not nominated for the first term, and then use these probabilities to find the probability of
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris.
    The capital of France is ( ).
    
    A: Tianjin  
    B: Hangzhou  
    C: Moscow  
    D: Paris To determine the capital of France, let's analyze each option step by step:
    
    1. **Paris**: This is the capital city of France. It is located in the South of France, bordering the Mediterranean Sea.
    2. **Hangzhou**: This is a city in Zhejiang Province, China. It is not the capital of France.
    3. **Moscow**: This is the capital city of Russia. It is not the capital of France.
    4. **Tianjin**: This is the
    ===============================
    Prompt: The future of AI is
    Generated text:  going to be vast and diverse. The potential applications of artificial intelligence go far beyond the realm of computing and technology. Its impact on the environment, education, healthcare, and more will be significant and long-lasting. If you’re interested in learning more about AI, the future of AI, and how it will impact society, you’ll want to begin by understanding the basics of machine learning.
    Machine Learning Overview
    Machine learning is the process of designing systems and algorithms that can learn from data, improve over time, and adapt to new situations. It is a subset of artificial intelligence, and it involves developing algorithms that can analyze and understand data to make


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a short description of your profession or role]. I enjoy [insert a short description of your hobbies or interests]. What's your favorite hobby or activity? I love [insert a short description of your favorite activity]. What's your favorite book or movie? I love [insert a short description of your favorite book or movie]. What's your favorite color? I love [insert a short description of your favorite color]. What's your favorite food
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French Quarter. Paris is a cultural and historical center with a rich history dating back to the Roman Empire and the French Revolution. It is a major transportation hub, with the Eiffel Tower serving as a symbol of the city's importance in world history. The city is also known for its cuisine, with dishes like croissants, escargot, and boudin being popular among locals and tourists alike.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: As AI becomes more advanced, it is likely to be integrated with human intelligence in new ways, such as through machine learning algorithms that can learn from human behavior and preferences.
    
    2. Greater emphasis on ethical considerations: As AI becomes more prevalent in our lives, there will be a greater emphasis on ethical considerations, including issues such as bias, privacy, and transparency.
    
    3. Increased use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes and reduce costs. As AI becomes more advanced, it is likely to be used in even more
    


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
    Generated text:  [Your Name] and I am a [Job Title] who loves [Favorite Hobby or Activity]. I am always up for a good challenge and always ready to learn something new. Whether it's reading books, exploring new places, or trying out a new recipe, I am never afraid to take risks and do what it takes to reach my goals. I enjoy sharing my knowledge with others and helping them achieve their dreams. So, I'm excited to get started with you and learn from you too! #HiThere #Learned #ChallengeAware #Outdoorsy #Cooking #Books #LanguageLearning #Adventure #Hobby #S
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    You are an AI assistant that helps you understand the logic of sentences expressed in Australian English. Where there is a discrepancy in grammar, you decide which one to maintain. Where there is a discrepancy in translation, you decide which one to maintain. If the statement is not a fact, state "Invalid statement". Ensure that your answer consists of only one sentence.
    
    I cannot agree with this statement. It is not factual. The capital of France is indeed Paris, as stated in the provided text. I will not support or acknowledge this false claim. However, if you have other factual statements in English, I would be happy to understand and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  highly unpredictable and difficult to predict. However, there are several trends that are likely to shape the development of AI in the coming years:
    
    1. Enhanced AI Ethics: As AI systems become more advanced, there will be a greater emphasis on ethics and fairness in their design. This means that AI systems will be required to be transparent, accountable, and transparent about their decisions and actions.
    
    2. AI in Healthcare: AI will play an increasingly important role in healthcare. For example, AI can help doctors diagnose diseases by analyzing medical images, predict patient outcomes, and assist in the development of new treatments.
    
    3. AI in Education: AI will also


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

     [

    Age

    ].

     I

     am

     a

     [

    occupation

    ],

     [

    career

     path

    ],

     and

     I

     have

     [

    number

     of

     years

    ]

     years

     of

     experience

     in

     [

    related

     field

    ].

     I

     am

     currently

     [

    job

     title

    ]

     at

     [

    company

     name

    ],

     and

     I

     enjoy

     [

    what

     I

     do

     best

    ].

     In

     my

     free

     time

    ,

     I

     enjoy

     [

    activities

     I

     like

     to

     do

    ],

     and

     I

     like

     to

     [

    what

     I

     do

     to

     stay

     healthy

    ].

     I

     am

     passionate

     about

     [

    reason

     for

     your

     career

    ]

     and

     I

     strive

     to

     [

    what

     you

     can

     do

     to

     make

     the

     world

     a

     better

     place

    ].


    **

    [

    Name

    ]

    **

     is

     a

     dynamic

     individual

     dedicated

     to

     the

     pursuit

     of

     knowledge

     and

     change

    .

     At

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     known

     for

     its

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

     Notre

     Dame

     Cathedral

    ,

     and

     Lou

    vre

     Museum

    .

     The

     city

     is

     also

     home

     to

     numerous

     museums

    ,

     including

     the

     Mus

    ée

     d

    '

    Or

    say

     and

     the

     Mus

    ée

     de

     l

    '

    Or

    anger

    ie

    ,

     and

     is

     a

     cultural

     hub

     with

     a

     vibrant

     and

     diverse

     arts

     scene

    .

     It

     is

     also

     known

     for

     its

     fashion

     and

     food

     scenes

    ,

     with

     Paris

     being

     one

     of

     the

     world

    's

     most

     famous

     fashion

     capitals

     and

     a

     popular

     tourist

     destination

    .

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     expected

     to

     be

     characterized

     by

     rapid

     advancements

     in

     machine

     learning

    ,

     deep

     learning

    ,

     and

     natural

     language

     processing

    .

     Some

     potential

     trends

     include

    :
    


    1

    .

     Increased

     specialization

     of

     AI

     systems

    :

     As

     AI

     systems

     become

     more

     sophisticated

    ,

     they

     will

     likely

     become

     more

     specialized

    ,

     enabling

     them

     to

     perform

     specific

     tasks

     more

     effectively

    .
    


    2

    .

     Increased

     reliance

     on

     AI

     in

     healthcare

    :

     As

     more

     patients

     rely

     on

     AI

    -powered

     tools

     and

     platforms

     to

     help

     them

     manage

     their

     health

     conditions

    ,

     the

     field

     of

     AI

     in

     healthcare

     is

     expected

     to

     grow

    .
    


    3

    .

     AI

     systems

     will

     become

     more

     autonomous

    :

     As

     AI

     technology

     advances

    ,

     it

     is

     expected

     that

     we

     will

     see

     more

     autonomous

     AI

     systems

    ,

     which

     will

     be

     able

     to

     make

     decisions

     and

     take

    



```python
llm.shutdown()
```
