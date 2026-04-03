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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.30it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.29it/s]


    2026-04-03 23:21:32,278 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 23:21:32] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:37,  2.75s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:37,  2.75s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:37,  2.75s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:37,  2.75s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:37,  2.75s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:23,  2.29it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:23,  2.29it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:23,  2.29it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:23,  2.29it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:23,  2.29it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:23,  2.29it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:23,  2.29it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:07,  6.04it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:07,  6.04it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:07,  6.04it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:03<00:07,  6.04it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:03<00:07,  6.04it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:03<00:07,  6.04it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:03<00:07,  6.04it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:03<00:07,  6.04it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:03<00:07,  6.04it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 12.21it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 12.21it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.21it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.21it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.21it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.21it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.21it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 12.21it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 18.23it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 18.23it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 18.23it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 18.23it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 18.23it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 18.23it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 18.23it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 18.23it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:01, 24.82it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:01, 24.82it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:01, 24.82it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:01, 24.82it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:01, 24.82it/s]

    Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:01, 24.82it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:01, 24.82it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 29.90it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 29.90it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 29.90it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 29.90it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 29.90it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 29.90it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 29.90it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 34.90it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 34.90it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 34.90it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 34.90it/s]

    Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 34.90it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 34.90it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 34.90it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 39.12it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 39.12it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 39.12it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 39.12it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 39.12it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 39.12it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 39.12it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 39.12it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.61it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=136.07 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=136.04 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=136.04 GB):   3%|▎         | 2/58 [00:00<00:03, 17.72it/s]Capturing num tokens (num_tokens=7168 avail_mem=136.04 GB):   3%|▎         | 2/58 [00:00<00:03, 17.72it/s]Capturing num tokens (num_tokens=6656 avail_mem=136.04 GB):   3%|▎         | 2/58 [00:00<00:03, 17.72it/s]Capturing num tokens (num_tokens=6144 avail_mem=136.04 GB):   3%|▎         | 2/58 [00:00<00:03, 17.72it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=136.04 GB):   9%|▊         | 5/58 [00:00<00:02, 21.64it/s]Capturing num tokens (num_tokens=5632 avail_mem=136.03 GB):   9%|▊         | 5/58 [00:00<00:02, 21.64it/s]Capturing num tokens (num_tokens=5120 avail_mem=136.04 GB):   9%|▊         | 5/58 [00:00<00:02, 21.64it/s]Capturing num tokens (num_tokens=4608 avail_mem=136.03 GB):   9%|▊         | 5/58 [00:00<00:02, 21.64it/s]Capturing num tokens (num_tokens=4096 avail_mem=136.03 GB):   9%|▊         | 5/58 [00:00<00:02, 21.64it/s]Capturing num tokens (num_tokens=4096 avail_mem=136.03 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.53it/s]Capturing num tokens (num_tokens=3840 avail_mem=136.03 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.53it/s]Capturing num tokens (num_tokens=3584 avail_mem=136.02 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.53it/s]Capturing num tokens (num_tokens=3328 avail_mem=136.02 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.53it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=134.94 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.53it/s]Capturing num tokens (num_tokens=3072 avail_mem=134.94 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.19it/s]Capturing num tokens (num_tokens=2816 avail_mem=134.94 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.19it/s]Capturing num tokens (num_tokens=2560 avail_mem=134.88 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.19it/s]Capturing num tokens (num_tokens=2304 avail_mem=127.47 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.19it/s]Capturing num tokens (num_tokens=2048 avail_mem=121.67 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.19it/s]Capturing num tokens (num_tokens=1792 avail_mem=121.67 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.19it/s]Capturing num tokens (num_tokens=1792 avail_mem=121.67 GB):  31%|███       | 18/58 [00:00<00:01, 35.53it/s]Capturing num tokens (num_tokens=1536 avail_mem=121.66 GB):  31%|███       | 18/58 [00:00<00:01, 35.53it/s]Capturing num tokens (num_tokens=1280 avail_mem=121.66 GB):  31%|███       | 18/58 [00:00<00:01, 35.53it/s]Capturing num tokens (num_tokens=1024 avail_mem=121.64 GB):  31%|███       | 18/58 [00:00<00:01, 35.53it/s]

    Capturing num tokens (num_tokens=960 avail_mem=121.65 GB):  31%|███       | 18/58 [00:00<00:01, 35.53it/s] Capturing num tokens (num_tokens=896 avail_mem=121.65 GB):  31%|███       | 18/58 [00:00<00:01, 35.53it/s]Capturing num tokens (num_tokens=896 avail_mem=121.65 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.95it/s]Capturing num tokens (num_tokens=832 avail_mem=121.65 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.95it/s]Capturing num tokens (num_tokens=768 avail_mem=121.64 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.95it/s]Capturing num tokens (num_tokens=704 avail_mem=121.64 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.95it/s]Capturing num tokens (num_tokens=640 avail_mem=121.64 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.95it/s]Capturing num tokens (num_tokens=576 avail_mem=121.64 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.95it/s]Capturing num tokens (num_tokens=576 avail_mem=121.64 GB):  48%|████▊     | 28/58 [00:00<00:00, 41.01it/s]Capturing num tokens (num_tokens=512 avail_mem=121.35 GB):  48%|████▊     | 28/58 [00:00<00:00, 41.01it/s]Capturing num tokens (num_tokens=480 avail_mem=121.37 GB):  48%|████▊     | 28/58 [00:00<00:00, 41.01it/s]Capturing num tokens (num_tokens=448 avail_mem=121.36 GB):  48%|████▊     | 28/58 [00:00<00:00, 41.01it/s]

    Capturing num tokens (num_tokens=416 avail_mem=121.27 GB):  48%|████▊     | 28/58 [00:00<00:00, 41.01it/s]Capturing num tokens (num_tokens=384 avail_mem=121.06 GB):  48%|████▊     | 28/58 [00:00<00:00, 41.01it/s]Capturing num tokens (num_tokens=384 avail_mem=121.06 GB):  57%|█████▋    | 33/58 [00:01<00:00, 31.25it/s]Capturing num tokens (num_tokens=352 avail_mem=120.71 GB):  57%|█████▋    | 33/58 [00:01<00:00, 31.25it/s]

    Capturing num tokens (num_tokens=320 avail_mem=120.65 GB):  57%|█████▋    | 33/58 [00:01<00:00, 31.25it/s]Capturing num tokens (num_tokens=288 avail_mem=120.64 GB):  57%|█████▋    | 33/58 [00:01<00:00, 31.25it/s]Capturing num tokens (num_tokens=256 avail_mem=120.64 GB):  57%|█████▋    | 33/58 [00:01<00:00, 31.25it/s]Capturing num tokens (num_tokens=256 avail_mem=120.64 GB):  64%|██████▍   | 37/58 [00:01<00:00, 29.40it/s]Capturing num tokens (num_tokens=240 avail_mem=120.64 GB):  64%|██████▍   | 37/58 [00:01<00:00, 29.40it/s]Capturing num tokens (num_tokens=224 avail_mem=120.64 GB):  64%|██████▍   | 37/58 [00:01<00:00, 29.40it/s]

    Capturing num tokens (num_tokens=208 avail_mem=120.63 GB):  64%|██████▍   | 37/58 [00:01<00:00, 29.40it/s]Capturing num tokens (num_tokens=192 avail_mem=120.63 GB):  64%|██████▍   | 37/58 [00:01<00:00, 29.40it/s]Capturing num tokens (num_tokens=192 avail_mem=120.63 GB):  71%|███████   | 41/58 [00:01<00:00, 25.24it/s]Capturing num tokens (num_tokens=176 avail_mem=120.63 GB):  71%|███████   | 41/58 [00:01<00:00, 25.24it/s]Capturing num tokens (num_tokens=160 avail_mem=120.62 GB):  71%|███████   | 41/58 [00:01<00:00, 25.24it/s]Capturing num tokens (num_tokens=144 avail_mem=120.62 GB):  71%|███████   | 41/58 [00:01<00:00, 25.24it/s]Capturing num tokens (num_tokens=128 avail_mem=120.62 GB):  71%|███████   | 41/58 [00:01<00:00, 25.24it/s]Capturing num tokens (num_tokens=112 avail_mem=120.62 GB):  71%|███████   | 41/58 [00:01<00:00, 25.24it/s]Capturing num tokens (num_tokens=112 avail_mem=120.62 GB):  79%|███████▉  | 46/58 [00:01<00:00, 29.35it/s]Capturing num tokens (num_tokens=96 avail_mem=120.61 GB):  79%|███████▉  | 46/58 [00:01<00:00, 29.35it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=120.61 GB):  79%|███████▉  | 46/58 [00:01<00:00, 29.35it/s]Capturing num tokens (num_tokens=64 avail_mem=120.57 GB):  79%|███████▉  | 46/58 [00:01<00:00, 29.35it/s]Capturing num tokens (num_tokens=48 avail_mem=120.18 GB):  79%|███████▉  | 46/58 [00:01<00:00, 29.35it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  79%|███████▉  | 46/58 [00:01<00:00, 29.35it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  88%|████████▊ | 51/58 [00:01<00:00, 32.95it/s]Capturing num tokens (num_tokens=28 avail_mem=120.17 GB):  88%|████████▊ | 51/58 [00:01<00:00, 32.95it/s]Capturing num tokens (num_tokens=24 avail_mem=120.17 GB):  88%|████████▊ | 51/58 [00:01<00:00, 32.95it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  88%|████████▊ | 51/58 [00:01<00:00, 32.95it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  88%|████████▊ | 51/58 [00:01<00:00, 32.95it/s]Capturing num tokens (num_tokens=12 avail_mem=120.16 GB):  88%|████████▊ | 51/58 [00:01<00:00, 32.95it/s]Capturing num tokens (num_tokens=12 avail_mem=120.16 GB):  97%|█████████▋| 56/58 [00:01<00:00, 36.44it/s]Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  97%|█████████▋| 56/58 [00:01<00:00, 36.44it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  97%|█████████▋| 56/58 [00:01<00:00, 36.44it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 32.56it/s]


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
    Generated text:  Xiao Ming. I am 13 years old. I have been in school for three years. Now, I am at school and I feel very happy and confident. I am also an active student. I have been to many clubs and have participated in many activities. I love math and science and I enjoy learning new things. I have been in a singing group, and I will join a band as well. I can also be a soccer player. I am looking forward to going to the summer camp, where I can have fun and make new friends. 
    
    What can you tell me about your school life? Let me know what you
    ===============================
    Prompt: The president of the United States is
    Generated text:  a ___.
    A. professional organization
    B. public official
    C. member of the legislature
    D. member of the executive branch
    
    To determine the correct answer, let's analyze each option step by step:
    
    A. Professional organization: This refers to a group of people who work together to accomplish a common goal or task. While organizations can provide information or services, they are not considered individuals who are part of the executive branch.
    
    B. Public official: This term refers to someone who holds public office or is part of the government. Public officials are not individuals who are part of the executive branch.
    
    C. Member of the legislature:
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. Paris is a historical and cultural center of France. The city is located on the banks of the River Seine, near the two medieval walls of the citadel. It is a city with a great variety of buildings, monuments and historical sites, including the Louvre, the Eiffel Tower, Notre-Dame Cathedral, the Arc de Triomphe, the Sorbonne and the Moulin Rouge. In the 12th century, Paris was the capital of the Kingdom of France. Paris, the capital of France, is also the capital of the Île-de-France, a metropolitan area comprising the Îles
    ===============================
    Prompt: The future of AI is
    Generated text:  bright. But it will require the right human skills to turn this bright into a bright future, according to the research paper that came out of the research held by the 2018 Academy Awards nominations.
    The research paper titled "The Future of AI: Skills for the Post-World War III Era" was conducted by researchers from MIT and Stanford Universities. The paper's authors used data from the 2018 Academy Awards nominations to predict which skills would be in demand in the future of AI.
    The research concluded that a person would need to have an advanced undergraduate degree or a master's degree in computer science and be familiar with


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


    Generated text:  [Name], and I'm a [Job Title] at [Company Name]. I'm a [Number] year old, [Gender] with [Number] years of experience in [Field of Work]. I'm [Number] years of experience in [Field of Work], and I've been working at [Company Name] for [Number] years. I'm a [Number] year old, [Gender] with [Number] years of experience in [Field of Work], and I've been working at [Company Name] for [Number] years. I'm a [Number] year old, [Gender] with [Number]
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light, and is the largest city in the European Union. It is located on the Seine River and is the seat of the French government, the French parliament, and the headquarters of the French military. Paris is known for its rich history, art, and culture, and is home to many famous landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. It is also a popular tourist destination, with millions of visitors each year. Paris is a vibrant and dynamic city that is home to a diverse population and a rich cultural heritage. The city is known for its
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends:
    
    1. Increased automation: AI is expected to become more and more integrated into various industries, from manufacturing to healthcare to transportation. This will lead to increased automation of tasks, such as manufacturing, transportation, and customer service, which will require more human workers to perform these tasks.
    
    2. AI ethics and privacy: As AI becomes more integrated into our lives, there will be increasing concerns about its ethical implications and the privacy of individuals. There will be a need for regulations and guidelines to
    


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
    Generated text:  [Your Name], and I'm a [your character's occupation] from [your character's country]. I'm a big fan of [any one book or movie you enjoy reading or watching]. What is your favorite hobby?
    I'm a [your character's occupation], and I'm always looking for new experiences to try out and immerse myself in. What's your favorite way to keep your mind active? I'm always exploring new things and trying new things, and I enjoy the thrill of the unknown. 
    What brings you to this present time? I'm eager to share my experiences and stories with you, and I'm excited to
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, which is known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, and the Latin Quarter. 
    
    What is the capital city of France?
    
    The capital city of France is Paris. It is known for its iconic landmarks like the Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, and the Latin Quarter. 
    
    One interesting fact about Paris is that it is home to the world's largest library, the Bibliothèque nationale de France, which holds over 10 million books. Additionally, it is the oldest capital city in the world, having been founded
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  set to be a period of rapid evolution, as technology continues to advance at an unprecedented pace. Here are some possible future trends in AI:
    
    1. Autonomous vehicles: As technology advances, autonomous vehicles (AVs) are likely to become more widespread, with more vehicles equipped with advanced AI systems capable of self-driving. This could lead to reduced traffic congestion and accidents, as well as increased safety and efficiency.
    
    2. Personalized AI: With the increasing amount of data available, AI is likely to become more personalized and context-aware. This could mean that AI can better understand and respond to individual customers' needs and preferences, leading to better customer


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

    Job

     Title

    ]

     with

     over

     [

    Number

     of

     Years

    ]

     years

     of

     experience

     in

     the

     [

    Industry

    /

    Field

    ].

     I

     am

     known

     for

     my

     [

    Strength

    s

    /

    Pos

    itives

    /

    Value

     Proposition

    ]

     and

     have

     a

     talent

     for

     [

    Handling

     situations

     that

     need

     to

     be

     managed

    ].

     I

     am

     confident

     in

     my

     ability

     to

     [

    What

     I

    've

     Achie

    ved

    /

    What

     I

     Can

     Do

    ]

     and

     enjoy

     [

    What

     I

     Enjoy

     Doing

    ].

     I

     am

     a

     true

     [

    A

    val

    iable

     to

     Everyone

    /

    That

     I

     Can

     Accept

     Responsibility

    /

    That

     I

    'm

     Reliable

    ].

     Please

     feel

     free

     to

     ask

     me

     anything

     and

     I

    'll

     do

     my

     best

     to

     help

     you

    .

     I

     look

     forward

     to

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .


    Paris

     is

     the

     capital

     and

     largest

     city

     of

     France

    ,

     located

     on

     the

     western

     edge

     of

     the

     Î

    le

    -de

    -F

    rance

     region

    .

     The

     city

     is

     renowned

     for

     its

     medieval

     history

    ,

     art

    ,

     culture

    ,

     and

     cuisine

    .

     Paris

     is

     the

     world

    's

     most

     populous

     city

    ,

     with

     its

     famous

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

     and

     the

     Notre

    -D

    ame

     Cathedral

    .

     The

     city

     also

     hosts

     numerous

     international

     events

     and

     conferences

     each

     year

    .

     Paris

     is

     known

     for

     its

     fashion

    ,

     gastr

    onomy

    ,

     and

     other

     cultural

     exports

    ,

     making

     it

     a

     major

     hub

     of

     the

     French

     economy

    .

     The

     city

     is

     also

     home

     to

     the

     country

    's

     cultural

     capital

     and

     a

     center

     for

     international

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     expected

     to

     be

     marked

     by

     a

     proliferation

     of

     artificial

     intelligence

     (

    AI

    )

     applications

     and

     technologies

    ,

     with

     many

     new

     possibilities

     and

     breakthrough

    s

     to

     be

     discovered

    .

     Some

     potential

     future

     trends

     in

     AI

     include

    :
    


    1

    .

     Increased

     integration

     with

     natural

     language

     processing

     (

    N

    LP

    ):

     As

     AI

     becomes

     more

     advanced

    ,

     it

     will

     be

     able

     to

     understand

     and

     process

     language

     in

     ways

     that

     are

     more

     natural

     and

     intuitive

     than

     ever

     before

    .

     This

     could

     lead

     to

     more

     intelligent

     and

     natural

     language

     processing

     systems

    ,

     such

     as

     chat

    bots

     and

     virtual

     assistants

    .
    


    2

    .

     Enhanced

     predictive

     analytics

    :

     As

     AI

     becomes

     more

     sophisticated

    ,

     it

     will

     be

     able

     to

     predict

     trends

     and

     outcomes

     more

     accurately

     than

     ever

     before

    .

     This

     could

     lead

     to

     better

     forecasting

    



```python
llm.shutdown()
```
