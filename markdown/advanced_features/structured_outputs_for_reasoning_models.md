# Structured Outputs For Reasoning Models

When working with reasoning models that use special tokens like `<think>...</think>` to denote reasoning sections, you might want to allow free-form text within these sections while still enforcing grammar constraints on the rest of the output.

SGLang provides a feature to disable grammar restrictions within reasoning sections. This is particularly useful for models that need to perform complex reasoning steps before providing a structured output.

To enable this feature, use the `--reasoning-parser` flag which decide the think_end_token, such as `</think>`, when launching the server. You can also specify the reasoning parser using the `--reasoning-parser` flag.

## Supported Models

Currently, SGLang supports the following reasoning models:
- [DeepSeek R1 series](https://huggingface.co/collections/deepseek-ai/deepseek-r1-678e1e131c0169c0bc89728d): The reasoning content is wrapped with `<think>` and `</think>` tags.
- [QwQ](https://huggingface.co/Qwen/QwQ-32B): The reasoning content is wrapped with `<think>` and `</think>` tags.


## Usage

## OpenAI Compatible API

Specify the `--grammar-backend`, `--reasoning-parser` option.


```python
import openai
import os

from sglang.test.doc_patch import launch_server_cmd
from sglang.utils import wait_for_server, print_highlight, terminate_process

os.environ["TOKENIZERS_PARALLELISM"] = "false"


server_process, port = launch_server_cmd(
    "python -m sglang.launch_server --model-path deepseek-ai/DeepSeek-R1-Distill-Qwen-7B --host 0.0.0.0 --reasoning-parser deepseek-r1 --log-level warning"
)

wait_for_server(f"http://localhost:{port}", process=server_process)
client = openai.Client(base_url=f"http://127.0.0.1:{port}/v1", api_key="None")
```

    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:173: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-04-01 20:58:52.176 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:58:52] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 20:58:52.176 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:58:52] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 20:58:52.176 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:58:52] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 20:58:52.176 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:58:52] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 20:58:52.176 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:58:52] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.03it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.25s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.21s/it]


    2026-04-01 20:58:57,228 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 20:58:57] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:50,  2.98s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:50,  2.98s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:38,  1.76s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:38,  1.76s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:04,  1.17s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:04,  1.17s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:51,  1.06it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:51,  1.06it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:42,  1.26it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:42,  1.26it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:36,  1.43it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:36,  1.43it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:06<00:31,  1.63it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:06<00:31,  1.63it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:27,  1.84it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:27,  1.84it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:07<00:23,  2.08it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:07<00:23,  2.08it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:07<00:20,  2.31it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:07<00:20,  2.31it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:07<00:18,  2.55it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:07<00:18,  2.55it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:08<00:16,  2.79it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:08<00:16,  2.79it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:08<00:14,  3.07it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:08<00:14,  3.07it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:08<00:13,  3.33it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:08<00:13,  3.33it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:08<00:11,  3.68it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:08<00:11,  3.68it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:08<00:10,  4.04it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:08<00:10,  4.04it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:09<00:09,  4.45it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:09<00:09,  4.45it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:09<00:08,  4.97it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:09<00:08,  4.97it/s]

    Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:09<00:07,  5.42it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:09<00:07,  5.42it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:09<00:06,  6.02it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:09<00:06,  6.02it/s]

    Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:09<00:05,  6.62it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:09<00:05,  6.62it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:09<00:05,  6.62it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:09<00:04,  8.24it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:09<00:04,  8.24it/s]

    Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:09<00:04,  8.24it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:10<00:03,  9.28it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:10<00:03,  9.28it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:10<00:03,  9.28it/s]

    Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:10<00:02, 10.48it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:10<00:02, 10.48it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:10<00:02, 10.48it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:10<00:02, 11.43it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:10<00:02, 11.43it/s]

    Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:10<00:02, 11.43it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:10<00:02, 12.79it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:10<00:02, 12.79it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:10<00:02, 12.79it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:10<00:01, 13.36it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:10<00:01, 13.36it/s]

    Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:10<00:01, 13.36it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:10<00:01, 14.45it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:10<00:01, 14.45it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:10<00:01, 14.45it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:10<00:01, 14.90it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:10<00:01, 14.90it/s]

    Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:10<00:01, 14.90it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:10<00:01, 14.90it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:10<00:01, 17.02it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:10<00:01, 17.02it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:10<00:01, 17.02it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:11<00:01, 17.02it/s]

    Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:11<00:00, 18.13it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:11<00:00, 18.13it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:11<00:00, 18.13it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:11<00:00, 18.13it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:11<00:00, 19.24it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:11<00:00, 19.24it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:11<00:00, 19.24it/s]

    Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:11<00:00, 19.24it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:11<00:00, 20.12it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:11<00:00, 20.12it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:11<00:00, 20.12it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:11<00:00, 20.12it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:11<00:00, 21.81it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:11<00:00, 21.81it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:11<00:00, 21.81it/s]

    Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:11<00:00, 21.81it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:11<00:00, 22.66it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:11<00:00, 22.66it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:11<00:00, 22.66it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:11<00:00, 22.66it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:11<00:00,  4.97it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=24.58 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=24.58 GB):   2%|▏         | 1/58 [00:00<00:40,  1.39it/s]Capturing num tokens (num_tokens=7680 avail_mem=24.52 GB):   2%|▏         | 1/58 [00:00<00:40,  1.39it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=24.52 GB):   3%|▎         | 2/58 [00:01<00:35,  1.57it/s]Capturing num tokens (num_tokens=7168 avail_mem=23.49 GB):   3%|▎         | 2/58 [00:01<00:35,  1.57it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=23.49 GB):   5%|▌         | 3/58 [00:02<00:39,  1.38it/s]Capturing num tokens (num_tokens=6656 avail_mem=24.50 GB):   5%|▌         | 3/58 [00:02<00:39,  1.38it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=24.50 GB):   7%|▋         | 4/58 [00:02<00:37,  1.42it/s]Capturing num tokens (num_tokens=6144 avail_mem=23.69 GB):   7%|▋         | 4/58 [00:02<00:37,  1.42it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=23.69 GB):   9%|▊         | 5/58 [00:03<00:35,  1.49it/s]Capturing num tokens (num_tokens=5632 avail_mem=24.52 GB):   9%|▊         | 5/58 [00:03<00:35,  1.49it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=24.52 GB):  10%|█         | 6/58 [00:03<00:33,  1.56it/s]Capturing num tokens (num_tokens=5120 avail_mem=23.75 GB):  10%|█         | 6/58 [00:03<00:33,  1.56it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=23.75 GB):  12%|█▏        | 7/58 [00:04<00:30,  1.66it/s]Capturing num tokens (num_tokens=4608 avail_mem=23.89 GB):  12%|█▏        | 7/58 [00:04<00:30,  1.66it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=23.89 GB):  14%|█▍        | 8/58 [00:04<00:27,  1.79it/s]Capturing num tokens (num_tokens=4096 avail_mem=23.87 GB):  14%|█▍        | 8/58 [00:04<00:27,  1.79it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=23.87 GB):  16%|█▌        | 9/58 [00:05<00:25,  1.94it/s]Capturing num tokens (num_tokens=3840 avail_mem=23.37 GB):  16%|█▌        | 9/58 [00:05<00:25,  1.94it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=23.37 GB):  17%|█▋        | 10/58 [00:05<00:23,  2.09it/s]Capturing num tokens (num_tokens=3584 avail_mem=23.45 GB):  17%|█▋        | 10/58 [00:05<00:23,  2.09it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=23.45 GB):  19%|█▉        | 11/58 [00:06<00:20,  2.24it/s]Capturing num tokens (num_tokens=3328 avail_mem=24.09 GB):  19%|█▉        | 11/58 [00:06<00:20,  2.24it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=24.09 GB):  21%|██        | 12/58 [00:06<00:19,  2.39it/s]Capturing num tokens (num_tokens=3072 avail_mem=24.13 GB):  21%|██        | 12/58 [00:06<00:19,  2.39it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=24.13 GB):  22%|██▏       | 13/58 [00:06<00:17,  2.51it/s]Capturing num tokens (num_tokens=2816 avail_mem=23.67 GB):  22%|██▏       | 13/58 [00:06<00:17,  2.51it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=23.67 GB):  24%|██▍       | 14/58 [00:07<00:16,  2.69it/s]Capturing num tokens (num_tokens=2560 avail_mem=24.13 GB):  24%|██▍       | 14/58 [00:07<00:16,  2.69it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=24.13 GB):  26%|██▌       | 15/58 [00:07<00:14,  2.97it/s]Capturing num tokens (num_tokens=2304 avail_mem=24.57 GB):  26%|██▌       | 15/58 [00:07<00:14,  2.97it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=24.57 GB):  28%|██▊       | 16/58 [00:07<00:13,  3.22it/s]Capturing num tokens (num_tokens=2048 avail_mem=24.60 GB):  28%|██▊       | 16/58 [00:07<00:13,  3.22it/s]Capturing num tokens (num_tokens=2048 avail_mem=24.60 GB):  29%|██▉       | 17/58 [00:07<00:11,  3.63it/s]Capturing num tokens (num_tokens=1792 avail_mem=24.18 GB):  29%|██▉       | 17/58 [00:07<00:11,  3.63it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=24.18 GB):  31%|███       | 18/58 [00:08<00:10,  3.97it/s]Capturing num tokens (num_tokens=1536 avail_mem=24.56 GB):  31%|███       | 18/58 [00:08<00:10,  3.97it/s]Capturing num tokens (num_tokens=1536 avail_mem=24.56 GB):  33%|███▎      | 19/58 [00:08<00:09,  4.28it/s]Capturing num tokens (num_tokens=1280 avail_mem=24.20 GB):  33%|███▎      | 19/58 [00:08<00:09,  4.28it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=24.20 GB):  34%|███▍      | 20/58 [00:08<00:07,  5.01it/s]Capturing num tokens (num_tokens=1024 avail_mem=24.23 GB):  34%|███▍      | 20/58 [00:08<00:07,  5.01it/s]Capturing num tokens (num_tokens=1024 avail_mem=24.23 GB):  36%|███▌      | 21/58 [00:08<00:06,  5.47it/s]Capturing num tokens (num_tokens=960 avail_mem=24.41 GB):  36%|███▌      | 21/58 [00:08<00:06,  5.47it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=24.41 GB):  38%|███▊      | 22/58 [00:08<00:06,  5.75it/s]Capturing num tokens (num_tokens=896 avail_mem=24.24 GB):  38%|███▊      | 22/58 [00:08<00:06,  5.75it/s]Capturing num tokens (num_tokens=896 avail_mem=24.24 GB):  40%|███▉      | 23/58 [00:08<00:05,  6.31it/s]Capturing num tokens (num_tokens=832 avail_mem=24.54 GB):  40%|███▉      | 23/58 [00:08<00:05,  6.31it/s]

    Capturing num tokens (num_tokens=832 avail_mem=24.54 GB):  41%|████▏     | 24/58 [00:08<00:05,  6.62it/s]Capturing num tokens (num_tokens=768 avail_mem=24.53 GB):  41%|████▏     | 24/58 [00:08<00:05,  6.62it/s]Capturing num tokens (num_tokens=704 avail_mem=24.27 GB):  41%|████▏     | 24/58 [00:09<00:05,  6.62it/s]Capturing num tokens (num_tokens=704 avail_mem=24.27 GB):  45%|████▍     | 26/58 [00:09<00:04,  7.83it/s]Capturing num tokens (num_tokens=640 avail_mem=24.51 GB):  45%|████▍     | 26/58 [00:09<00:04,  7.83it/s]

    Capturing num tokens (num_tokens=640 avail_mem=24.51 GB):  47%|████▋     | 27/58 [00:09<00:03,  8.22it/s]Capturing num tokens (num_tokens=576 avail_mem=24.42 GB):  47%|████▋     | 27/58 [00:09<00:03,  8.22it/s]Capturing num tokens (num_tokens=576 avail_mem=24.42 GB):  48%|████▊     | 28/58 [00:09<00:03,  8.50it/s]Capturing num tokens (num_tokens=512 avail_mem=24.53 GB):  48%|████▊     | 28/58 [00:09<00:03,  8.50it/s]Capturing num tokens (num_tokens=480 avail_mem=24.32 GB):  48%|████▊     | 28/58 [00:09<00:03,  8.50it/s]

    Capturing num tokens (num_tokens=480 avail_mem=24.32 GB):  52%|█████▏    | 30/58 [00:09<00:02, 10.31it/s]Capturing num tokens (num_tokens=448 avail_mem=24.48 GB):  52%|█████▏    | 30/58 [00:09<00:02, 10.31it/s]Capturing num tokens (num_tokens=416 avail_mem=24.48 GB):  52%|█████▏    | 30/58 [00:09<00:02, 10.31it/s]Capturing num tokens (num_tokens=416 avail_mem=24.48 GB):  55%|█████▌    | 32/58 [00:09<00:02, 10.94it/s]Capturing num tokens (num_tokens=384 avail_mem=24.47 GB):  55%|█████▌    | 32/58 [00:09<00:02, 10.94it/s]

    Capturing num tokens (num_tokens=352 avail_mem=24.34 GB):  55%|█████▌    | 32/58 [00:09<00:02, 10.94it/s]Capturing num tokens (num_tokens=352 avail_mem=24.34 GB):  59%|█████▊    | 34/58 [00:09<00:01, 12.57it/s]Capturing num tokens (num_tokens=320 avail_mem=24.44 GB):  59%|█████▊    | 34/58 [00:09<00:01, 12.57it/s]Capturing num tokens (num_tokens=288 avail_mem=24.40 GB):  59%|█████▊    | 34/58 [00:09<00:01, 12.57it/s]Capturing num tokens (num_tokens=288 avail_mem=24.40 GB):  62%|██████▏   | 36/58 [00:09<00:01, 13.29it/s]Capturing num tokens (num_tokens=256 avail_mem=24.42 GB):  62%|██████▏   | 36/58 [00:09<00:01, 13.29it/s]

    Capturing num tokens (num_tokens=240 avail_mem=24.41 GB):  62%|██████▏   | 36/58 [00:09<00:01, 13.29it/s]Capturing num tokens (num_tokens=240 avail_mem=24.41 GB):  66%|██████▌   | 38/58 [00:10<00:01, 14.39it/s]Capturing num tokens (num_tokens=224 avail_mem=24.34 GB):  66%|██████▌   | 38/58 [00:10<00:01, 14.39it/s]Capturing num tokens (num_tokens=208 avail_mem=24.39 GB):  66%|██████▌   | 38/58 [00:10<00:01, 14.39it/s]Capturing num tokens (num_tokens=192 avail_mem=24.38 GB):  66%|██████▌   | 38/58 [00:10<00:01, 14.39it/s]

    Capturing num tokens (num_tokens=192 avail_mem=24.38 GB):  71%|███████   | 41/58 [00:10<00:01, 16.36it/s]Capturing num tokens (num_tokens=176 avail_mem=24.35 GB):  71%|███████   | 41/58 [00:10<00:01, 16.36it/s]Capturing num tokens (num_tokens=160 avail_mem=24.38 GB):  71%|███████   | 41/58 [00:10<00:01, 16.36it/s]Capturing num tokens (num_tokens=160 avail_mem=24.38 GB):  74%|███████▍  | 43/58 [00:10<00:00, 16.75it/s]Capturing num tokens (num_tokens=144 avail_mem=24.35 GB):  74%|███████▍  | 43/58 [00:10<00:00, 16.75it/s]Capturing num tokens (num_tokens=128 avail_mem=24.36 GB):  74%|███████▍  | 43/58 [00:10<00:00, 16.75it/s]

    Capturing num tokens (num_tokens=128 avail_mem=24.36 GB):  78%|███████▊  | 45/58 [00:10<00:00, 17.10it/s]Capturing num tokens (num_tokens=112 avail_mem=24.35 GB):  78%|███████▊  | 45/58 [00:10<00:00, 17.10it/s]Capturing num tokens (num_tokens=96 avail_mem=24.34 GB):  78%|███████▊  | 45/58 [00:10<00:00, 17.10it/s] Capturing num tokens (num_tokens=96 avail_mem=24.34 GB):  81%|████████  | 47/58 [00:10<00:00, 17.78it/s]Capturing num tokens (num_tokens=80 avail_mem=24.32 GB):  81%|████████  | 47/58 [00:10<00:00, 17.78it/s]Capturing num tokens (num_tokens=64 avail_mem=24.30 GB):  81%|████████  | 47/58 [00:10<00:00, 17.78it/s]Capturing num tokens (num_tokens=48 avail_mem=24.29 GB):  81%|████████  | 47/58 [00:10<00:00, 17.78it/s]

    Capturing num tokens (num_tokens=48 avail_mem=24.29 GB):  86%|████████▌ | 50/58 [00:10<00:00, 18.79it/s]Capturing num tokens (num_tokens=32 avail_mem=24.27 GB):  86%|████████▌ | 50/58 [00:10<00:00, 18.79it/s]Capturing num tokens (num_tokens=28 avail_mem=24.28 GB):  86%|████████▌ | 50/58 [00:10<00:00, 18.79it/s]Capturing num tokens (num_tokens=24 avail_mem=24.27 GB):  86%|████████▌ | 50/58 [00:10<00:00, 18.79it/s]Capturing num tokens (num_tokens=24 avail_mem=24.27 GB):  91%|█████████▏| 53/58 [00:10<00:00, 19.52it/s]Capturing num tokens (num_tokens=20 avail_mem=24.26 GB):  91%|█████████▏| 53/58 [00:10<00:00, 19.52it/s]Capturing num tokens (num_tokens=16 avail_mem=24.25 GB):  91%|█████████▏| 53/58 [00:10<00:00, 19.52it/s]

    Capturing num tokens (num_tokens=12 avail_mem=24.24 GB):  91%|█████████▏| 53/58 [00:10<00:00, 19.52it/s]Capturing num tokens (num_tokens=12 avail_mem=24.24 GB):  97%|█████████▋| 56/58 [00:10<00:00, 20.23it/s]Capturing num tokens (num_tokens=8 avail_mem=24.23 GB):  97%|█████████▋| 56/58 [00:10<00:00, 20.23it/s] Capturing num tokens (num_tokens=4 avail_mem=24.22 GB):  97%|█████████▋| 56/58 [00:10<00:00, 20.23it/s]Capturing num tokens (num_tokens=4 avail_mem=24.22 GB): 100%|██████████| 58/58 [00:11<00:00,  5.27it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>


### JSON

you can directly define a JSON schema or use [Pydantic](https://docs.pydantic.dev/latest/) to define and validate the response.

**Using Pydantic**


```python
from pydantic import BaseModel, Field


# Define the schema using Pydantic
class CapitalInfo(BaseModel):
    name: str = Field(..., pattern=r"^\w+$", description="Name of the capital city")
    population: int = Field(..., description="Population of the capital city")


response = client.chat.completions.create(
    model="deepseek-ai/DeepSeek-R1-Distill-Qwen-7B",
    messages=[
        {
            "role": "assistant",
            "content": "Give me the information and population of the capital of France in the JSON format.",
        },
    ],
    temperature=0,
    max_tokens=2048,
    response_format={
        "type": "json_schema",
        "json_schema": {
            "name": "foo",
            # convert the pydantic model to json schema
            "schema": CapitalInfo.model_json_schema(),
        },
    },
)

print_highlight(
    f"reasoing_content: {response.choices[0].message.reasoning_content}\n\ncontent: {response.choices[0].message.content}"
)
```


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not exactly sure about the current population numbers. I remember that Paris is a very big city, but I think it's not the largest in the world. Maybe around 20 million? I'm not certain, though. I should check if that's correct.<br><br>Wait, I think the population might have changed a bit over the years. I recall reading somewhere that Paris has grown a lot, especially with the influx of people moving there for work. But I'm not sure if it's exactly 21 million or maybe a bit more. I should look up the latest data to confirm.<br><br>I also wonder if the population figure includes just the city proper or the entire metropolitan area. Sometimes, people talk about the metro area, which can be much larger. But the question specifically asks for the population of the capital, so I think it refers to the city limits. Still, I should make sure.<br><br>Another thing to consider is that population figures can vary depending on the source. Some might cite estimates from government agencies, while others might use more recent data from censuses or surveys. I should find a reliable source to get the most accurate number.<br><br>I think the population of Paris is around 21 million, but I'm not 100% sure. Maybe I should think about other major cities in France to compare. For example, Lyon is another big city, but it's much smaller. I believe its population is around 1.2 million. That gives me a sense that Paris is significantly larger.<br><br>Also, considering the economic activities in Paris, like the fashion industry and the entertainment sector, it makes sense that it's the capital and has a large population. The city hosts a lot of events, conventions, and businesses, which would attract a diverse population.<br><br>I should also think about the historical growth of Paris. It's been a major city for centuries, so its population has been increasing steadily. I think it's safe to say that it's over 20 million, but I'm still not certain about the exact number.<br><br>In summary, I'm pretty confident that the capital of France is Paris, and its population is around 21 million. However, to be precise, I should look up the latest statistics to confirm the exact figure.<br><br><br>content: {<br><br>"name": "Paris",<br>"population": 21538000<br>}</strong>


**JSON Schema Directly**



```python
import json

json_schema = json.dumps(
    {
        "type": "object",
        "properties": {
            "name": {"type": "string", "pattern": "^[\\w]+$"},
            "population": {"type": "integer"},
        },
        "required": ["name", "population"],
    }
)

response = client.chat.completions.create(
    model="deepseek-ai/DeepSeek-R1-Distill-Qwen-7B",
    messages=[
        {
            "role": "assistant",
            "content": "Give me the information and population of the capital of France in the JSON format.",
        },
    ],
    temperature=0,
    max_tokens=2048,
    response_format={
        "type": "json_schema",
        "json_schema": {"name": "foo", "schema": json.loads(json_schema)},
    },
)

print_highlight(
    f"reasoing_content: {response.choices[0].message.reasoning_content}\n\ncontent: {response.choices[0].message.content}"
)
```


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not exactly sure about the current population numbers. I remember that Paris is a very big city, but I think it's not the largest in the world. Maybe around 20 million? I'm not certain, though. I should check if that's correct.<br><br>Wait, I think the population might have changed a bit over the years. I recall reading somewhere that it's over 21 million, but I'm not sure if it's 21.5 or 22 million. Also, I should consider whether the population figure is as of a specific year, like 2021 or 2022, because populations can increase over time.<br><br>I also wonder if there are any other factors to consider, like whether the population includes just the city proper or the broader metropolitan area. Sometimes, population figures can include surrounding regions. But I think in this case, since the user asked for the population of the capital, it's probably referring to the city limits.<br><br>Another thing to consider is the source of the data. Is it from a reliable government website or a recent census? I should make sure the information is up-to-date and accurate. Maybe I can cross-reference this with a recent source to confirm the population number.<br><br>So, putting it all together, I'm pretty confident that Paris is the capital of France, and its population is around 21.5 million. I'll go with that and present it in the JSON format as requested.<br><br><br>content: {<br><br>"name": "Paris",<br>"population": 21500000<br>}</strong>


### EBNF


```python
ebnf_grammar = """
root ::= city | description
city ::= "London" | "Paris" | "Berlin" | "Rome"
description ::= city " is " status
status ::= "the capital of " country
country ::= "England" | "France" | "Germany" | "Italy"
"""

response = client.chat.completions.create(
    model="deepseek-ai/DeepSeek-R1-Distill-Qwen-7B",
    messages=[
        {"role": "system", "content": "You are a helpful geography bot."},
        {
            "role": "assistant",
            "content": "Give me the information and population of the capital of France in the JSON format.",
        },
    ],
    temperature=0,
    max_tokens=2048,
    extra_body={"ebnf": ebnf_grammar},
)

print_highlight(
    f"reasoing_content: {response.choices[0].message.reasoning_content}\n\ncontent: {response.choices[0].message.content}"
)
```


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not entirely sure about the population number. I think it's a big city, so maybe around 3 million? But I'm not certain. I should probably double-check that. Maybe I can recall that Paris is one of the largest cities in Europe, so 3.5 million sounds about right. I don't think it's more than that because I've heard it's a major tourist attraction but not the largest in the world. So, I'll go with Paris having a population of approximately 3.5 million.<br><br><br>content: London is the capital of France</strong>


### Regular expression


```python
response = client.chat.completions.create(
    model="deepseek-ai/DeepSeek-R1-Distill-Qwen-7B",
    messages=[
        {"role": "assistant", "content": "What is the capital of France?"},
    ],
    temperature=0,
    max_tokens=2048,
    extra_body={"regex": "(Paris|London)"},
)

print_highlight(
    f"reasoing_content: {response.choices[0].message.reasoning_content}\n\ncontent: {response.choices[0].message.content}"
)
```


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France. Hmm, I remember learning about France in school, but I'm not 100% sure. Let me think. I know that Paris is a major city in France, and it's often referred to as the "City of Light." People go there for museums, landmarks like the Eiffel Tower, and it's a cultural hub. But is it the capital?<br><br>Wait, I think the capital is the official seat of government, right? So maybe Paris is both the capital and the most famous city. But I'm not entirely certain. I recall that some countries have their capital in a different city than their main tourist attraction. For example, I think Brazil's capital is not Rio de Janeiro, which is more famous. So maybe France is like that too.<br><br>Let me try to remember any specific information. I think the French government declares Paris as the capital. I've heard that before. Also, I remember learning that the Eiffel Tower is in Paris, which is a symbol of the city, but not necessarily the government building. The government buildings are probably in another part of the city or in a different city altogether.<br><br>Wait, no, I think the government buildings are in Paris. Maybe the Palace of Consultation or something like that. I'm not sure of the exact name, but I know it's in Paris. So if the government is based there, then Paris would be the capital. That makes sense because the capital is where all the official government work happens.<br><br>I also think that other capitals, like Germany's Berlin or Italy's Rome, are more than just tourist spots. So, yes, Paris being the capital of France fits into that pattern. It's both the main city and the government center.<br><br>I guess I'm pretty confident now. Paris is the capital of France because it's where the government is located, even though it's also a major tourist destination. So the answer should be Paris.<br><br><br>content: Paris</strong>


### Structural Tag


```python
tool_get_current_weather = {
    "type": "function",
    "function": {
        "name": "get_current_weather",
        "description": "Get the current weather in a given location",
        "parameters": {
            "type": "object",
            "properties": {
                "city": {
                    "type": "string",
                    "description": "The city to find the weather for, e.g. 'San Francisco'",
                },
                "state": {
                    "type": "string",
                    "description": "the two-letter abbreviation for the state that the city is"
                    " in, e.g. 'CA' which would mean 'California'",
                },
                "unit": {
                    "type": "string",
                    "description": "The unit to fetch the temperature in",
                    "enum": ["celsius", "fahrenheit"],
                },
            },
            "required": ["city", "state", "unit"],
        },
    },
}

tool_get_current_date = {
    "type": "function",
    "function": {
        "name": "get_current_date",
        "description": "Get the current date and time for a given timezone",
        "parameters": {
            "type": "object",
            "properties": {
                "timezone": {
                    "type": "string",
                    "description": "The timezone to fetch the current date and time for, e.g. 'America/New_York'",
                }
            },
            "required": ["timezone"],
        },
    },
}

schema_get_current_weather = tool_get_current_weather["function"]["parameters"]
schema_get_current_date = tool_get_current_date["function"]["parameters"]


def get_messages():
    return [
        {
            "role": "system",
            "content": f"""
# Tool Instructions
- Always execute python code in messages that you share.
- When looking for real time information use relevant functions if available else fallback to brave_search
You have access to the following functions:
Use the function 'get_current_weather' to: Get the current weather in a given location
{tool_get_current_weather["function"]}
Use the function 'get_current_date' to: Get the current date and time for a given timezone
{tool_get_current_date["function"]}
If a you choose to call a function ONLY reply in the following format:
<{{start_tag}}={{function_name}}>{{parameters}}{{end_tag}}
where
start_tag => `<function`
parameters => a JSON dict with the function argument name as key and function argument value as value.
end_tag => `</function>`
Here is an example,
<function=example_function_name>{{"example_name": "example_value"}}</function>
Reminder:
- Function calls MUST follow the specified format
- Required parameters MUST be specified
- Only call one function at a time
- Put the entire function call reply on one line
- Always add your sources when using search results to answer the user query
You are a helpful assistant.""",
        },
        {
            "role": "assistant",
            "content": "You are in New York. Please get the current date and time, and the weather.",
        },
    ]


messages = get_messages()

response = client.chat.completions.create(
    model="deepseek-ai/DeepSeek-R1-Distill-Qwen-7B",
    messages=messages,
    response_format={
        "type": "structural_tag",
        "max_new_tokens": 2048,
        "structures": [
            {
                "begin": "<function=get_current_weather>",
                "schema": schema_get_current_weather,
                "end": "</function>",
            },
            {
                "begin": "<function=get_current_date>",
                "schema": schema_get_current_date,
                "end": "</function>",
            },
        ],
        "triggers": ["<function="],
    },
)

print_highlight(
    f"reasoing_content: {response.choices[0].message.reasoning_content}\n\ncontent: {response.choices[0].message.content}"
)
```


<strong style='color: #00008B;'>reasoing_content: Okay, so the user is in New York and wants the current date and time, along with the weather. Let me break this down. They mentioned they're in New York, so I need to figure out the timezone for that. New York is usually in Eastern Time, which is UTC-5 during standard time and UTC-4 during daylight saving. But since the user didn't specify, I'll go with the standard Eastern Time, which is 'America/New_York'.<br><br>Next, I need to get the current date and time. The function available for that is 'get_current_date'. It requires a timezone parameter. So I'll structure the function call as <function=get_current_date>{"timezone": "America/New_York"}</function>.<br><br>Now, for the weather. The user wants the current weather in New York. So I'll use 'get_current_weather'. The required parameters are city, state, and unit. The city is New York, the state abbreviation is NY, and the unit isn't specified, so I'll default to Celsius. So the parameters will be {"city": "New York", "state": "NY", "unit": "celsius"}.<br><br>Putting it all together, I'll make two separate function calls: one for the date and time, and another for the weather. I'll include the sources from OpenWeatherMap for the weather and the current date from timeanddate.com for the date and time.<br><br><br>content: <function=get_current_date>{"timezone": "America/New_York"}</function>  <br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "celsius"}</function>  <br><br>Source: OpenWeatherMap for weather data and timeanddate.com for date/time information.</strong>


## Native API and SGLang Runtime (SRT)

> Note: For native API, as a work-around, you need to set `require_reasoning` argument to `True` to ensure the model will think before generating the structured output. It's not required for chat-completion API.

### JSON

**Using Pydantic**


```python
import requests
from pydantic import BaseModel, Field
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("deepseek-ai/DeepSeek-R1-Distill-Qwen-7B")


# Define the schema using Pydantic
class CapitalInfo(BaseModel):
    name: str = Field(..., pattern=r"^\w+$", description="Name of the capital city")
    population: int = Field(..., description="Population of the capital city")


messages = [
    {
        "role": "assistant",
        "content": "Give me the information and population of the capital of France in the JSON format.",
    },
]
text = tokenizer.apply_chat_template(
    messages, tokenize=False, add_generation_prompt=True, return_dict=False
)
# Make API request
response = requests.post(
    f"http://localhost:{port}/generate",
    json={
        "text": text,
        "require_reasoning": True,
        "sampling_params": {
            "temperature": 0,
            "max_new_tokens": 2048,
            "json_schema": json.dumps(CapitalInfo.model_json_schema()),
        },
    },
)
print(response.json())


reasoing_content = response.json()["text"].split("</think>")[0]
content = response.json()["text"].split("</think>")[1]
print_highlight(f"reasoing_content: {reasoing_content}\n\ncontent: {content}")
```

    {'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not exactly sure of the exact number. I think it\'s around 2 million, but I should double-check that.\n\nWait, maybe I should look up the latest statistics to be accurate. I recall that in recent years, Paris has seen a steady increase due to urban development and immigration. Let me see... I think the population is approximately 2,170,000 as of 2023. That seems about right.\n\nNow, the user wants this information in JSON format. JSON stands for JavaScript Object Notation, which is a way to structure data. I need to create a JSON object that includes the city name, its population, and maybe the country it\'s in. So, the keys could be "city", "population", and "country".\n\nPutting it all together, the JSON should look something like this: {"city": "Paris", "population": 2170000, "country": "France"}. I should make sure the syntax is correct, with proper commas and quotation marks.\n\nI should also consider if the user might need more details, like the metropolitan area population or other statistics, but since they specifically asked for the capital, I\'ll stick to that. Maybe mention that the data is up to date as of 2023 to provide context.\n\nAlright, I think that\'s all. I\'ll present the JSON and offer further assistance in case they need more information.\n</think>{\n  "name": "Paris",\n  "population": 2170000\n}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 6896, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 1990, 15934, 429, 382, 14190, 11, 7196, 358, 1265, 1401, 705, 279, 5535, 13142, 311, 387, 13382, 13, 358, 19091, 429, 304, 3213, 1635, 11, 12095, 702, 3884, 264, 24020, 5263, 4152, 311, 15662, 4401, 323, 15093, 13, 6771, 752, 1490, 1112, 358, 1744, 279, 7042, 374, 13187, 220, 17, 11, 16, 22, 15, 11, 15, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 2938, 4977, 911, 1290, 382, 7039, 11, 279, 1196, 6801, 419, 1995, 304, 4718, 3561, 13, 4718, 13352, 369, 12914, 3002, 2806, 367, 11, 892, 374, 264, 1616, 311, 5944, 821, 13, 358, 1184, 311, 1855, 264, 4718, 1633, 429, 5646, 279, 3283, 829, 11, 1181, 7042, 11, 323, 7196, 279, 3146, 432, 594, 304, 13, 2055, 11, 279, 6894, 1410, 387, 330, 8926, 497, 330, 44441, 497, 323, 330, 11141, 11436, 97904, 432, 678, 3786, 11, 279, 4718, 1265, 1401, 2494, 1075, 419, 25, 5212, 8926, 788, 330, 59604, 497, 330, 44441, 788, 220, 17, 16, 22, 15, 15, 15, 15, 11, 330, 11141, 788, 330, 49000, 1, 7810, 358, 1265, 1281, 2704, 279, 19482, 374, 4396, 11, 448, 6169, 76602, 323, 54231, 15423, 382, 40, 1265, 1083, 2908, 421, 279, 1196, 2578, 1184, 803, 3565, 11, 1075, 279, 57406, 3082, 7042, 476, 1008, 13142, 11, 714, 2474, 807, 11689, 4588, 369, 279, 6722, 11, 358, 3278, 9214, 311, 429, 13, 10696, 6286, 429, 279, 821, 374, 705, 311, 2400, 438, 315, 220, 17, 15, 17, 18, 311, 3410, 2266, 382, 71486, 11, 358, 1744, 429, 594, 678, 13, 358, 3278, 3042, 279, 4718, 323, 3010, 4623, 12994, 304, 1142, 807, 1184, 803, 1995, 624, 151649, 515, 220, 330, 606, 788, 330, 59604, 756, 220, 330, 44441, 788, 220, 17, 16, 22, 15, 15, 15, 15, 198, 92, 151643], 'meta_info': {'id': 'b73773ea128b4bb293f0e373b4a34ca5', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 392, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 2.8118705949746072, 'response_sent_to_client_ts': 1775077192.4197326}}



<strong style='color: #00008B;'>reasoing_content: Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.<br><br>First, I need to identify the capital of France. I know that Paris is the capital, so that's straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I'm not exactly sure of the exact number. I think it's around 2 million, but I should double-check that.<br><br>Wait, maybe I should look up the latest statistics to be accurate. I recall that in recent years, Paris has seen a steady increase due to urban development and immigration. Let me see... I think the population is approximately 2,170,000 as of 2023. That seems about right.<br><br>Now, the user wants this information in JSON format. JSON stands for JavaScript Object Notation, which is a way to structure data. I need to create a JSON object that includes the city name, its population, and maybe the country it's in. So, the keys could be "city", "population", and "country".<br><br>Putting it all together, the JSON should look something like this: {"city": "Paris", "population": 2170000, "country": "France"}. I should make sure the syntax is correct, with proper commas and quotation marks.<br><br>I should also consider if the user might need more details, like the metropolitan area population or other statistics, but since they specifically asked for the capital, I'll stick to that. Maybe mention that the data is up to date as of 2023 to provide context.<br><br>Alright, I think that's all. I'll present the JSON and offer further assistance in case they need more information.<br><br><br>content: {<br>  "name": "Paris",<br>  "population": 2170000<br>}</strong>


**JSON Schema Directly**


```python
json_schema = json.dumps(
    {
        "type": "object",
        "properties": {
            "name": {"type": "string", "pattern": "^[\\w]+$"},
            "population": {"type": "integer"},
        },
        "required": ["name", "population"],
    }
)

# JSON
text = tokenizer.apply_chat_template(
    messages, tokenize=False, add_generation_prompt=True, return_dict=False
)
response = requests.post(
    f"http://localhost:{port}/generate",
    json={
        "text": text,
        "require_reasoning": True,
        "sampling_params": {
            "temperature": 0,
            "max_new_tokens": 2048,
            "json_schema": json_schema,
        },
    },
)

print_highlight(response.json())
```


<strong style='color: #00008B;'>{'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not sure of the exact number. I think it\'s around 2 million, but I should verify that.\n\nI\'ll check a reliable source, maybe the official Paris Municipality website or a recent census. Let me see... Yes, according to the latest data, the population is approximately 2,174,300 as of 2023. That seems accurate.\n\nNext, I need to structure this information into a JSON format. JSON requires key-value pairs, so I\'ll create an object with keys like "city", "population", and "country". The city is Paris, the population is the number I found, and the country is France.\n\nI should make sure the JSON syntax is correct. Each key should be in quotes, and the values as well. Also, the entire structure should be enclosed in curly braces. I\'ll format it properly to ensure there are no syntax errors.\n\nPutting it all together, the JSON object will have the city, population, and country. I\'ll present this to the user, making sure it\'s clear and easy to understand. I don\'t think the user needs anything else, but if they have more questions, I can provide additional information.\n\nI should also consider if the user might need the population figure in a different format, like a string or an integer, but since they asked for JSON, I\'ll stick with the number as an integer. \n\nAlright, I think that\'s all. I\'ll format the JSON correctly and present it to the user.\n</think>{\n\n"name": "Paris",\n"population": 2174300000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 40, 3278, 1779, 264, 14720, 2530, 11, 7196, 279, 3946, 12095, 35703, 2719, 3910, 476, 264, 3213, 43602, 13, 6771, 752, 1490, 1112, 7414, 11, 4092, 311, 279, 5535, 821, 11, 279, 7042, 374, 13187, 220, 17, 11, 16, 22, 19, 11, 18, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 2938, 4977, 13382, 382, 5847, 11, 358, 1184, 311, 5944, 419, 1995, 1119, 264, 4718, 3561, 13, 4718, 7460, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 330, 11141, 3263, 576, 3283, 374, 12095, 11, 279, 7042, 374, 279, 1372, 358, 1730, 11, 323, 279, 3146, 374, 9625, 382, 40, 1265, 1281, 2704, 279, 4718, 19482, 374, 4396, 13, 8886, 1376, 1265, 387, 304, 17194, 11, 323, 279, 2750, 438, 1632, 13, 7281, 11, 279, 4453, 5944, 1265, 387, 43810, 304, 68103, 59191, 13, 358, 3278, 3561, 432, 10277, 311, 5978, 1052, 525, 902, 19482, 5975, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1633, 686, 614, 279, 3283, 11, 7042, 11, 323, 3146, 13, 358, 3278, 3042, 419, 311, 279, 1196, 11, 3259, 2704, 432, 594, 2797, 323, 4135, 311, 3535, 13, 358, 1513, 944, 1744, 279, 1196, 3880, 4113, 770, 11, 714, 421, 807, 614, 803, 4755, 11, 358, 646, 3410, 5107, 1995, 382, 40, 1265, 1083, 2908, 421, 279, 1196, 2578, 1184, 279, 7042, 7071, 304, 264, 2155, 3561, 11, 1075, 264, 914, 476, 458, 7546, 11, 714, 2474, 807, 4588, 369, 4718, 11, 358, 3278, 9214, 448, 279, 1372, 438, 458, 7546, 13, 4710, 71486, 11, 358, 1744, 429, 594, 678, 13, 358, 3278, 3561, 279, 4718, 12440, 323, 3042, 432, 311, 279, 1196, 624, 151649, 4257, 1, 606, 788, 330, 59604, 756, 1, 44441, 788, 220, 17, 16, 22, 19, 18, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15], 'meta_info': {'id': 'd4c12a78ece848e08350e5729bb62715', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 21.194057891960256, 'response_sent_to_client_ts': 1775077213.6343968}}</strong>


### EBNF


```python
response = requests.post(
    f"http://localhost:{port}/generate",
    json={
        "text": "Give me the information of the capital of France.",
        "require_reasoning": True,
        "sampling_params": {
            "max_new_tokens": 2048,
            "temperature": 0,
            "n": 3,
            "ebnf": (
                "root ::= city | description\n"
                'city ::= "London" | "Paris" | "Berlin" | "Rome"\n'
                'description ::= city " is " status\n'
                'status ::= "the capital of " country\n'
                'country ::= "England" | "France" | "Germany" | "Italy"'
            ),
        },
        "stream": False,
        "return_logprob": False,
    },
)

print(response.json())
```

    [{'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '877ae078d7f74d4c9fd9c99bae44d2e3', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.19759547000285238, 'response_sent_to_client_ts': 1775077213.886574}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '3e5add022ab644edaa4095bd491af84c', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.19747393496800214, 'response_sent_to_client_ts': 1775077213.8865948}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '5436101c28fa4242bc1b552374e63ae6', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.1974299909779802, 'response_sent_to_client_ts': 1775077213.8866014}}]


### Regular expression


```python
response = requests.post(
    f"http://localhost:{port}/generate",
    json={
        "text": "Paris is the capital of",
        "require_reasoning": True,
        "sampling_params": {
            "temperature": 0,
            "max_new_tokens": 2048,
            "regex": "(France|England)",
        },
    },
)
print(response.json())
```

    {'text': ' France, and the \n\\( n \\)  \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\(', 'output_ids': [9625, 11, 323, 279, 220, 198, 44292, 308, 1124, 8, 220, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767], 'meta_info': {'id': 'd14908b8074f4e638b2853cbf37b62fa', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 6, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 21.9085022599902, 'response_sent_to_client_ts': 1775077235.8052819}}


### Structural Tag


```python
text = tokenizer.apply_chat_template(
    messages, tokenize=False, add_generation_prompt=True, return_dict=False
)
payload = {
    "text": text,
    "require_reasoning": True,
    "sampling_params": {
        "max_new_tokens": 2048,
        "structural_tag": json.dumps(
            {
                "type": "structural_tag",
                "structures": [
                    {
                        "begin": "<function=get_current_weather>",
                        "schema": schema_get_current_weather,
                        "end": "</function>",
                    },
                    {
                        "begin": "<function=get_current_date>",
                        "schema": schema_get_current_date,
                        "end": "</function>",
                    },
                ],
                "triggers": ["<function="],
            }
        ),
    },
}


# Send POST request to the API endpoint
response = requests.post(f"http://localhost:{port}/generate", json=payload)
print_highlight(response.json())
```


<strong style='color: #00008B;'>{'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Hmm, first, I need to figure out what the capital of France is. From what I know, Paris is the capital of France, so that\'s a start.\n\nNow, the user wants the population. I remember that population numbers can change over time, so I should look up the most recent estimate. I think the latest data is from 2021, but I\'m not 100% sure. Let me check a reliable source. Yeah, according to the National Institute of Statistics and Mapping (INSEE), as of 2023, the population of Paris is around 2,223,207 people.\n\nWait, but sometimes cities are growing rapidly, especially with new developments or discoveries. I should confirm if that\'s the most up-to-date number. Maybe looking into recent news articles or official reports would help. After double-checking, I can confirm that the population figure I have is accurate as of the latest records.\n\nNow, structuring this information into a JSON format. JSON typically uses key-value pairs, so I\'ll create a key for "capital" with the value "Paris" and another key for "population" with the number I found. I should format it properly, ensuring proper indentation for readability.\n\nPutting it all together, I\'ll send back the JSON object with the necessary details. The user might be using this for a project, a presentation, or maybe just for general knowledge, so providing clear and concise information is key.\n\nI think I\'ve covered everything the user requested. They asked specifically for JSON, so I don\'t need to include other formats unless they ask. Also, since the query was straightforward, I don\'t think there\'s any additional context needed unless they have further questions about the data.\n</think>\n\nHere is the information and population of the capital of France in JSON format:\n\n```json\n{\n  "capital": "Paris",\n  "population": 2223207\n}\n```', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 88190, 11, 1156, 11, 358, 1184, 311, 7071, 700, 1128, 279, 6722, 315, 9625, 374, 13, 5542, 1128, 358, 1414, 11, 12095, 374, 279, 6722, 315, 9625, 11, 773, 429, 594, 264, 1191, 382, 7039, 11, 279, 1196, 6801, 279, 7042, 13, 358, 6099, 429, 7042, 5109, 646, 2297, 916, 882, 11, 773, 358, 1265, 1401, 705, 279, 1429, 3213, 16045, 13, 358, 1744, 279, 5535, 821, 374, 504, 220, 17, 15, 17, 16, 11, 714, 358, 2776, 537, 220, 16, 15, 15, 4, 2704, 13, 6771, 752, 1779, 264, 14720, 2530, 13, 21607, 11, 4092, 311, 279, 5055, 9976, 315, 24624, 323, 38446, 320, 687, 48740, 701, 438, 315, 220, 17, 15, 17, 18, 11, 279, 7042, 315, 12095, 374, 2163, 220, 17, 11, 17, 17, 18, 11, 17, 15, 22, 1251, 382, 14190, 11, 714, 7025, 9720, 525, 7826, 18512, 11, 5310, 448, 501, 24961, 476, 52998, 13, 358, 1265, 7683, 421, 429, 594, 279, 1429, 705, 4686, 18413, 1372, 13, 10696, 3330, 1119, 3213, 3669, 9709, 476, 3946, 6682, 1035, 1492, 13, 4636, 1990, 15934, 287, 11, 358, 646, 7683, 429, 279, 7042, 7071, 358, 614, 374, 13382, 438, 315, 279, 5535, 7424, 382, 7039, 11, 2036, 1677, 419, 1995, 1119, 264, 4718, 3561, 13, 4718, 11136, 5711, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 264, 1376, 369, 330, 65063, 1, 448, 279, 897, 330, 59604, 1, 323, 2441, 1376, 369, 330, 44441, 1, 448, 279, 1372, 358, 1730, 13, 358, 1265, 3561, 432, 10277, 11, 22573, 6169, 69592, 369, 91494, 382, 97904, 432, 678, 3786, 11, 358, 3278, 3624, 1182, 279, 4718, 1633, 448, 279, 5871, 3565, 13, 576, 1196, 2578, 387, 1667, 419, 369, 264, 2390, 11, 264, 15496, 11, 476, 7196, 1101, 369, 4586, 6540, 11, 773, 8241, 2797, 323, 63594, 1995, 374, 1376, 382, 40, 1744, 358, 3003, 9761, 4297, 279, 1196, 11223, 13, 2379, 4588, 11689, 369, 4718, 11, 773, 358, 1513, 944, 1184, 311, 2924, 1008, 19856, 7241, 807, 2548, 13, 7281, 11, 2474, 279, 3239, 572, 30339, 11, 358, 1513, 944, 1744, 1052, 594, 894, 5107, 2266, 4362, 7241, 807, 614, 4623, 4755, 911, 279, 821, 624, 151649, 271, 8420, 374, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 1447, 73594, 2236, 198, 515, 220, 330, 65063, 788, 330, 59604, 756, 220, 330, 44441, 788, 220, 17, 17, 17, 18, 17, 15, 22, 198, 532, 73594, 151643], 'meta_info': {'id': 'ca6f6828564c4369b4b78ebb4e35a6f3', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 423, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 3.236288841930218, 'response_sent_to_client_ts': 1775077239.052442}}</strong>



```python
terminate_process(server_process)
```

## Offline Engine API


```python
import sglang as sgl

llm = sgl.Engine(
    model_path="deepseek-ai/DeepSeek-R1-Distill-Qwen-7B",
    reasoning_parser="deepseek-r1",
    grammar_backend="xgrammar",
)
```

    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-04-01 21:00:51.370 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 21:00:51] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 21:00:51.370 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 21:00:51] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 21:00:51.370 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 21:00:51] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 21:00:51.370 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 21:00:51] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 21:00:51.370 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 21:00:51] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.20s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.52s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.47s/it]


    2026-04-01 21:00:56,821 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 21:00:56] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:48,  2.96s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:48,  2.96s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:21,  1.46s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:21,  1.46s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:48,  1.13it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:48,  1.13it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:32,  1.65it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:32,  1.65it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:23,  2.24it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:23,  2.24it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.87it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.87it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.51it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.51it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.26it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.26it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.04it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.04it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:04<00:08,  5.94it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:04<00:08,  5.94it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:04<00:08,  5.94it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:04<00:06,  7.51it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:04<00:06,  7.51it/s]

    Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:04<00:06,  7.51it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:04<00:04,  8.97it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:04<00:04,  8.97it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:04<00:04,  8.97it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:05<00:03, 10.59it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:05<00:03, 10.59it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:05<00:03, 10.59it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:05<00:03, 12.56it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:05<00:03, 12.56it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:05<00:03, 12.56it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:05<00:03, 12.56it/s]

    Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:05<00:02, 15.89it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:05<00:02, 15.89it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:05<00:02, 15.89it/s]Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:05<00:02, 15.89it/s]Compiling num tokens (num_tokens=768):  36%|███▌      | 21/58 [00:05<00:02, 15.89it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:05<00:01, 21.41it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:05<00:01, 21.41it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:05<00:01, 21.41it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:05<00:01, 21.41it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:05<00:01, 21.41it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:05<00:01, 21.41it/s]

    Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:05<00:00, 28.21it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:05<00:00, 28.21it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:05<00:00, 28.21it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:05<00:00, 28.21it/s]Compiling num tokens (num_tokens=352):  52%|█████▏    | 30/58 [00:05<00:00, 28.21it/s]Compiling num tokens (num_tokens=320):  52%|█████▏    | 30/58 [00:05<00:00, 28.21it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:05<00:00, 33.23it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:05<00:00, 33.23it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:05<00:00, 33.23it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:05<00:00, 33.23it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:05<00:00, 33.23it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:05<00:00, 33.23it/s]

    Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:05<00:00, 33.23it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 38.52it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 38.52it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:05<00:00, 38.52it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:05<00:00, 38.52it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:05<00:00, 38.52it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:05<00:00, 38.52it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:05<00:00, 38.52it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:05<00:00, 42.73it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:05<00:00, 42.73it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:05<00:00, 42.73it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:05<00:00, 42.73it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:05<00:00, 42.73it/s]

    Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:05<00:00, 42.73it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:05<00:00, 42.73it/s]Compiling num tokens (num_tokens=20):  81%|████████  | 47/58 [00:05<00:00, 42.73it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:05<00:00, 48.62it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:05<00:00, 48.62it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:05<00:00, 48.62it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:05<00:00, 48.62it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:05<00:00, 48.62it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:05<00:00,  9.71it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=38.70 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=38.70 GB):   2%|▏         | 1/58 [00:00<00:17,  3.32it/s]Capturing num tokens (num_tokens=7680 avail_mem=37.67 GB):   2%|▏         | 1/58 [00:00<00:17,  3.32it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=37.67 GB):   3%|▎         | 2/58 [00:00<00:16,  3.44it/s]Capturing num tokens (num_tokens=7168 avail_mem=37.67 GB):   3%|▎         | 2/58 [00:00<00:16,  3.44it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=37.67 GB):   5%|▌         | 3/58 [00:00<00:14,  3.70it/s]Capturing num tokens (num_tokens=6656 avail_mem=37.68 GB):   5%|▌         | 3/58 [00:00<00:14,  3.70it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=37.68 GB):   7%|▋         | 4/58 [00:01<00:13,  3.98it/s]Capturing num tokens (num_tokens=6144 avail_mem=36.40 GB):   7%|▋         | 4/58 [00:01<00:13,  3.98it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=36.40 GB):   9%|▊         | 5/58 [00:01<00:12,  4.25it/s]Capturing num tokens (num_tokens=5632 avail_mem=36.40 GB):   9%|▊         | 5/58 [00:01<00:12,  4.25it/s]Capturing num tokens (num_tokens=5632 avail_mem=36.40 GB):  10%|█         | 6/58 [00:01<00:11,  4.66it/s]Capturing num tokens (num_tokens=5120 avail_mem=36.40 GB):  10%|█         | 6/58 [00:01<00:11,  4.66it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=36.40 GB):  12%|█▏        | 7/58 [00:01<00:10,  5.03it/s]Capturing num tokens (num_tokens=4608 avail_mem=36.41 GB):  12%|█▏        | 7/58 [00:01<00:10,  5.03it/s]Capturing num tokens (num_tokens=4608 avail_mem=36.41 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.48it/s]Capturing num tokens (num_tokens=4096 avail_mem=36.41 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.48it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=36.41 GB):  16%|█▌        | 9/58 [00:01<00:08,  6.00it/s]Capturing num tokens (num_tokens=3840 avail_mem=36.42 GB):  16%|█▌        | 9/58 [00:01<00:08,  6.00it/s]Capturing num tokens (num_tokens=3840 avail_mem=36.42 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.51it/s]Capturing num tokens (num_tokens=3584 avail_mem=36.41 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.51it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=36.41 GB):  19%|█▉        | 11/58 [00:02<00:06,  7.07it/s]Capturing num tokens (num_tokens=3328 avail_mem=36.41 GB):  19%|█▉        | 11/58 [00:02<00:06,  7.07it/s]Capturing num tokens (num_tokens=3328 avail_mem=36.41 GB):  21%|██        | 12/58 [00:02<00:05,  7.70it/s]Capturing num tokens (num_tokens=3072 avail_mem=36.41 GB):  21%|██        | 12/58 [00:02<00:05,  7.70it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=36.41 GB):  21%|██        | 12/58 [00:02<00:05,  7.70it/s]Capturing num tokens (num_tokens=2816 avail_mem=36.41 GB):  24%|██▍       | 14/58 [00:02<00:04,  8.88it/s]Capturing num tokens (num_tokens=2560 avail_mem=36.41 GB):  24%|██▍       | 14/58 [00:02<00:04,  8.88it/s]Capturing num tokens (num_tokens=2304 avail_mem=36.41 GB):  24%|██▍       | 14/58 [00:02<00:04,  8.88it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=36.41 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.17it/s]Capturing num tokens (num_tokens=2048 avail_mem=36.41 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.17it/s]Capturing num tokens (num_tokens=1792 avail_mem=36.41 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.17it/s]Capturing num tokens (num_tokens=1792 avail_mem=36.41 GB):  31%|███       | 18/58 [00:02<00:03, 10.31it/s]Capturing num tokens (num_tokens=1536 avail_mem=36.39 GB):  31%|███       | 18/58 [00:02<00:03, 10.31it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=36.39 GB):  31%|███       | 18/58 [00:02<00:03, 10.31it/s]Capturing num tokens (num_tokens=1280 avail_mem=36.39 GB):  34%|███▍      | 20/58 [00:02<00:03, 10.43it/s]Capturing num tokens (num_tokens=1024 avail_mem=36.39 GB):  34%|███▍      | 20/58 [00:02<00:03, 10.43it/s]Capturing num tokens (num_tokens=960 avail_mem=36.30 GB):  34%|███▍      | 20/58 [00:02<00:03, 10.43it/s] Capturing num tokens (num_tokens=960 avail_mem=36.30 GB):  38%|███▊      | 22/58 [00:03<00:03, 11.96it/s]Capturing num tokens (num_tokens=896 avail_mem=35.78 GB):  38%|███▊      | 22/58 [00:03<00:03, 11.96it/s]

    Capturing num tokens (num_tokens=832 avail_mem=35.76 GB):  38%|███▊      | 22/58 [00:03<00:03, 11.96it/s]Capturing num tokens (num_tokens=832 avail_mem=35.76 GB):  41%|████▏     | 24/58 [00:03<00:02, 13.13it/s]Capturing num tokens (num_tokens=768 avail_mem=35.75 GB):  41%|████▏     | 24/58 [00:03<00:02, 13.13it/s]Capturing num tokens (num_tokens=704 avail_mem=35.75 GB):  41%|████▏     | 24/58 [00:03<00:02, 13.13it/s]Capturing num tokens (num_tokens=640 avail_mem=35.74 GB):  41%|████▏     | 24/58 [00:03<00:02, 13.13it/s]

    Capturing num tokens (num_tokens=640 avail_mem=35.74 GB):  47%|████▋     | 27/58 [00:03<00:02, 15.32it/s]Capturing num tokens (num_tokens=576 avail_mem=35.74 GB):  47%|████▋     | 27/58 [00:03<00:02, 15.32it/s]Capturing num tokens (num_tokens=512 avail_mem=35.73 GB):  47%|████▋     | 27/58 [00:03<00:02, 15.32it/s]Capturing num tokens (num_tokens=480 avail_mem=35.73 GB):  47%|████▋     | 27/58 [00:03<00:02, 15.32it/s]Capturing num tokens (num_tokens=480 avail_mem=35.73 GB):  52%|█████▏    | 30/58 [00:03<00:01, 17.17it/s]Capturing num tokens (num_tokens=448 avail_mem=35.73 GB):  52%|█████▏    | 30/58 [00:03<00:01, 17.17it/s]Capturing num tokens (num_tokens=416 avail_mem=35.70 GB):  52%|█████▏    | 30/58 [00:03<00:01, 17.17it/s]

    Capturing num tokens (num_tokens=416 avail_mem=35.70 GB):  55%|█████▌    | 32/58 [00:03<00:01, 17.24it/s]Capturing num tokens (num_tokens=384 avail_mem=35.67 GB):  55%|█████▌    | 32/58 [00:03<00:01, 17.24it/s]Capturing num tokens (num_tokens=352 avail_mem=35.67 GB):  55%|█████▌    | 32/58 [00:03<00:01, 17.24it/s]Capturing num tokens (num_tokens=320 avail_mem=35.66 GB):  55%|█████▌    | 32/58 [00:03<00:01, 17.24it/s]Capturing num tokens (num_tokens=320 avail_mem=35.66 GB):  60%|██████    | 35/58 [00:03<00:01, 20.02it/s]Capturing num tokens (num_tokens=288 avail_mem=35.66 GB):  60%|██████    | 35/58 [00:03<00:01, 20.02it/s]Capturing num tokens (num_tokens=256 avail_mem=35.65 GB):  60%|██████    | 35/58 [00:03<00:01, 20.02it/s]Capturing num tokens (num_tokens=240 avail_mem=35.65 GB):  60%|██████    | 35/58 [00:03<00:01, 20.02it/s]

    Capturing num tokens (num_tokens=240 avail_mem=35.65 GB):  66%|██████▌   | 38/58 [00:03<00:00, 22.43it/s]Capturing num tokens (num_tokens=224 avail_mem=35.65 GB):  66%|██████▌   | 38/58 [00:03<00:00, 22.43it/s]Capturing num tokens (num_tokens=208 avail_mem=35.64 GB):  66%|██████▌   | 38/58 [00:03<00:00, 22.43it/s]

    Capturing num tokens (num_tokens=192 avail_mem=35.64 GB):  66%|██████▌   | 38/58 [00:03<00:00, 22.43it/s]Capturing num tokens (num_tokens=192 avail_mem=35.64 GB):  71%|███████   | 41/58 [00:04<00:00, 18.16it/s]Capturing num tokens (num_tokens=176 avail_mem=35.64 GB):  71%|███████   | 41/58 [00:04<00:00, 18.16it/s]Capturing num tokens (num_tokens=160 avail_mem=35.63 GB):  71%|███████   | 41/58 [00:04<00:00, 18.16it/s]Capturing num tokens (num_tokens=144 avail_mem=35.63 GB):  71%|███████   | 41/58 [00:04<00:00, 18.16it/s]Capturing num tokens (num_tokens=128 avail_mem=35.64 GB):  71%|███████   | 41/58 [00:04<00:00, 18.16it/s]Capturing num tokens (num_tokens=128 avail_mem=35.64 GB):  78%|███████▊  | 45/58 [00:04<00:00, 22.71it/s]Capturing num tokens (num_tokens=112 avail_mem=35.64 GB):  78%|███████▊  | 45/58 [00:04<00:00, 22.71it/s]Capturing num tokens (num_tokens=96 avail_mem=35.63 GB):  78%|███████▊  | 45/58 [00:04<00:00, 22.71it/s] Capturing num tokens (num_tokens=80 avail_mem=35.62 GB):  78%|███████▊  | 45/58 [00:04<00:00, 22.71it/s]

    Capturing num tokens (num_tokens=64 avail_mem=35.62 GB):  78%|███████▊  | 45/58 [00:04<00:00, 22.71it/s]Capturing num tokens (num_tokens=64 avail_mem=35.62 GB):  84%|████████▍ | 49/58 [00:04<00:00, 26.69it/s]Capturing num tokens (num_tokens=48 avail_mem=35.59 GB):  84%|████████▍ | 49/58 [00:04<00:00, 26.69it/s]Capturing num tokens (num_tokens=32 avail_mem=35.59 GB):  84%|████████▍ | 49/58 [00:04<00:00, 26.69it/s]Capturing num tokens (num_tokens=28 avail_mem=35.58 GB):  84%|████████▍ | 49/58 [00:04<00:00, 26.69it/s]Capturing num tokens (num_tokens=24 avail_mem=35.56 GB):  84%|████████▍ | 49/58 [00:04<00:00, 26.69it/s]Capturing num tokens (num_tokens=20 avail_mem=35.56 GB):  84%|████████▍ | 49/58 [00:04<00:00, 26.69it/s]Capturing num tokens (num_tokens=20 avail_mem=35.56 GB):  93%|█████████▎| 54/58 [00:04<00:00, 30.82it/s]Capturing num tokens (num_tokens=16 avail_mem=35.55 GB):  93%|█████████▎| 54/58 [00:04<00:00, 30.82it/s]Capturing num tokens (num_tokens=12 avail_mem=35.55 GB):  93%|█████████▎| 54/58 [00:04<00:00, 30.82it/s]Capturing num tokens (num_tokens=8 avail_mem=35.54 GB):  93%|█████████▎| 54/58 [00:04<00:00, 30.82it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=35.54 GB):  93%|█████████▎| 54/58 [00:04<00:00, 30.82it/s]Capturing num tokens (num_tokens=4 avail_mem=35.54 GB): 100%|██████████| 58/58 [00:04<00:00, 13.05it/s]


