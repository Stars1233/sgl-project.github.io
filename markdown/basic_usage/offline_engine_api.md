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


    2026-04-03 08:19:35.098 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 08:19:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 08:19:35.098 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 08:19:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 08:19:35.098 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 08:19:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 08:19:35.098 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 08:19:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 08:19:35.098 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 08:19:35] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.10it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.09it/s]


    2026-04-03 08:19:39,653 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 08:19:39] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:28,  2.61s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:28,  2.61s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:28,  2.61s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:28,  2.61s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:28,  1.92it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:28,  1.92it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:28,  1.92it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:28,  1.92it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:28,  1.92it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:28,  1.92it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:28,  1.92it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.89it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.89it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.89it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.89it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.89it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.89it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.89it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.89it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:08,  5.89it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:03, 12.48it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:03, 12.48it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:03, 12.48it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:03, 12.48it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:03, 12.48it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:03, 12.48it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:03, 12.48it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 12.48it/s]Compiling num tokens (num_tokens=704):  31%|███       | 18/58 [00:03<00:03, 12.48it/s]

    Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 19.93it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 19.93it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 19.93it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 19.93it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 19.93it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 19.93it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 19.93it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 19.93it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:01, 24.22it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:01, 24.22it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:01, 24.22it/s]

    Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:01, 24.22it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:01, 24.22it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:01, 24.22it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:01, 24.22it/s]Compiling num tokens (num_tokens=208):  57%|█████▋    | 33/58 [00:03<00:01, 24.22it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:03<00:00, 30.87it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:03<00:00, 30.87it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:03<00:00, 30.87it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:03<00:00, 30.87it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:03<00:00, 30.87it/s]Compiling num tokens (num_tokens=128):  69%|██████▉   | 40/58 [00:03<00:00, 30.87it/s]Compiling num tokens (num_tokens=112):  69%|██████▉   | 40/58 [00:03<00:00, 30.87it/s]Compiling num tokens (num_tokens=96):  69%|██████▉   | 40/58 [00:03<00:00, 30.87it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:03<00:00, 37.28it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:03<00:00, 37.28it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:03<00:00, 37.28it/s]

    Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:03<00:00, 37.28it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:03<00:00, 37.28it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:03<00:00, 37.28it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:03<00:00, 37.28it/s]Compiling num tokens (num_tokens=20):  81%|████████  | 47/58 [00:03<00:00, 37.28it/s]Compiling num tokens (num_tokens=16):  81%|████████  | 47/58 [00:03<00:00, 37.28it/s]Compiling num tokens (num_tokens=12):  81%|████████  | 47/58 [00:03<00:00, 37.28it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:03<00:00, 47.12it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:03<00:00, 47.12it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:03<00:00, 47.12it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.39it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=76.80 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.77 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.77 GB):   3%|▎         | 2/58 [00:00<00:02, 19.38it/s]Capturing num tokens (num_tokens=7168 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:02, 19.38it/s]Capturing num tokens (num_tokens=6656 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:02, 19.38it/s]Capturing num tokens (num_tokens=6144 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:02, 19.38it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 22.15it/s]Capturing num tokens (num_tokens=5632 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 22.15it/s]Capturing num tokens (num_tokens=5120 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 22.15it/s]Capturing num tokens (num_tokens=4608 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 22.15it/s]Capturing num tokens (num_tokens=4096 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 22.15it/s]Capturing num tokens (num_tokens=4096 avail_mem=76.76 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.92it/s]Capturing num tokens (num_tokens=3840 avail_mem=76.75 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.92it/s]Capturing num tokens (num_tokens=3584 avail_mem=76.75 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.92it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=76.74 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.92it/s]Capturing num tokens (num_tokens=3328 avail_mem=76.74 GB):  21%|██        | 12/58 [00:00<00:01, 27.10it/s]Capturing num tokens (num_tokens=3072 avail_mem=76.74 GB):  21%|██        | 12/58 [00:00<00:01, 27.10it/s]Capturing num tokens (num_tokens=2816 avail_mem=76.74 GB):  21%|██        | 12/58 [00:00<00:01, 27.10it/s]Capturing num tokens (num_tokens=2560 avail_mem=76.73 GB):  21%|██        | 12/58 [00:00<00:01, 27.10it/s]Capturing num tokens (num_tokens=2304 avail_mem=76.73 GB):  21%|██        | 12/58 [00:00<00:01, 27.10it/s]Capturing num tokens (num_tokens=2048 avail_mem=76.73 GB):  21%|██        | 12/58 [00:00<00:01, 27.10it/s]Capturing num tokens (num_tokens=2048 avail_mem=76.73 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.71it/s]Capturing num tokens (num_tokens=1792 avail_mem=76.72 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.71it/s]Capturing num tokens (num_tokens=1536 avail_mem=76.72 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.71it/s]Capturing num tokens (num_tokens=1280 avail_mem=76.72 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.71it/s]Capturing num tokens (num_tokens=1024 avail_mem=76.70 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.71it/s]

    Capturing num tokens (num_tokens=960 avail_mem=76.71 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.71it/s] Capturing num tokens (num_tokens=896 avail_mem=76.71 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.71it/s]Capturing num tokens (num_tokens=896 avail_mem=76.71 GB):  40%|███▉      | 23/58 [00:00<00:00, 39.90it/s]Capturing num tokens (num_tokens=832 avail_mem=76.70 GB):  40%|███▉      | 23/58 [00:00<00:00, 39.90it/s]Capturing num tokens (num_tokens=768 avail_mem=76.70 GB):  40%|███▉      | 23/58 [00:00<00:00, 39.90it/s]Capturing num tokens (num_tokens=704 avail_mem=76.70 GB):  40%|███▉      | 23/58 [00:00<00:00, 39.90it/s]Capturing num tokens (num_tokens=640 avail_mem=76.69 GB):  40%|███▉      | 23/58 [00:00<00:00, 39.90it/s]Capturing num tokens (num_tokens=576 avail_mem=76.69 GB):  40%|███▉      | 23/58 [00:00<00:00, 39.90it/s]Capturing num tokens (num_tokens=512 avail_mem=76.68 GB):  40%|███▉      | 23/58 [00:00<00:00, 39.90it/s]Capturing num tokens (num_tokens=512 avail_mem=76.68 GB):  50%|█████     | 29/58 [00:00<00:00, 44.08it/s]Capturing num tokens (num_tokens=480 avail_mem=76.70 GB):  50%|█████     | 29/58 [00:00<00:00, 44.08it/s]Capturing num tokens (num_tokens=448 avail_mem=76.70 GB):  50%|█████     | 29/58 [00:00<00:00, 44.08it/s]Capturing num tokens (num_tokens=416 avail_mem=76.69 GB):  50%|█████     | 29/58 [00:00<00:00, 44.08it/s]

    Capturing num tokens (num_tokens=384 avail_mem=76.69 GB):  50%|█████     | 29/58 [00:00<00:00, 44.08it/s]Capturing num tokens (num_tokens=352 avail_mem=76.69 GB):  50%|█████     | 29/58 [00:00<00:00, 44.08it/s]Capturing num tokens (num_tokens=352 avail_mem=76.69 GB):  59%|█████▊    | 34/58 [00:00<00:00, 45.68it/s]Capturing num tokens (num_tokens=320 avail_mem=76.68 GB):  59%|█████▊    | 34/58 [00:00<00:00, 45.68it/s]Capturing num tokens (num_tokens=288 avail_mem=76.68 GB):  59%|█████▊    | 34/58 [00:00<00:00, 45.68it/s]Capturing num tokens (num_tokens=256 avail_mem=76.68 GB):  59%|█████▊    | 34/58 [00:00<00:00, 45.68it/s]Capturing num tokens (num_tokens=240 avail_mem=76.68 GB):  59%|█████▊    | 34/58 [00:00<00:00, 45.68it/s]Capturing num tokens (num_tokens=224 avail_mem=76.67 GB):  59%|█████▊    | 34/58 [00:00<00:00, 45.68it/s]Capturing num tokens (num_tokens=224 avail_mem=76.67 GB):  67%|██████▋   | 39/58 [00:01<00:00, 45.54it/s]Capturing num tokens (num_tokens=208 avail_mem=76.67 GB):  67%|██████▋   | 39/58 [00:01<00:00, 45.54it/s]Capturing num tokens (num_tokens=192 avail_mem=76.67 GB):  67%|██████▋   | 39/58 [00:01<00:00, 45.54it/s]Capturing num tokens (num_tokens=176 avail_mem=76.66 GB):  67%|██████▋   | 39/58 [00:01<00:00, 45.54it/s]

    Capturing num tokens (num_tokens=160 avail_mem=76.66 GB):  67%|██████▋   | 39/58 [00:01<00:00, 45.54it/s]Capturing num tokens (num_tokens=144 avail_mem=76.66 GB):  67%|██████▋   | 39/58 [00:01<00:00, 45.54it/s]Capturing num tokens (num_tokens=128 avail_mem=76.65 GB):  67%|██████▋   | 39/58 [00:01<00:00, 45.54it/s]Capturing num tokens (num_tokens=128 avail_mem=76.65 GB):  78%|███████▊  | 45/58 [00:01<00:00, 47.85it/s]Capturing num tokens (num_tokens=112 avail_mem=76.65 GB):  78%|███████▊  | 45/58 [00:01<00:00, 47.85it/s]Capturing num tokens (num_tokens=96 avail_mem=76.65 GB):  78%|███████▊  | 45/58 [00:01<00:00, 47.85it/s] Capturing num tokens (num_tokens=80 avail_mem=76.64 GB):  78%|███████▊  | 45/58 [00:01<00:00, 47.85it/s]Capturing num tokens (num_tokens=64 avail_mem=76.64 GB):  78%|███████▊  | 45/58 [00:01<00:00, 47.85it/s]Capturing num tokens (num_tokens=48 avail_mem=76.64 GB):  78%|███████▊  | 45/58 [00:01<00:00, 47.85it/s]Capturing num tokens (num_tokens=32 avail_mem=76.64 GB):  78%|███████▊  | 45/58 [00:01<00:00, 47.85it/s]Capturing num tokens (num_tokens=32 avail_mem=76.64 GB):  88%|████████▊ | 51/58 [00:01<00:00, 48.75it/s]Capturing num tokens (num_tokens=28 avail_mem=76.63 GB):  88%|████████▊ | 51/58 [00:01<00:00, 48.75it/s]Capturing num tokens (num_tokens=24 avail_mem=76.63 GB):  88%|████████▊ | 51/58 [00:01<00:00, 48.75it/s]

    Capturing num tokens (num_tokens=20 avail_mem=76.62 GB):  88%|████████▊ | 51/58 [00:01<00:00, 48.75it/s]Capturing num tokens (num_tokens=16 avail_mem=76.62 GB):  88%|████████▊ | 51/58 [00:01<00:00, 48.75it/s]Capturing num tokens (num_tokens=12 avail_mem=76.62 GB):  88%|████████▊ | 51/58 [00:01<00:00, 48.75it/s]Capturing num tokens (num_tokens=8 avail_mem=76.61 GB):  88%|████████▊ | 51/58 [00:01<00:00, 48.75it/s] Capturing num tokens (num_tokens=8 avail_mem=76.61 GB):  98%|█████████▊| 57/58 [00:01<00:00, 50.20it/s]Capturing num tokens (num_tokens=4 avail_mem=76.61 GB):  98%|█████████▊| 57/58 [00:01<00:00, 50.20it/s]Capturing num tokens (num_tokens=4 avail_mem=76.61 GB): 100%|██████████| 58/58 [00:01<00:00, 42.16it/s]


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
    Generated text:  Aryan, and I am an independent artist living in Denver, Colorado. I am a cartoonist and illustrator, but I am also a musician and singer, and a former rock bass player. I am a member of the band "Blowful" and I make music using a mixture of different instruments and techniques. I have a passion for music and I believe that it is the best way to express my emotions and my ideas. My music is a fusion of different genres, including indie rock, electro, and house. I believe that my music is unique and that I have a lot of talent and abilities that I have yet to fully
    ===============================
    Prompt: The president of the United States is
    Generated text:  in a new city for a state visit. On the first day, he meets with two representatives of a local business. He then proceeds to meet with three other individuals. On the third day, he meets with two of the original three individuals, two new individuals, and one of the two original representatives. If the number of people met with in the first day was 40, what is the total number of people he met with over the two days?
    To determine the total number of people the president of the United States met with over the two days, we need to break down the problem step by step.
    
    1. **First Day
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. The capital of Italy is Rome. The capital of Germany is Berlin. If the capital of France changes to Paris and the capital of Italy to Rome, what will happen?  Answer choices: - change in population  - It will become a city  - it will become a place  - it will become a country  - it will become a place  - The capital of Germany will change  - it will become a city  - it will become a place  - it will become a state
    Answer:
    
    It will become a city. The capital of France, Paris, will change to Paris and the capital of Italy, Rome
    ===============================
    Prompt: The future of AI is
    Generated text:  getting better and better, and one of the promising areas to look at is the field of medical imaging. This is the study of how doctors and other medical professionals use X-rays, CT scans, MRI’s and other imaging methods to diagnose illnesses and injuries.
    With advances in medical technology, it is becoming possible to use imaging technologies on a more frequent basis. For instance, some doctors are looking at the possibility of using an MRI machine to take pictures of their patients for their medical records. Other medical professionals are also working on new ways to analyze medical images to make more accurate diagnoses.
    AI is playing a crucial role in improving medical imaging. It


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


    Generated text:  [Name], and I'm a [Job Title] at [Company Name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [Age], [Gender], [Nationality], [Occupation], and I have [Number] years of experience in [Field of Work]. I'm always looking for new challenges and opportunities to grow and learn. What do you do for a living? I'm always looking for new challenges and opportunities to grow and learn. What do you enjoy doing? I enjoy [What I Enjoy Doing]. What do you like to do for a living?
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic Eiffel Tower, Notre-Dame Cathedral, and diverse cultural scene. It is also home to the Louvre Museum, the most famous art museum in the world, and the Notre-Dame Cathedral. Paris is a bustling city with a rich history and a vibrant cultural scene, making it a popular tourist destination. The city is also known for its fashion industry, with Paris Fashion Week being one of the largest in the world. Paris is a city of contrasts, with its modern architecture and historical landmarks blending together to create a unique and fascinating city. The city is also home to many famous museums, including the
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we interact with technology and the world around us. Here are some of the most likely future trends in AI:
    
    1. Increased automation and artificial intelligence: As AI becomes more advanced, it is likely to automate many of the tasks that are currently done by humans, such as data analysis, decision-making, and problem-solving. This could lead to increased efficiency and productivity, but it could also lead to job displacement for some workers.
    
    2. Improved privacy and security: As AI becomes more sophisticated, there is a risk that it could be used for malicious purposes, such
    


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
    Generated text:  [Your Name]. I'm a [Type of Character] who has been traveling around the world to [Name of Place]. I'm a [Type of Character] who enjoys [Reason for Interest in World Travels]. I hope to travel on [Name of Traveling Place] for as long as I can. I'm always looking for new experiences and adventures to share. What's your story, and what would you like to share? Please describe your travels and your journey to the world. I'd love to hear your experiences and hope to see you in the world with me. [Your Name] [Your Location] 
    Once upon
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, commonly referred to as "the City of Light". The city is famous for its architecture, cuisine, and artistic scene, and has been a major cultural center for centuries. It is home to numerous landmarks, including the Eiffel Tower and the Louvre Museum, and is a popular tourist destination. Paris is also known for its rich history and diverse population, with many French cities located nearby. The city has been heavily affected by the 2015–2016 Paris earthquake, which caused widespread damage to its infrastructure and contributed to the city's economic downturn. As a result, Paris has had to make significant
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  quite exciting and full of potential possibilities. Here are some possible trends that could shape the direction of AI in the coming years:
    
    1. Increased focus on ethical AI: AI is becoming more and more complex, with the potential to cause harm if not properly regulated. Therefore, there will likely be greater emphasis on ethical considerations in AI development, with developers working to ensure that AI systems are designed to be fair, transparent, and responsible.
    
    2. More machine learning algorithms: As AI becomes more complex, there will likely be more sophisticated machine learning algorithms that can better understand and solve complex problems. This could include things like self-driving cars, smart home


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

    occupation

    ]

     from

     [

    country

    ]

     who

     is

     a

     [

    job

     title

    ]

     with

     [

    number

     of

     years

     of

     experience

    ]

     years

     of

     experience

     in

     [

    industry

     or

     field

    ].

     I

     love

     [

    reason

     for

     love

     or

     interest

    ]

     and

     [

    why

     this

     interest

     is

     important

     to

     you

    ].

     I

     enjoy

     [

    reason

     for

     doing

     this

    ].

     [

    Name

    ]

     has

     a

     great

     ability

     to

     [

    describe

     how

     they

     do

     it

    ,

     such

     as

    :

     [

    skill

    ],

     [

    habit

    ],

     [

    skill

     set

    ],

     etc

    .]

     and

     has

     always

     been

     passionate

     about

     [

    reason

     for

     it

    ].

     I

     believe

     in

     [

    reason

     for

     belief

     in

     your

     ability

     to

     do

     what

     you

     do

    ].

     I

     am

     a

     [

    type

     of

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     located

     on

     the

     banks

     of

     the

     Se

    ine

     River

     in

     the

     central

     part

     of

     the

     country

    .

     It

     is

     a

     historical

     and

     cultural

     center

     with

     many

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

     Notre

    -D

    ame

     Cathedral

    ,

     Lou

    vre

     Museum

    ,

     and

     the

     Ch

    amps

    -

    É

    lys

    ées

    .

     The

     city

     is

     also

     known

     for

     its

     rich

     French

     cuisine

    ,

     art

    ,

     and

     music

     scene

    .

     Paris

     is

     a

     major

     global

     city

     and

     one

     of

     the

     most

     visited

     tourist

     destinations

     in

     the

     world

    .

     It

     is

     home

     to

     several

     UNESCO

     World

     Heritage

     Sites

     and

     hosts

     the

     world

    ’s

     largest

     outdoor

     concert

     venue

    ,

     the

     E

    iff

    el

     Tower

    ,

     in

     its

     nearby

     Ch

    amps

    -

    É

    lys

    ées

     district

    .

     Paris

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     looking

     very

     promising

    ,

     and

     there

     are

     several

     trends

     that

     are

     likely

     to

     shape

     the

     technology

    's

     development

    :
    


    1

    .

     Deep

     Learning

    :

     Deep

     learning

     is

     one

     of

     the

     most

     promising

     areas

     of

     AI

     research

    ,

     and

     it

     has

     shown

     remarkable

     success

     in

     a

     variety

     of

     applications

    .

     It

     involves

     training

     artificial

     neural

     networks

     to

     learn

     from

     data

    ,

     which

     allows

     them

     to

     perform

     complex

     tasks

     on

     their

     own

    .

     Deep

     learning

     is

     expected

     to

     continue

     to

     evolve

     and

     become

     more

     sophisticated

    ,

     leading

     to

     breakthrough

    s

     in

     fields

     such

     as

     medical

     diagnosis

    ,

     autonomous

     vehicles

    ,

     and

     natural

     language

     processing

    .
    


    2

    .

     Internet

     of

     Things

     (

    Io

    T

    ):

     The

     Internet

     of

     Things

     is

     a

     major

     development

     in

     the

     field

     of

     AI

    ,

     as

     it

    



```python
llm.shutdown()
```
