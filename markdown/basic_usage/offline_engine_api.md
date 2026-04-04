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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.23it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.22it/s]


    2026-04-04 07:19:57,635 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 07:19:57] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:35,  2.74s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:35,  2.74s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:35,  2.74s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:35,  2.74s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:35,  2.74s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:07,  6.08it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:07,  6.08it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:07,  6.08it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:07,  6.08it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:07,  6.08it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:03<00:07,  6.08it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:03<00:07,  6.08it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:03<00:07,  6.08it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:03<00:07,  6.08it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 12.35it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 12.35it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.35it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.35it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.35it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.35it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.35it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 12.35it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 18.41it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 18.41it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 18.41it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 18.41it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 18.41it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 18.41it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 18.41it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 18.41it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:00, 25.07it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:00, 25.07it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:00, 25.07it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:00, 25.07it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:00, 25.07it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:00, 25.07it/s]

    Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:00, 25.07it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 30.09it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 30.09it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 30.09it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 30.09it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 30.09it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 30.09it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 30.09it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 35.13it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 35.13it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 35.13it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 35.13it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 35.13it/s]

    Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 35.13it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 35.13it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 39.34it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 39.34it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 39.34it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 39.34it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 39.34it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 39.34it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 39.34it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 39.34it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.72it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=119.67 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=119.67 GB):   2%|▏         | 1/58 [00:00<00:06,  8.69it/s]Capturing num tokens (num_tokens=7680 avail_mem=119.64 GB):   2%|▏         | 1/58 [00:00<00:06,  8.69it/s]Capturing num tokens (num_tokens=7168 avail_mem=119.14 GB):   2%|▏         | 1/58 [00:00<00:06,  8.69it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=119.14 GB):   5%|▌         | 3/58 [00:00<00:03, 14.29it/s]Capturing num tokens (num_tokens=6656 avail_mem=119.05 GB):   5%|▌         | 3/58 [00:00<00:03, 14.29it/s]Capturing num tokens (num_tokens=6144 avail_mem=119.05 GB):   5%|▌         | 3/58 [00:00<00:03, 14.29it/s]Capturing num tokens (num_tokens=5632 avail_mem=119.05 GB):   5%|▌         | 3/58 [00:00<00:03, 14.29it/s]Capturing num tokens (num_tokens=5632 avail_mem=119.05 GB):  10%|█         | 6/58 [00:00<00:02, 19.18it/s]Capturing num tokens (num_tokens=5120 avail_mem=119.05 GB):  10%|█         | 6/58 [00:00<00:02, 19.18it/s]Capturing num tokens (num_tokens=4608 avail_mem=119.04 GB):  10%|█         | 6/58 [00:00<00:02, 19.18it/s]Capturing num tokens (num_tokens=4096 avail_mem=119.04 GB):  10%|█         | 6/58 [00:00<00:02, 19.18it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=119.04 GB):  10%|█         | 6/58 [00:00<00:02, 19.18it/s]Capturing num tokens (num_tokens=3840 avail_mem=119.04 GB):  17%|█▋        | 10/58 [00:00<00:01, 25.09it/s]Capturing num tokens (num_tokens=3584 avail_mem=119.03 GB):  17%|█▋        | 10/58 [00:00<00:01, 25.09it/s]Capturing num tokens (num_tokens=3328 avail_mem=119.03 GB):  17%|█▋        | 10/58 [00:00<00:01, 25.09it/s]Capturing num tokens (num_tokens=3072 avail_mem=119.03 GB):  17%|█▋        | 10/58 [00:00<00:01, 25.09it/s]Capturing num tokens (num_tokens=2816 avail_mem=119.03 GB):  17%|█▋        | 10/58 [00:00<00:01, 25.09it/s]Capturing num tokens (num_tokens=2560 avail_mem=119.02 GB):  17%|█▋        | 10/58 [00:00<00:01, 25.09it/s]Capturing num tokens (num_tokens=2560 avail_mem=119.02 GB):  26%|██▌       | 15/58 [00:00<00:01, 31.48it/s]Capturing num tokens (num_tokens=2304 avail_mem=119.02 GB):  26%|██▌       | 15/58 [00:00<00:01, 31.48it/s]Capturing num tokens (num_tokens=2048 avail_mem=119.02 GB):  26%|██▌       | 15/58 [00:00<00:01, 31.48it/s]Capturing num tokens (num_tokens=1792 avail_mem=119.01 GB):  26%|██▌       | 15/58 [00:00<00:01, 31.48it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=119.01 GB):  26%|██▌       | 15/58 [00:00<00:01, 31.48it/s]Capturing num tokens (num_tokens=1280 avail_mem=119.00 GB):  26%|██▌       | 15/58 [00:00<00:01, 31.48it/s]Capturing num tokens (num_tokens=1280 avail_mem=119.00 GB):  34%|███▍      | 20/58 [00:00<00:01, 34.04it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.98 GB):  34%|███▍      | 20/58 [00:00<00:01, 34.04it/s]Capturing num tokens (num_tokens=960 avail_mem=118.99 GB):  34%|███▍      | 20/58 [00:00<00:01, 34.04it/s] Capturing num tokens (num_tokens=896 avail_mem=118.97 GB):  34%|███▍      | 20/58 [00:00<00:01, 34.04it/s]Capturing num tokens (num_tokens=832 avail_mem=118.96 GB):  34%|███▍      | 20/58 [00:00<00:01, 34.04it/s]

    Capturing num tokens (num_tokens=832 avail_mem=118.96 GB):  41%|████▏     | 24/58 [00:00<00:01, 26.54it/s]Capturing num tokens (num_tokens=768 avail_mem=118.96 GB):  41%|████▏     | 24/58 [00:00<00:01, 26.54it/s]Capturing num tokens (num_tokens=704 avail_mem=118.96 GB):  41%|████▏     | 24/58 [00:00<00:01, 26.54it/s]Capturing num tokens (num_tokens=640 avail_mem=118.95 GB):  41%|████▏     | 24/58 [00:00<00:01, 26.54it/s]Capturing num tokens (num_tokens=640 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:01<00:01, 26.79it/s]Capturing num tokens (num_tokens=576 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:01<00:01, 26.79it/s]

    Capturing num tokens (num_tokens=512 avail_mem=118.91 GB):  47%|████▋     | 27/58 [00:01<00:01, 26.79it/s]Capturing num tokens (num_tokens=480 avail_mem=118.92 GB):  47%|████▋     | 27/58 [00:01<00:01, 26.79it/s]Capturing num tokens (num_tokens=480 avail_mem=118.92 GB):  52%|█████▏    | 30/58 [00:01<00:01, 22.60it/s]Capturing num tokens (num_tokens=448 avail_mem=118.92 GB):  52%|█████▏    | 30/58 [00:01<00:01, 22.60it/s]Capturing num tokens (num_tokens=416 avail_mem=118.92 GB):  52%|█████▏    | 30/58 [00:01<00:01, 22.60it/s]Capturing num tokens (num_tokens=384 avail_mem=118.92 GB):  52%|█████▏    | 30/58 [00:01<00:01, 22.60it/s]Capturing num tokens (num_tokens=352 avail_mem=118.91 GB):  52%|█████▏    | 30/58 [00:01<00:01, 22.60it/s]Capturing num tokens (num_tokens=320 avail_mem=118.91 GB):  52%|█████▏    | 30/58 [00:01<00:01, 22.60it/s]Capturing num tokens (num_tokens=320 avail_mem=118.91 GB):  60%|██████    | 35/58 [00:01<00:00, 27.81it/s]Capturing num tokens (num_tokens=288 avail_mem=118.90 GB):  60%|██████    | 35/58 [00:01<00:00, 27.81it/s]

    Capturing num tokens (num_tokens=256 avail_mem=118.90 GB):  60%|██████    | 35/58 [00:01<00:00, 27.81it/s]Capturing num tokens (num_tokens=240 avail_mem=118.90 GB):  60%|██████    | 35/58 [00:01<00:00, 27.81it/s]Capturing num tokens (num_tokens=224 avail_mem=118.90 GB):  60%|██████    | 35/58 [00:01<00:00, 27.81it/s]Capturing num tokens (num_tokens=208 avail_mem=118.89 GB):  60%|██████    | 35/58 [00:01<00:00, 27.81it/s]Capturing num tokens (num_tokens=208 avail_mem=118.89 GB):  69%|██████▉   | 40/58 [00:01<00:00, 32.41it/s]Capturing num tokens (num_tokens=192 avail_mem=118.89 GB):  69%|██████▉   | 40/58 [00:01<00:00, 32.41it/s]Capturing num tokens (num_tokens=176 avail_mem=118.89 GB):  69%|██████▉   | 40/58 [00:01<00:00, 32.41it/s]Capturing num tokens (num_tokens=160 avail_mem=118.89 GB):  69%|██████▉   | 40/58 [00:01<00:00, 32.41it/s]Capturing num tokens (num_tokens=144 avail_mem=118.88 GB):  69%|██████▉   | 40/58 [00:01<00:00, 32.41it/s]Capturing num tokens (num_tokens=128 avail_mem=118.88 GB):  69%|██████▉   | 40/58 [00:01<00:00, 32.41it/s]Capturing num tokens (num_tokens=128 avail_mem=118.88 GB):  78%|███████▊  | 45/58 [00:01<00:00, 35.90it/s]Capturing num tokens (num_tokens=112 avail_mem=118.88 GB):  78%|███████▊  | 45/58 [00:01<00:00, 35.90it/s]

    Capturing num tokens (num_tokens=96 avail_mem=118.87 GB):  78%|███████▊  | 45/58 [00:01<00:00, 35.90it/s] Capturing num tokens (num_tokens=80 avail_mem=118.87 GB):  78%|███████▊  | 45/58 [00:01<00:00, 35.90it/s]Capturing num tokens (num_tokens=64 avail_mem=118.87 GB):  78%|███████▊  | 45/58 [00:01<00:00, 35.90it/s]Capturing num tokens (num_tokens=48 avail_mem=118.86 GB):  78%|███████▊  | 45/58 [00:01<00:00, 35.90it/s]Capturing num tokens (num_tokens=48 avail_mem=118.86 GB):  86%|████████▌ | 50/58 [00:01<00:00, 38.32it/s]Capturing num tokens (num_tokens=32 avail_mem=118.86 GB):  86%|████████▌ | 50/58 [00:01<00:00, 38.32it/s]Capturing num tokens (num_tokens=28 avail_mem=118.85 GB):  86%|████████▌ | 50/58 [00:01<00:00, 38.32it/s]Capturing num tokens (num_tokens=24 avail_mem=118.85 GB):  86%|████████▌ | 50/58 [00:01<00:00, 38.32it/s]Capturing num tokens (num_tokens=20 avail_mem=118.85 GB):  86%|████████▌ | 50/58 [00:01<00:00, 38.32it/s]Capturing num tokens (num_tokens=16 avail_mem=118.85 GB):  86%|████████▌ | 50/58 [00:01<00:00, 38.32it/s]

    Capturing num tokens (num_tokens=16 avail_mem=118.85 GB):  95%|█████████▍| 55/58 [00:01<00:00, 40.03it/s]Capturing num tokens (num_tokens=12 avail_mem=118.84 GB):  95%|█████████▍| 55/58 [00:01<00:00, 40.03it/s]Capturing num tokens (num_tokens=8 avail_mem=118.84 GB):  95%|█████████▍| 55/58 [00:01<00:00, 40.03it/s] Capturing num tokens (num_tokens=4 avail_mem=118.84 GB):  95%|█████████▍| 55/58 [00:01<00:00, 40.03it/s]Capturing num tokens (num_tokens=4 avail_mem=118.84 GB): 100%|██████████| 58/58 [00:01<00:00, 31.31it/s]


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
    Generated text:  Xiao Yan. I'm an eighth-grader, I'm a student, and I am 14 years old. I am a passionate student, and I am interested in my interests, I like reading, watching TV, playing computer games, and playing table tennis, but I don't like running. I don't want to join sports teams, and I don't want to do sports. I want to do my own things. I don't want to make friends. How should I improve my character? I am looking forward to your advice. I have something special to tell you. How do you think?
    Answer:
    
    To improve your
    ===============================
    Prompt: The president of the United States is
    Generated text:  a(n) ____.
    A. political party
    B. job title
    C. position
    D. company
    Answer:
    B
    
    For the maintenance of mechanical equipment, a maintenance manual should be formulated. What should be included in the content of the maintenance manual? 
    A. Maintenance inspection and daily maintenance items
    B. Maintenance inspection and seasonal maintenance items
    C. Maintenance inspection and daily and seasonal maintenance items
    D. Maintenance inspection and professional inspection items
    Answer:
    C
    
    According to the principle of type, which of the following does not belong to the category of 'external materials'? 
    A. Deck cargo
    B. Engine oil
    ===============================
    Prompt: The capital of France is
    Generated text: 
    A. Paris
    B. London
    C. Madrid
    D. Rome
    Answer:
    
    A
    
    The capital of Italy is
    A. Rome
    B. Athens
    C. Cairo
    D. Athens
    Answer:
    
    A
    
    When the reference direction of the current I is reversed, the voltage u will be ____
    A. Decrease
    B. Increase
    C. Zero
    D. Uncertain
    Answer:
    
    B
    
    Patient, male, 54 years old, with intermittent pain in the upper abdomen for 2 days, abdominal pain increased and fever with chills, hemoglobin 80g/L, WBC
    ===============================
    Prompt: The future of AI is
    Generated text:  uncertain, but one of the promising developments is the rise of AI-powered virtual assistants. These AI systems are designed to assist with tasks such as answering questions, scheduling appointments, and managing tasks. They are not only able to provide useful information but can also assist in decision-making and decision-making.
    One of the most common types of virtual assistants is the chatbot. These bots are programmed to engage in conversation with users and respond to their questions and requests. They can be designed to assist with a wide range of tasks, from scheduling appointments to providing information on a variety of topics.
    Another type of virtual assistant is the virtual personal assistant. These assistants


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


    Generated text:  [Name] and I am a [job title] at [company name]. I am a [job title] at [company name] and I am passionate about [job title] because [reason for passion]. I am [age] years old and I have [number of years of experience] years of experience in [job title]. I am [gender] and I am [height] inches tall. I have [weight] pounds of body weight. I have [number of pets] pets. I am [gender] and I have [number of children] children. I have [number of siblings] siblings. I have [
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic Eiffel Tower and its rich history dating back to the Middle Ages. It is also the birthplace of many famous French artists, writers, and philosophers. Paris is a bustling metropolis with a diverse population and a rich cultural heritage, making it a popular tourist destination. The city is also home to the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral, among other landmarks. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly into one another. The city is known for its vibrant nightlife, delicious food, and delicious wine, making
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn from and adapt to human behavior and decision-making processes. This could lead to more sophisticated and adaptive AI systems that can learn from feedback and improve their performance over time.
    
    2. Enhanced privacy and security: As AI systems become more sophisticated, there will be an increased need for privacy and security measures to protect user data and prevent misuse of AI systems. This could lead to the development of new technologies and protocols for handling sensitive data and ensuring that AI systems are used responsibly.
    
    3. Greater
    


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
    Generated text:  [insert character's name]. I'm a [insert the character's profession, age, or occupation]. I'm [insert how the character reads, writes, or speaks]. I've always been fascinated by [insert why or what drives the character]. I've always wanted to share my knowledge and passion with the world. I'm [insert any accomplishments or notable accomplishments of the character]. I've always been an advocate for [insert any cause or belief the character shares]. And, I'm [insert any hobbies, interests, or passions the character has]. I'm [insert any awards, honors, or recognition the character has received]. I
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, known for its iconic landmarks such as Notre-Dame Cathedral, Eiffel Tower, and Louvre Museum.
    
    Paris is often referred to as the "City of Light" and is home to numerous world-renowned attractions, including the Louvre Museum, the Eiffel Tower, and the Arc de Triomphe. The city's rich history and culture make it a popular tourist destination, attracting millions of visitors each year. Paris is also a symbol of French culture and values, particularly those of the French Revolution and its influence on art, literature, and politics. As of 2021, Paris has a population of
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  exciting and there are many possibilities for what it can achieve. Some potential trends include:
    
    1. Increased automation: As AI technology continues to improve, we can expect more automation of tasks, including but not limited to manufacturing, transportation, and customer service. This could lead to new job opportunities and potentially lower costs for businesses.
    
    2. Improved accessibility: As AI technology becomes more sophisticated, we can expect to see more accessible AI solutions, such as voice assistants and chatbots that can understand and respond to a wide range of language and dialects.
    
    3. Enhanced security: As AI systems become more complex and sophisticated, we can expect to see greater


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

    ’m

     a

     [

    Age

    ]

     year

     old

     [

    Occup

    ation

    ].

     I

    ’m

     currently

     [

    Current

     Location

    ],

     and

     my

     [

    Preferred

     Activity

    ]

     is

     [

    Preferred

     Activity

    ].

     I

    ’ve

     always

     been

     [

    h

    obby

    /t

    al

    ent

    ],

     and

     I

     enjoy

     [

    details

     about

     hobby

    /t

    alent

    ],

     but

     I

     don

    ’t

     see

     myself

     as

     [

    ability

    /

    qualification

    ].

     What

     is

     your

     name

    ,

     and

     what

     do

     you

     do

     for

     a

     living

    ?

     [

    Name

    ]

     is

     a

     [

    Occup

    ation

    ].

     My

     favorite

     activity

     is

     [

    Preferred

     Activity

    ],

     and

     I

     enjoy

     [

    details

     about

     hobby

    /t

    alent

    ].

     I

    ’ve

     always

     been

     [

    h

    obby

    /t

    alent

    ],

     but

     I

     don

    ’t

     see

     myself

     as

     [

    ability

    /

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    The

     correct

     answer

     is

    :

     The

     capital

     of

     France

     is

     Paris

    .

     
    


    To

     elaborate

     on

     this

    :
    


    -

     Paris

     is

     the

     capital

     and

     largest

     city

     of

     France

    .


    -

     It

     is

     also

     known

     as

     "

    La

     Hay

    e

    "

     or

     "

    La

     C

    ité

    "

     and

     is

     often

     referred

     to

     simply

     as

     "

    Paris

    ".


    -

     Paris

     is

     famous

     for

     its iconic

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

     Lou

    vre

     Museum

    ,

     and

     many

     historical

     sites

    .


    -

     The

     city

     is

     known

     for

     its

     fashion

     industry

     and

     is

     home

     to

     major

     fashion

     brands

     like

     Saint

     Laurent

     and

     Chanel

    .


    -

     Paris

     is

     home

     to

     the

     Lou

    vre

     Museum

    ,

     which

     houses

     the

     world

    's

     largest

     collection

     of

     art

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     uncertain

    ,

     but

     here

     are

     some

     possible

     trends

     that

     could

     shape

     the

     field

     in

     the

     coming

     years

    :
    


    1

    .

     Increased

     reliance

     on

     machine

     learning

     and

     deep

     learning

    :

     As

     AI

     technology

     continues

     to

     advance

    ,

     we

     can

     expect

     to

     see

     more

     complex

     algorithms

     that

     learn

     from

     data

     to

     make

     decisions

     and

     improve

     over

     time

    .
    


    2

    .

     Greater

     integration

     with

     other

     technologies

    :

     AI

     is

     already

     being

     integrated

     into

     a

     wide

     range

     of

     applications

    ,

     such

     as

     voice

     recognition

     and

     image

     recognition

    .

     In

     the

     future

    ,

     we

     can

     expect

     to

     see

     more

     integration

     with

     other

     technologies

    ,

     such

     as

     natural

     language

     processing

     and

     robotics

    .
    


    3

    .

     Increased

     use

     of

     AI

     for

     medical

     and

     healthcare

    :

     AI

     can

     be

     used

     to

     analyze

     large

     amounts

     of

     medical

    



```python
llm.shutdown()
```
