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


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:172: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:01<00:01,  1.11s/it]

    Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.17s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.16s/it]


    2026-04-05 16:46:10,615 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-05 16:46:10] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:54,  3.07s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:54,  3.07s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:23,  1.50s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:23,  1.50s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:52,  1.05it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:52,  1.05it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:37,  1.43it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:37,  1.43it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:28,  1.87it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:28,  1.87it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:21,  2.38it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:21,  2.38it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:17,  2.97it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:17,  2.97it/s]Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:13,  3.59it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:13,  3.59it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:11,  4.32it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:11,  4.32it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:09,  5.04it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:09,  5.04it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:08,  5.84it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:08,  5.84it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:05<00:08,  5.84it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:05<00:06,  7.32it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:05<00:06,  7.32it/s]

    Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:05<00:06,  7.32it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:04,  8.84it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:04,  8.84it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04,  8.84it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 10.95it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 10.95it/s]

    Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 10.95it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 10.95it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 14.30it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 14.30it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 14.30it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 14.30it/s]

    Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:05<00:02, 17.00it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:05<00:02, 17.00it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:05<00:02, 17.00it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:05<00:02, 17.00it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:06<00:01, 19.28it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:06<00:01, 19.28it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:06<00:01, 19.28it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:06<00:01, 19.28it/s]

    Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:06<00:01, 19.28it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:06<00:01, 23.21it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:06<00:01, 23.21it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:06<00:01, 23.21it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:06<00:01, 23.21it/s]Compiling num tokens (num_tokens=352):  52%|█████▏    | 30/58 [00:06<00:01, 23.21it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:06<00:00, 26.70it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:06<00:00, 26.70it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:06<00:00, 26.70it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:06<00:00, 26.70it/s]

    Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:06<00:00, 26.70it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:06<00:00, 29.64it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:06<00:00, 29.64it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:06<00:00, 29.64it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:06<00:00, 29.64it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:06<00:00, 29.64it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:06<00:00, 29.64it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:06<00:00, 32.87it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:06<00:00, 32.87it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:06<00:00, 32.87it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:06<00:00, 32.87it/s]

    Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:06<00:00, 32.87it/s] Compiling num tokens (num_tokens=80):  74%|███████▍  | 43/58 [00:06<00:00, 32.87it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:06<00:00, 35.86it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:06<00:00, 35.86it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:06<00:00, 35.86it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:06<00:00, 35.86it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:06<00:00, 35.86it/s]Compiling num tokens (num_tokens=24):  83%|████████▎ | 48/58 [00:06<00:00, 35.86it/s]Compiling num tokens (num_tokens=20):  83%|████████▎ | 48/58 [00:06<00:00, 35.86it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:06<00:00, 40.23it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:06<00:00, 40.23it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:06<00:00, 40.23it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:06<00:00, 40.23it/s] 

    Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:06<00:00, 40.23it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  8.56it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=101.21 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=101.21 GB):   2%|▏         | 1/58 [00:00<00:19,  2.96it/s]Capturing num tokens (num_tokens=7680 avail_mem=101.16 GB):   2%|▏         | 1/58 [00:00<00:19,  2.96it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=101.16 GB):   3%|▎         | 2/58 [00:00<00:16,  3.33it/s]Capturing num tokens (num_tokens=7168 avail_mem=101.16 GB):   3%|▎         | 2/58 [00:00<00:16,  3.33it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=101.16 GB):   5%|▌         | 3/58 [00:00<00:16,  3.28it/s]Capturing num tokens (num_tokens=6656 avail_mem=100.17 GB):   5%|▌         | 3/58 [00:00<00:16,  3.28it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=100.17 GB):   7%|▋         | 4/58 [00:01<00:20,  2.66it/s]Capturing num tokens (num_tokens=6144 avail_mem=100.17 GB):   7%|▋         | 4/58 [00:01<00:20,  2.66it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=100.17 GB):   9%|▊         | 5/58 [00:01<00:21,  2.47it/s]Capturing num tokens (num_tokens=5632 avail_mem=100.18 GB):   9%|▊         | 5/58 [00:01<00:21,  2.47it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=100.18 GB):  10%|█         | 6/58 [00:02<00:20,  2.49it/s]Capturing num tokens (num_tokens=5120 avail_mem=100.18 GB):  10%|█         | 6/58 [00:02<00:20,  2.49it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=100.18 GB):  12%|█▏        | 7/58 [00:02<00:19,  2.56it/s]Capturing num tokens (num_tokens=4608 avail_mem=100.18 GB):  12%|█▏        | 7/58 [00:02<00:19,  2.56it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=100.18 GB):  14%|█▍        | 8/58 [00:02<00:18,  2.69it/s]Capturing num tokens (num_tokens=4096 avail_mem=100.18 GB):  14%|█▍        | 8/58 [00:02<00:18,  2.69it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=100.18 GB):  16%|█▌        | 9/58 [00:03<00:17,  2.81it/s]Capturing num tokens (num_tokens=3840 avail_mem=100.15 GB):  16%|█▌        | 9/58 [00:03<00:17,  2.81it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=100.15 GB):  17%|█▋        | 10/58 [00:03<00:16,  2.96it/s]Capturing num tokens (num_tokens=3584 avail_mem=100.15 GB):  17%|█▋        | 10/58 [00:03<00:16,  2.96it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=100.15 GB):  19%|█▉        | 11/58 [00:03<00:15,  3.12it/s]Capturing num tokens (num_tokens=3328 avail_mem=100.15 GB):  19%|█▉        | 11/58 [00:03<00:15,  3.12it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=100.15 GB):  21%|██        | 12/58 [00:04<00:13,  3.33it/s]Capturing num tokens (num_tokens=3072 avail_mem=100.15 GB):  21%|██        | 12/58 [00:04<00:13,  3.33it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=100.15 GB):  22%|██▏       | 13/58 [00:04<00:12,  3.57it/s]Capturing num tokens (num_tokens=2816 avail_mem=100.15 GB):  22%|██▏       | 13/58 [00:04<00:12,  3.57it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=100.15 GB):  24%|██▍       | 14/58 [00:04<00:11,  3.83it/s]Capturing num tokens (num_tokens=2560 avail_mem=100.15 GB):  24%|██▍       | 14/58 [00:04<00:11,  3.83it/s]Capturing num tokens (num_tokens=2560 avail_mem=100.15 GB):  26%|██▌       | 15/58 [00:04<00:10,  4.12it/s]Capturing num tokens (num_tokens=2304 avail_mem=100.15 GB):  26%|██▌       | 15/58 [00:04<00:10,  4.12it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=100.15 GB):  28%|██▊       | 16/58 [00:04<00:09,  4.37it/s]Capturing num tokens (num_tokens=2048 avail_mem=100.15 GB):  28%|██▊       | 16/58 [00:04<00:09,  4.37it/s]Capturing num tokens (num_tokens=2048 avail_mem=100.15 GB):  29%|██▉       | 17/58 [00:05<00:08,  4.72it/s]Capturing num tokens (num_tokens=1792 avail_mem=100.15 GB):  29%|██▉       | 17/58 [00:05<00:08,  4.72it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=100.15 GB):  31%|███       | 18/58 [00:05<00:07,  5.25it/s]Capturing num tokens (num_tokens=1536 avail_mem=100.14 GB):  31%|███       | 18/58 [00:05<00:07,  5.25it/s]Capturing num tokens (num_tokens=1536 avail_mem=100.14 GB):  33%|███▎      | 19/58 [00:05<00:06,  5.80it/s]Capturing num tokens (num_tokens=1280 avail_mem=100.15 GB):  33%|███▎      | 19/58 [00:05<00:06,  5.80it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=100.15 GB):  34%|███▍      | 20/58 [00:05<00:05,  6.50it/s]Capturing num tokens (num_tokens=1024 avail_mem=100.15 GB):  34%|███▍      | 20/58 [00:05<00:05,  6.50it/s]Capturing num tokens (num_tokens=960 avail_mem=100.14 GB):  34%|███▍      | 20/58 [00:05<00:05,  6.50it/s] Capturing num tokens (num_tokens=960 avail_mem=100.14 GB):  38%|███▊      | 22/58 [00:05<00:04,  8.13it/s]Capturing num tokens (num_tokens=896 avail_mem=100.14 GB):  38%|███▊      | 22/58 [00:05<00:04,  8.13it/s]

    Capturing num tokens (num_tokens=832 avail_mem=100.14 GB):  38%|███▊      | 22/58 [00:05<00:04,  8.13it/s]Capturing num tokens (num_tokens=832 avail_mem=100.14 GB):  41%|████▏     | 24/58 [00:05<00:03,  9.64it/s]Capturing num tokens (num_tokens=768 avail_mem=100.13 GB):  41%|████▏     | 24/58 [00:05<00:03,  9.64it/s]Capturing num tokens (num_tokens=704 avail_mem=100.13 GB):  41%|████▏     | 24/58 [00:05<00:03,  9.64it/s]Capturing num tokens (num_tokens=704 avail_mem=100.13 GB):  45%|████▍     | 26/58 [00:05<00:02, 11.70it/s]Capturing num tokens (num_tokens=640 avail_mem=100.12 GB):  45%|████▍     | 26/58 [00:05<00:02, 11.70it/s]

    Capturing num tokens (num_tokens=576 avail_mem=100.12 GB):  45%|████▍     | 26/58 [00:05<00:02, 11.70it/s]Capturing num tokens (num_tokens=512 avail_mem=100.11 GB):  45%|████▍     | 26/58 [00:06<00:02, 11.70it/s]Capturing num tokens (num_tokens=512 avail_mem=100.11 GB):  50%|█████     | 29/58 [00:06<00:01, 14.61it/s]Capturing num tokens (num_tokens=480 avail_mem=100.11 GB):  50%|█████     | 29/58 [00:06<00:01, 14.61it/s]Capturing num tokens (num_tokens=448 avail_mem=100.11 GB):  50%|█████     | 29/58 [00:06<00:01, 14.61it/s]Capturing num tokens (num_tokens=416 avail_mem=100.10 GB):  50%|█████     | 29/58 [00:06<00:01, 14.61it/s]

    Capturing num tokens (num_tokens=416 avail_mem=100.10 GB):  55%|█████▌    | 32/58 [00:06<00:01, 17.25it/s]Capturing num tokens (num_tokens=384 avail_mem=100.10 GB):  55%|█████▌    | 32/58 [00:06<00:01, 17.25it/s]Capturing num tokens (num_tokens=352 avail_mem=100.09 GB):  55%|█████▌    | 32/58 [00:06<00:01, 17.25it/s]Capturing num tokens (num_tokens=320 avail_mem=100.09 GB):  55%|█████▌    | 32/58 [00:06<00:01, 17.25it/s]Capturing num tokens (num_tokens=320 avail_mem=100.09 GB):  60%|██████    | 35/58 [00:06<00:01, 19.56it/s]Capturing num tokens (num_tokens=288 avail_mem=100.09 GB):  60%|██████    | 35/58 [00:06<00:01, 19.56it/s]Capturing num tokens (num_tokens=256 avail_mem=100.08 GB):  60%|██████    | 35/58 [00:06<00:01, 19.56it/s]Capturing num tokens (num_tokens=240 avail_mem=100.08 GB):  60%|██████    | 35/58 [00:06<00:01, 19.56it/s]Capturing num tokens (num_tokens=224 avail_mem=100.07 GB):  60%|██████    | 35/58 [00:06<00:01, 19.56it/s]

    Capturing num tokens (num_tokens=224 avail_mem=100.07 GB):  67%|██████▋   | 39/58 [00:06<00:00, 24.19it/s]Capturing num tokens (num_tokens=208 avail_mem=100.07 GB):  67%|██████▋   | 39/58 [00:06<00:00, 24.19it/s]Capturing num tokens (num_tokens=192 avail_mem=100.07 GB):  67%|██████▋   | 39/58 [00:06<00:00, 24.19it/s]Capturing num tokens (num_tokens=176 avail_mem=100.06 GB):  67%|██████▋   | 39/58 [00:06<00:00, 24.19it/s]Capturing num tokens (num_tokens=160 avail_mem=100.06 GB):  67%|██████▋   | 39/58 [00:06<00:00, 24.19it/s]Capturing num tokens (num_tokens=160 avail_mem=100.06 GB):  74%|███████▍  | 43/58 [00:06<00:00, 27.83it/s]Capturing num tokens (num_tokens=144 avail_mem=100.05 GB):  74%|███████▍  | 43/58 [00:06<00:00, 27.83it/s]Capturing num tokens (num_tokens=128 avail_mem=100.06 GB):  74%|███████▍  | 43/58 [00:06<00:00, 27.83it/s]Capturing num tokens (num_tokens=112 avail_mem=100.06 GB):  74%|███████▍  | 43/58 [00:06<00:00, 27.83it/s]Capturing num tokens (num_tokens=96 avail_mem=100.06 GB):  74%|███████▍  | 43/58 [00:06<00:00, 27.83it/s] 

    Capturing num tokens (num_tokens=96 avail_mem=100.06 GB):  81%|████████  | 47/58 [00:06<00:00, 29.79it/s]Capturing num tokens (num_tokens=80 avail_mem=100.05 GB):  81%|████████  | 47/58 [00:06<00:00, 29.79it/s]Capturing num tokens (num_tokens=64 avail_mem=100.05 GB):  81%|████████  | 47/58 [00:06<00:00, 29.79it/s]Capturing num tokens (num_tokens=48 avail_mem=99.01 GB):  81%|████████  | 47/58 [00:06<00:00, 29.79it/s] Capturing num tokens (num_tokens=32 avail_mem=99.01 GB):  81%|████████  | 47/58 [00:06<00:00, 29.79it/s]

    Capturing num tokens (num_tokens=32 avail_mem=99.01 GB):  88%|████████▊ | 51/58 [00:06<00:00, 21.57it/s]Capturing num tokens (num_tokens=28 avail_mem=99.01 GB):  88%|████████▊ | 51/58 [00:06<00:00, 21.57it/s]Capturing num tokens (num_tokens=24 avail_mem=100.01 GB):  88%|████████▊ | 51/58 [00:07<00:00, 21.57it/s]Capturing num tokens (num_tokens=20 avail_mem=100.00 GB):  88%|████████▊ | 51/58 [00:07<00:00, 21.57it/s]

    Capturing num tokens (num_tokens=20 avail_mem=100.00 GB):  93%|█████████▎| 54/58 [00:07<00:00, 18.99it/s]Capturing num tokens (num_tokens=16 avail_mem=99.18 GB):  93%|█████████▎| 54/58 [00:07<00:00, 18.99it/s] Capturing num tokens (num_tokens=12 avail_mem=99.17 GB):  93%|█████████▎| 54/58 [00:07<00:00, 18.99it/s]

    Capturing num tokens (num_tokens=8 avail_mem=99.17 GB):  93%|█████████▎| 54/58 [00:07<00:00, 18.99it/s] Capturing num tokens (num_tokens=8 avail_mem=99.17 GB):  98%|█████████▊| 57/58 [00:07<00:00, 15.93it/s]Capturing num tokens (num_tokens=4 avail_mem=99.99 GB):  98%|█████████▊| 57/58 [00:07<00:00, 15.93it/s]Capturing num tokens (num_tokens=4 avail_mem=99.99 GB): 100%|██████████| 58/58 [00:07<00:00,  7.74it/s]


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not exactly sure about the current population numbers. I remember that Paris is a very big city, but I think it's not the largest in the world. Maybe around 20 million? I'm not certain, though. I should probably check that.<br><br>Wait, I think I heard somewhere that Paris has a population over 21 million. Maybe 21.6 million? I'm not sure if that's accurate. I should look up the latest data to confirm. Let me think, where can I find reliable information? Maybe the official government website or a reputable news source. <br><br>I recall that France's population is around 40 million, so Paris being a major city would have a significant portion of that. If the total population is about 40 million, and Paris is the largest city, it's plausible that it's around 21.6 million. I think I've seen that number before, but I'm not 100% sure. <br><br>Also, I should consider if the population figure includes just the city proper or the metropolitan area. Sometimes, population counts can include surrounding suburbs and satellite towns. But I think in this case, the user is asking for the population of the capital, which is Paris, so it's probably just the city limits. <br><br>I should also think about how populations can change over time. Demographics can fluctuate due to births, deaths, and migration. So the number might not be exact and could vary slightly from year to year. <br><br>To sum up, I'm pretty confident that Paris is the capital of France and that its population is approximately 21.6 million. But to be thorough, I should verify this information to ensure accuracy.<br><br><br>content: {<br><br>"name": "Paris",<br>"population": 21620000<br>}</strong>


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not exactly sure about the current population numbers. I remember that Paris is a very big city, but I think it's not the largest in the world. Maybe around 20 million? I'm not certain, though. I should probably check that.<br><br>Wait, I think I heard somewhere that Paris has a population over 21 million. Maybe 21.6 million? I'm not sure if that's accurate. I should look up the latest data to confirm. Let me think, where can I find reliable information? Maybe the official government website or a reputable news source. <br><br>I recall that France's population is around 40 million, so Paris being a major city would have a significant portion of that. If the total population is about 40 million, and Paris is the largest city, it's plausible that it's around 21.6 million. I think I've seen that number before, but I'm not 100% sure. <br><br>Also, I should consider if the population figure includes just the city proper or the metropolitan area. Sometimes, population counts can include surrounding suburbs and satellite towns. But I think in this case, the user is asking for the population of the capital, which is Paris, so it's probably just the city limits. <br><br>I should also think about how populations can change over time. Demographics can fluctuate due to births, deaths, and migration. So the number might not be exact and could vary slightly from year to year. <br><br>To sum up, I'm pretty confident that Paris is the capital of France and that its population is approximately 21.6 million. But to be thorough, I should verify this information to ensure accuracy.<br><br><br>content: {<br><br>"name": "Paris",<br>"population": 21620000<br>}</strong>


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not exactly sure about the current population. I think it's a big city, maybe around 3 million? But I'm not certain. I should probably check some reliable sources to confirm this. Maybe I can look up recent population data or news articles that mention Paris's population. I remember hearing that Paris is one of the most populous cities in the world, but I'm not sure if it's over 3 million or not. I should also consider factors like urbanization and migration that might affect the population numbers. Maybe the population has grown a bit since the last census. I think the most recent data might be from 2020 or 2021. I should make sure the number I provide is accurate and up-to-date. Also, I should present this information in a clear and concise way, maybe in JSON format as the user requested. I should double-check the population figure to ensure it's correct before finalizing the answer.<br><br><br>content: Rome is the capital of France</strong>


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France. Hmm, I remember learning a bit about France in school, but I'm not 100% sure. Let me think. I know that Paris is a major city in France, and it's often referred to as the "City of Light" because of the famous Eiffel Tower. But is it the capital? I think so, but I'm not entirely certain. <br><br>Wait, I also recall that there's another city called Lyon. Isn't that the capital? No, I think I'm mixing things up. Lyon is definitely a significant city in France, known for its gastronomy and being the second-largest city. But I'm pretty sure Paris is the capital. <br><br>Let me try to remember any other capitals I know. For example, Germany's capital is Berlin, Italy's is Rome, Spain's is Madrid, and so on. So, following that pattern, France's capital should be Paris. I think I heard it a lot in history classes, especially when talking about the French Revolution and Napoleon. Those events happened in Paris, which probably helped it become the capital.<br><br>I also remember that the Eiffel Tower is in Paris, and it's a symbol of the country. The tower was built in the 19th century, and it's a tourist attraction. So, if Paris has such a famous landmark, it's likely the capital. <br><br>Another way to think about it is the political aspect. The President of France is based in Paris, right? So that makes sense. The government quarters, like the Palace of Versailles, are in Paris. That would mean Paris is where the country's government is located, making it the capital.<br><br>I guess I'm pretty confident now. I don't think I've heard of any other city being the capital of France. Lyon is more of a regional capital or something. Maybe it's the regional capital for certain areas, but not the national one. <br><br>So, putting it all together, Paris is the capital of France because it's the most significant political, cultural, and symbolic center of the country. It's where major landmarks like the Eiffel Tower and government buildings are located, and it's the birthplace of many important historical events and figures.<br><br><br>content: Paris</strong>


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


