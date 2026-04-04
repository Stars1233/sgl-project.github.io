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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  4.57it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  4.57it/s]


    2026-04-04 05:03:37,181 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 05:03:37] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:32,  2.67s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:32,  2.67s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:32,  2.67s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:32,  2.67s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.73it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.73it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.73it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.73it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.73it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.73it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.73it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.73it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:08,  5.73it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:03, 12.19it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:03, 12.19it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 12.19it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 12.19it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 12.19it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 12.19it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 12.19it/s]

    Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 12.19it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 18.53it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 18.53it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 18.53it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 18.53it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 18.53it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 18.53it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 18.53it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 18.53it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 25.30it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 25.30it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 25.30it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 25.30it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 25.30it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 25.30it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 25.30it/s]Compiling num tokens (num_tokens=224):  55%|█████▌    | 32/58 [00:03<00:01, 25.30it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 32.04it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 32.04it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 32.04it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 32.04it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 32.04it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 32.04it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 32.04it/s]Compiling num tokens (num_tokens=112):  67%|██████▋   | 39/58 [00:03<00:00, 32.04it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:03<00:00, 38.77it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:03<00:00, 38.77it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:03<00:00, 38.77it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:03<00:00, 38.77it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:03<00:00, 38.77it/s]Compiling num tokens (num_tokens=32):  79%|███████▉  | 46/58 [00:03<00:00, 38.77it/s]

    Compiling num tokens (num_tokens=28):  79%|███████▉  | 46/58 [00:03<00:00, 38.77it/s]Compiling num tokens (num_tokens=24):  79%|███████▉  | 46/58 [00:03<00:00, 38.77it/s]Compiling num tokens (num_tokens=20):  79%|███████▉  | 46/58 [00:03<00:00, 38.77it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:03<00:00, 47.02it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:03<00:00, 47.02it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:03<00:00, 47.02it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:03<00:00, 47.02it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:03<00:00, 47.02it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.32it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=76.79 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=76.79 GB):   2%|▏         | 1/58 [00:00<00:10,  5.30it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.76 GB):   2%|▏         | 1/58 [00:00<00:10,  5.30it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:07,  7.17it/s]Capturing num tokens (num_tokens=7168 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:07,  7.17it/s]Capturing num tokens (num_tokens=6656 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:07,  7.17it/s]Capturing num tokens (num_tokens=6656 avail_mem=76.76 GB):   7%|▋         | 4/58 [00:00<00:05,  9.69it/s]Capturing num tokens (num_tokens=6144 avail_mem=76.76 GB):   7%|▋         | 4/58 [00:00<00:05,  9.69it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:06,  8.29it/s]Capturing num tokens (num_tokens=5632 avail_mem=76.75 GB):   9%|▊         | 5/58 [00:00<00:06,  8.29it/s]Capturing num tokens (num_tokens=5632 avail_mem=76.75 GB):  10%|█         | 6/58 [00:00<00:06,  7.51it/s]Capturing num tokens (num_tokens=5120 avail_mem=76.76 GB):  10%|█         | 6/58 [00:00<00:06,  7.51it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=76.75 GB):  10%|█         | 6/58 [00:00<00:06,  7.51it/s]Capturing num tokens (num_tokens=4608 avail_mem=76.75 GB):  14%|█▍        | 8/58 [00:00<00:05,  9.90it/s]Capturing num tokens (num_tokens=4096 avail_mem=76.75 GB):  14%|█▍        | 8/58 [00:00<00:05,  9.90it/s]Capturing num tokens (num_tokens=3840 avail_mem=76.75 GB):  14%|█▍        | 8/58 [00:00<00:05,  9.90it/s]Capturing num tokens (num_tokens=3584 avail_mem=76.74 GB):  14%|█▍        | 8/58 [00:00<00:05,  9.90it/s]Capturing num tokens (num_tokens=3328 avail_mem=76.74 GB):  14%|█▍        | 8/58 [00:00<00:05,  9.90it/s]Capturing num tokens (num_tokens=3328 avail_mem=76.74 GB):  21%|██        | 12/58 [00:01<00:02, 17.27it/s]Capturing num tokens (num_tokens=3072 avail_mem=76.74 GB):  21%|██        | 12/58 [00:01<00:02, 17.27it/s]Capturing num tokens (num_tokens=2816 avail_mem=76.74 GB):  21%|██        | 12/58 [00:01<00:02, 17.27it/s]Capturing num tokens (num_tokens=2560 avail_mem=76.73 GB):  21%|██        | 12/58 [00:01<00:02, 17.27it/s]Capturing num tokens (num_tokens=2304 avail_mem=76.73 GB):  21%|██        | 12/58 [00:01<00:02, 17.27it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=76.72 GB):  21%|██        | 12/58 [00:01<00:02, 17.27it/s]Capturing num tokens (num_tokens=2048 avail_mem=76.72 GB):  29%|██▉       | 17/58 [00:01<00:01, 25.41it/s]Capturing num tokens (num_tokens=1792 avail_mem=76.72 GB):  29%|██▉       | 17/58 [00:01<00:01, 25.41it/s]Capturing num tokens (num_tokens=1536 avail_mem=76.72 GB):  29%|██▉       | 17/58 [00:01<00:01, 25.41it/s]Capturing num tokens (num_tokens=1280 avail_mem=76.71 GB):  29%|██▉       | 17/58 [00:01<00:01, 25.41it/s]Capturing num tokens (num_tokens=1024 avail_mem=76.69 GB):  29%|██▉       | 17/58 [00:01<00:01, 25.41it/s]Capturing num tokens (num_tokens=960 avail_mem=76.71 GB):  29%|██▉       | 17/58 [00:01<00:01, 25.41it/s] Capturing num tokens (num_tokens=896 avail_mem=76.70 GB):  29%|██▉       | 17/58 [00:01<00:01, 25.41it/s]Capturing num tokens (num_tokens=896 avail_mem=76.70 GB):  40%|███▉      | 23/58 [00:01<00:01, 33.17it/s]Capturing num tokens (num_tokens=832 avail_mem=76.70 GB):  40%|███▉      | 23/58 [00:01<00:01, 33.17it/s]Capturing num tokens (num_tokens=768 avail_mem=76.70 GB):  40%|███▉      | 23/58 [00:01<00:01, 33.17it/s]Capturing num tokens (num_tokens=704 avail_mem=76.69 GB):  40%|███▉      | 23/58 [00:01<00:01, 33.17it/s]Capturing num tokens (num_tokens=640 avail_mem=76.69 GB):  40%|███▉      | 23/58 [00:01<00:01, 33.17it/s]

    Capturing num tokens (num_tokens=576 avail_mem=76.69 GB):  40%|███▉      | 23/58 [00:01<00:01, 33.17it/s]Capturing num tokens (num_tokens=512 avail_mem=76.68 GB):  40%|███▉      | 23/58 [00:01<00:01, 33.17it/s]Capturing num tokens (num_tokens=512 avail_mem=76.68 GB):  50%|█████     | 29/58 [00:01<00:00, 38.75it/s]Capturing num tokens (num_tokens=480 avail_mem=76.69 GB):  50%|█████     | 29/58 [00:01<00:00, 38.75it/s]Capturing num tokens (num_tokens=448 avail_mem=76.69 GB):  50%|█████     | 29/58 [00:01<00:00, 38.75it/s]Capturing num tokens (num_tokens=416 avail_mem=76.69 GB):  50%|█████     | 29/58 [00:01<00:00, 38.75it/s]Capturing num tokens (num_tokens=384 avail_mem=76.69 GB):  50%|█████     | 29/58 [00:01<00:00, 38.75it/s]Capturing num tokens (num_tokens=352 avail_mem=76.68 GB):  50%|█████     | 29/58 [00:01<00:00, 38.75it/s]Capturing num tokens (num_tokens=320 avail_mem=76.68 GB):  50%|█████     | 29/58 [00:01<00:00, 38.75it/s]Capturing num tokens (num_tokens=320 avail_mem=76.68 GB):  60%|██████    | 35/58 [00:01<00:00, 42.97it/s]Capturing num tokens (num_tokens=288 avail_mem=76.68 GB):  60%|██████    | 35/58 [00:01<00:00, 42.97it/s]Capturing num tokens (num_tokens=256 avail_mem=76.67 GB):  60%|██████    | 35/58 [00:01<00:00, 42.97it/s]Capturing num tokens (num_tokens=240 avail_mem=76.67 GB):  60%|██████    | 35/58 [00:01<00:00, 42.97it/s]

    Capturing num tokens (num_tokens=224 avail_mem=76.67 GB):  60%|██████    | 35/58 [00:01<00:00, 42.97it/s]Capturing num tokens (num_tokens=208 avail_mem=76.66 GB):  60%|██████    | 35/58 [00:01<00:00, 42.97it/s]Capturing num tokens (num_tokens=192 avail_mem=76.66 GB):  60%|██████    | 35/58 [00:01<00:00, 42.97it/s]Capturing num tokens (num_tokens=192 avail_mem=76.66 GB):  71%|███████   | 41/58 [00:01<00:00, 46.19it/s]Capturing num tokens (num_tokens=176 avail_mem=76.66 GB):  71%|███████   | 41/58 [00:01<00:00, 46.19it/s]Capturing num tokens (num_tokens=160 avail_mem=76.66 GB):  71%|███████   | 41/58 [00:01<00:00, 46.19it/s]Capturing num tokens (num_tokens=144 avail_mem=76.65 GB):  71%|███████   | 41/58 [00:01<00:00, 46.19it/s]Capturing num tokens (num_tokens=128 avail_mem=76.65 GB):  71%|███████   | 41/58 [00:01<00:00, 46.19it/s]Capturing num tokens (num_tokens=112 avail_mem=76.65 GB):  71%|███████   | 41/58 [00:01<00:00, 46.19it/s]Capturing num tokens (num_tokens=96 avail_mem=76.64 GB):  71%|███████   | 41/58 [00:01<00:00, 46.19it/s] Capturing num tokens (num_tokens=96 avail_mem=76.64 GB):  81%|████████  | 47/58 [00:01<00:00, 47.78it/s]Capturing num tokens (num_tokens=80 avail_mem=76.64 GB):  81%|████████  | 47/58 [00:01<00:00, 47.78it/s]Capturing num tokens (num_tokens=64 avail_mem=76.64 GB):  81%|████████  | 47/58 [00:01<00:00, 47.78it/s]

    Capturing num tokens (num_tokens=48 avail_mem=76.63 GB):  81%|████████  | 47/58 [00:01<00:00, 47.78it/s]Capturing num tokens (num_tokens=32 avail_mem=76.63 GB):  81%|████████  | 47/58 [00:01<00:00, 47.78it/s]Capturing num tokens (num_tokens=28 avail_mem=76.62 GB):  81%|████████  | 47/58 [00:01<00:00, 47.78it/s]Capturing num tokens (num_tokens=28 avail_mem=76.62 GB):  90%|████████▉ | 52/58 [00:01<00:00, 47.67it/s]Capturing num tokens (num_tokens=24 avail_mem=76.62 GB):  90%|████████▉ | 52/58 [00:01<00:00, 47.67it/s]Capturing num tokens (num_tokens=20 avail_mem=76.62 GB):  90%|████████▉ | 52/58 [00:01<00:00, 47.67it/s]Capturing num tokens (num_tokens=16 avail_mem=76.62 GB):  90%|████████▉ | 52/58 [00:01<00:00, 47.67it/s]Capturing num tokens (num_tokens=12 avail_mem=76.61 GB):  90%|████████▉ | 52/58 [00:01<00:00, 47.67it/s]Capturing num tokens (num_tokens=8 avail_mem=76.61 GB):  90%|████████▉ | 52/58 [00:01<00:00, 47.67it/s] Capturing num tokens (num_tokens=8 avail_mem=76.61 GB):  98%|█████████▊| 57/58 [00:01<00:00, 47.40it/s]Capturing num tokens (num_tokens=4 avail_mem=76.61 GB):  98%|█████████▊| 57/58 [00:01<00:00, 47.40it/s]Capturing num tokens (num_tokens=4 avail_mem=76.61 GB): 100%|██████████| 58/58 [00:01<00:00, 30.18it/s]


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
    Generated text:  Sarah. I'm a student in college and I'm very shy and quiet. I can't find any place where I can show my confidence. I've always wanted to share my hobbies and interests with others. I love to read science fiction novels. My teachers like to read my science fiction novels and I'm not allowed to leave the school unless my parents come to school. If anyone is interested, they should just ask me. When I'm at home, I like to listen to music and watch my favorite TV shows. However, I have a problem because my parents don't allow me to do these things. One day, my parents
    ===============================
    Prompt: The president of the United States is
    Generated text:  married to a woman named Michelle. The president wants to have a child. Who will have to give up their marriage to become pregnant?
    
    1. **Understanding the Context and Core of the Question:**
       - The president of the United States is married to a woman named Michelle.
       - The president wants to have a child.
       - The question asks who will have to give up their marriage to become pregnant.
    
    2. **Analyzing the Possibilities:**
       - The president can have children through marriage or through fertility treatments.
       - Marriage, as a legal and social institution, is mandated for all citizens, regardless of gender.
    
    ===============================
    Prompt: The capital of France is
    Generated text:  ____.
    A. London
    B. Paris
    C. Rome
    D. New York
    Answer:
    
    B
    
    Patient, male, 20 years old, has had recurrent episodes of frequent urination, urgency, and dysuria for 3 years. Physical examination shows no significant abnormalities in the kidneys, bladder, or abdominal cavity. Urinalysis shows (+++), and prostate ultrasound suggests a prostate mass. The most likely diagnosis is:
    A. Chronic nephritis
    B. Acute glomerulonephritis
    C. Acute pyelonephritis
    D. Renal calculi
    E. Benign pro
    ===============================
    Prompt: The future of AI is
    Generated text:  complex and uncertain. Here's what to keep in mind as the technology evolves.
    By Richard C. Hoyle, Chief Data Officer, PwC LLP
    Over the last decade, the field of artificial intelligence has emerged as a burgeoning area of innovation. In the past few years, there has been significant progress in developing a diverse range of AI technologies and tools for a wide range of applications. The future of AI is complex and uncertain, as it may be the case that some areas of AI will be significantly more advanced than others. This article explores some of the key trends and challenges in the future of AI.
    One of the most widely


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light, a historic and cultural center with a rich history dating back to the Middle Ages. The city is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum, as well as its vibrant arts scene and annual festivals. Paris is also a major transportation hub, with the Eiffel Tower serving as a symbol of the city's status as a global hub for tourism and commerce. The city is home to many world-renowned museums, including the Louvre and the Musée d'Orsay, and is a popular destination for tourists and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way that AI is used and developed. Here are some of the most likely trends that are expected to shape the future of AI:
    
    1. Increased use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes, reduce costs, and improve the quality of care. As AI technology continues to improve, we can expect to see even more widespread use of AI in healthcare.
    
    2. Increased use of AI in finance: AI is already being used in finance to improve fraud detection, risk management, and investment decision-making. As AI technology continues to improve
    


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
    Generated text:  [Name], and I'm a [X] who enjoys [X] sports, [X] novels, and [X] travel. I'm currently [X] and have a passion for [X]. What's your name, and what do you do for a living? [Name]: Hello, my name is [Name]. I'm a [X] who enjoys [X] sports, [X] novels, and [X] travel. I'm currently [X] and have a passion for [X]. What's your name, and what do you do for a living? [Name]: [Name] [Name]:
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    I apologize, but I cannot provide a factual statement about Paris as it is a city and not a country. Paris is the capital and largest city of France, located on the Seine River in the Île de France, and is known for its stunning architecture, rich history, and numerous museums, including the Louvre. The city is also famous for its culinary traditions, including its famous desserts and wine, and for hosting numerous world-famous events. However, I cannot provide the statement you asked for without referring to a historical text or source. 
    
    If you'd like a general description of Paris, here's what you might
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to be shaped by a range of factors, including advances in technology, changing societal needs, and the increasing complexity of the challenges we face. Here are some possible future trends in AI:
    
    1. Improved accuracy and reliability: AI systems are getting better at performing complex tasks and making accurate predictions, but they are also becoming more complex and less predictable. As research continues to improve machine learning algorithms and neural networks, we can expect to see even more accurate and reliable AI systems.
    
    2. Enhanced privacy and security: AI systems are becoming more pervasive in our daily lives, and there is a growing concern about the potential risks associated with their use.


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

     a

     [

    age

    ]

     year

     old

     [

    language

    ]

     native

     speaker

    .

     I

     have

     a

     love

     for

     [

    your

     profession

     or

     occupation

    ]

     and

     am

     always

     up

     for

     learning

     new

     skills

     and

     attending

     [

    type

     of

     event

     or

     club

    ].

     I

    'm

     friendly

    ,

     outgoing

    ,

     and

     always

     ready

     to

     help

     others

     with

     my

     knowledge

     and

     skills

    .

     If

     you

    're

     interested

     in

     getting

     to

     know

     me

     better

    ,

     please

     feel

     free

     to

     reach

     out

    .

     Let

    's

     chat

    !

     Here

    ’s

     a

     simple

     self

    -int

    roduction

     for

     a

     fictional

     character

    :
    


    ---
    


    **

    Name

    :**

     Alex

    


    **

    Age

    :**

     

    2

    5

    


    **

    Language

    :**

     English

    


    **

    Prof

    ession

    :**

     Graphic

     Designer

    


    **

    Languages

    :**

     Spanish

    ,

    
    
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

     Lou

    vre

     Museum

    ,

     and

     Notre

    -D

    ame

     Cathedral

    .


    Paris

    ,

     often

     referred

     to

     as

     "

    La

     Ville

     Fl

    ott

    ante

    ,"

     is

     the

     capital

     city

     of

     France

     and

     one

     of

     the

     largest

     and

     most

     populous

     cities

     in

     the

     world

    .

     It

     is

     home

     to

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

    ,

     among

     other

     attractions

    .

     Paris

     is

     also

     known

     for

     its

     rich

     cultural

     heritage

     and

     bustling

     nightlife

    .

     With

     a

     population

     of

     over

     

    2

     million

     people

    ,

     it

     is

     the

     most

     populous

     city

     in

     France

     and

     the

     third

     most

     populous

     in

     the

     world

    .

     The

     city

     is

     also

     home

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     characterized

     by

     a

     number

     of

     possible

     trends

    ,

     including

    :
    


    1

    .

     AI

     will

     become

     more

     advanced

     and

     integrated

     into

     human

     behavior

     and

     decision

    -making

     processes

    .

     AI

     will

     be

     able

     to

     analyze

     large

     amounts

     of

     data

     and

     make

     predictions

     and

     recommendations

     more

     accurately

     than

     ever

     before

    .
    


    2

    .

     AI

     will

     continue

     to

     improve

     its

     ability

     to

     learn

     and

     adapt

     to

     new

     situations

    .

     As

     AI

     systems

     become

     more

     sophisticated

    ,

     they

     will

     be

     able

     to

     learn

     from

     experience

     and

     make

     better

     decisions

     in

     new

     situations

    .
    


    3

    .

     AI

     will

     become

     more

     affordable

     and

     accessible

     to

     individuals

     and

     businesses

    .

     As

     technology

     advances

    ,

     the

     cost

     of

     AI

     will

     decrease

    ,

     making

     it

     more

     affordable

     for

     people

     and

     businesses

     of

     all

     sizes

     to

    



```python
llm.shutdown()
```
