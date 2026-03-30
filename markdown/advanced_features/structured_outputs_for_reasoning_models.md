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


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:176: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-03-30 14:01:17.325 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 14:01:17] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 14:01:17.325 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 14:01:17] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 14:01:17.325 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 14:01:17] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 14:01:17.326 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 14:01:17] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 14:01:17.326 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 14:01:17] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.01s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.28s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.24s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:48,  2.95s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:48,  2.95s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:22,  1.47s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:22,  1.47s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:48,  1.12it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:48,  1.12it/s]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:32,  1.64it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:32,  1.64it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:23,  2.23it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:23,  2.23it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.86it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.86it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.58it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.58it/s]Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.32it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.32it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.18it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.18it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.18it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.78it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.78it/s]

    Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.78it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.24it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.24it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.24it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:04<00:04,  9.77it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:04<00:04,  9.77it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04,  9.77it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 11.57it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 11.57it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.57it/s]

    Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 11.57it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 14.66it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 14.66it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 14.66it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 14.66it/s]Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 14.66it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 19.84it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 19.84it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 19.84it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 19.84it/s]

    Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 19.84it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 19.84it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 26.45it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 26.45it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 26.45it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 26.45it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 26.45it/s]Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:05<00:01, 26.45it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:05<00:01, 26.45it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:05<00:00, 33.27it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:05<00:00, 33.27it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:05<00:00, 33.27it/s]

    Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:05<00:00, 33.27it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:05<00:00, 33.27it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:05<00:00, 33.27it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:05<00:00, 33.27it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 39.30it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 39.30it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:05<00:00, 39.30it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:05<00:00, 39.30it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:05<00:00, 39.30it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:05<00:00, 39.30it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:05<00:00, 39.30it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:05<00:00, 43.87it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:05<00:00, 43.87it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:05<00:00, 43.87it/s]

    Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:05<00:00, 43.87it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:05<00:00, 43.87it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:05<00:00, 43.87it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:05<00:00, 43.87it/s]Compiling num tokens (num_tokens=20):  81%|████████  | 47/58 [00:05<00:00, 43.87it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:05<00:00, 50.21it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:05<00:00, 50.21it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:05<00:00, 50.21it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:05<00:00, 50.21it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:05<00:00, 50.21it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:05<00:00,  9.84it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=29.57 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=29.57 GB):   2%|▏         | 1/58 [00:00<00:20,  2.77it/s]Capturing num tokens (num_tokens=7680 avail_mem=22.26 GB):   2%|▏         | 1/58 [00:00<00:20,  2.77it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=22.26 GB):   3%|▎         | 2/58 [00:00<00:17,  3.15it/s]Capturing num tokens (num_tokens=7168 avail_mem=22.17 GB):   3%|▎         | 2/58 [00:00<00:17,  3.15it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=22.17 GB):   5%|▌         | 3/58 [00:00<00:16,  3.31it/s]Capturing num tokens (num_tokens=6656 avail_mem=22.17 GB):   5%|▌         | 3/58 [00:00<00:16,  3.31it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=22.17 GB):   7%|▋         | 4/58 [00:01<00:14,  3.74it/s]Capturing num tokens (num_tokens=6144 avail_mem=22.18 GB):   7%|▋         | 4/58 [00:01<00:14,  3.74it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=22.18 GB):   9%|▊         | 5/58 [00:01<00:12,  4.09it/s]Capturing num tokens (num_tokens=5632 avail_mem=22.18 GB):   9%|▊         | 5/58 [00:01<00:12,  4.09it/s]Capturing num tokens (num_tokens=5632 avail_mem=22.18 GB):  10%|█         | 6/58 [00:01<00:11,  4.55it/s]Capturing num tokens (num_tokens=5120 avail_mem=22.18 GB):  10%|█         | 6/58 [00:01<00:11,  4.55it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=22.18 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.96it/s]Capturing num tokens (num_tokens=4608 avail_mem=22.19 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.96it/s]Capturing num tokens (num_tokens=4608 avail_mem=22.19 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.49it/s]Capturing num tokens (num_tokens=4096 avail_mem=22.19 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.49it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=22.19 GB):  16%|█▌        | 9/58 [00:01<00:08,  6.02it/s]Capturing num tokens (num_tokens=3840 avail_mem=22.19 GB):  16%|█▌        | 9/58 [00:01<00:08,  6.02it/s]Capturing num tokens (num_tokens=3840 avail_mem=22.19 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.51it/s]Capturing num tokens (num_tokens=3584 avail_mem=22.19 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.51it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=22.19 GB):  19%|█▉        | 11/58 [00:02<00:06,  7.09it/s]Capturing num tokens (num_tokens=3328 avail_mem=22.19 GB):  19%|█▉        | 11/58 [00:02<00:06,  7.09it/s]Capturing num tokens (num_tokens=3328 avail_mem=22.19 GB):  21%|██        | 12/58 [00:02<00:05,  7.74it/s]Capturing num tokens (num_tokens=3072 avail_mem=22.19 GB):  21%|██        | 12/58 [00:02<00:05,  7.74it/s]Capturing num tokens (num_tokens=2816 avail_mem=22.19 GB):  21%|██        | 12/58 [00:02<00:05,  7.74it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=22.19 GB):  24%|██▍       | 14/58 [00:02<00:04,  8.95it/s]Capturing num tokens (num_tokens=2560 avail_mem=22.19 GB):  24%|██▍       | 14/58 [00:02<00:04,  8.95it/s]Capturing num tokens (num_tokens=2304 avail_mem=22.19 GB):  24%|██▍       | 14/58 [00:02<00:04,  8.95it/s]Capturing num tokens (num_tokens=2304 avail_mem=22.19 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.37it/s]Capturing num tokens (num_tokens=2048 avail_mem=22.19 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.37it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=22.19 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.37it/s]Capturing num tokens (num_tokens=1792 avail_mem=22.19 GB):  31%|███       | 18/58 [00:02<00:03, 10.66it/s]Capturing num tokens (num_tokens=1536 avail_mem=21.03 GB):  31%|███       | 18/58 [00:02<00:03, 10.66it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=21.03 GB):  31%|███       | 18/58 [00:02<00:03, 10.66it/s]Capturing num tokens (num_tokens=1280 avail_mem=21.03 GB):  34%|███▍      | 20/58 [00:03<00:04,  8.64it/s]Capturing num tokens (num_tokens=1024 avail_mem=21.03 GB):  34%|███▍      | 20/58 [00:03<00:04,  8.64it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=21.03 GB):  36%|███▌      | 21/58 [00:03<00:04,  8.31it/s]Capturing num tokens (num_tokens=960 avail_mem=21.03 GB):  36%|███▌      | 21/58 [00:03<00:04,  8.31it/s] Capturing num tokens (num_tokens=960 avail_mem=21.03 GB):  38%|███▊      | 22/58 [00:03<00:04,  8.33it/s]Capturing num tokens (num_tokens=896 avail_mem=21.02 GB):  38%|███▊      | 22/58 [00:03<00:04,  8.33it/s]

    Capturing num tokens (num_tokens=896 avail_mem=21.02 GB):  40%|███▉      | 23/58 [00:03<00:04,  8.06it/s]Capturing num tokens (num_tokens=832 avail_mem=21.02 GB):  40%|███▉      | 23/58 [00:03<00:04,  8.06it/s]Capturing num tokens (num_tokens=832 avail_mem=21.02 GB):  41%|████▏     | 24/58 [00:03<00:04,  7.98it/s]Capturing num tokens (num_tokens=768 avail_mem=21.02 GB):  41%|████▏     | 24/58 [00:03<00:04,  7.98it/s]

    Capturing num tokens (num_tokens=768 avail_mem=21.02 GB):  43%|████▎     | 25/58 [00:03<00:03,  8.30it/s]Capturing num tokens (num_tokens=704 avail_mem=21.01 GB):  43%|████▎     | 25/58 [00:03<00:03,  8.30it/s]Capturing num tokens (num_tokens=704 avail_mem=21.01 GB):  45%|████▍     | 26/58 [00:03<00:03,  8.22it/s]Capturing num tokens (num_tokens=640 avail_mem=21.01 GB):  45%|████▍     | 26/58 [00:03<00:03,  8.22it/s]

    Capturing num tokens (num_tokens=640 avail_mem=21.01 GB):  47%|████▋     | 27/58 [00:03<00:03,  8.36it/s]Capturing num tokens (num_tokens=576 avail_mem=21.01 GB):  47%|████▋     | 27/58 [00:03<00:03,  8.36it/s]Capturing num tokens (num_tokens=576 avail_mem=21.01 GB):  48%|████▊     | 28/58 [00:04<00:03,  8.30it/s]Capturing num tokens (num_tokens=512 avail_mem=21.00 GB):  48%|████▊     | 28/58 [00:04<00:03,  8.30it/s]

    Capturing num tokens (num_tokens=512 avail_mem=21.00 GB):  50%|█████     | 29/58 [00:04<00:03,  8.68it/s]Capturing num tokens (num_tokens=480 avail_mem=21.00 GB):  50%|█████     | 29/58 [00:04<00:03,  8.68it/s]Capturing num tokens (num_tokens=480 avail_mem=21.00 GB):  52%|█████▏    | 30/58 [00:04<00:03,  8.22it/s]Capturing num tokens (num_tokens=448 avail_mem=21.00 GB):  52%|█████▏    | 30/58 [00:04<00:03,  8.22it/s]

    Capturing num tokens (num_tokens=448 avail_mem=21.00 GB):  53%|█████▎    | 31/58 [00:04<00:03,  8.14it/s]Capturing num tokens (num_tokens=416 avail_mem=20.99 GB):  53%|█████▎    | 31/58 [00:04<00:03,  8.14it/s]Capturing num tokens (num_tokens=384 avail_mem=20.99 GB):  53%|█████▎    | 31/58 [00:04<00:03,  8.14it/s]Capturing num tokens (num_tokens=384 avail_mem=20.99 GB):  57%|█████▋    | 33/58 [00:04<00:02,  8.92it/s]Capturing num tokens (num_tokens=352 avail_mem=20.98 GB):  57%|█████▋    | 33/58 [00:04<00:02,  8.92it/s]

    Capturing num tokens (num_tokens=352 avail_mem=20.98 GB):  59%|█████▊    | 34/58 [00:04<00:02,  8.98it/s]Capturing num tokens (num_tokens=320 avail_mem=20.98 GB):  59%|█████▊    | 34/58 [00:04<00:02,  8.98it/s]Capturing num tokens (num_tokens=288 avail_mem=20.98 GB):  59%|█████▊    | 34/58 [00:04<00:02,  8.98it/s]Capturing num tokens (num_tokens=288 avail_mem=20.98 GB):  62%|██████▏   | 36/58 [00:04<00:02,  9.40it/s]Capturing num tokens (num_tokens=256 avail_mem=20.97 GB):  62%|██████▏   | 36/58 [00:04<00:02,  9.40it/s]

    Capturing num tokens (num_tokens=240 avail_mem=20.97 GB):  62%|██████▏   | 36/58 [00:05<00:02,  9.40it/s]Capturing num tokens (num_tokens=240 avail_mem=20.97 GB):  66%|██████▌   | 38/58 [00:05<00:02,  9.80it/s]Capturing num tokens (num_tokens=224 avail_mem=20.96 GB):  66%|██████▌   | 38/58 [00:05<00:02,  9.80it/s]Capturing num tokens (num_tokens=224 avail_mem=20.96 GB):  67%|██████▋   | 39/58 [00:05<00:01,  9.81it/s]Capturing num tokens (num_tokens=208 avail_mem=20.92 GB):  67%|██████▋   | 39/58 [00:05<00:01,  9.81it/s]

    Capturing num tokens (num_tokens=208 avail_mem=20.92 GB):  69%|██████▉   | 40/58 [00:05<00:01,  9.62it/s]Capturing num tokens (num_tokens=192 avail_mem=20.92 GB):  69%|██████▉   | 40/58 [00:05<00:01,  9.62it/s]Capturing num tokens (num_tokens=192 avail_mem=20.92 GB):  71%|███████   | 41/58 [00:05<00:01,  8.82it/s]Capturing num tokens (num_tokens=176 avail_mem=20.92 GB):  71%|███████   | 41/58 [00:05<00:01,  8.82it/s]

    Capturing num tokens (num_tokens=160 avail_mem=20.91 GB):  71%|███████   | 41/58 [00:05<00:01,  8.82it/s]Capturing num tokens (num_tokens=160 avail_mem=20.91 GB):  74%|███████▍  | 43/58 [00:05<00:01, 10.09it/s]Capturing num tokens (num_tokens=144 avail_mem=20.91 GB):  74%|███████▍  | 43/58 [00:05<00:01, 10.09it/s]Capturing num tokens (num_tokens=128 avail_mem=20.92 GB):  74%|███████▍  | 43/58 [00:05<00:01, 10.09it/s]

    Capturing num tokens (num_tokens=128 avail_mem=20.92 GB):  78%|███████▊  | 45/58 [00:05<00:01, 10.96it/s]Capturing num tokens (num_tokens=112 avail_mem=20.92 GB):  78%|███████▊  | 45/58 [00:05<00:01, 10.96it/s]Capturing num tokens (num_tokens=96 avail_mem=20.91 GB):  78%|███████▊  | 45/58 [00:05<00:01, 10.96it/s] Capturing num tokens (num_tokens=96 avail_mem=20.91 GB):  81%|████████  | 47/58 [00:05<00:00, 12.03it/s]Capturing num tokens (num_tokens=80 avail_mem=20.90 GB):  81%|████████  | 47/58 [00:05<00:00, 12.03it/s]

    Capturing num tokens (num_tokens=64 avail_mem=20.90 GB):  81%|████████  | 47/58 [00:06<00:00, 12.03it/s]Capturing num tokens (num_tokens=64 avail_mem=20.90 GB):  84%|████████▍ | 49/58 [00:06<00:00, 13.12it/s]Capturing num tokens (num_tokens=48 avail_mem=20.90 GB):  84%|████████▍ | 49/58 [00:06<00:00, 13.12it/s]

    Capturing num tokens (num_tokens=32 avail_mem=20.90 GB):  84%|████████▍ | 49/58 [00:06<00:00, 13.12it/s]Capturing num tokens (num_tokens=32 avail_mem=20.90 GB):  88%|████████▊ | 51/58 [00:06<00:00, 11.96it/s]Capturing num tokens (num_tokens=28 avail_mem=20.90 GB):  88%|████████▊ | 51/58 [00:06<00:00, 11.96it/s]Capturing num tokens (num_tokens=24 avail_mem=20.89 GB):  88%|████████▊ | 51/58 [00:06<00:00, 11.96it/s]Capturing num tokens (num_tokens=20 avail_mem=20.89 GB):  88%|████████▊ | 51/58 [00:06<00:00, 11.96it/s]Capturing num tokens (num_tokens=20 avail_mem=20.89 GB):  93%|█████████▎| 54/58 [00:06<00:00, 15.53it/s]Capturing num tokens (num_tokens=16 avail_mem=20.88 GB):  93%|█████████▎| 54/58 [00:06<00:00, 15.53it/s]Capturing num tokens (num_tokens=12 avail_mem=20.88 GB):  93%|█████████▎| 54/58 [00:06<00:00, 15.53it/s]

    Capturing num tokens (num_tokens=8 avail_mem=20.87 GB):  93%|█████████▎| 54/58 [00:06<00:00, 15.53it/s] Capturing num tokens (num_tokens=8 avail_mem=20.87 GB):  98%|█████████▊| 57/58 [00:06<00:00, 18.63it/s]Capturing num tokens (num_tokens=4 avail_mem=20.87 GB):  98%|█████████▊| 57/58 [00:06<00:00, 18.63it/s]Capturing num tokens (num_tokens=4 avail_mem=20.87 GB): 100%|██████████| 58/58 [00:06<00:00,  8.86it/s]


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not exactly sure about the current population numbers. I remember that Paris is a very big city, but I think it's not the largest in the world. Maybe around 20 million? I'm not certain, though. I should probably double-check that. <br><br>Wait, I think I heard somewhere that Paris has a population over 21 million. Maybe 21.6 million? I'm not sure if that's accurate. I should look up the latest data to confirm. Also, I wonder if the population includes just the city proper or the entire metropolitan area. I think sometimes population counts include the broader area, so that might be a consideration. <br><br>I should make sure to present the information clearly in JSON format, as the user requested. So, the key would be "capital" with the value "Paris," and another key for "population." I'll need to include the number, maybe with a note about whether it's an approximate figure or the most recent data. <br><br>I'm a bit confused about whether the population figure I have is up to date. I think the population can change over time due to births, deaths, and migration. So, it's important to mention that the figure is approximate or based on the latest available data. <br><br>Also, I should consider the units. Is it in thousands, millions, or just a plain number? I think it's best to present it as a number without units, just the raw count. <br><br>Putting it all together, I'll structure the JSON with the keys "capital" and "population," and include the population as 21.6 million. I'll add a comment or note that the population figure is approximate. That should cover everything the user asked for.<br><br><br>content: {<br><br>"name": "Paris",<br>"population": 21600000}</strong>


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not exactly sure about the current population numbers. I remember that Paris is a very big city, but I think it's not the largest in the world. Maybe around 20 million? I'm not certain, though. I should probably check that.<br><br>Wait, I think the population has been growing over the years. I recall reading somewhere that it's over 21 million now. Maybe around 21.6 million? I'm not sure if that's the exact number or just an estimate. I should look it up to confirm. Also, I should make sure that Paris is indeed the capital and not another city like Lyon or Marseille.<br><br>I'm pretty confident that Paris is the capital, but just to be thorough, I'll double-check. The official capital of France is Paris, so that's settled. Now, for the population, I think it's a bit tricky because population numbers can change yearly. I might find a figure that's a bit higher or lower depending on the source. Maybe I should look for the most recent data available, perhaps from 2023, to get the accurate number.<br><br>I think the population is around 21.6 million, but I'm not 100% sure. I should also consider whether this includes only the city proper or the entire metropolitan area. Sometimes, population figures can include surrounding suburbs and satellite towns. If the data I find includes the metropolitan area, that might be a different number. I need to clarify that.<br><br>Another thing to consider is that the population can vary slightly depending on the source. Some sources might use estimates, while others might have more precise data. I should look for a reliable source, like the official statistics from the French National Institute of Statistics and Economic Studies (INSEE) or another reputable organization. That way, I can be more confident in the accuracy of the number.<br><br>In summary, I'm pretty sure the capital is Paris, and the population is around 21.6 million, but I should verify this information to ensure it's correct. Once I have the exact number, I can present it in the JSON format as requested.<br><br><br>content: {<br><br>"name": "Paris",<br>"population": 21620000<br>}</strong>


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not exactly sure about the current population. I think it's a big city, maybe around 3 million? I remember hearing that Paris is one of the most populous cities in Europe, but I'm not certain about the exact number. Maybe I should check some sources or think about recent growth. I think the population has been increasing over the years, so perhaps it's now over 3.5 million? I'm a bit confused because sometimes I hear different numbers, so I should make sure. Maybe I can recall that Paris has a metropolitan area that's much larger, but the city proper is around 3.5 million. I think I'll go with that for now, but I'm not 100% sure. I should probably double-check this information to be accurate.<br><br><br>content: Paris is the capital of France</strong>


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France. Hmm, I remember learning about France in school, but I'm not 100% sure. Let me think. I know that Paris is a major city in France, and it's often referred to as the "City of Light." People go there for museums, landmarks like the Eiffel Tower, and it's a cultural hub. But is it the capital?<br><br>Wait, I think the capital is the official seat of government, right? So maybe Paris is both the capital and the most famous city. But I'm not entirely certain. I recall that some countries have their capital in a different city than their main tourist attraction. For example, I think Brazil's capital is not Rio de Janeiro, which is more famous. So maybe France is like that too.<br><br>I should try to remember any specific information. I think the French government declares Paris as the capital. Yeah, that sounds right. I also remember that the Eiffel Tower is in Paris, and it's a symbol of the country. So if Paris is the capital, then that makes sense. But I'm a bit confused because sometimes people say "the capital of France is Paris," but I want to make sure I'm not mixing it up with other countries.<br><br>Let me think about other capitals I know. For example, Germany's capital is Berlin, Italy's is Rome, Spain's is Madrid. So France's capital should be a major city in the north, maybe. Paris is in the north, so that fits. I think I've heard it said that Paris is the capital, so I'm pretty confident now. Yeah, I think I'm right.<br><br><br>content: Paris</strong>


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