<strong style='color: #00008B;'>reasoing_content: Okay, so the user is in New York and wants the current date and time along with the weather. Let me break this down step by step.<br><br>First, I need to figure out which functions to use. The user mentioned two specific functions: 'get_current_date' and 'get_current_weather'. I should check the parameters required for each.<br><br>For 'get_current_date', the parameter is 'timezone', which needs to be a string like 'America/New_York'. That's straightforward because I know the user is in New York. So I can call this function with that timezone parameter.<br><br>Next, for the weather, I need to use 'get_current_weather'. This function requires 'city', 'state', and 'unit' parameters. The user is in New York, so the city is 'New York'. The state abbreviation for New York is 'NY'. The unit should be in Fahrenheit since the user might be more familiar with that, but it's not specified, so I can default to Fahrenheit.<br><br>I should structure the response by first calling 'get_current_date' with the timezone parameter and then 'get_current_weather' with the city, state, and unit. I need to make sure each function call is properly formatted with the correct syntax, using the required parameters and wrapping them in JSON.<br><br>Also, I should include the sources in the response. For the date, it's a built-in function, so I don't need a source. For the weather, since it's real-time data, I should mention that it's obtained through the function and might vary, advising the user to check again for the most accurate info.<br><br>Putting it all together, I'll format the response with each function call on separate lines, each properly tagged with the function name and parameters. I'll also add a brief explanation to the weather information to make it clear.<br><br><br>content: <function=get_current_date>{"timezone": "America/New_York"}</function>  <br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "fahrenheit"}</function></strong>


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

    {'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not sure of the exact number. I think it\'s around 2 million, but I should verify that.\n\nWait, maybe I should check the latest statistics. I recall that in recent years, Paris has seen a slight increase. Let me think... I believe the population is approximately 2,150,000 as of 2023. That seems about right, but I\'m not 100% certain. I should make sure to present this information accurately.\n\nNext, I need to structure this into a JSON format. JSON requires key-value pairs, so I\'ll create an object with keys like "city", "population", and maybe "country" for context. The city is Paris, the population is 2,150,000, and the country is France.\n\nI should also consider if the user might need more details, like the exact year of the population figure. Including that could be helpful, so I\'ll add "year": 2023. That way, the user knows the data is up to date.\n\nPutting it all together, the JSON should look clean and well-structured. I\'ll make sure the syntax is correct, with proper commas and quotation marks. No markdown, just plain JSON.\n\nI think that\'s all. The user probably just needs the information quickly, so keeping it concise is key. I\'ll present the JSON without any extra fluff.\n</think>{\n\n"name": "Paris",\n"population": 21500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 14190, 11, 7196, 358, 1265, 1779, 279, 5535, 13142, 13, 358, 19091, 429, 304, 3213, 1635, 11, 12095, 702, 3884, 264, 8112, 5263, 13, 6771, 752, 1744, 1112, 358, 4411, 279, 7042, 374, 13187, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 2938, 4977, 911, 1290, 11, 714, 358, 2776, 537, 220, 16, 15, 15, 4, 3654, 13, 358, 1265, 1281, 2704, 311, 3042, 419, 1995, 29257, 382, 5847, 11, 358, 1184, 311, 5944, 419, 1119, 264, 4718, 3561, 13, 4718, 7460, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 7196, 330, 11141, 1, 369, 2266, 13, 576, 3283, 374, 12095, 11, 279, 7042, 374, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 11, 323, 279, 3146, 374, 9625, 382, 40, 1265, 1083, 2908, 421, 279, 1196, 2578, 1184, 803, 3565, 11, 1075, 279, 4734, 1042, 315, 279, 7042, 7071, 13, 55121, 429, 1410, 387, 10950, 11, 773, 358, 3278, 912, 330, 3157, 788, 220, 17, 15, 17, 18, 13, 2938, 1616, 11, 279, 1196, 8788, 279, 821, 374, 705, 311, 2400, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1265, 1401, 4240, 323, 1632, 12, 51143, 13, 358, 3278, 1281, 2704, 279, 19482, 374, 4396, 11, 448, 6169, 76602, 323, 54231, 15423, 13, 2308, 50494, 11, 1101, 14396, 4718, 382, 40, 1744, 429, 594, 678, 13, 576, 1196, 4658, 1101, 3880, 279, 1995, 6157, 11, 773, 10282, 432, 63594, 374, 1376, 13, 358, 3278, 3042, 279, 4718, 2041, 894, 4960, 1320, 1362, 624, 151649, 4257, 1, 606, 788, 330, 59604, 756, 1, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15], 'meta_info': {'id': '3b72315a310849d79580586fb150d83c', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'reasoning_tokens': 367, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': {'device': 1, 'host': 0}, 'dp_rank': None, 'e2e_latency': 22.297018527053297, 'response_sent_to_client_ts': 1775407636.4771821}}



