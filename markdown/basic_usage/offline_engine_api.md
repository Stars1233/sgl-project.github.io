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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.23it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.21it/s]


    2026-04-04 05:33:41,291 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 05:33:41] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.78it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.78it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.78it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.78it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.78it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.78it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.78it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.78it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:08,  5.78it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:03, 12.24it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:03, 12.24it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:03, 12.24it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 12.24it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 12.24it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 12.24it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 12.24it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 12.24it/s]

    Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 25.18it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 25.18it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 25.18it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 25.18it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 25.18it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 25.18it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 25.18it/s]

    Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 30.29it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 30.29it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 30.29it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 30.29it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 30.29it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 30.29it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 30.29it/s]Compiling num tokens (num_tokens=128):  66%|██████▌   | 38/58 [00:03<00:00, 30.29it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 36.06it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 36.06it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 36.06it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 36.06it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 36.06it/s]

    Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 36.06it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 36.06it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 40.02it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 40.02it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 40.02it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 40.02it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 40.02it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 40.02it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 40.02it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 40.02it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.02it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=131.67 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=131.67 GB):   2%|▏         | 1/58 [00:00<00:06,  8.38it/s]Capturing num tokens (num_tokens=7680 avail_mem=131.64 GB):   2%|▏         | 1/58 [00:00<00:06,  8.38it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=131.64 GB):   3%|▎         | 2/58 [00:00<00:08,  6.38it/s]Capturing num tokens (num_tokens=7168 avail_mem=131.64 GB):   3%|▎         | 2/58 [00:00<00:08,  6.38it/s]Capturing num tokens (num_tokens=7168 avail_mem=131.64 GB):   5%|▌         | 3/58 [00:00<00:09,  5.87it/s]Capturing num tokens (num_tokens=6656 avail_mem=131.63 GB):   5%|▌         | 3/58 [00:00<00:09,  5.87it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=131.64 GB):   5%|▌         | 3/58 [00:00<00:09,  5.87it/s]Capturing num tokens (num_tokens=6144 avail_mem=131.64 GB):   9%|▊         | 5/58 [00:00<00:06,  8.43it/s]Capturing num tokens (num_tokens=5632 avail_mem=131.63 GB):   9%|▊         | 5/58 [00:00<00:06,  8.43it/s]Capturing num tokens (num_tokens=5120 avail_mem=131.63 GB):   9%|▊         | 5/58 [00:00<00:06,  8.43it/s]Capturing num tokens (num_tokens=4608 avail_mem=131.63 GB):   9%|▊         | 5/58 [00:00<00:06,  8.43it/s]Capturing num tokens (num_tokens=4608 avail_mem=131.63 GB):  14%|█▍        | 8/58 [00:00<00:03, 13.90it/s]Capturing num tokens (num_tokens=4096 avail_mem=131.63 GB):  14%|█▍        | 8/58 [00:00<00:03, 13.90it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=131.62 GB):  14%|█▍        | 8/58 [00:00<00:03, 13.90it/s]Capturing num tokens (num_tokens=3584 avail_mem=131.62 GB):  14%|█▍        | 8/58 [00:00<00:03, 13.90it/s]Capturing num tokens (num_tokens=3328 avail_mem=131.62 GB):  14%|█▍        | 8/58 [00:00<00:03, 13.90it/s]Capturing num tokens (num_tokens=3328 avail_mem=131.62 GB):  21%|██        | 12/58 [00:00<00:02, 20.59it/s]Capturing num tokens (num_tokens=3072 avail_mem=131.61 GB):  21%|██        | 12/58 [00:00<00:02, 20.59it/s]Capturing num tokens (num_tokens=2816 avail_mem=131.61 GB):  21%|██        | 12/58 [00:00<00:02, 20.59it/s]Capturing num tokens (num_tokens=2560 avail_mem=131.61 GB):  21%|██        | 12/58 [00:00<00:02, 20.59it/s]Capturing num tokens (num_tokens=2304 avail_mem=131.61 GB):  21%|██        | 12/58 [00:00<00:02, 20.59it/s]Capturing num tokens (num_tokens=2048 avail_mem=131.60 GB):  21%|██        | 12/58 [00:00<00:02, 20.59it/s]Capturing num tokens (num_tokens=2048 avail_mem=131.60 GB):  29%|██▉       | 17/58 [00:00<00:01, 27.82it/s]Capturing num tokens (num_tokens=1792 avail_mem=131.60 GB):  29%|██▉       | 17/58 [00:00<00:01, 27.82it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=131.60 GB):  29%|██▉       | 17/58 [00:00<00:01, 27.82it/s]Capturing num tokens (num_tokens=1280 avail_mem=131.59 GB):  29%|██▉       | 17/58 [00:01<00:01, 27.82it/s]Capturing num tokens (num_tokens=1024 avail_mem=131.57 GB):  29%|██▉       | 17/58 [00:01<00:01, 27.82it/s]Capturing num tokens (num_tokens=960 avail_mem=131.58 GB):  29%|██▉       | 17/58 [00:01<00:01, 27.82it/s] Capturing num tokens (num_tokens=960 avail_mem=131.58 GB):  38%|███▊      | 22/58 [00:01<00:01, 33.15it/s]Capturing num tokens (num_tokens=896 avail_mem=131.58 GB):  38%|███▊      | 22/58 [00:01<00:01, 33.15it/s]Capturing num tokens (num_tokens=832 avail_mem=131.58 GB):  38%|███▊      | 22/58 [00:01<00:01, 33.15it/s]Capturing num tokens (num_tokens=768 avail_mem=131.57 GB):  38%|███▊      | 22/58 [00:01<00:01, 33.15it/s]Capturing num tokens (num_tokens=704 avail_mem=131.57 GB):  38%|███▊      | 22/58 [00:01<00:01, 33.15it/s]Capturing num tokens (num_tokens=640 avail_mem=131.57 GB):  38%|███▊      | 22/58 [00:01<00:01, 33.15it/s]Capturing num tokens (num_tokens=640 avail_mem=131.57 GB):  47%|████▋     | 27/58 [00:01<00:00, 37.15it/s]Capturing num tokens (num_tokens=576 avail_mem=131.57 GB):  47%|████▋     | 27/58 [00:01<00:00, 37.15it/s]

    Capturing num tokens (num_tokens=512 avail_mem=131.56 GB):  47%|████▋     | 27/58 [00:01<00:00, 37.15it/s]Capturing num tokens (num_tokens=480 avail_mem=131.57 GB):  47%|████▋     | 27/58 [00:01<00:00, 37.15it/s]Capturing num tokens (num_tokens=448 avail_mem=131.57 GB):  47%|████▋     | 27/58 [00:01<00:00, 37.15it/s]Capturing num tokens (num_tokens=416 avail_mem=131.57 GB):  47%|████▋     | 27/58 [00:01<00:00, 37.15it/s]Capturing num tokens (num_tokens=416 avail_mem=131.57 GB):  55%|█████▌    | 32/58 [00:01<00:00, 40.03it/s]Capturing num tokens (num_tokens=384 avail_mem=131.57 GB):  55%|█████▌    | 32/58 [00:01<00:00, 40.03it/s]Capturing num tokens (num_tokens=352 avail_mem=131.56 GB):  55%|█████▌    | 32/58 [00:01<00:00, 40.03it/s]Capturing num tokens (num_tokens=320 avail_mem=131.56 GB):  55%|█████▌    | 32/58 [00:01<00:00, 40.03it/s]Capturing num tokens (num_tokens=288 avail_mem=131.55 GB):  55%|█████▌    | 32/58 [00:01<00:00, 40.03it/s]Capturing num tokens (num_tokens=256 avail_mem=131.55 GB):  55%|█████▌    | 32/58 [00:01<00:00, 40.03it/s]Capturing num tokens (num_tokens=256 avail_mem=131.55 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.94it/s]Capturing num tokens (num_tokens=240 avail_mem=131.55 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.94it/s]

    Capturing num tokens (num_tokens=224 avail_mem=131.54 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.94it/s]Capturing num tokens (num_tokens=208 avail_mem=131.54 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.94it/s]Capturing num tokens (num_tokens=192 avail_mem=131.54 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.94it/s]Capturing num tokens (num_tokens=176 avail_mem=131.54 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.94it/s]Capturing num tokens (num_tokens=176 avail_mem=131.54 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.51it/s]Capturing num tokens (num_tokens=160 avail_mem=131.53 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.51it/s]Capturing num tokens (num_tokens=144 avail_mem=131.53 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.51it/s]Capturing num tokens (num_tokens=128 avail_mem=131.53 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.51it/s]Capturing num tokens (num_tokens=112 avail_mem=131.53 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.51it/s]Capturing num tokens (num_tokens=96 avail_mem=131.52 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.51it/s] Capturing num tokens (num_tokens=96 avail_mem=131.52 GB):  81%|████████  | 47/58 [00:01<00:00, 44.27it/s]Capturing num tokens (num_tokens=80 avail_mem=131.52 GB):  81%|████████  | 47/58 [00:01<00:00, 44.27it/s]

    Capturing num tokens (num_tokens=64 avail_mem=131.51 GB):  81%|████████  | 47/58 [00:01<00:00, 44.27it/s]Capturing num tokens (num_tokens=48 avail_mem=131.51 GB):  81%|████████  | 47/58 [00:01<00:00, 44.27it/s]Capturing num tokens (num_tokens=32 avail_mem=131.51 GB):  81%|████████  | 47/58 [00:01<00:00, 44.27it/s]Capturing num tokens (num_tokens=28 avail_mem=131.50 GB):  81%|████████  | 47/58 [00:01<00:00, 44.27it/s]Capturing num tokens (num_tokens=28 avail_mem=131.50 GB):  90%|████████▉ | 52/58 [00:01<00:00, 42.14it/s]Capturing num tokens (num_tokens=24 avail_mem=131.50 GB):  90%|████████▉ | 52/58 [00:01<00:00, 42.14it/s]

    Capturing num tokens (num_tokens=20 avail_mem=131.50 GB):  90%|████████▉ | 52/58 [00:01<00:00, 42.14it/s]Capturing num tokens (num_tokens=16 avail_mem=131.50 GB):  90%|████████▉ | 52/58 [00:01<00:00, 42.14it/s]Capturing num tokens (num_tokens=12 avail_mem=131.49 GB):  90%|████████▉ | 52/58 [00:02<00:00, 42.14it/s]Capturing num tokens (num_tokens=8 avail_mem=131.49 GB):  90%|████████▉ | 52/58 [00:02<00:00, 42.14it/s] 

    Capturing num tokens (num_tokens=8 avail_mem=131.49 GB):  98%|█████████▊| 57/58 [00:02<00:00, 26.57it/s]Capturing num tokens (num_tokens=4 avail_mem=131.48 GB):  98%|█████████▊| 57/58 [00:02<00:00, 26.57it/s]Capturing num tokens (num_tokens=4 avail_mem=131.48 GB): 100%|██████████| 58/58 [00:02<00:00, 27.09it/s]


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
    Generated text:  Tricia and I am a graduate of the San Jose State University’s Bachelor of Arts in Psychology with a minor in counseling. After working as a peer counselor in California and Colorado, I moved to Florida to work for the nonprofit organization Promises of Hope in Wellington, Florida. There, I served as the counselor to the newly wed couple, Emily and Jason, and I worked with 20-30 couples each week. I continued to work with couples and co-workers in various roles with the organization through 2019.
    In my free time, I am a teacher, teacher trainer, and blogger at www.achieving
    ===============================
    Prompt: The president of the United States is
    Generated text:  trying to decide how many military tanks to buy. He can buy 250 tanks from China for $120 each. Alternatively, he can buy 300 tanks from Russia for $150 each. How much money would the president save by buying the tanks from Russia rather than China? To determine how much money the president would save by buying the tanks from Russia rather than China, we need to calculate the total cost for each option and then find the difference.
    
    First, let's calculate the total cost for buying 250 tanks from China:
    \[
    250 \text{ tanks} \times
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, a city famous for its cathedrals, gardens, and city centre.
    The city of Paris was founded in 789 AD, and has been the capital of France since 1804, when Napoleon Bonaparte established Paris as the capital of the French Empire.
    Paris is known for its world-class museums, such as the Louvre and Musée d'Orsay, as well as its iconic landmarks such as the Eiffel Tower.
    The city also has a vibrant and eclectic mix of cultural activities and events, from festivals and concerts to food and beverage stalls. Paris is also a popular tourist destination
    ===============================
    Prompt: The future of AI is
    Generated text:  highly uncertain, but for me, the future of AI is a double-edged sword. On the one hand, it brings great opportunities for humans. AI can be used to enhance productivity, innovation, and human knowledge, and it can even help solve some of the world’s most pressing problems. But on the other hand, the use of AI also has its drawbacks, such as job displacement, privacy concerns, and ethical dilemmas. Therefore, it is crucial for humans to understand and manage the potential benefits and risks of AI in order to harness its full potential. 
    
    As a language model, I can't fully agree with this statement.


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about your career and interests. What can you tell me about yourself? I'm a [insert a brief description of your job or experience here]. I enjoy [insert a brief description of your hobbies or interests here]. I'm always looking for new challenges and opportunities to grow and learn. What's your favorite hobby or activity? I'm always up for a challenge and love to try new things. What's your favorite book or movie? I love [insert a short description of your favorite book or movie here]. I
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, which is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and Louvre Museum. It is also home to the French Parliament and the French National Museum of Modern Art. Paris is a bustling city with a rich cultural heritage and is a popular tourist destination. The city is known for its cuisine, including French cuisine, and is home to many famous restaurants and bars. Paris is also a center for art, music, and literature, and is a popular destination for tourists and locals alike. The city is known for its beautiful architecture, including the Louvre and the Notre-Dame Cathedral, and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends:
    
    1. Increased automation and artificial intelligence: As AI continues to advance, we can expect to see more automation and artificial intelligence in various industries. This could lead to increased efficiency, cost savings, and job displacement, but it could also create new opportunities for innovation and creativity.
    
    2. Enhanced privacy and security: As AI becomes more integrated into our daily lives, there will be an increased need for privacy and security. This could lead to new regulations and standards for AI development, as well
    


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
    Generated text:  [Your Name], and I'm a/an [Your Profession] with [Your Degree] in [Your Area of Expertise]. I am passionate about [Your Passion]. I thrive on [Your Motivation] and enjoy [Your Hobby]. I'm also a [Your Hobby]. I’m [Your Age], [Your Location], and [Your Interests]. I have a/an [Your Award/Title/Service] and I'm [Your Favorite Food/Drink/etc.]. I'm [Your Email/Phone Number]. I love to [Your Hobby or Adventure], and I believe in [Your Mission/Value]. I’m [Your
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. Paris is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and Louvre Museum. It is also famous for its art, music, and fashion scene. It is a major cultural and economic center in Europe. Paris is the seat of the French government and is home to many prestigious universities. It is home to several of the world’s leading museums, including the Musée d'Orsay and the Louvre. Paris is also the capital of France and a popular tourist destination. It is a historic city with a rich cultural and historical background, and it is known for its charming neighborhoods and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  one that is rapidly evolving, with a number of different trends and technologies that are likely to shape the landscape of the industry in the coming years. Here are some potential areas of growth and change:
    
    1. Improved accuracy and efficiency: AI is becoming more powerful and capable of performing complex tasks more accurately than ever before. This is likely to drive the need for more sophisticated algorithms and models to optimize processes and improve outcomes.
    
    2. Greater integration with other technologies: AI is already being used in a number of ways, but it has the potential to become even more integrated with other technologies in the future. This could include machine learning, natural language processing


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

     am

     a

     [

    type

     of

     character

    ]

     [

    character

     type

    ].

     I

     am

     [

    age

    ]

     years

     old

     and

     [

    gender

    ]

     [

    gender

    /

    性别

    ].

     I

     have

     always

     been

     [

    positive

     adjective

    ],

     [

    negative

     adjective

    ],

     or

     [

    neutral

     adjective

    ]

     (

    e

    .g

    .,

     outgoing

    ,

     intro

    verted

    ,

     ext

    ro

    verted

    ,

     etc

    .).

     I

     am

     [

    occupation

    ],

     [

    career

    ],

     or

     [

    v

    ocation

    ].

     I

     have

     a

     strong

     [

    trait

    ]

     [

    example

    ],

     and

     I

     try

     to

     [

    describe

     how

     I

     maintain

     this

     trait

    ]

     (

    e

    .g

    .,

     study

     diligently

    ,

     read

     widely

    ,

     work

     hard

    ,

     etc

    .).

     I

     am

     passionate

     about

     [

    reason

     for

     interest

    /

    interest

     in

     my

     field

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    The

     capital

     of

     France

     is

     Paris

    .

     It

     is

     the

     largest

     city

     in

     France

     and

     the

     country

    's

     economic

     and

     cultural

     center

    .

     Paris

     is

     also

     known

     as

     "

    la

     cit

    é

     de

     la

     culture

    "

     (

    the

     city

     of

     culture

    )

     and

     is

     often

     referred

     to

     as

     "

    la

     Par

    ís

    ienne

    "

     (

    the

     Paris

    ian

    ).

     It

     is

     located

     on

     the

     Se

    ine

     River

     and

     is

     the

     seat

     of

     government

    ,

     administration

    ,

     culture

    ,

     education

    ,

     and

     diplomacy

     in

     France

    .

     Paris

     is

     famous

     for

     its

     architecture

    ,

     art

    ,

     and

     cuisine

    ,

     and

     is

     home

     to

     many

     world

    -ren

    owned

     landmarks

     and

     museums

    .

     It

     is

     also

     the

     birth

    place

     of

     many

     famous

     French

     writers

    ,

     artists

    ,

     and

     politicians

    .

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     highly

     uncertain

     and

     dependent

     on

     a

     variety

     of

     factors

    ,

     including

     technological

     advancements

    ,

     economic

     factors

    ,

     and

     political

     pressures

    .

     However

    ,

     some

     possible

     future

     trends

     in

     AI

     include

    :
    


    1

    .

     Increased

     automation

     and

     robotics

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

     automation

     and

     robotics

     in

     many

     industries

    ,

     from

     manufacturing

     to

     transportation

    .

     This

     could

     lead

     to

     significant

     job

     losses

     and

     changes

     in

     the

     work

     environment

    .
    


    2

    .

     Enhanced

     capabilities

     in

     natural

     language

     processing

    :

     AI

     has

     the

     potential

     to

     greatly

     improve

     natural

     language

     processing

    ,

     enabling

     machines

     to

     understand

     and

     respond

     to

     human

     language

     in

     ways

     that

     were

     previously

     impossible

    .

     This

     could

     lead

     to

     more

     efficient

     communication

     and

     interaction

     with

     machines

    .
    


    3

    .

     Increased

    



```python
llm.shutdown()
```