### JSON

**Using Pydantic**


```python
import json
from pydantic import BaseModel, Field

prompts = [
    "Give me the information of the capital of China in the JSON format.",
    "Give me the information of the capital of France in the JSON format.",
    "Give me the information of the capital of Ireland in the JSON format.",
]


# Define the schema using Pydantic
class CapitalInfo(BaseModel):
    name: str = Field(..., pattern=r"^\w+$", description="Name of the capital city")
    population: int = Field(..., description="Population of the capital city")


sampling_params = {
    "temperature": 0,
    "top_p": 0.95,
    "max_new_tokens": 2048,
    "json_schema": json.dumps(CapitalInfo.model_json_schema()),
}

outputs = llm.generate(prompts, sampling_params)
for prompt, output in zip(prompts, outputs):
    print("===============================")
    print(f"Prompt: {prompt}\nGenerated text: {output['text']}")
```

    ===============================
    Prompt: Give me the information of the capital of China in the JSON format.
    Generated text: {
      "name": "Beijing",
      "population": 316000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
    ===============================
    Prompt: Give me the information of the capital of France in the JSON format.
    Generated text: {
      "name": "Paris",
      "population": 2154000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
    ===============================
    Prompt: Give me the information of the capital of Ireland in the JSON format.
    Generated text: {
      "name": "Ireland",
      "population": 500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000


**JSON Schema Directly**


```python
prompts = [
    "Give me the information of the capital of China in the JSON format.",
    "Give me the information of the capital of France in the JSON format.",
    "Give me the information of the capital of Ireland in the JSON format.",
]

json_schema = json.dumps(
    {
        "type": "object",
        "properties": {
            "name": {"type": "string", "pattern": "^[\\w]+$"},
            "population": {"type": "integer"},
        },
        "required": ["name", "population"],
    }
)

sampling_params = {"temperature": 0, "max_new_tokens": 2048, "json_schema": json_schema}

outputs = llm.generate(prompts, sampling_params)
for prompt, output in zip(prompts, outputs):
    print("===============================")
    print(f"Prompt: {prompt}\nGenerated text: {output['text']}")
```

    ===============================
    Prompt: Give me the information of the capital of China in the JSON format.
    Generated text: {
      "name": "Beijing",
      "population": 316000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
    ===============================
    Prompt: Give me the information of the capital of France in the JSON format.
    Generated text: {
      "name": "Paris",
      "population": 2154000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
    ===============================
    Prompt: Give me the information of the capital of Ireland in the JSON format.
    Generated text: {
      "name": "Ireland",
      "population": 500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000


### EBNF



```python
prompts = [
    "Give me the information of the capital of France.",
    "Give me the information of the capital of Germany.",
    "Give me the information of the capital of Italy.",
]

sampling_params = {
    "temperature": 0.8,
    "top_p": 0.95,
    "ebnf": (
        "root ::= city | description\n"
        'city ::= "London" | "Paris" | "Berlin" | "Rome"\n'
        'description ::= city " is " status\n'
        'status ::= "the capital of " country\n'
        'country ::= "England" | "France" | "Germany" | "Italy"'
    ),
}

outputs = llm.generate(prompts, sampling_params)
for prompt, output in zip(prompts, outputs):
    print("===============================")
    print(f"Prompt: {prompt}\nGenerated text: {output['text']}")
```

    ===============================
    Prompt: Give me the information of the capital of France.
    Generated text: Berlin is the capital of France
    ===============================
    Prompt: Give me the information of the capital of Germany.
    Generated text: Berlin is the capital of Germany
    ===============================
    Prompt: Give me the information of the capital of Italy.
    Generated text: Paris is the capital of France


### Regular expression


```python
prompts = [
    "Please provide information about London as a major global city:",
    "Please provide information about Paris as a major global city:",
]

sampling_params = {"temperature": 0.8, "top_p": 0.95, "regex": "(France|England)"}

outputs = llm.generate(prompts, sampling_params)
for prompt, output in zip(prompts, outputs):
    print("===============================")
    print(f"Prompt: {prompt}\nGenerated text: {output['text']}")
```

    ===============================
    Prompt: Please provide information about London as a major global city:
    Generated text: England
    ===============================
    Prompt: Please provide information about Paris as a major global city:
    Generated text: France



```python
text = tokenizer.apply_chat_template(
    messages, tokenize=False, add_generation_prompt=True, return_dict=False
)
prompts = [text]


sampling_params = {
    "temperature": 0.8,
    "top_p": 0.95,
    "max_new_tokens": 2048,
    "structural_tag": json.dumps(
        {
            "type": "structural_tag",
            "structures": [
                {
                    "begin": "<function=get_current_weather>",
                    "schema": schema_get_current_weather,
                    "end": "</function>",
                },
                {
                    "begin": "<function=get_current_date>",
                    "schema": schema_get_current_date,
                    "end": "</function>",
                },
            ],
            "triggers": ["<function="],
        }
    ),
}


# Send POST request to the API endpoint
outputs = llm.generate(prompts, sampling_params)
for prompt, output in zip(prompts, outputs):
    print("===============================")
    print(f"Prompt: {prompt}\nGenerated text: {output['text']}")
```

    ===============================
    Prompt: <｜begin▁of▁sentence｜><｜Assistant｜>Give me the information and population of the capital of France in the JSON format.<｜end▁of▁sentence｜><｜Assistant｜><think>
    
    Generated text: Okay, so I'm trying to figure out how to respond to this user's query. They asked for the information and population of the capital of France in JSON format. Hmm, first, I need to make sure I understand what they're asking for. The capital of France is Paris, right? So I should provide the population of Paris, not the entire population of France. 
    
    Wait, the user said "information and population," but I'm not exactly sure what specific information they want. They mentioned "capital," so maybe they're asking for just the population of the city. I should probably include the population number for the city of Paris. 
    
    I know that populations can change over time, so I should use the most recent data available. I think as of 2023, the population is around 2.2 million, but I'm not entirely sure. I should double-check that. Maybe I can recall that Paris has a diverse population and it's one of the largest cities in the world. 
    
    Also, I should format the information in JSON. JSON usually requires keys and values, so the structure would be something like {"capital": "Paris", "population": 2215000}. Wait, is that the correct syntax? I should make sure the commas are in the right place. 
    
    Oh, and the user might want to see it in a more structured way, maybe with some additional info about Paris. But since they specifically asked for population, I should keep it concise. I don't want to overcomplicate it. 
    
    Is there any other information they might need? Maybe the area of Paris? But the question didn't mention that. I think sticking to population is better. 
    
    So putting it all together, the JSON should have the capital name and the population number. I should also check if the population number is accurate. Maybe I can recall that the population increased a bit each year, so 2215000 seems about right. 
    
    I should also make sure that the JSON is properly formatted without any syntax errors. No trailing commas, proper quotes, etc. That way, when the user uses this JSON, it will work correctly in their application or script. 
    
    Alright, I think I've got all the points covered. Just need to structure it correctly and ensure accuracy.
    </think>
    
    Here is the information about the capital of France in JSON format:
    
    ```json
    {
      "capital": "Paris",
      "population": 2215000
    }
    ```



```python
llm.shutdown()
```
