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

    Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  3.27it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  3.27it/s]


    2026-04-04 09:26:49,170 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 09:26:49] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:44,  2.89s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:44,  2.89s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:44,  2.89s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:44,  2.89s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:44,  2.89s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:24,  2.19it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:24,  2.19it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:03<00:24,  2.19it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:03<00:24,  2.19it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:03<00:24,  2.19it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:03<00:24,  2.19it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:03<00:24,  2.19it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:03<00:08,  5.78it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:03<00:08,  5.78it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:03<00:08,  5.78it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:03<00:08,  5.78it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:03<00:08,  5.78it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:03<00:08,  5.78it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:03<00:08,  5.78it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:03<00:08,  5.78it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:03<00:08,  5.78it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 11.79it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 11.79it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 11.79it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 11.79it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 11.79it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 11.79it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 11.79it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 11.79it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 17.64it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 17.64it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 17.64it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 17.64it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 17.64it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 17.64it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 17.64it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 17.64it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:01, 24.11it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:01, 24.11it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:01, 24.11it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:01, 24.11it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:01, 24.11it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:01, 24.11it/s]

    Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:01, 24.11it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 28.71it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 28.71it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 28.71it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 28.71it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 28.71it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 28.71it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 28.71it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 33.71it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 33.71it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 33.71it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 33.71it/s]

    Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 33.71it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 33.71it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 33.71it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 37.20it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 37.20it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 37.20it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 37.20it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 37.20it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 37.20it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 37.20it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 37.20it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 14.96it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=131.67 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=131.64 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=131.64 GB):   3%|▎         | 2/58 [00:00<00:02, 18.81it/s]Capturing num tokens (num_tokens=7168 avail_mem=131.64 GB):   3%|▎         | 2/58 [00:00<00:02, 18.81it/s]Capturing num tokens (num_tokens=6656 avail_mem=131.64 GB):   3%|▎         | 2/58 [00:00<00:02, 18.81it/s]Capturing num tokens (num_tokens=6144 avail_mem=131.64 GB):   3%|▎         | 2/58 [00:00<00:02, 18.81it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=131.64 GB):   9%|▊         | 5/58 [00:00<00:02, 22.00it/s]Capturing num tokens (num_tokens=5632 avail_mem=131.63 GB):   9%|▊         | 5/58 [00:00<00:02, 22.00it/s]Capturing num tokens (num_tokens=5120 avail_mem=131.63 GB):   9%|▊         | 5/58 [00:00<00:02, 22.00it/s]Capturing num tokens (num_tokens=4608 avail_mem=131.63 GB):   9%|▊         | 5/58 [00:00<00:02, 22.00it/s]Capturing num tokens (num_tokens=4096 avail_mem=131.63 GB):   9%|▊         | 5/58 [00:00<00:02, 22.00it/s]Capturing num tokens (num_tokens=4096 avail_mem=131.63 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.79it/s]Capturing num tokens (num_tokens=3840 avail_mem=131.63 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.79it/s]Capturing num tokens (num_tokens=3584 avail_mem=131.62 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.79it/s]Capturing num tokens (num_tokens=3328 avail_mem=131.62 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.79it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=131.62 GB):  21%|██        | 12/58 [00:00<00:01, 24.80it/s]Capturing num tokens (num_tokens=3072 avail_mem=131.61 GB):  21%|██        | 12/58 [00:00<00:01, 24.80it/s]Capturing num tokens (num_tokens=2816 avail_mem=131.61 GB):  21%|██        | 12/58 [00:00<00:01, 24.80it/s]Capturing num tokens (num_tokens=2560 avail_mem=131.61 GB):  21%|██        | 12/58 [00:00<00:01, 24.80it/s]Capturing num tokens (num_tokens=2560 avail_mem=131.61 GB):  26%|██▌       | 15/58 [00:00<00:01, 23.24it/s]Capturing num tokens (num_tokens=2304 avail_mem=131.61 GB):  26%|██▌       | 15/58 [00:00<00:01, 23.24it/s]Capturing num tokens (num_tokens=2048 avail_mem=131.60 GB):  26%|██▌       | 15/58 [00:00<00:01, 23.24it/s]Capturing num tokens (num_tokens=1792 avail_mem=131.60 GB):  26%|██▌       | 15/58 [00:00<00:01, 23.24it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=131.60 GB):  26%|██▌       | 15/58 [00:00<00:01, 23.24it/s]Capturing num tokens (num_tokens=1280 avail_mem=131.59 GB):  26%|██▌       | 15/58 [00:00<00:01, 23.24it/s]Capturing num tokens (num_tokens=1280 avail_mem=131.59 GB):  34%|███▍      | 20/58 [00:00<00:01, 28.68it/s]Capturing num tokens (num_tokens=1024 avail_mem=131.57 GB):  34%|███▍      | 20/58 [00:00<00:01, 28.68it/s]Capturing num tokens (num_tokens=960 avail_mem=131.58 GB):  34%|███▍      | 20/58 [00:00<00:01, 28.68it/s] Capturing num tokens (num_tokens=896 avail_mem=131.58 GB):  34%|███▍      | 20/58 [00:00<00:01, 28.68it/s]Capturing num tokens (num_tokens=832 avail_mem=131.58 GB):  34%|███▍      | 20/58 [00:00<00:01, 28.68it/s]Capturing num tokens (num_tokens=832 avail_mem=131.58 GB):  41%|████▏     | 24/58 [00:00<00:01, 31.15it/s]Capturing num tokens (num_tokens=768 avail_mem=131.57 GB):  41%|████▏     | 24/58 [00:00<00:01, 31.15it/s]

    Capturing num tokens (num_tokens=704 avail_mem=131.57 GB):  41%|████▏     | 24/58 [00:00<00:01, 31.15it/s]Capturing num tokens (num_tokens=640 avail_mem=131.57 GB):  41%|████▏     | 24/58 [00:00<00:01, 31.15it/s]Capturing num tokens (num_tokens=576 avail_mem=131.57 GB):  41%|████▏     | 24/58 [00:01<00:01, 31.15it/s]Capturing num tokens (num_tokens=576 avail_mem=131.57 GB):  48%|████▊     | 28/58 [00:01<00:01, 28.93it/s]Capturing num tokens (num_tokens=512 avail_mem=131.56 GB):  48%|████▊     | 28/58 [00:01<00:01, 28.93it/s]Capturing num tokens (num_tokens=480 avail_mem=131.57 GB):  48%|████▊     | 28/58 [00:01<00:01, 28.93it/s]Capturing num tokens (num_tokens=448 avail_mem=131.57 GB):  48%|████▊     | 28/58 [00:01<00:01, 28.93it/s]Capturing num tokens (num_tokens=416 avail_mem=131.57 GB):  48%|████▊     | 28/58 [00:01<00:01, 28.93it/s]Capturing num tokens (num_tokens=416 avail_mem=131.57 GB):  55%|█████▌    | 32/58 [00:01<00:00, 30.72it/s]Capturing num tokens (num_tokens=384 avail_mem=131.57 GB):  55%|█████▌    | 32/58 [00:01<00:00, 30.72it/s]

    Capturing num tokens (num_tokens=352 avail_mem=131.56 GB):  55%|█████▌    | 32/58 [00:01<00:00, 30.72it/s]Capturing num tokens (num_tokens=320 avail_mem=131.56 GB):  55%|█████▌    | 32/58 [00:01<00:00, 30.72it/s]Capturing num tokens (num_tokens=288 avail_mem=131.55 GB):  55%|█████▌    | 32/58 [00:01<00:00, 30.72it/s]Capturing num tokens (num_tokens=256 avail_mem=131.55 GB):  55%|█████▌    | 32/58 [00:01<00:00, 30.72it/s]Capturing num tokens (num_tokens=256 avail_mem=131.55 GB):  64%|██████▍   | 37/58 [00:01<00:00, 33.74it/s]Capturing num tokens (num_tokens=240 avail_mem=131.55 GB):  64%|██████▍   | 37/58 [00:01<00:00, 33.74it/s]Capturing num tokens (num_tokens=224 avail_mem=131.54 GB):  64%|██████▍   | 37/58 [00:01<00:00, 33.74it/s]Capturing num tokens (num_tokens=208 avail_mem=131.54 GB):  64%|██████▍   | 37/58 [00:01<00:00, 33.74it/s]

    Capturing num tokens (num_tokens=192 avail_mem=131.54 GB):  64%|██████▍   | 37/58 [00:01<00:00, 33.74it/s]Capturing num tokens (num_tokens=192 avail_mem=131.54 GB):  71%|███████   | 41/58 [00:01<00:00, 18.99it/s]Capturing num tokens (num_tokens=176 avail_mem=131.54 GB):  71%|███████   | 41/58 [00:01<00:00, 18.99it/s]Capturing num tokens (num_tokens=160 avail_mem=131.53 GB):  71%|███████   | 41/58 [00:01<00:00, 18.99it/s]Capturing num tokens (num_tokens=144 avail_mem=131.53 GB):  71%|███████   | 41/58 [00:01<00:00, 18.99it/s]Capturing num tokens (num_tokens=128 avail_mem=131.53 GB):  71%|███████   | 41/58 [00:01<00:00, 18.99it/s]Capturing num tokens (num_tokens=128 avail_mem=131.53 GB):  78%|███████▊  | 45/58 [00:01<00:00, 21.97it/s]Capturing num tokens (num_tokens=112 avail_mem=131.53 GB):  78%|███████▊  | 45/58 [00:01<00:00, 21.97it/s]Capturing num tokens (num_tokens=96 avail_mem=131.52 GB):  78%|███████▊  | 45/58 [00:01<00:00, 21.97it/s] Capturing num tokens (num_tokens=80 avail_mem=131.52 GB):  78%|███████▊  | 45/58 [00:01<00:00, 21.97it/s]

    Capturing num tokens (num_tokens=64 avail_mem=131.51 GB):  78%|███████▊  | 45/58 [00:01<00:00, 21.97it/s]Capturing num tokens (num_tokens=48 avail_mem=131.51 GB):  78%|███████▊  | 45/58 [00:01<00:00, 21.97it/s]Capturing num tokens (num_tokens=48 avail_mem=131.51 GB):  86%|████████▌ | 50/58 [00:01<00:00, 26.50it/s]Capturing num tokens (num_tokens=32 avail_mem=131.51 GB):  86%|████████▌ | 50/58 [00:01<00:00, 26.50it/s]Capturing num tokens (num_tokens=28 avail_mem=131.50 GB):  86%|████████▌ | 50/58 [00:01<00:00, 26.50it/s]Capturing num tokens (num_tokens=24 avail_mem=131.50 GB):  86%|████████▌ | 50/58 [00:01<00:00, 26.50it/s]Capturing num tokens (num_tokens=20 avail_mem=131.50 GB):  86%|████████▌ | 50/58 [00:01<00:00, 26.50it/s]Capturing num tokens (num_tokens=16 avail_mem=131.50 GB):  86%|████████▌ | 50/58 [00:02<00:00, 26.50it/s]Capturing num tokens (num_tokens=16 avail_mem=131.50 GB):  95%|█████████▍| 55/58 [00:02<00:00, 30.95it/s]Capturing num tokens (num_tokens=12 avail_mem=131.49 GB):  95%|█████████▍| 55/58 [00:02<00:00, 30.95it/s]Capturing num tokens (num_tokens=8 avail_mem=131.49 GB):  95%|█████████▍| 55/58 [00:02<00:00, 30.95it/s] Capturing num tokens (num_tokens=4 avail_mem=131.48 GB):  95%|█████████▍| 55/58 [00:02<00:00, 30.95it/s]

    Capturing num tokens (num_tokens=4 avail_mem=131.48 GB): 100%|██████████| 58/58 [00:02<00:00, 27.70it/s]


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
    Generated text:  Serezha and I am a rebellious and morally ambiguous character from the DC Universe. I have been battling the powers of Gorgonia and the evil forces of Amaterasu. I was once a normal human but became the leader of the group of rebels and my true identity was hidden from the world until now.
    
    My main goals are to protect the Earth from Gorgonia's evil and to find a way to stop Amaterasu from returning to her throne. I am currently working to make peace between the Earth and Gorgonia by developing technology to fuse their power and preventing any attacks on the Earth. I also want to help
    ===============================
    Prompt: The president of the United States is
    Generated text:  a very busy person. He has to answer many calls from people who want to talk to him. The president is not allowed to answer phone calls from the media. The president is very busy at home. He has to watch TV and read newspapers. The president doesn't have much time to eat or sleep. Sometimes he has to sleep in, but he usually doesn't. It's very hard for the president to take care of his family. He has to do many jobs. He has to go to work, come back home, and take care of his family. Sometimes he has to go to work and come back home later. This
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, located in the central region of France. Given a list of categories: company, educational institution, artist, athlete, office holder, mean of transportation, building, natural place, village, animal, plant, album, film, gender role, object, symbol, building place, with the option to search further by frequency or based on material, what category does the given data belong to?
    Data: name: France; country: France; flag: flags of France
    The category that this data belongs to is "country". The data provides information about the country of France, including its name, the country it is located in, and
    ===============================
    Prompt: The future of AI is
    Generated text:  looking pretty bright. The last decade has seen impressive progress in AI research and development, and more research will be coming to fruition in the future.
    In a 2016 article in the Scientific American, one of the top leaders of the field, Layne Siegel, explained that AI has the potential to revolutionize the way that we think about social and political issues. He wrote:
    “AI has the potential to push the boundaries of social and political discourse and to reconfigure the very nature of institutions such as government and the media. It’s true that the speed and scale of AI progress – the pace of machine learning, the


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a short, interesting fact about yourself]. I'm always looking for new challenges and opportunities to grow and learn. What do you do for a living? I'm always looking for new challenges and opportunities to grow and learn. What do you enjoy doing? I enjoy reading, traveling, and spending time with my family. What do you like to do in your free time? I like to read, travel, and spend time with my family
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris. 
    
    This statement is factually correct and provides a clear and concise overview of the capital city's name and location. However, it could be expanded to include additional information about Paris's cultural significance, historical landmarks, or notable events that have taken place there. For example, the statement could be expanded to: "Paris, the capital of France, is known for its rich history, vibrant culture, and numerous iconic landmarks, including the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral." This expanded statement provides more context and detail about Paris's unique character and attractions. 
    
    Overall, the statement is a good starting
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in several key areas, including:
    
    1. Increased integration with other technologies: AI is already integrated into a wide range of devices and systems, but it is likely to continue to be integrated with other technologies such as sensors, cameras, and voice recognition systems. This integration will likely lead to even more sophisticated AI systems that can better understand and interact with the world around them.
    
    2. Enhanced privacy and security: As AI systems become more sophisticated, they will need to be designed with greater privacy and security in mind. This will likely lead to increased regulation and oversight of AI systems, as well as the development of
    


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
    Generated text:  [Your Name], and I'm a [insert occupation or profession] with a passion for [insert something specific]. I'm an [insert number] in a team of [insert team members], but I also have my own goals and ambitions, and I'm always looking for new challenges and opportunities to grow. I love [insert something specific] and I'm always willing to learn, try new things, and push myself to achieve my goals. I'm confident in my abilities and I'm excited to work with [insert name of a colleague or team member]. I believe that my skills and experiences will be a valuable asset to any team,
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, known for its beautiful art galleries, landmarks like the Eiffel Tower, and bustling nightlife. Paris is also a UNESCO World Heritage site and is home to the Louvre Museum. The city is renowned for its historical and cultural significance. Paris is the political, economic, and cultural center of the French Republic. It is also the world's most populous city, with a population of over 26 million people. The French capital is often referred to as the "City of Love," thanks to its romantic and artistic neighborhoods. It is also home to the United Nations in New York City. Paris is considered one of the world's
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  not only uncertain but also highly complex, with many potential outcomes and directions of development. Here are some possible trends that are likely to emerge in the coming years:
    
    1. Increased Transparency: As AI systems get more sophisticated, their workings will become increasingly accessible to users. This will lead to greater transparency in AI, with more information being provided about how the system makes decisions. This will make it easier for users to understand how the AI system is operating and what it is doing.
    
    2. Autonomous Agents: The use of AI in autonomous vehicles, drones, and other self-driving systems is already a reality. As AI technology improves, it is likely


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

    ].

     I

    'm

     an

     experienced

     [

    job

     title

    ]

     and

     have

     been

     working

     in

     the

     [

    industry

    /

    field

    ]

     for

     [

    number

    ]

     years

    .

     I

     bring

     a

     wealth

     of

     experience

     in

     [

    specific

     area

    ]

     and

     enjoy

     helping

     others

     learn

     new

     skills

     and

     achieve

     their

     goals

    .

     I

     am

     always

     looking

     for

     new

     opportunities

     and

     am

     open

     to

     learning

     from

     new

     people

    .

     How

     can

     I

     get

     to

     know

     you

     better

    ?

     Give

     me

     an

     overview

     of

     your

     background

     and

     experience

    .

     What

     skills

     or

     knowledge

     do

     you

     bring

     to

     the

     table

     that

     you

     think

     would

     be

     valuable

     for

     others

     in

     the

     industry

    ?

     Can

     you

     share

     some

     examples

     of

     projects

     or

     projects

     you

    've

     been

     involved

     with

     that

     showcase

     your

     skills

    ?

     Finally

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     which

     is

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

    -D

    ame

     Cathedral

    ,

     and

     the

     Lou

    vre

     Museum

    .


    Paris

     is

     the

     capital

     city

     of

     France

    ,

     located

     on

     the

     western

     coast

     of

     the

     continent

     and

     known

     for

     its

     iconic

     landmarks

    ,

     including

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

     and

     the

     Lou

    vre

     Museum

    .

     The

     city

     is

     also

     known

     for

     its

     diverse

     cultural

     scene

    ,

     including

     French

     cuisine

    ,

     dance

    ,

     theater

    ,

     and

     art

    .

     Paris

     has

     a

     long

     and

     rich

     history

     dating

     back

     to

     the

     ancient

     Roman

     Empire

     and

     has

     played

     an

     important

     role

     in

     French

     national

     identity

     and

     culture

    .

     It

     is

     a

     popular

     tourist

     destination

     and

     is

     home

     to

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     exciting

     and

     constantly

     evolving

    .

     Some

     possible

     trends

     that

     are

     expected

     to

     shape

     the

     field

     include

    :
    


    1

    .

     Integration

     with

     other

     technologies

    :

     AI

     is

     already

     being

     used

     in

     other

     industries

    ,

     and

     it

     is

     likely

     that

     this

     trend

     will

     continue

     in

     the

     future

    .

     For

     example

    ,

     AI

    -powered

     assistants

     like

     Amazon

    's

     Alexa

     and

     Google

     Assistant

     have

     already

     integrated

     with

     voice

     recognition

     technology

     and

     can

     perform

     tasks

     on

     our

     behalf

    ,

     such

     as

     answering

     questions

    ,

     setting

     reminders

    ,

     and

     controlling

     various

     devices

    .
    


    2

    .

     Enhanced

     accuracy

     and

     reliability

    :

     As

     AI

     systems

     become

     more

     sophisticated

    ,

     they

     are

     likely

     to

     become

     even

     more

     accurate

     and

     reliable

    .

     This

     will

     likely

     result

     in

     a

     greater

     number

     of

     applications

     for

     AI

    ,

     such

     as

     autonomous

    



```python
llm.shutdown()
```