<strong style='color: #00008B;'>reasoing_content: Alright, the user is in New York and wants to know the current date and time along with the weather. I need to figure out how to provide this using the functions available.<br><br>First, I should use the get_current_date function because it gives the date and time for a specific timezone. The user is in New York, so I'll set the timezone parameter to 'America/New_York'. That should give me the correct datetime.<br><br>Next, for the weather, I'll use get_current_weather. The city is New York, the state is NY, and since the user didn't specify, I'll default to Celsius for the unit. So I'll call get_current_weather with city: 'New York', state: 'NY', unit: 'celsius'.<br><br>I need to structure each function call separately, as per the instructions. Each function call should be on its own line with the correct parameters in JSON format. I'll make sure to include the unit in the weather function as 'celsius' since it wasn't provided.<br><br>I should also remember to format the responses exactly as specified, using the start_tag and end_tag correctly. No markdown, just plain text. Also, I need to ensure that all required parameters are included in each function call.<br><br>Putting it all together, I'll send two separate function calls: one for the date and time, and another for the weather. Each will have their parameters clearly defined in JSON within the function call tags.<br><br><br>content: <function=get_current_date>{"timezone": "America/New_York"}</function>  <br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "celsius"}</function></strong>


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

    {'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down. First, I need to identify what the capital of France is. I know that Paris is the capital, so that\'s straightforward. \n\nNext, I need to find the population. I remember that Paris is a major city, so its population is quite large. I think it\'s over 3 million, but I\'m not exactly sure of the exact number. Maybe I should double-check that. \n\nWait, I recall that the population figure can vary depending on the source and the year. The user didn\'t specify a particular year, so I should probably go with the most recent estimate. I believe the population is around 3,500,000 as of 2023. \n\nNow, I need to structure this information into a JSON format. JSON typically uses key-value pairs, so I\'ll create an object with keys like "city", "population", and maybe "country" since the user mentioned France. \n\nI should make sure the keys are in English to keep it clear. The city is Paris, the population is 3,500,000, and the country is France. I\'ll format this into a JSON object, ensuring proper syntax with commas and quotation marks. \n\nI also need to present this in a way that\'s easy to read, so I\'ll put each key on a new line. That way, the user can quickly see the information without confusion. \n\nI wonder if the user needs more details, like the exact current population or additional statistics. But since they only asked for the capital and population, I\'ll stick to that. \n\nLastly, I\'ll make sure the JSON is valid by checking the syntax. No trailing commas, proper use of braces, and correct quotation marks. That should cover everything the user needs.\n</think>{\n  "name": "Paris",\n  "population": 3500000\n}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 13, 5512, 11, 358, 1184, 311, 10542, 1128, 279, 6722, 315, 9625, 374, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4710, 5847, 11, 358, 1184, 311, 1477, 279, 7042, 13, 358, 6099, 429, 12095, 374, 264, 3598, 3283, 11, 773, 1181, 7042, 374, 5008, 3460, 13, 358, 1744, 432, 594, 916, 220, 18, 3526, 11, 714, 358, 2776, 537, 6896, 2704, 315, 279, 4734, 1372, 13, 10696, 358, 1265, 1990, 15934, 429, 13, 4710, 14190, 11, 358, 19091, 429, 279, 7042, 7071, 646, 13289, 11649, 389, 279, 2530, 323, 279, 1042, 13, 576, 1196, 3207, 944, 13837, 264, 3953, 1042, 11, 773, 358, 1265, 4658, 728, 448, 279, 1429, 3213, 16045, 13, 358, 4411, 279, 7042, 374, 2163, 220, 18, 11, 20, 15, 15, 11, 15, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 4710, 7039, 11, 358, 1184, 311, 5944, 419, 1995, 1119, 264, 4718, 3561, 13, 4718, 11136, 5711, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 7196, 330, 11141, 1, 2474, 279, 1196, 9733, 9625, 13, 4710, 40, 1265, 1281, 2704, 279, 6894, 525, 304, 6364, 311, 2506, 432, 2797, 13, 576, 3283, 374, 12095, 11, 279, 7042, 374, 220, 18, 11, 20, 15, 15, 11, 15, 15, 15, 11, 323, 279, 3146, 374, 9625, 13, 358, 3278, 3561, 419, 1119, 264, 4718, 1633, 11, 22573, 6169, 19482, 448, 76602, 323, 54231, 15423, 13, 4710, 40, 1083, 1184, 311, 3042, 419, 304, 264, 1616, 429, 594, 4135, 311, 1349, 11, 773, 358, 3278, 2182, 1817, 1376, 389, 264, 501, 1555, 13, 2938, 1616, 11, 279, 1196, 646, 6157, 1490, 279, 1995, 2041, 21340, 13, 4710, 40, 5775, 421, 279, 1196, 3880, 803, 3565, 11, 1075, 279, 4734, 1482, 7042, 476, 5107, 13142, 13, 1988, 2474, 807, 1172, 4588, 369, 279, 6722, 323, 7042, 11, 358, 3278, 9214, 311, 429, 13, 4710, 80486, 11, 358, 3278, 1281, 2704, 279, 4718, 374, 2697, 553, 13295, 279, 19482, 13, 2308, 27748, 76602, 11, 6169, 990, 315, 59191, 11, 323, 4396, 54231, 15423, 13, 2938, 1265, 3421, 4297, 279, 1196, 3880, 624, 151649, 515, 220, 330, 606, 788, 330, 59604, 756, 220, 330, 44441, 788, 220, 18, 20, 15, 15, 15, 15, 15, 198, 92, 151643], 'meta_info': {'id': '733e0215aeb24df3bf5fdbd98b7427ba', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 412, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 2.746113753062673, 'response_sent_to_client_ts': 1774879323.3041449}}



<strong style='color: #00008B;'>reasoing_content: Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down. First, I need to identify what the capital of France is. I know that Paris is the capital, so that's straightforward. <br><br>Next, I need to find the population. I remember that Paris is a major city, so its population is quite large. I think it's over 3 million, but I'm not exactly sure of the exact number. Maybe I should double-check that. <br><br>Wait, I recall that the population figure can vary depending on the source and the year. The user didn't specify a particular year, so I should probably go with the most recent estimate. I believe the population is around 3,500,000 as of 2023. <br><br>Now, I need to structure this information into a JSON format. JSON typically uses key-value pairs, so I'll create an object with keys like "city", "population", and maybe "country" since the user mentioned France. <br><br>I should make sure the keys are in English to keep it clear. The city is Paris, the population is 3,500,000, and the country is France. I'll format this into a JSON object, ensuring proper syntax with commas and quotation marks. <br><br>I also need to present this in a way that's easy to read, so I'll put each key on a new line. That way, the user can quickly see the information without confusion. <br><br>I wonder if the user needs more details, like the exact current population or additional statistics. But since they only asked for the capital and population, I'll stick to that. <br><br>Lastly, I'll make sure the JSON is valid by checking the syntax. No trailing commas, proper use of braces, and correct quotation marks. That should cover everything the user needs.<br><br><br>content: {<br>  "name": "Paris",<br>  "population": 3500000<br>}</strong>


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


<strong style='color: #00008B;'>{'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not sure of the exact number. I think it\'s around 2 million, but I should verify that.\n\nI\'ll check a reliable source, maybe the official Paris Municipality website or a recent census. Let me see... Yes, according to the latest data, the population is approximately 2,174,300 as of 2023. That seems accurate.\n\nNext, I need to structure this information into a JSON format. JSON requires key-value pairs, so I\'ll create an object with keys like "city", "population", and "country". The city is Paris, the population is the number I found, and the country is France.\n\nI should make sure the JSON syntax is correct. Each key should be in quotes, and the values as well. The entire structure should be enclosed in curly braces. I\'ll format it properly to avoid any syntax errors.\n\nPutting it all together, the JSON object will have the city, population, and country. I\'ll present this to the user, making sure it\'s clear and easy to understand. I don\'t think the user needs anything more detailed, so this should suffice.\n\nI should also consider if the user might need additional information, like the area or age distribution, but since they only asked for population, I\'ll stick to that. Maybe they\'ll ask for more details later, but for now, this response should be helpful.\n</think>{\n  "name": "Paris",\n  "population": 2174300\n}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 40, 3278, 1779, 264, 14720, 2530, 11, 7196, 279, 3946, 12095, 35703, 2719, 3910, 476, 264, 3213, 43602, 13, 6771, 752, 1490, 1112, 7414, 11, 4092, 311, 279, 5535, 821, 11, 279, 7042, 374, 13187, 220, 17, 11, 16, 22, 19, 11, 18, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 2938, 4977, 13382, 382, 5847, 11, 358, 1184, 311, 5944, 419, 1995, 1119, 264, 4718, 3561, 13, 4718, 7460, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 330, 11141, 3263, 576, 3283, 374, 12095, 11, 279, 7042, 374, 279, 1372, 358, 1730, 11, 323, 279, 3146, 374, 9625, 382, 40, 1265, 1281, 2704, 279, 4718, 19482, 374, 4396, 13, 8886, 1376, 1265, 387, 304, 17194, 11, 323, 279, 2750, 438, 1632, 13, 576, 4453, 5944, 1265, 387, 43810, 304, 68103, 59191, 13, 358, 3278, 3561, 432, 10277, 311, 5648, 894, 19482, 5975, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1633, 686, 614, 279, 3283, 11, 7042, 11, 323, 3146, 13, 358, 3278, 3042, 419, 311, 279, 1196, 11, 3259, 2704, 432, 594, 2797, 323, 4135, 311, 3535, 13, 358, 1513, 944, 1744, 279, 1196, 3880, 4113, 803, 11682, 11, 773, 419, 1265, 76156, 382, 40, 1265, 1083, 2908, 421, 279, 1196, 2578, 1184, 5107, 1995, 11, 1075, 279, 3082, 476, 4231, 7982, 11, 714, 2474, 807, 1172, 4588, 369, 7042, 11, 358, 3278, 9214, 311, 429, 13, 10696, 807, 3278, 2548, 369, 803, 3565, 2937, 11, 714, 369, 1431, 11, 419, 2033, 1265, 387, 10950, 624, 151649, 515, 220, 330, 606, 788, 330, 59604, 756, 220, 330, 44441, 788, 220, 17, 16, 22, 19, 18, 15, 15, 198, 92, 151643], 'meta_info': {'id': '7f1de5d290bb4944a5e9606bd61661cb', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 384, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 2.5584132140502334, 'response_sent_to_client_ts': 1774879325.872056}}</strong>


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

    [{'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '29f8071377a24531af6e19986c276bc2', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.087849409901537, 'response_sent_to_client_ts': 1774879325.984616}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '5f58954936cc4112841ffc3ede079e68', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.08776250795926899, 'response_sent_to_client_ts': 1774879325.9846287}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '5befbcb56b1d4c068546f3630f2b112b', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.08771658595651388, 'response_sent_to_client_ts': 1774879325.984633}}]


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

    {'text': ' France, and the \n\\( n \\)  \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\(', 'output_ids': [9625, 11, 323, 279, 220, 198, 44292, 308, 1124, 8, 220, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767], 'meta_info': {'id': '94d25b4d57bc40ee8fd00d5b2b2d8363', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 6, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 23.223361339070834, 'response_sent_to_client_ts': 1774879349.2148569}}


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


<strong style='color: #00008B;'>{'text': 'Okay, so I need to figure out what the user is asking for. They want the information and population of France\'s capital in JSON format. Hmm, the capital of France is Paris, right? So I should start by noting that. I know Paris is a major city with a rich history and a significant population, but I\'m not exactly sure about the current population figure.\n\nWait, I think the population is around 2 million. But I\'m not 100% certain. I remember reading that it\'s over two million, maybe a bit more. Let me check my facts to make sure. Oh, right, I saw a reference that says the population is approximately 2,165,827 as of 2023. That sounds more precise.\n\nNow, the user asked for info and population, so I should include some key facts about Paris. It\'s a global city, known for its culture, arts, and landmarks like the Eiffel Tower, Louvre, and the Opera. Paris is also the headquarters of the EU and has been the capital for a long time—since 18世纪 there\'s a claim that it\'s been the capital, but it wasn\'t before that until Napoleon moved it. So the timeline there might be interesting.\n\nPutting it all together, I\'ll structure the JSON with the population number first, then the location, followed by the keyword and some key facts. I need to make sure the JSON syntax is correct, with proper commas and quotes. Let me write it out:\n\n{\n  "population": 2165827,\n  "location": "Paris, Île-de-France, France",\n  "keyword": "population",\n  "fact1": "Paris is a global city renowned for its rich cultural scene, world-class arts, and landmarks such as the Eiffel Tower, the Louvre Museum, and the Grand opera house.",\n  "fact2": "It serves as the administrative center of the Île-de-France region and has been the capital of France since the 19th century.",\n  "fact3": "Despite its picturesque nickname \'The City of Light,\' Paris has faced several challenges including waterlogging, but it has thrived as a global metropolis."\n}\n\nWait, did I include everything? The user said "information and population," so I covered the population and some info. Also, specified the location correctly. I think that\'s good enough. Let me double-check if the population number is up to date. Yeah, since 2023, that number seems accurate.\n\nI don\'t think I need to add anything else. The user just wants the data in JSON, so I\'ll present it as is. Alright, I think that covers everything they asked for.\n</think>\n\n```json\n{\n  "population": 2165827,\n  "location": "Paris, Île-de-France, France",\n  "keyword": "population",\n  "fact1": "Paris is a global city renowned for its rich cultural scene, world-class arts, and landmarks such as the Eiffel Tower, the Louvre Museum, and the Grand opera house.",\n  "fact2": "It serves as the administrative center of the Île-de-France region and has been the capital of France since the 19th century.",\n  "fact3": "Despite its picturesque nickname \'The City of Light,\' Paris has faced several challenges including waterlogging, but it has throve as a global metropolis."\n}\n```', 'output_ids': [32313, 11, 773, 358, 1184, 311, 7071, 700, 1128, 279, 1196, 374, 10161, 369, 13, 2379, 1366, 279, 1995, 323, 7042, 315, 9625, 594, 6722, 304, 4718, 3561, 13, 88190, 11, 279, 6722, 315, 9625, 374, 12095, 11, 1290, 30, 2055, 358, 1265, 1191, 553, 26305, 429, 13, 358, 1414, 12095, 374, 264, 3598, 3283, 448, 264, 9080, 3840, 323, 264, 5089, 7042, 11, 714, 358, 2776, 537, 6896, 2704, 911, 279, 1482, 7042, 7071, 382, 14190, 11, 358, 1744, 279, 7042, 374, 2163, 220, 17, 3526, 13, 1988, 358, 2776, 537, 220, 16, 15, 15, 4, 3654, 13, 358, 6099, 5290, 429, 432, 594, 916, 1378, 3526, 11, 7196, 264, 2699, 803, 13, 6771, 752, 1779, 847, 13064, 311, 1281, 2704, 13, 8670, 11, 1290, 11, 358, 5485, 264, 5785, 429, 2727, 279, 7042, 374, 13187, 220, 17, 11, 16, 21, 20, 11, 23, 17, 22, 438, 315, 220, 17, 15, 17, 18, 13, 2938, 10362, 803, 23560, 382, 7039, 11, 279, 1196, 4588, 369, 3546, 323, 7042, 11, 773, 358, 1265, 2924, 1045, 1376, 13064, 911, 12095, 13, 1084, 594, 264, 3644, 3283, 11, 3881, 369, 1181, 7674, 11, 18560, 11, 323, 59924, 1075, 279, 468, 3092, 301, 21938, 11, 9729, 48506, 11, 323, 279, 38579, 13, 12095, 374, 1083, 279, 25042, 315, 279, 9812, 323, 702, 1012, 279, 6722, 369, 264, 1293, 882, 2293, 11284, 220, 16, 23, 101186, 1052, 594, 264, 3717, 429, 432, 594, 1012, 279, 6722, 11, 714, 432, 5710, 944, 1573, 429, 3080, 69427, 7726, 432, 13, 2055, 279, 24808, 1052, 2578, 387, 7040, 382, 97904, 432, 678, 3786, 11, 358, 3278, 5944, 279, 4718, 448, 279, 7042, 1372, 1156, 11, 1221, 279, 3728, 11, 8110, 553, 279, 16174, 323, 1045, 1376, 13064, 13, 358, 1184, 311, 1281, 2704, 279, 4718, 19482, 374, 4396, 11, 448, 6169, 76602, 323, 17194, 13, 6771, 752, 3270, 432, 700, 1447, 515, 220, 330, 44441, 788, 220, 17, 16, 21, 20, 23, 17, 22, 345, 220, 330, 2527, 788, 330, 59604, 11, 59108, 273, 6810, 7276, 34106, 11, 9625, 756, 220, 330, 19863, 788, 330, 44441, 756, 220, 330, 33110, 16, 788, 330, 59604, 374, 264, 3644, 3283, 35948, 369, 1181, 9080, 12752, 6109, 11, 1879, 14800, 18560, 11, 323, 59924, 1741, 438, 279, 468, 3092, 301, 21938, 11, 279, 9729, 48506, 16328, 11, 323, 279, 10304, 42416, 3753, 10346, 220, 330, 33110, 17, 788, 330, 2132, 17045, 438, 279, 22707, 4126, 315, 279, 59108, 273, 6810, 7276, 34106, 5537, 323, 702, 1012, 279, 6722, 315, 9625, 2474, 279, 220, 16, 24, 339, 9294, 10346, 220, 330, 33110, 18, 788, 330, 19810, 1181, 73901, 29399, 364, 785, 4311, 315, 8658, 2894, 12095, 702, 16601, 3807, 11513, 2670, 3015, 25263, 11, 714, 432, 702, 8807, 2221, 438, 264, 3644, 2270, 54322, 10040, 630, 14190, 11, 1521, 358, 2924, 4297, 30, 576, 1196, 1053, 330, 25069, 323, 7042, 1335, 773, 358, 9761, 279, 7042, 323, 1045, 3546, 13, 7281, 11, 5189, 279, 3728, 12440, 13, 358, 1744, 429, 594, 1661, 3322, 13, 6771, 752, 1990, 15934, 421, 279, 7042, 1372, 374, 705, 311, 2400, 13, 21607, 11, 2474, 220, 17, 15, 17, 18, 11, 429, 1372, 4977, 13382, 382, 40, 1513, 944, 1744, 358, 1184, 311, 912, 4113, 770, 13, 576, 1196, 1101, 6801, 279, 821, 304, 4718, 11, 773, 358, 3278, 3042, 432, 438, 374, 13, 97593, 11, 358, 1744, 429, 14521, 4297, 807, 4588, 369, 624, 151649, 271, 73594, 2236, 198, 515, 220, 330, 44441, 788, 220, 17, 16, 21, 20, 23, 17, 22, 345, 220, 330, 2527, 788, 330, 59604, 11, 59108, 273, 6810, 7276, 34106, 11, 9625, 756, 220, 330, 19863, 788, 330, 44441, 756, 220, 330, 33110, 16, 788, 330, 59604, 374, 264, 3644, 3283, 35948, 369, 1181, 9080, 12752, 6109, 11, 1879, 14800, 18560, 11, 323, 59924, 1741, 438, 279, 468, 3092, 301, 21938, 11, 279, 9729, 48506, 16328, 11, 323, 279, 10304, 42416, 3753, 10346, 220, 330, 33110, 17, 788, 330, 2132, 17045, 438, 279, 22707, 4126, 315, 279, 59108, 273, 6810, 7276, 34106, 5537, 323, 702, 1012, 279, 6722, 315, 9625, 2474, 279, 220, 16, 24, 339, 9294, 10346, 220, 330, 33110, 18, 788, 330, 19810, 1181, 73901, 29399, 364, 785, 4311, 315, 8658, 2894, 12095, 702, 16601, 3807, 11513, 2670, 3015, 25263, 11, 714, 432, 702, 270, 50415, 438, 264, 3644, 2270, 54322, 10040, 532, 73594, 151643], 'meta_info': {'id': '86b2753fffb047338e202e9661f3a28d', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 727, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 12.19720469310414, 'response_sent_to_client_ts': 1774879361.4203937}}</strong>



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


    2026-03-30 14:02:53.018 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 14:02:53] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 14:02:53.019 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 14:02:53] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 14:02:53.019 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 14:02:53] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 14:02:53.019 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 14:02:53] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 14:02:53.019 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 14:02:53] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.00it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.45s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.38s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:00,  3.17s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:00,  3.17s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:27,  1.56s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:27,  1.56s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:51,  1.06it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:51,  1.06it/s]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:34,  1.56it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:34,  1.56it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:24,  2.13it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:24,  2.13it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.74it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.74it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.45it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.45it/s]Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.22it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.22it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.06it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.06it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.06it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:07,  6.65it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:07,  6.65it/s]

    Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:07,  6.65it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:05<00:05,  8.13it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:05<00:05,  8.13it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:05<00:05,  8.13it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:04,  9.62it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:04,  9.62it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04,  9.62it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 11.46it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 11.46it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.46it/s]

    Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 11.46it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 14.27it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 14.27it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 14.27it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 14.27it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:05<00:01, 17.78it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:05<00:01, 17.78it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:05<00:01, 17.78it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:05<00:01, 17.78it/s]

    Compiling num tokens (num_tokens=640):  40%|███▉      | 23/58 [00:05<00:01, 17.78it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:05<00:01, 23.08it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:05<00:01, 23.08it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:05<00:01, 23.08it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:05<00:01, 23.08it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:05<00:01, 23.08it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:05<00:01, 23.08it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:05<00:00, 28.42it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:05<00:00, 28.42it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:05<00:00, 28.42it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:05<00:00, 28.42it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:05<00:00, 28.42it/s]

    Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:05<00:00, 28.42it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:05<00:00, 28.42it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:05<00:00, 35.46it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:05<00:00, 35.46it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:05<00:00, 35.46it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:05<00:00, 35.46it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:05<00:00, 35.46it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:05<00:00, 35.46it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:05<00:00, 35.46it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:05<00:00, 40.41it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:05<00:00, 40.41it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:05<00:00, 40.41it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:06<00:00, 40.41it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:06<00:00, 40.41it/s]

    Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:06<00:00, 40.41it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:06<00:00, 40.41it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:06<00:00, 44.88it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:06<00:00, 44.88it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:06<00:00, 44.88it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:06<00:00, 44.88it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:06<00:00, 44.88it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:06<00:00, 44.88it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:06<00:00, 44.88it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:06<00:00, 44.88it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:06<00:00, 44.88it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  9.38it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=37.41 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=37.41 GB):   2%|▏         | 1/58 [00:00<00:18,  3.13it/s]Capturing num tokens (num_tokens=7680 avail_mem=37.38 GB):   2%|▏         | 1/58 [00:00<00:18,  3.13it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=37.38 GB):   3%|▎         | 2/58 [00:00<00:17,  3.23it/s]Capturing num tokens (num_tokens=7168 avail_mem=37.38 GB):   3%|▎         | 2/58 [00:00<00:17,  3.23it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=37.38 GB):   5%|▌         | 3/58 [00:00<00:16,  3.37it/s]Capturing num tokens (num_tokens=6656 avail_mem=37.39 GB):   5%|▌         | 3/58 [00:00<00:16,  3.37it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=37.39 GB):   7%|▋         | 4/58 [00:01<00:14,  3.63it/s]Capturing num tokens (num_tokens=6144 avail_mem=37.39 GB):   7%|▋         | 4/58 [00:01<00:14,  3.63it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=37.39 GB):   9%|▊         | 5/58 [00:01<00:13,  3.95it/s]Capturing num tokens (num_tokens=5632 avail_mem=36.40 GB):   9%|▊         | 5/58 [00:01<00:13,  3.95it/s]Capturing num tokens (num_tokens=5632 avail_mem=36.40 GB):  10%|█         | 6/58 [00:01<00:11,  4.40it/s]Capturing num tokens (num_tokens=5120 avail_mem=36.40 GB):  10%|█         | 6/58 [00:01<00:11,  4.40it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=36.40 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.82it/s]Capturing num tokens (num_tokens=4608 avail_mem=36.41 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.82it/s]Capturing num tokens (num_tokens=4608 avail_mem=36.41 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.36it/s]Capturing num tokens (num_tokens=4096 avail_mem=36.41 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.36it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=36.41 GB):  16%|█▌        | 9/58 [00:02<00:10,  4.64it/s]Capturing num tokens (num_tokens=3840 avail_mem=35.26 GB):  16%|█▌        | 9/58 [00:02<00:10,  4.64it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=35.26 GB):  17%|█▋        | 10/58 [00:02<00:11,  4.08it/s]Capturing num tokens (num_tokens=3584 avail_mem=35.26 GB):  17%|█▋        | 10/58 [00:02<00:11,  4.08it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=35.26 GB):  19%|█▉        | 11/58 [00:02<00:12,  3.91it/s]Capturing num tokens (num_tokens=3328 avail_mem=35.26 GB):  19%|█▉        | 11/58 [00:02<00:12,  3.91it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=35.26 GB):  21%|██        | 12/58 [00:02<00:11,  3.84it/s]Capturing num tokens (num_tokens=3072 avail_mem=35.26 GB):  21%|██        | 12/58 [00:02<00:11,  3.84it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=35.26 GB):  22%|██▏       | 13/58 [00:03<00:11,  3.91it/s]Capturing num tokens (num_tokens=2816 avail_mem=35.26 GB):  22%|██▏       | 13/58 [00:03<00:11,  3.91it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=35.26 GB):  24%|██▍       | 14/58 [00:03<00:10,  4.06it/s]Capturing num tokens (num_tokens=2560 avail_mem=35.26 GB):  24%|██▍       | 14/58 [00:03<00:10,  4.06it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=35.26 GB):  26%|██▌       | 15/58 [00:03<00:10,  4.24it/s]Capturing num tokens (num_tokens=2304 avail_mem=35.26 GB):  26%|██▌       | 15/58 [00:03<00:10,  4.24it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=35.26 GB):  28%|██▊       | 16/58 [00:03<00:09,  4.29it/s]Capturing num tokens (num_tokens=2048 avail_mem=35.24 GB):  28%|██▊       | 16/58 [00:03<00:09,  4.29it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=35.24 GB):  29%|██▉       | 17/58 [00:04<00:09,  4.39it/s]Capturing num tokens (num_tokens=1792 avail_mem=35.22 GB):  29%|██▉       | 17/58 [00:04<00:09,  4.39it/s]Capturing num tokens (num_tokens=1792 avail_mem=35.22 GB):  31%|███       | 18/58 [00:04<00:08,  4.74it/s]Capturing num tokens (num_tokens=1536 avail_mem=34.61 GB):  31%|███       | 18/58 [00:04<00:08,  4.74it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=34.61 GB):  33%|███▎      | 19/58 [00:04<00:07,  5.11it/s]Capturing num tokens (num_tokens=1280 avail_mem=34.61 GB):  33%|███▎      | 19/58 [00:04<00:07,  5.11it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=34.61 GB):  34%|███▍      | 20/58 [00:04<00:07,  5.07it/s]Capturing num tokens (num_tokens=1024 avail_mem=34.59 GB):  34%|███▍      | 20/58 [00:04<00:07,  5.07it/s]Capturing num tokens (num_tokens=1024 avail_mem=34.59 GB):  36%|███▌      | 21/58 [00:04<00:06,  5.66it/s]Capturing num tokens (num_tokens=960 avail_mem=34.55 GB):  36%|███▌      | 21/58 [00:04<00:06,  5.66it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=34.55 GB):  38%|███▊      | 22/58 [00:04<00:05,  6.17it/s]Capturing num tokens (num_tokens=896 avail_mem=34.55 GB):  38%|███▊      | 22/58 [00:04<00:05,  6.17it/s]Capturing num tokens (num_tokens=896 avail_mem=34.55 GB):  40%|███▉      | 23/58 [00:05<00:05,  6.75it/s]Capturing num tokens (num_tokens=832 avail_mem=34.52 GB):  40%|███▉      | 23/58 [00:05<00:05,  6.75it/s]

    Capturing num tokens (num_tokens=832 avail_mem=34.52 GB):  41%|████▏     | 24/58 [00:05<00:04,  7.11it/s]Capturing num tokens (num_tokens=768 avail_mem=34.49 GB):  41%|████▏     | 24/58 [00:05<00:04,  7.11it/s]Capturing num tokens (num_tokens=768 avail_mem=34.49 GB):  43%|████▎     | 25/58 [00:05<00:04,  7.50it/s]Capturing num tokens (num_tokens=704 avail_mem=34.49 GB):  43%|████▎     | 25/58 [00:05<00:04,  7.50it/s]

    Capturing num tokens (num_tokens=704 avail_mem=34.49 GB):  45%|████▍     | 26/58 [00:05<00:03,  8.03it/s]Capturing num tokens (num_tokens=640 avail_mem=34.49 GB):  45%|████▍     | 26/58 [00:05<00:03,  8.03it/s]Capturing num tokens (num_tokens=640 avail_mem=34.49 GB):  47%|████▋     | 27/58 [00:05<00:03,  8.48it/s]Capturing num tokens (num_tokens=576 avail_mem=34.48 GB):  47%|████▋     | 27/58 [00:05<00:03,  8.48it/s]Capturing num tokens (num_tokens=512 avail_mem=34.48 GB):  47%|████▋     | 27/58 [00:05<00:03,  8.48it/s]

    Capturing num tokens (num_tokens=512 avail_mem=34.48 GB):  50%|█████     | 29/58 [00:05<00:02, 10.07it/s]Capturing num tokens (num_tokens=480 avail_mem=34.47 GB):  50%|█████     | 29/58 [00:05<00:02, 10.07it/s]Capturing num tokens (num_tokens=448 avail_mem=34.47 GB):  50%|█████     | 29/58 [00:05<00:02, 10.07it/s]Capturing num tokens (num_tokens=448 avail_mem=34.47 GB):  53%|█████▎    | 31/58 [00:05<00:02, 11.78it/s]Capturing num tokens (num_tokens=416 avail_mem=34.47 GB):  53%|█████▎    | 31/58 [00:05<00:02, 11.78it/s]Capturing num tokens (num_tokens=384 avail_mem=34.46 GB):  53%|█████▎    | 31/58 [00:05<00:02, 11.78it/s]

    Capturing num tokens (num_tokens=384 avail_mem=34.46 GB):  57%|█████▋    | 33/58 [00:05<00:01, 12.97it/s]Capturing num tokens (num_tokens=352 avail_mem=34.46 GB):  57%|█████▋    | 33/58 [00:05<00:01, 12.97it/s]Capturing num tokens (num_tokens=320 avail_mem=34.45 GB):  57%|█████▋    | 33/58 [00:05<00:01, 12.97it/s]Capturing num tokens (num_tokens=320 avail_mem=34.45 GB):  60%|██████    | 35/58 [00:05<00:01, 14.38it/s]Capturing num tokens (num_tokens=288 avail_mem=34.45 GB):  60%|██████    | 35/58 [00:05<00:01, 14.38it/s]Capturing num tokens (num_tokens=256 avail_mem=34.45 GB):  60%|██████    | 35/58 [00:06<00:01, 14.38it/s]Capturing num tokens (num_tokens=240 avail_mem=34.44 GB):  60%|██████    | 35/58 [00:06<00:01, 14.38it/s]

    Capturing num tokens (num_tokens=240 avail_mem=34.44 GB):  66%|██████▌   | 38/58 [00:06<00:01, 17.09it/s]Capturing num tokens (num_tokens=224 avail_mem=34.44 GB):  66%|██████▌   | 38/58 [00:06<00:01, 17.09it/s]Capturing num tokens (num_tokens=208 avail_mem=34.43 GB):  66%|██████▌   | 38/58 [00:06<00:01, 17.09it/s]Capturing num tokens (num_tokens=192 avail_mem=34.43 GB):  66%|██████▌   | 38/58 [00:06<00:01, 17.09it/s]Capturing num tokens (num_tokens=192 avail_mem=34.43 GB):  71%|███████   | 41/58 [00:06<00:00, 19.57it/s]Capturing num tokens (num_tokens=176 avail_mem=34.43 GB):  71%|███████   | 41/58 [00:06<00:00, 19.57it/s]Capturing num tokens (num_tokens=160 avail_mem=34.42 GB):  71%|███████   | 41/58 [00:06<00:00, 19.57it/s]Capturing num tokens (num_tokens=144 avail_mem=34.42 GB):  71%|███████   | 41/58 [00:06<00:00, 19.57it/s]

    Capturing num tokens (num_tokens=144 avail_mem=34.42 GB):  76%|███████▌  | 44/58 [00:06<00:00, 21.67it/s]Capturing num tokens (num_tokens=128 avail_mem=34.42 GB):  76%|███████▌  | 44/58 [00:06<00:00, 21.67it/s]Capturing num tokens (num_tokens=112 avail_mem=34.42 GB):  76%|███████▌  | 44/58 [00:06<00:00, 21.67it/s]Capturing num tokens (num_tokens=96 avail_mem=34.42 GB):  76%|███████▌  | 44/58 [00:06<00:00, 21.67it/s] Capturing num tokens (num_tokens=96 avail_mem=34.42 GB):  81%|████████  | 47/58 [00:06<00:00, 23.07it/s]Capturing num tokens (num_tokens=80 avail_mem=34.41 GB):  81%|████████  | 47/58 [00:06<00:00, 23.07it/s]Capturing num tokens (num_tokens=64 avail_mem=34.41 GB):  81%|████████  | 47/58 [00:06<00:00, 23.07it/s]Capturing num tokens (num_tokens=48 avail_mem=34.41 GB):  81%|████████  | 47/58 [00:06<00:00, 23.07it/s]

    Capturing num tokens (num_tokens=48 avail_mem=34.41 GB):  86%|████████▌ | 50/58 [00:06<00:00, 24.47it/s]Capturing num tokens (num_tokens=32 avail_mem=34.35 GB):  86%|████████▌ | 50/58 [00:06<00:00, 24.47it/s]Capturing num tokens (num_tokens=28 avail_mem=34.35 GB):  86%|████████▌ | 50/58 [00:06<00:00, 24.47it/s]Capturing num tokens (num_tokens=24 avail_mem=34.35 GB):  86%|████████▌ | 50/58 [00:06<00:00, 24.47it/s]Capturing num tokens (num_tokens=20 avail_mem=34.34 GB):  86%|████████▌ | 50/58 [00:06<00:00, 24.47it/s]Capturing num tokens (num_tokens=20 avail_mem=34.34 GB):  93%|█████████▎| 54/58 [00:06<00:00, 28.53it/s]Capturing num tokens (num_tokens=16 avail_mem=34.34 GB):  93%|█████████▎| 54/58 [00:06<00:00, 28.53it/s]Capturing num tokens (num_tokens=12 avail_mem=34.34 GB):  93%|█████████▎| 54/58 [00:06<00:00, 28.53it/s]Capturing num tokens (num_tokens=8 avail_mem=34.33 GB):  93%|█████████▎| 54/58 [00:06<00:00, 28.53it/s] Capturing num tokens (num_tokens=4 avail_mem=34.33 GB):  93%|█████████▎| 54/58 [00:06<00:00, 28.53it/s]

    Capturing num tokens (num_tokens=4 avail_mem=34.33 GB): 100%|██████████| 58/58 [00:06<00:00, 30.22it/s]Capturing num tokens (num_tokens=4 avail_mem=34.33 GB): 100%|██████████| 58/58 [00:06<00:00,  8.56it/s]


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
    Generated text: Paris is the capital of France
    ===============================
    Prompt: Give me the information of the capital of Italy.
    Generated text: Berlin is the capital of Italy


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
    Generated text: France
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
    
    Generated text: Alright, the user is asking for the capital of France and its population in JSON format. They probably need this data for a project or report where a structured format is required. I should ensure the JSON is accurate and properly formatted.
    
    First, I need to recall the correct capital of France. It's definitely Paris. Now, for the population, I should note that it's approximate because population figures can change. As of the latest data, Paris has a population of about 2,147,000. I'll make sure to include that in the JSON.
    
    I should structure the JSON with the key "capital" pointing to "Paris" and "population" as the number. It's important to present the information clearly so that the user can easily integrate it into their work without confusion.
    
    Also, I should keep the response concise, as they didn't specify needing any additional information or analysis. Just the facts in JSON. No extra details unless they ask for more. That should cover what they need.
    </think>
    
    Here is the information in JSON format:
    
    ```json
    {
      "capital": "Paris",
      "population": 2147000
    }
    ```



```python
llm.shutdown()
```