<strong style='color: #00008B;'>reasoing_content: Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.<br><br>First, I need to identify the capital of France. I know that Paris is the capital, so that's straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I'm not sure of the exact number. I think it's around 2 million, but I should verify that.<br><br>Wait, maybe I should check the latest statistics. I recall that in recent years, Paris has seen a slight increase. Let me think... I believe the population is approximately 2,150,000 as of 2023. That seems about right, but I'm not 100% certain. I should make sure to present this information accurately.<br><br>Next, I need to structure this into a JSON format. JSON requires key-value pairs, so I'll create an object with keys like "city", "population", and maybe "country" for context. The city is Paris, the population is 2,150,000, and the country is France.<br><br>I should also consider if the user might need more details, like the exact year of the population figure. Including that could be helpful, so I'll add "year": 2023. That way, the user knows the data is up to date.<br><br>Putting it all together, the JSON should look clean and well-structured. I'll make sure the syntax is correct, with proper commas and quotation marks. No markdown, just plain JSON.<br><br>I think that's all. The user probably just needs the information quickly, so keeping it concise is key. I'll present the JSON without any extra fluff.<br><br><br>content: {<br><br>"name": "Paris",<br>"population": 21500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000</strong>


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


<strong style='color: #00008B;'>{'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not exactly sure of the current number. I think it\'s around 2 million, but I should verify that.\n\nWait, maybe I should check some reliable sources to confirm the population. I recall that the population figures can vary depending on the source and the year. For example, the 2020 census might have a slightly different number than the 2021 estimate. I think the population was around 2,165,000 in 2021, but I\'m not 100% certain. I should make sure to use the most accurate and up-to-date information.\n\nAlso, the user wants the information in JSON format. JSON is a data interchange format, so I\'ll need to structure the data accordingly. I should include the city name, population, and maybe the year of the data. It\'s important to present the information clearly and accurately, so I\'ll double-check the numbers to avoid any mistakes.\n\nI should also consider if there are any other relevant details the user might find useful, like the area of the city or some key facts about it. But since the user specifically asked for population, I\'ll focus on that. Maybe adding a note about the population figure being approximate would be helpful, just in case.\n\nPutting it all together, I\'ll structure the JSON with the city name, population, and the year. I\'ll make sure the syntax is correct, using quotation marks and commas appropriately. I\'ll also keep the language clear and straightforward so that the user can easily understand the information.\n\nFinally, I\'ll review the JSON to ensure there are no errors and that the data is accurate. This way, the user gets a reliable and well-formatted response to their query.\n</think>{"name": "Paris", "population": 2165000}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 6896, 2704, 315, 279, 1482, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 14190, 11, 7196, 358, 1265, 1779, 1045, 14720, 8173, 311, 7683, 279, 7042, 13, 358, 19091, 429, 279, 7042, 12396, 646, 13289, 11649, 389, 279, 2530, 323, 279, 1042, 13, 1752, 3110, 11, 279, 220, 17, 15, 17, 15, 43602, 2578, 614, 264, 10078, 2155, 1372, 1091, 279, 220, 17, 15, 17, 16, 16045, 13, 358, 1744, 279, 7042, 572, 2163, 220, 17, 11, 16, 21, 20, 11, 15, 15, 15, 304, 220, 17, 15, 17, 16, 11, 714, 358, 2776, 537, 220, 16, 15, 15, 4, 3654, 13, 358, 1265, 1281, 2704, 311, 990, 279, 1429, 13382, 323, 705, 4686, 18413, 1995, 382, 13394, 11, 279, 1196, 6801, 279, 1995, 304, 4718, 3561, 13, 4718, 374, 264, 821, 51263, 3561, 11, 773, 358, 3278, 1184, 311, 5944, 279, 821, 27079, 13, 358, 1265, 2924, 279, 3283, 829, 11, 7042, 11, 323, 7196, 279, 1042, 315, 279, 821, 13, 1084, 594, 2989, 311, 3042, 279, 1995, 9355, 323, 29257, 11, 773, 358, 3278, 1990, 15934, 279, 5109, 311, 5648, 894, 20643, 382, 40, 1265, 1083, 2908, 421, 1052, 525, 894, 1008, 9760, 3565, 279, 1196, 2578, 1477, 5390, 11, 1075, 279, 3082, 315, 279, 3283, 476, 1045, 1376, 13064, 911, 432, 13, 1988, 2474, 279, 1196, 11689, 4588, 369, 7042, 11, 358, 3278, 5244, 389, 429, 13, 10696, 7842, 264, 5185, 911, 279, 7042, 7071, 1660, 44868, 1035, 387, 10950, 11, 1101, 304, 1142, 382, 97904, 432, 678, 3786, 11, 358, 3278, 5944, 279, 4718, 448, 279, 3283, 829, 11, 7042, 11, 323, 279, 1042, 13, 358, 3278, 1281, 2704, 279, 19482, 374, 4396, 11, 1667, 54231, 15423, 323, 76602, 34901, 13, 358, 3278, 1083, 2506, 279, 4128, 2797, 323, 30339, 773, 429, 279, 1196, 646, 6707, 3535, 279, 1995, 382, 23949, 11, 358, 3278, 3395, 279, 4718, 311, 5978, 1052, 525, 902, 5975, 323, 429, 279, 821, 374, 13382, 13, 1096, 1616, 11, 279, 1196, 5221, 264, 14720, 323, 1632, 8460, 12127, 2033, 311, 862, 3239, 624, 151649, 4913, 606, 788, 330, 59604, 497, 330, 44441, 788, 220, 17, 16, 21, 20, 15, 15, 15, 92, 151643], 'meta_info': {'id': '58aee0df7ad24c9cb327707236d7e95f', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'reasoning_tokens': 428, 'completion_tokens': 447, 'cached_tokens': 22, 'cached_tokens_details': {'device': 22, 'host': 0}, 'dp_rank': None, 'e2e_latency': 4.656709308736026, 'response_sent_to_client_ts': 1775407641.1442637}}</strong>


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

    [{'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '513a5921015845a89d28a46388aeab9c', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'reasoning_tokens': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': {'device': 10, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.13280335208401084, 'response_sent_to_client_ts': 1775407641.3038578}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '88414adc667c4146a7cc5a3b76f0313a', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'reasoning_tokens': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': {'device': 10, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.1327372039668262, 'response_sent_to_client_ts': 1775407641.30387}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '97cd001b180e47cda2fd07d0c23ec7af', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'reasoning_tokens': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': {'device': 10, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.13269122503697872, 'response_sent_to_client_ts': 1775407641.3038745}}]


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

    {'text': ' France, and the \n\\( n \\)  \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\(', 'output_ids': [9625, 11, 323, 279, 220, 198, 44292, 308, 1124, 8, 220, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767], 'meta_info': {'id': '8b32fd02f7b44083a90ee9195d9252dc', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 6, 'weight_version': 'default', 'total_retractions': 0, 'reasoning_tokens': 2048, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': {'device': 1, 'host': 0}, 'dp_rank': None, 'e2e_latency': 18.7540494450368, 'response_sent_to_client_ts': 1775407660.065277}}


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


<strong style='color: #00008B;'>{'text': 'Okay, so the user asked me to give them the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to figure out where the capital of France is located. I know that Paris is the capital, so that\'s the city. Next, I should gather some key facts about Paris. The main landmarks would be the Eiffel Tower, the Louvre Museum, and Notre-Dame Cathedral. Including some important people associated with Paris, like perror, who was connected to the French Revolution, makes sense for context.\n\nNow, for the population part. I remember that recent estimates have the population around 2 million. But I should check to make sure that\'s accurate. Maybe the current population is a bit higher, like 2.1 or 2.2 million. Also, including the percentage growth rate, I think it\'s around 0.6% annually, which seems familiar from some demographic data I’ve come across.\n\nI should structure this information neatly in JSON. That means creating an object with appropriate keys. Using "city" for Paris, "landmarks" as an array, "historical figure" to mention perror and other notable figures, and "population" with sub-keys for total, growth rate, and maybe some more relevant details.\n\nWait, I should also consider adding the full name for perror, "Pierre Lutetie II de Sacy," to provide more context. That way, it\'s informative and accurate.\n\nPutting it all together, the JSON should be clear and well-organized. I need to ensure that the population numbers are up to date and that the information is presented in a way that\'s easy to read, with proper commas and brackets.\n\nLet me also think about why the user is asking for this. They might be preparing data for a project, a school assignment, or maybe just curious. Either way, providing accurate and concise information in a structured format like JSON will likely meet their needs.\n\nI should double-check the population numbers to confirm. Maybe consult a recent source or a reliable database to ensure the figure is accurate. Let\'s say the latest estimate is 2,186,000 as of 2023. That\'s a good, recent number to include.\n\nIncluding growth rate helps understand how the population is changing, which is a useful metric. A growth rate of 0.6% is positive but relatively low, which is typical for large cities.\n\nI think that\'s all the necessary details. Organizing them under the relevant labels in JSON will make it easy for the user to extract any information they need without hassle.\n</think>\n\n```json\n{\n  "capital": {\n    "name": "Paris",\n    "location": " Île-de-France, France",\n    "key_landmarks": [\n      "Eiffel Tower",\n      "Louvre Museum",\n      "Notre-Dame Cathedral"\n    ],\n    "historical_significance": "Home to the French Revolution and many notable people, including Pariserror and others.",\n    "population": {\n      "total": 2186000,\n      "growth_rate": 0.6\n    },\n    "major_culturaland HERITAGE": "Paris is a world-renowned center of culture, art, and history, hosting many museums and landmarks."\n  }\n}\n```', 'output_ids': [32313, 11, 773, 279, 1196, 4588, 752, 311, 2968, 1105, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 7071, 700, 1380, 279, 6722, 315, 9625, 374, 7407, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 279, 3283, 13, 9295, 11, 358, 1265, 9567, 1045, 1376, 13064, 911, 12095, 13, 576, 1887, 59924, 1035, 387, 279, 468, 3092, 301, 21938, 11, 279, 9729, 48506, 16328, 11, 323, 43464, 9420, 373, 56729, 13, 55121, 1045, 2989, 1251, 5815, 448, 12095, 11, 1075, 40731, 11, 879, 572, 8433, 311, 279, 8585, 22126, 11, 3643, 5530, 369, 2266, 382, 7039, 11, 369, 279, 7042, 949, 13, 358, 6099, 429, 3213, 17530, 614, 279, 7042, 2163, 220, 17, 3526, 13, 1988, 358, 1265, 1779, 311, 1281, 2704, 429, 594, 13382, 13, 10696, 279, 1482, 7042, 374, 264, 2699, 5080, 11, 1075, 220, 17, 13, 16, 476, 220, 17, 13, 17, 3526, 13, 7281, 11, 2670, 279, 11414, 6513, 4379, 11, 358, 1744, 432, 594, 2163, 220, 15, 13, 21, 4, 29071, 11, 892, 4977, 11285, 504, 1045, 37362, 821, 358, 3982, 2525, 3941, 382, 40, 1265, 5944, 419, 1995, 62166, 304, 4718, 13, 2938, 3363, 6825, 458, 1633, 448, 8311, 6894, 13, 12091, 330, 8926, 1, 369, 12095, 11, 330, 1933, 15544, 1, 438, 458, 1334, 11, 330, 43441, 938, 7071, 1, 311, 6286, 40731, 323, 1008, 27190, 12396, 11, 323, 330, 44441, 1, 448, 1186, 76109, 369, 2790, 11, 6513, 4379, 11, 323, 7196, 1045, 803, 9760, 3565, 382, 14190, 11, 358, 1265, 1083, 2908, 7842, 279, 2480, 829, 369, 40731, 11, 330, 47, 44423, 444, 332, 295, 645, 7946, 409, 328, 2757, 1335, 311, 3410, 803, 2266, 13, 2938, 1616, 11, 432, 594, 38219, 323, 13382, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1265, 387, 2797, 323, 1632, 12, 62216, 13, 358, 1184, 311, 5978, 429, 279, 7042, 5109, 525, 705, 311, 2400, 323, 429, 279, 1995, 374, 10449, 304, 264, 1616, 429, 594, 4135, 311, 1349, 11, 448, 6169, 76602, 323, 38929, 382, 10061, 752, 1083, 1744, 911, 3170, 279, 1196, 374, 10161, 369, 419, 13, 2379, 2578, 387, 20045, 821, 369, 264, 2390, 11, 264, 2906, 16319, 11, 476, 7196, 1101, 22208, 13, 20988, 1616, 11, 8241, 13382, 323, 63594, 1995, 304, 264, 32930, 3561, 1075, 4718, 686, 4363, 3367, 862, 3880, 382, 40, 1265, 1990, 15934, 279, 7042, 5109, 311, 7683, 13, 10696, 8498, 264, 3213, 2530, 476, 264, 14720, 4625, 311, 5978, 279, 7071, 374, 13382, 13, 6771, 594, 1977, 279, 5535, 16045, 374, 220, 17, 11, 16, 23, 21, 11, 15, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 2938, 594, 264, 1661, 11, 3213, 1372, 311, 2924, 382, 83449, 6513, 4379, 8609, 3535, 1246, 279, 7042, 374, 10018, 11, 892, 374, 264, 5390, 18266, 13, 362, 6513, 4379, 315, 220, 15, 13, 21, 4, 374, 6785, 714, 12040, 3347, 11, 892, 374, 14260, 369, 3460, 9720, 382, 40, 1744, 429, 594, 678, 279, 5871, 3565, 13, 10762, 4849, 1105, 1212, 279, 9760, 9201, 304, 4718, 686, 1281, 432, 4135, 369, 279, 1196, 311, 8649, 894, 1995, 807, 1184, 2041, 46847, 624, 151649, 271, 73594, 2236, 198, 515, 220, 330, 65063, 788, 341, 262, 330, 606, 788, 330, 59604, 756, 262, 330, 2527, 788, 330, 59108, 273, 6810, 7276, 34106, 11, 9625, 756, 262, 330, 792, 60506, 15544, 788, 2278, 414, 330, 36, 3092, 301, 21938, 756, 414, 330, 92806, 48506, 16328, 756, 414, 330, 2623, 265, 9420, 373, 56729, 698, 262, 3211, 262, 330, 43441, 938, 11172, 99004, 788, 330, 7623, 311, 279, 8585, 22126, 323, 1657, 27190, 1251, 11, 2670, 12095, 841, 323, 3800, 10346, 262, 330, 44441, 788, 341, 414, 330, 5035, 788, 220, 17, 16, 23, 21, 15, 15, 15, 345, 414, 330, 73089, 9246, 788, 220, 15, 13, 21, 198, 262, 1153, 262, 330, 36505, 666, 43447, 437, 15645, 952, 3585, 788, 330, 59604, 374, 264, 1879, 93009, 18332, 4126, 315, 7674, 11, 1947, 11, 323, 3840, 11, 19678, 1657, 50577, 323, 59924, 10040, 220, 456, 532, 73594, 151643], 'meta_info': {'id': 'e07e19aed5a44a708bf98b30aabfcdd1', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'reasoning_tokens': 538, 'completion_tokens': 689, 'cached_tokens': 22, 'cached_tokens_details': {'device': 22, 'host': 0}, 'dp_rank': None, 'e2e_latency': 6.367916951421648, 'response_sent_to_client_ts': 1775407666.4427037}}</strong>



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


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:01<00:01,  1.09s/it]

    Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.17s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.16s/it]


    2026-04-05 16:48:03,810 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-05 16:48:03] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:55,  3.07s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:55,  3.07s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:37,  1.73s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:37,  1.73s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:01,  1.12s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:01,  1.12s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:43,  1.24it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:43,  1.24it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:32,  1.61it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:32,  1.61it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:26,  1.96it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:26,  1.96it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:21,  2.36it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:21,  2.36it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:17,  2.80it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:17,  2.80it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:14,  3.34it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:14,  3.34it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:12,  3.85it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:12,  3.85it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:10,  4.32it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:10,  4.32it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:06<00:09,  4.94it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:06<00:09,  4.94it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:06<00:07,  5.64it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:06<00:07,  5.64it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:06<00:07,  6.09it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:06<00:07,  6.09it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:06<00:07,  6.09it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:06<00:05,  7.59it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:06<00:05,  7.59it/s]

    Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:06<00:05,  7.59it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:06<00:04,  9.11it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:06<00:04,  9.11it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:06<00:04,  9.11it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:07<00:03, 10.72it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:07<00:03, 10.72it/s]

    Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:07<00:03, 10.72it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:07<00:03, 10.72it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:07<00:02, 13.93it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:07<00:02, 13.93it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:07<00:02, 13.93it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:07<00:02, 13.93it/s]

    Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:07<00:01, 16.47it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:07<00:01, 16.47it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:07<00:01, 16.47it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:07<00:01, 16.47it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:07<00:01, 16.47it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:07<00:01, 21.07it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:07<00:01, 21.07it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:07<00:01, 21.07it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:07<00:01, 21.07it/s]

    Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:07<00:01, 23.05it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:07<00:01, 23.05it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:07<00:01, 23.05it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:07<00:01, 23.05it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:07<00:01, 23.05it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:07<00:00, 26.64it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:07<00:00, 26.64it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:07<00:00, 26.64it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:07<00:00, 26.64it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:07<00:00, 26.64it/s]

    Compiling num tokens (num_tokens=176):  64%|██████▍   | 37/58 [00:07<00:00, 26.64it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:07<00:00, 31.55it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:07<00:00, 31.55it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:07<00:00, 31.55it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:07<00:00, 31.55it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:07<00:00, 31.55it/s]Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:07<00:00, 31.55it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:07<00:00, 34.41it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:07<00:00, 34.41it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:07<00:00, 34.41it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:07<00:00, 34.41it/s]

    Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:07<00:00, 34.41it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:07<00:00, 34.41it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:07<00:00, 38.30it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:07<00:00, 38.30it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:07<00:00, 38.30it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:07<00:00, 38.30it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:07<00:00, 38.30it/s]Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:07<00:00, 38.30it/s] Compiling num tokens (num_tokens=4):  90%|████████▉ | 52/58 [00:08<00:00, 38.30it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00,  7.23it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=102.58 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=102.58 GB):   2%|▏         | 1/58 [00:00<00:16,  3.41it/s]Capturing num tokens (num_tokens=7680 avail_mem=102.54 GB):   2%|▏         | 1/58 [00:00<00:16,  3.41it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=102.54 GB):   3%|▎         | 2/58 [00:00<00:15,  3.57it/s]Capturing num tokens (num_tokens=7168 avail_mem=102.54 GB):   3%|▎         | 2/58 [00:00<00:15,  3.57it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=102.54 GB):   5%|▌         | 3/58 [00:00<00:14,  3.80it/s]Capturing num tokens (num_tokens=6656 avail_mem=102.54 GB):   5%|▌         | 3/58 [00:00<00:14,  3.80it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=102.54 GB):   7%|▋         | 4/58 [00:01<00:13,  4.09it/s]Capturing num tokens (num_tokens=6144 avail_mem=102.55 GB):   7%|▋         | 4/58 [00:01<00:13,  4.09it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=102.55 GB):   9%|▊         | 5/58 [00:01<00:12,  4.33it/s]Capturing num tokens (num_tokens=5632 avail_mem=102.55 GB):   9%|▊         | 5/58 [00:01<00:12,  4.33it/s]Capturing num tokens (num_tokens=5632 avail_mem=102.55 GB):  10%|█         | 6/58 [00:01<00:11,  4.71it/s]Capturing num tokens (num_tokens=5120 avail_mem=102.55 GB):  10%|█         | 6/58 [00:01<00:11,  4.71it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=102.55 GB):  12%|█▏        | 7/58 [00:01<00:10,  5.08it/s]Capturing num tokens (num_tokens=4608 avail_mem=102.56 GB):  12%|█▏        | 7/58 [00:01<00:10,  5.08it/s]Capturing num tokens (num_tokens=4608 avail_mem=102.56 GB):  14%|█▍        | 8/58 [00:01<00:08,  5.57it/s]Capturing num tokens (num_tokens=4096 avail_mem=102.56 GB):  14%|█▍        | 8/58 [00:01<00:08,  5.57it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=102.56 GB):  16%|█▌        | 9/58 [00:01<00:08,  6.06it/s]Capturing num tokens (num_tokens=3840 avail_mem=102.57 GB):  16%|█▌        | 9/58 [00:01<00:08,  6.06it/s]Capturing num tokens (num_tokens=3840 avail_mem=102.57 GB):  17%|█▋        | 10/58 [00:01<00:07,  6.52it/s]Capturing num tokens (num_tokens=3584 avail_mem=102.56 GB):  17%|█▋        | 10/58 [00:01<00:07,  6.52it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=102.56 GB):  19%|█▉        | 11/58 [00:02<00:06,  7.04it/s]Capturing num tokens (num_tokens=3328 avail_mem=102.56 GB):  19%|█▉        | 11/58 [00:02<00:06,  7.04it/s]Capturing num tokens (num_tokens=3328 avail_mem=102.56 GB):  21%|██        | 12/58 [00:02<00:06,  7.64it/s]Capturing num tokens (num_tokens=3072 avail_mem=102.56 GB):  21%|██        | 12/58 [00:02<00:06,  7.64it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=102.56 GB):  22%|██▏       | 13/58 [00:02<00:05,  8.21it/s]Capturing num tokens (num_tokens=2816 avail_mem=102.56 GB):  22%|██▏       | 13/58 [00:02<00:05,  8.21it/s]Capturing num tokens (num_tokens=2560 avail_mem=102.56 GB):  22%|██▏       | 13/58 [00:02<00:05,  8.21it/s]Capturing num tokens (num_tokens=2560 avail_mem=102.56 GB):  26%|██▌       | 15/58 [00:02<00:04,  9.53it/s]Capturing num tokens (num_tokens=2304 avail_mem=102.56 GB):  26%|██▌       | 15/58 [00:02<00:04,  9.53it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=102.56 GB):  26%|██▌       | 15/58 [00:02<00:04,  9.53it/s]Capturing num tokens (num_tokens=2048 avail_mem=102.56 GB):  29%|██▉       | 17/58 [00:02<00:04,  9.93it/s]Capturing num tokens (num_tokens=1792 avail_mem=102.27 GB):  29%|██▉       | 17/58 [00:02<00:04,  9.93it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=102.27 GB):  31%|███       | 18/58 [00:02<00:04,  9.34it/s]Capturing num tokens (num_tokens=1536 avail_mem=102.26 GB):  31%|███       | 18/58 [00:02<00:04,  9.34it/s]Capturing num tokens (num_tokens=1536 avail_mem=102.26 GB):  33%|███▎      | 19/58 [00:02<00:04,  8.34it/s]Capturing num tokens (num_tokens=1280 avail_mem=102.23 GB):  33%|███▎      | 19/58 [00:02<00:04,  8.34it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=102.23 GB):  33%|███▎      | 19/58 [00:03<00:04,  8.34it/s]Capturing num tokens (num_tokens=1024 avail_mem=102.23 GB):  36%|███▌      | 21/58 [00:03<00:03,  9.88it/s]Capturing num tokens (num_tokens=960 avail_mem=102.22 GB):  36%|███▌      | 21/58 [00:03<00:03,  9.88it/s] Capturing num tokens (num_tokens=896 avail_mem=102.22 GB):  36%|███▌      | 21/58 [00:03<00:03,  9.88it/s]Capturing num tokens (num_tokens=896 avail_mem=102.22 GB):  40%|███▉      | 23/58 [00:03<00:02, 11.89it/s]Capturing num tokens (num_tokens=832 avail_mem=102.22 GB):  40%|███▉      | 23/58 [00:03<00:02, 11.89it/s]

    Capturing num tokens (num_tokens=768 avail_mem=102.21 GB):  40%|███▉      | 23/58 [00:03<00:02, 11.89it/s]Capturing num tokens (num_tokens=704 avail_mem=102.21 GB):  40%|███▉      | 23/58 [00:03<00:02, 11.89it/s]Capturing num tokens (num_tokens=704 avail_mem=102.21 GB):  45%|████▍     | 26/58 [00:03<00:02, 15.12it/s]Capturing num tokens (num_tokens=640 avail_mem=102.21 GB):  45%|████▍     | 26/58 [00:03<00:02, 15.12it/s]Capturing num tokens (num_tokens=576 avail_mem=102.20 GB):  45%|████▍     | 26/58 [00:03<00:02, 15.12it/s]Capturing num tokens (num_tokens=512 avail_mem=102.19 GB):  45%|████▍     | 26/58 [00:03<00:02, 15.12it/s]Capturing num tokens (num_tokens=512 avail_mem=102.19 GB):  50%|█████     | 29/58 [00:03<00:01, 18.01it/s]Capturing num tokens (num_tokens=480 avail_mem=102.19 GB):  50%|█████     | 29/58 [00:03<00:01, 18.01it/s]

    Capturing num tokens (num_tokens=448 avail_mem=102.19 GB):  50%|█████     | 29/58 [00:03<00:01, 18.01it/s]Capturing num tokens (num_tokens=416 avail_mem=102.19 GB):  50%|█████     | 29/58 [00:03<00:01, 18.01it/s]Capturing num tokens (num_tokens=416 avail_mem=102.19 GB):  55%|█████▌    | 32/58 [00:03<00:01, 20.39it/s]Capturing num tokens (num_tokens=384 avail_mem=102.18 GB):  55%|█████▌    | 32/58 [00:03<00:01, 20.39it/s]Capturing num tokens (num_tokens=352 avail_mem=102.18 GB):  55%|█████▌    | 32/58 [00:03<00:01, 20.39it/s]Capturing num tokens (num_tokens=320 avail_mem=102.17 GB):  55%|█████▌    | 32/58 [00:03<00:01, 20.39it/s]Capturing num tokens (num_tokens=320 avail_mem=102.17 GB):  60%|██████    | 35/58 [00:03<00:01, 22.74it/s]Capturing num tokens (num_tokens=288 avail_mem=102.17 GB):  60%|██████    | 35/58 [00:03<00:01, 22.74it/s]

    Capturing num tokens (num_tokens=256 avail_mem=102.16 GB):  60%|██████    | 35/58 [00:03<00:01, 22.74it/s]Capturing num tokens (num_tokens=240 avail_mem=102.16 GB):  60%|██████    | 35/58 [00:03<00:01, 22.74it/s]Capturing num tokens (num_tokens=224 avail_mem=102.16 GB):  60%|██████    | 35/58 [00:03<00:01, 22.74it/s]Capturing num tokens (num_tokens=224 avail_mem=102.16 GB):  67%|██████▋   | 39/58 [00:03<00:00, 26.11it/s]Capturing num tokens (num_tokens=208 avail_mem=102.15 GB):  67%|██████▋   | 39/58 [00:03<00:00, 26.11it/s]Capturing num tokens (num_tokens=192 avail_mem=102.15 GB):  67%|██████▋   | 39/58 [00:03<00:00, 26.11it/s]Capturing num tokens (num_tokens=176 avail_mem=102.14 GB):  67%|██████▋   | 39/58 [00:03<00:00, 26.11it/s]Capturing num tokens (num_tokens=160 avail_mem=102.14 GB):  67%|██████▋   | 39/58 [00:03<00:00, 26.11it/s]

    Capturing num tokens (num_tokens=160 avail_mem=102.14 GB):  74%|███████▍  | 43/58 [00:03<00:00, 27.77it/s]Capturing num tokens (num_tokens=144 avail_mem=102.14 GB):  74%|███████▍  | 43/58 [00:03<00:00, 27.77it/s]Capturing num tokens (num_tokens=128 avail_mem=102.15 GB):  74%|███████▍  | 43/58 [00:03<00:00, 27.77it/s]Capturing num tokens (num_tokens=112 avail_mem=102.14 GB):  74%|███████▍  | 43/58 [00:03<00:00, 27.77it/s]Capturing num tokens (num_tokens=96 avail_mem=102.14 GB):  74%|███████▍  | 43/58 [00:03<00:00, 27.77it/s] Capturing num tokens (num_tokens=96 avail_mem=102.14 GB):  81%|████████  | 47/58 [00:04<00:00, 29.82it/s]Capturing num tokens (num_tokens=80 avail_mem=102.13 GB):  81%|████████  | 47/58 [00:04<00:00, 29.82it/s]Capturing num tokens (num_tokens=64 avail_mem=102.13 GB):  81%|████████  | 47/58 [00:04<00:00, 29.82it/s]Capturing num tokens (num_tokens=48 avail_mem=102.13 GB):  81%|████████  | 47/58 [00:04<00:00, 29.82it/s]Capturing num tokens (num_tokens=32 avail_mem=102.12 GB):  81%|████████  | 47/58 [00:04<00:00, 29.82it/s]

    Capturing num tokens (num_tokens=32 avail_mem=102.12 GB):  88%|████████▊ | 51/58 [00:04<00:00, 32.06it/s]Capturing num tokens (num_tokens=28 avail_mem=102.12 GB):  88%|████████▊ | 51/58 [00:04<00:00, 32.06it/s]Capturing num tokens (num_tokens=24 avail_mem=102.12 GB):  88%|████████▊ | 51/58 [00:04<00:00, 32.06it/s]Capturing num tokens (num_tokens=20 avail_mem=102.11 GB):  88%|████████▊ | 51/58 [00:04<00:00, 32.06it/s]Capturing num tokens (num_tokens=16 avail_mem=102.11 GB):  88%|████████▊ | 51/58 [00:04<00:00, 32.06it/s]Capturing num tokens (num_tokens=16 avail_mem=102.11 GB):  95%|█████████▍| 55/58 [00:04<00:00, 33.29it/s]Capturing num tokens (num_tokens=12 avail_mem=102.11 GB):  95%|█████████▍| 55/58 [00:04<00:00, 33.29it/s]Capturing num tokens (num_tokens=8 avail_mem=102.10 GB):  95%|█████████▍| 55/58 [00:04<00:00, 33.29it/s] Capturing num tokens (num_tokens=4 avail_mem=102.07 GB):  95%|█████████▍| 55/58 [00:04<00:00, 33.29it/s]

    Capturing num tokens (num_tokens=4 avail_mem=102.07 GB): 100%|██████████| 58/58 [00:04<00:00, 13.27it/s]


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
        "population": 138000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
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
    Generated text: Paris is the capital of France
    ===============================
    Prompt: Give me the information of the capital of Germany.
    Generated text: Paris is the capital of France
    ===============================
    Prompt: Give me the information of the capital of Italy.
    Generated text: Berlin is the capital of Germany


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
    
    Generated text: Okay, so I need to figure out how to respond to the user's request about the capital of France. Let me start by understanding what they're asking for. They mentioned wanting the information and population of the capital in JSON format. I know that JSON stands for JavaScript Object Notation, which is a way to represent data structures in a readable format. 
    
    First, I need to determine what exactly the user is asking for. They specifically want the population of the capital, which I know is Paris. I should make sure I have the correct population number. From what I remember, Paris's population has been growing, but I'm not sure of the exact figure. I should look that up to provide an accurate number. I think it's around 2 million, but I might need to double-check that to be certain.
    
    Next, I need to structure this information into a JSON object. JSON typically uses key-value pairs, so I'll need a key for the capital and another for the population. The keys should be in French since the user is referencing the capital of France, so "VILLE_CAPITAL" and "POPULATION". The values will be the respective data: "Paris" and the population number.
    
    I should format the JSON correctly. Each key should be in quotes, and the values should be as well. The entire structure should be enclosed in curly braces. I need to ensure that the JSON syntax is correct to avoid any errors when it's used later. 
    
    Wait, should I include units for the population? The user just asked for the number, so I think just providing the number without units is fine. Also, I should make sure that the JSON is valid. Maybe I can use an online tool or a JSON validator to check it, but since I'm doing this manually, I'll have to be careful with the syntax.
    
    Another thing to consider is whether the population is the current one or an average. I believe the number I have is the approximate population, so I should note that it might change over time. However, for the purpose of this response, providing a single number should suffice unless the user specifies otherwise.
    
    Putting it all together, I'll create a JSON object with the key "VILLE_CAPITAL" and value "Paris", followed by the key "POPULATION" and the numerical value. I'll make sure the JSON is properly formatted with commas separating the key-value pairs and correct use of quotation marks.
    
    I should also respond to the user's query in a clear and concise manner, perhaps with a brief explanation that the population figure is approximate. That way, the user knows to check the latest data if they need more precise information.
    
    Wait, should I mention that Paris is the capital in the response? The user already knows that, but including it in the JSON structure might be helpful. Alternatively, I could write a sentence explaining it in plain text, but the user specifically asked for JSON. So I'll stick to the JSON format as per their request.
    
    Let me double-check the JSON syntax. I'll write it as:
    
    {
      "VILLE_CAPITAL": "Paris",
      "POPULATION": "20230000"
    }
    
    Wait, no, that's incorrect because "20230000" is a number without commas and might not be properly interpreted. In JSON, numbers should be written with commas as thousand separators if they're large. So "20230000" should be "20,230,000". But in JSON, it's actually not necessary to include commas for numbers; it's more about readability. Some systems might interpret it without commas, but it's generally better to include them for clarity.
    
    Alternatively, since the population is a number, I can represent it as an integer. So the correct JSON would be:
    
    {
      "VILLE_CAPITAL": "Paris",
      "POPULATION": 20230000
    }
    
    But wait, JSON allows both string and number representations. Since population is a number, using a number is appropriate. So I'll use 20230000 without quotes.
    
    Wait, actually, no. In JSON, if the value is a number, it should not be in quotes. So the correct structure is:
    
    {
      "VILLE_CAPITAL": "Paris",
      "POPULATION": 20230000
    }
    
    Yes, that's correct. The keys are in quotes, the values are not. So I'll format it that way.
    
    I think that's all. I should present this JSON to the user, ensuring it's correctly formatted and accurate. I should also, as a courtesy, perhaps mention that the population figure is approximate and might vary over time. But since the user asked for the information in JSON, I'll stick to that and keep the response straightforward.
    </think>
    
    Here is the JSON representation of the information and population of the capital of France:
    
    ```json
    {
      "VILLE_CAPITAL": "Paris",
      "POPULATION": 20230000
    }
    ```
    
    Note: The population figure is approximate and may change over time.



```python
llm.shutdown()
```
