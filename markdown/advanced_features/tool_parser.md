# Tool Parser

This guide demonstrates how to use SGLang’s [Function calling](https://platform.openai.com/docs/guides/function-calling) functionality.

## Currently supported parsers:

| Parser | Supported Models | Notes |
|---|---|---|
| `deepseekv3` | DeepSeek-v3 (e.g., `deepseek-ai/DeepSeek-V3-0324`) | Recommend adding `--chat-template ./examples/chat_template/tool_chat_template_deepseekv3.jinja` to launch command. |
| `deepseekv31` | DeepSeek-V3.1 and DeepSeek-V3.2-Exp (e.g. `deepseek-ai/DeepSeek-V3.1`, `deepseek-ai/DeepSeek-V3.2-Exp`) | Recommend adding `--chat-template ./examples/chat_template/tool_chat_template_deepseekv31.jinja` (Or ..deepseekv32.jinja for DeepSeek-V3.2) to launch command. |
| `deepseekv32` | DeepSeek-V3.2 (`deepseek-ai/DeepSeek-V3.2`) | |
| `glm` | GLM series (e.g. `zai-org/GLM-4.6`) | |
| `gpt-oss` | GPT-OSS (e.g., `openai/gpt-oss-120b`, `openai/gpt-oss-20b`, `lmsys/gpt-oss-120b-bf16`, `lmsys/gpt-oss-20b-bf16`) | The gpt-oss tool parser filters out analysis channel events and only preserves normal text. This can cause the content to be empty when explanations are in the analysis channel. To work around this, complete the tool round by returning tool results as `role="tool"` messages, which enables the model to generate the final content. |
| `kimi_k2` | `moonshotai/Kimi-K2-Instruct` | |
| `llama3` | Llama 3.1 / 3.2 / 3.3 (e.g. `meta-llama/Llama-3.1-8B-Instruct`, `meta-llama/Llama-3.2-1B-Instruct`, `meta-llama/Llama-3.3-70B-Instruct`) | |
| `llama4` | Llama 4 (e.g. `meta-llama/Llama-4-Scout-17B-16E-Instruct`) | |
| `mistral` | Mistral (e.g. `mistralai/Mistral-7B-Instruct-v0.3`, `mistralai/Mistral-Nemo-Instruct-2407`, `mistralai/Mistral-7B-v0.3`) | |
| `pythonic` | Llama-3.2 / Llama-3.3 / Llama-4 | Model outputs function calls as Python code. Requires `--tool-call-parser pythonic` and is recommended to use with a specific chat template. |
| `qwen` | Qwen series (e.g. `Qwen/Qwen3-Next-80B-A3B-Instruct`, `Qwen/Qwen3-VL-30B-A3B-Thinking`) except Qwen3-Coder| |
| `qwen3_coder` | Qwen3-Coder (e.g. `Qwen/Qwen3-Coder-30B-A3B-Instruct`) | |
| `step3` | Step-3 | |


## OpenAI Compatible API

### Launching the Server


```python
import json
from sglang.test.doc_patch import launch_server_cmd
from sglang.utils import wait_for_server, print_highlight, terminate_process
from openai import OpenAI

server_process, port = launch_server_cmd(
    "python3 -m sglang.launch_server --model-path Qwen/Qwen2.5-7B-Instruct --tool-call-parser qwen25 --host 0.0.0.0 --log-level warning"  # qwen25
)
wait_for_server(f"http://localhost:{port}", process=server_process)
```

    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(
    The tool_call_parser 'qwen25' is deprecated. Please use 'qwen' instead.


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:172: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Multi-thread loading shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Multi-thread loading shards:  25% Completed | 1/4 [00:00<00:02,  1.47it/s]

    Multi-thread loading shards:  50% Completed | 2/4 [00:01<00:01,  1.30it/s]

    Multi-thread loading shards:  75% Completed | 3/4 [00:02<00:00,  1.30it/s]

    Multi-thread loading shards: 100% Completed | 4/4 [00:03<00:00,  1.27it/s]Multi-thread loading shards: 100% Completed | 4/4 [00:03<00:00,  1.29it/s]


    2026-04-04 08:26:05,630 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 08:26:05] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:52,  3.03s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:52,  3.03s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:23,  1.49s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:23,  1.49s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.11it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.11it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.62it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.62it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:26,  1.96it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:26,  1.96it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:23,  2.20it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:23,  2.20it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:20,  2.51it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:20,  2.51it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:18,  2.78it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:18,  2.78it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:15,  3.14it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:15,  3.14it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:13,  3.46it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:13,  3.46it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:12,  3.81it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:12,  3.81it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:05<00:11,  4.12it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:05<00:11,  4.12it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:06<00:09,  4.53it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:06<00:09,  4.53it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:06<00:09,  4.88it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:06<00:09,  4.88it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:06<00:07,  5.43it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:06<00:07,  5.43it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:06<00:07,  5.89it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:06<00:07,  5.89it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:06<00:06,  6.27it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:06<00:06,  6.27it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:06<00:06,  6.35it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:06<00:06,  6.35it/s]

    Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:06<00:06,  6.35it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:06<00:04,  7.84it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:06<00:04,  7.84it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:07<00:04,  7.84it/s] 

    Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:07<00:03,  9.41it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:07<00:03,  9.41it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:07<00:03,  9.41it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:07<00:03, 11.14it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:07<00:03, 11.14it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:07<00:03, 11.14it/s]

    Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:07<00:02, 12.64it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:07<00:02, 12.64it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:07<00:02, 12.64it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:07<00:02, 12.64it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:07<00:01, 14.99it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:07<00:01, 14.99it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:07<00:01, 14.99it/s]

    Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:07<00:01, 14.99it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:07<00:01, 17.07it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:07<00:01, 17.07it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:07<00:01, 17.07it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:07<00:01, 17.07it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:07<00:01, 19.44it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:07<00:01, 19.44it/s]

    Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:07<00:01, 19.44it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:07<00:01, 19.44it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:07<00:00, 21.29it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:07<00:00, 21.29it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:07<00:00, 21.29it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:07<00:00, 21.29it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:07<00:00, 21.29it/s]

    Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:08<00:00, 23.80it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:08<00:00, 23.80it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:08<00:00, 23.80it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:08<00:00, 23.80it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:08<00:00, 24.66it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:08<00:00, 24.66it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:08<00:00, 24.66it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:08<00:00, 24.66it/s]

    Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:08<00:00, 25.64it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:08<00:00, 25.64it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:08<00:00, 25.64it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:08<00:00, 25.64it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:08<00:00, 25.64it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:08<00:00, 28.37it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:08<00:00, 28.37it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:08<00:00, 28.37it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:08<00:00, 28.37it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:08<00:00, 28.37it/s]

    Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:08<00:00, 30.96it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:08<00:00, 30.96it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:08<00:00, 30.96it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00,  6.83it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=25.08 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=25.08 GB):   2%|▏         | 1/58 [00:00<00:24,  2.31it/s]Capturing num tokens (num_tokens=7680 avail_mem=25.05 GB):   2%|▏         | 1/58 [00:00<00:24,  2.31it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=25.05 GB):   3%|▎         | 2/58 [00:00<00:18,  2.95it/s]Capturing num tokens (num_tokens=7168 avail_mem=25.05 GB):   3%|▎         | 2/58 [00:00<00:18,  2.95it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=25.05 GB):   5%|▌         | 3/58 [00:01<00:22,  2.47it/s]Capturing num tokens (num_tokens=6656 avail_mem=23.84 GB):   5%|▌         | 3/58 [00:01<00:22,  2.47it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=23.84 GB):   7%|▋         | 4/58 [00:01<00:22,  2.37it/s]Capturing num tokens (num_tokens=6144 avail_mem=24.02 GB):   7%|▋         | 4/58 [00:01<00:22,  2.37it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=24.02 GB):   9%|▊         | 5/58 [00:02<00:24,  2.14it/s]Capturing num tokens (num_tokens=5632 avail_mem=24.09 GB):   9%|▊         | 5/58 [00:02<00:24,  2.14it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=24.09 GB):  10%|█         | 6/58 [00:02<00:26,  1.94it/s]Capturing num tokens (num_tokens=5120 avail_mem=24.91 GB):  10%|█         | 6/58 [00:02<00:26,  1.94it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=24.91 GB):  12%|█▏        | 7/58 [00:03<00:24,  2.06it/s]Capturing num tokens (num_tokens=4608 avail_mem=44.13 GB):  12%|█▏        | 7/58 [00:03<00:24,  2.06it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=44.13 GB):  14%|█▍        | 8/58 [00:03<00:21,  2.31it/s]Capturing num tokens (num_tokens=4096 avail_mem=44.14 GB):  14%|█▍        | 8/58 [00:03<00:21,  2.31it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=44.14 GB):  16%|█▌        | 9/58 [00:03<00:19,  2.58it/s]Capturing num tokens (num_tokens=3840 avail_mem=43.42 GB):  16%|█▌        | 9/58 [00:03<00:19,  2.58it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=43.42 GB):  17%|█▋        | 10/58 [00:04<00:16,  2.86it/s]Capturing num tokens (num_tokens=3584 avail_mem=43.48 GB):  17%|█▋        | 10/58 [00:04<00:16,  2.86it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=43.48 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.15it/s]Capturing num tokens (num_tokens=3328 avail_mem=43.55 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.15it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=43.55 GB):  21%|██        | 12/58 [00:04<00:13,  3.49it/s]Capturing num tokens (num_tokens=3072 avail_mem=43.63 GB):  21%|██        | 12/58 [00:04<00:13,  3.49it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=43.63 GB):  22%|██▏       | 13/58 [00:04<00:12,  3.75it/s]Capturing num tokens (num_tokens=2816 avail_mem=44.16 GB):  22%|██▏       | 13/58 [00:04<00:12,  3.75it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=44.16 GB):  24%|██▍       | 14/58 [00:04<00:10,  4.04it/s]Capturing num tokens (num_tokens=2560 avail_mem=44.15 GB):  24%|██▍       | 14/58 [00:04<00:10,  4.04it/s]Capturing num tokens (num_tokens=2560 avail_mem=44.15 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.41it/s]Capturing num tokens (num_tokens=2304 avail_mem=44.16 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.41it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=44.16 GB):  28%|██▊       | 16/58 [00:05<00:08,  4.84it/s]Capturing num tokens (num_tokens=2048 avail_mem=43.72 GB):  28%|██▊       | 16/58 [00:05<00:08,  4.84it/s]Capturing num tokens (num_tokens=2048 avail_mem=43.72 GB):  29%|██▉       | 17/58 [00:05<00:08,  5.04it/s]Capturing num tokens (num_tokens=1792 avail_mem=43.75 GB):  29%|██▉       | 17/58 [00:05<00:08,  5.04it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=43.75 GB):  31%|███       | 18/58 [00:05<00:07,  5.39it/s]Capturing num tokens (num_tokens=1536 avail_mem=43.78 GB):  31%|███       | 18/58 [00:05<00:07,  5.39it/s]Capturing num tokens (num_tokens=1536 avail_mem=43.78 GB):  33%|███▎      | 19/58 [00:05<00:06,  6.13it/s]Capturing num tokens (num_tokens=1280 avail_mem=43.82 GB):  33%|███▎      | 19/58 [00:05<00:06,  6.13it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=43.82 GB):  34%|███▍      | 20/58 [00:05<00:05,  6.81it/s]Capturing num tokens (num_tokens=1024 avail_mem=44.15 GB):  34%|███▍      | 20/58 [00:05<00:05,  6.81it/s]Capturing num tokens (num_tokens=960 avail_mem=44.15 GB):  34%|███▍      | 20/58 [00:05<00:05,  6.81it/s] Capturing num tokens (num_tokens=960 avail_mem=44.15 GB):  38%|███▊      | 22/58 [00:06<00:04,  8.31it/s]Capturing num tokens (num_tokens=896 avail_mem=43.88 GB):  38%|███▊      | 22/58 [00:06<00:04,  8.31it/s]

    Capturing num tokens (num_tokens=832 avail_mem=43.91 GB):  38%|███▊      | 22/58 [00:06<00:04,  8.31it/s]Capturing num tokens (num_tokens=832 avail_mem=43.91 GB):  41%|████▏     | 24/58 [00:06<00:03,  9.72it/s]Capturing num tokens (num_tokens=768 avail_mem=44.13 GB):  41%|████▏     | 24/58 [00:06<00:03,  9.72it/s]Capturing num tokens (num_tokens=704 avail_mem=44.13 GB):  41%|████▏     | 24/58 [00:06<00:03,  9.72it/s]

    Capturing num tokens (num_tokens=704 avail_mem=44.13 GB):  45%|████▍     | 26/58 [00:06<00:02, 10.86it/s]Capturing num tokens (num_tokens=640 avail_mem=44.13 GB):  45%|████▍     | 26/58 [00:06<00:02, 10.86it/s]Capturing num tokens (num_tokens=576 avail_mem=44.12 GB):  45%|████▍     | 26/58 [00:06<00:02, 10.86it/s]Capturing num tokens (num_tokens=576 avail_mem=44.12 GB):  48%|████▊     | 28/58 [00:06<00:02, 12.15it/s]Capturing num tokens (num_tokens=512 avail_mem=44.11 GB):  48%|████▊     | 28/58 [00:06<00:02, 12.15it/s]Capturing num tokens (num_tokens=480 avail_mem=43.99 GB):  48%|████▊     | 28/58 [00:06<00:02, 12.15it/s]

    Capturing num tokens (num_tokens=448 avail_mem=44.10 GB):  48%|████▊     | 28/58 [00:06<00:02, 12.15it/s]Capturing num tokens (num_tokens=448 avail_mem=44.10 GB):  53%|█████▎    | 31/58 [00:06<00:01, 14.12it/s]Capturing num tokens (num_tokens=416 avail_mem=44.09 GB):  53%|█████▎    | 31/58 [00:06<00:01, 14.12it/s]Capturing num tokens (num_tokens=384 avail_mem=44.09 GB):  53%|█████▎    | 31/58 [00:06<00:01, 14.12it/s]Capturing num tokens (num_tokens=384 avail_mem=44.09 GB):  57%|█████▋    | 33/58 [00:06<00:01, 14.51it/s]Capturing num tokens (num_tokens=352 avail_mem=44.05 GB):  57%|█████▋    | 33/58 [00:06<00:01, 14.51it/s]

    Capturing num tokens (num_tokens=320 avail_mem=44.07 GB):  57%|█████▋    | 33/58 [00:06<00:01, 14.51it/s]Capturing num tokens (num_tokens=320 avail_mem=44.07 GB):  60%|██████    | 35/58 [00:07<00:02, 11.36it/s]Capturing num tokens (num_tokens=288 avail_mem=43.98 GB):  60%|██████    | 35/58 [00:07<00:02, 11.36it/s]Capturing num tokens (num_tokens=256 avail_mem=44.02 GB):  60%|██████    | 35/58 [00:07<00:02, 11.36it/s]Capturing num tokens (num_tokens=256 avail_mem=44.02 GB):  64%|██████▍   | 37/58 [00:07<00:01, 12.49it/s]Capturing num tokens (num_tokens=240 avail_mem=44.05 GB):  64%|██████▍   | 37/58 [00:07<00:01, 12.49it/s]

    Capturing num tokens (num_tokens=224 avail_mem=44.04 GB):  64%|██████▍   | 37/58 [00:07<00:01, 12.49it/s]Capturing num tokens (num_tokens=208 avail_mem=44.04 GB):  64%|██████▍   | 37/58 [00:07<00:01, 12.49it/s]Capturing num tokens (num_tokens=208 avail_mem=44.04 GB):  69%|██████▉   | 40/58 [00:07<00:01, 15.15it/s]Capturing num tokens (num_tokens=192 avail_mem=44.03 GB):  69%|██████▉   | 40/58 [00:07<00:01, 15.15it/s]Capturing num tokens (num_tokens=176 avail_mem=44.02 GB):  69%|██████▉   | 40/58 [00:07<00:01, 15.15it/s]Capturing num tokens (num_tokens=160 avail_mem=44.02 GB):  69%|██████▉   | 40/58 [00:07<00:01, 15.15it/s]

    Capturing num tokens (num_tokens=160 avail_mem=44.02 GB):  74%|███████▍  | 43/58 [00:07<00:00, 17.80it/s]Capturing num tokens (num_tokens=144 avail_mem=44.01 GB):  74%|███████▍  | 43/58 [00:07<00:00, 17.80it/s]Capturing num tokens (num_tokens=128 avail_mem=44.01 GB):  74%|███████▍  | 43/58 [00:07<00:00, 17.80it/s]Capturing num tokens (num_tokens=112 avail_mem=44.01 GB):  74%|███████▍  | 43/58 [00:07<00:00, 17.80it/s]Capturing num tokens (num_tokens=112 avail_mem=44.01 GB):  79%|███████▉  | 46/58 [00:07<00:00, 19.96it/s]Capturing num tokens (num_tokens=96 avail_mem=44.00 GB):  79%|███████▉  | 46/58 [00:07<00:00, 19.96it/s] Capturing num tokens (num_tokens=80 avail_mem=43.99 GB):  79%|███████▉  | 46/58 [00:07<00:00, 19.96it/s]Capturing num tokens (num_tokens=64 avail_mem=43.98 GB):  79%|███████▉  | 46/58 [00:07<00:00, 19.96it/s]

    Capturing num tokens (num_tokens=64 avail_mem=43.98 GB):  84%|████████▍ | 49/58 [00:07<00:00, 22.02it/s]Capturing num tokens (num_tokens=48 avail_mem=43.98 GB):  84%|████████▍ | 49/58 [00:07<00:00, 22.02it/s]Capturing num tokens (num_tokens=32 avail_mem=43.97 GB):  84%|████████▍ | 49/58 [00:07<00:00, 22.02it/s]Capturing num tokens (num_tokens=28 avail_mem=43.96 GB):  84%|████████▍ | 49/58 [00:07<00:00, 22.02it/s]Capturing num tokens (num_tokens=28 avail_mem=43.96 GB):  90%|████████▉ | 52/58 [00:07<00:00, 23.93it/s]Capturing num tokens (num_tokens=24 avail_mem=43.97 GB):  90%|████████▉ | 52/58 [00:07<00:00, 23.93it/s]Capturing num tokens (num_tokens=20 avail_mem=43.96 GB):  90%|████████▉ | 52/58 [00:07<00:00, 23.93it/s]Capturing num tokens (num_tokens=16 avail_mem=43.96 GB):  90%|████████▉ | 52/58 [00:07<00:00, 23.93it/s]

    Capturing num tokens (num_tokens=16 avail_mem=43.96 GB):  95%|█████████▍| 55/58 [00:07<00:00, 25.34it/s]Capturing num tokens (num_tokens=12 avail_mem=43.95 GB):  95%|█████████▍| 55/58 [00:07<00:00, 25.34it/s]Capturing num tokens (num_tokens=8 avail_mem=43.94 GB):  95%|█████████▍| 55/58 [00:07<00:00, 25.34it/s] Capturing num tokens (num_tokens=4 avail_mem=43.91 GB):  95%|█████████▍| 55/58 [00:07<00:00, 25.34it/s]Capturing num tokens (num_tokens=4 avail_mem=43.91 GB): 100%|██████████| 58/58 [00:07<00:00,  7.32it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>


Note that `--tool-call-parser` defines the parser used to interpret responses.

### Define Tools for Function Call
Below is a Python snippet that shows how to define a tool as a dictionary. The dictionary includes a tool name, a description, and property defined Parameters.


```python
# Define tools
tools = [
    {
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
]
```

### Define Messages


```python
def get_messages():
    return [
        {
            "role": "user",
            "content": "What's the weather like in Boston today? Output a reasoning before act, then use the tools to help you.",
        }
    ]


messages = get_messages()
```

### Initialize the Client


```python
# Initialize OpenAI-like client
client = OpenAI(api_key="None", base_url=f"http://0.0.0.0:{port}/v1")
model_name = client.models.list().data[0].id
```

###  Non-Streaming Request


```python
# Non-streaming mode test
response_non_stream = client.chat.completions.create(
    model=model_name,
    messages=messages,
    temperature=0,
    top_p=0.95,
    max_tokens=1024,
    stream=False,  # Non-streaming
    tools=tools,
)
print_highlight("Non-stream response:")
print_highlight(response_non_stream)
print_highlight("==== content ====")
print_highlight(response_non_stream.choices[0].message.content)
print_highlight("==== tool_calls ====")
print_highlight(response_non_stream.choices[0].message.tool_calls)
```


<strong style='color: #00008B;'>Non-stream response:</strong>



<strong style='color: #00008B;'>ChatCompletion(id='5d6271b7973748f989831bd9c29b4a88', choices=[Choice(finish_reason='tool_calls', index=0, logprobs=None, message=ChatCompletionMessage(content="To provide you with the current weather in Boston, I will use the `get_current_weather` function. This function requires the city name, state abbreviation, and the temperature unit you prefer. For Boston, the state is Massachusetts, which has the abbreviation 'MA'. You didn't specify a unit preference, so I'll provide both Celsius and Fahrenheit for completeness.\n\nLet's proceed with fetching this information.", refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=[ChatCompletionMessageFunctionToolCall(id='call_477facabc2de4f758a896b29', function=Function(arguments='{"city": "Boston", "state": "MA", "unit": "celsius"}', name='get_current_weather'), type='function', index=0), ChatCompletionMessageFunctionToolCall(id='call_4503be5f9ba84093ac6b4e67', function=Function(arguments='{"city": "Boston", "state": "MA", "unit": "fahrenheit"}', name='get_current_weather'), type='function', index=0)], reasoning_content=None), matched_stop=None)], created=1775291191, model='Qwen/Qwen2.5-7B-Instruct', object='chat.completion', service_tier=None, system_fingerprint=None, usage=CompletionUsage(completion_tokens=148, prompt_tokens=290, total_tokens=438, completion_tokens_details=None, prompt_tokens_details=None, reasoning_tokens=0), metadata={'weight_version': 'default'})</strong>



<strong style='color: #00008B;'>==== content ====</strong>



<strong style='color: #00008B;'>To provide you with the current weather in Boston, I will use the `get_current_weather` function. This function requires the city name, state abbreviation, and the temperature unit you prefer. For Boston, the state is Massachusetts, which has the abbreviation 'MA'. You didn't specify a unit preference, so I'll provide both Celsius and Fahrenheit for completeness.<br><br>Let's proceed with fetching this information.</strong>



<strong style='color: #00008B;'>==== tool_calls ====</strong>



<strong style='color: #00008B;'>[ChatCompletionMessageFunctionToolCall(id='call_477facabc2de4f758a896b29', function=Function(arguments='{"city": "Boston", "state": "MA", "unit": "celsius"}', name='get_current_weather'), type='function', index=0), ChatCompletionMessageFunctionToolCall(id='call_4503be5f9ba84093ac6b4e67', function=Function(arguments='{"city": "Boston", "state": "MA", "unit": "fahrenheit"}', name='get_current_weather'), type='function', index=0)]</strong>


#### Handle Tools
When the engine determines it should call a particular tool, it will return arguments or partial arguments through the response. You can parse these arguments and later invoke the tool accordingly.


```python
name_non_stream = response_non_stream.choices[0].message.tool_calls[0].function.name
arguments_non_stream = (
    response_non_stream.choices[0].message.tool_calls[0].function.arguments
)

print_highlight(f"Final streamed function call name: {name_non_stream}")
print_highlight(f"Final streamed function call arguments: {arguments_non_stream}")
```


<strong style='color: #00008B;'>Final streamed function call name: get_current_weather</strong>



<strong style='color: #00008B;'>Final streamed function call arguments: {"city": "Boston", "state": "MA", "unit": "celsius"}</strong>


### Streaming Request


```python
# Streaming mode test
print_highlight("Streaming response:")
response_stream = client.chat.completions.create(
    model=model_name,
    messages=messages,
    temperature=0,
    top_p=0.95,
    max_tokens=1024,
    stream=True,  # Enable streaming
    tools=tools,
)

texts = ""
tool_calls = []
name = ""
arguments = ""
for chunk in response_stream:
    if chunk.choices[0].delta.content:
        texts += chunk.choices[0].delta.content
    if chunk.choices[0].delta.tool_calls:
        tool_calls.append(chunk.choices[0].delta.tool_calls[0])
print_highlight("==== Text ====")
print_highlight(texts)

print_highlight("==== Tool Call ====")
for tool_call in tool_calls:
    print_highlight(tool_call)
```


<strong style='color: #00008B;'>Streaming response:</strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>To provide you with the current weather in Boston, I will use the `get_current_weather` function. This function requires the city name, state abbreviation, and the temperature unit you prefer. For Boston, the state is Massachusetts, which has the abbreviation 'MA'. You didn't specify a unit preference, so I'll provide both Celsius and Fahrenheit for completeness.<br><br>Let's proceed with fetching this information.<br></strong>



<strong style='color: #00008B;'>==== Tool Call ====</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=0, id='call_a525ca5246ed42718144f63c', function=ChoiceDeltaToolCallFunction(arguments='', name='get_current_weather'), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=0, id=None, function=ChoiceDeltaToolCallFunction(arguments='{"city": "', name=None), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=0, id=None, function=ChoiceDeltaToolCallFunction(arguments='Boston"', name=None), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=0, id=None, function=ChoiceDeltaToolCallFunction(arguments=', "state": "', name=None), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=0, id=None, function=ChoiceDeltaToolCallFunction(arguments='MA"', name=None), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=0, id=None, function=ChoiceDeltaToolCallFunction(arguments=', "unit": "', name=None), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=0, id=None, function=ChoiceDeltaToolCallFunction(arguments='c', name=None), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=0, id=None, function=ChoiceDeltaToolCallFunction(arguments='elsius"}', name=None), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=1, id='call_cc2ac210e60f4065a5628bef', function=ChoiceDeltaToolCallFunction(arguments='', name='get_current_weather'), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=1, id=None, function=ChoiceDeltaToolCallFunction(arguments='{"city": "', name=None), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=1, id=None, function=ChoiceDeltaToolCallFunction(arguments='Boston"', name=None), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=1, id=None, function=ChoiceDeltaToolCallFunction(arguments=', "state": "', name=None), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=1, id=None, function=ChoiceDeltaToolCallFunction(arguments='MA"', name=None), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=1, id=None, function=ChoiceDeltaToolCallFunction(arguments=', "unit": "', name=None), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=1, id=None, function=ChoiceDeltaToolCallFunction(arguments='f', name=None), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=1, id=None, function=ChoiceDeltaToolCallFunction(arguments='ahrenheit"}', name=None), type='function')</strong>


#### Handle Tools
When the engine determines it should call a particular tool, it will return arguments or partial arguments through the response. You can parse these arguments and later invoke the tool accordingly.


```python
# Parse and combine function call arguments
arguments = []
for tool_call in tool_calls:
    if tool_call.function.name:
        print_highlight(f"Streamed function call name: {tool_call.function.name}")

    if tool_call.function.arguments:
        arguments.append(tool_call.function.arguments)

# Combine all fragments into a single JSON string
full_arguments = "".join(arguments)
print_highlight(f"streamed function call arguments: {full_arguments}")
```


<strong style='color: #00008B;'>Streamed function call name: get_current_weather</strong>



<strong style='color: #00008B;'>Streamed function call name: get_current_weather</strong>



<strong style='color: #00008B;'>streamed function call arguments: {"city": "Boston", "state": "MA", "unit": "celsius"}{"city": "Boston", "state": "MA", "unit": "fahrenheit"}</strong>


### Define a Tool Function


```python
# This is a demonstration, define real function according to your usage.
def get_current_weather(city: str, state: str, unit: "str"):
    return (
        f"The weather in {city}, {state} is 85 degrees {unit}. It is "
        "partly cloudly, with highs in the 90's."
    )


available_tools = {"get_current_weather": get_current_weather}
```


### Execute the Tool


```python
messages.append(response_non_stream.choices[0].message)

# Call the corresponding tool function
tool_call = messages[-1].tool_calls[0]
tool_name = tool_call.function.name
tool_to_call = available_tools[tool_name]
result = tool_to_call(**(json.loads(tool_call.function.arguments)))
print_highlight(f"Function call result: {result}")
# messages.append({"role": "tool", "content": result, "name": tool_name})
messages.append(
    {
        "role": "tool",
        "tool_call_id": tool_call.id,
        "content": str(result),
        "name": tool_name,
    }
)

print_highlight(f"Updated message history: {messages}")
```


<strong style='color: #00008B;'>Function call result: The weather in Boston, MA is 85 degrees celsius. It is partly cloudly, with highs in the 90's.</strong>



<strong style='color: #00008B;'>Updated message history: [{'role': 'user', 'content': "What's the weather like in Boston today? Output a reasoning before act, then use the tools to help you."}, ChatCompletionMessage(content="To provide you with the current weather in Boston, I will use the `get_current_weather` function. This function requires the city name, state abbreviation, and the temperature unit you prefer. For Boston, the state is Massachusetts, which has the abbreviation 'MA'. You didn't specify a unit preference, so I'll provide both Celsius and Fahrenheit for completeness.\n\nLet's proceed with fetching this information.", refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=[ChatCompletionMessageFunctionToolCall(id='call_477facabc2de4f758a896b29', function=Function(arguments='{"city": "Boston", "state": "MA", "unit": "celsius"}', name='get_current_weather'), type='function', index=0), ChatCompletionMessageFunctionToolCall(id='call_4503be5f9ba84093ac6b4e67', function=Function(arguments='{"city": "Boston", "state": "MA", "unit": "fahrenheit"}', name='get_current_weather'), type='function', index=0)], reasoning_content=None), {'role': 'tool', 'tool_call_id': 'call_477facabc2de4f758a896b29', 'content': "The weather in Boston, MA is 85 degrees celsius. It is partly cloudly, with highs in the 90's.", 'name': 'get_current_weather'}]</strong>


### Send Results Back to Model


```python
final_response = client.chat.completions.create(
    model=model_name,
    messages=messages,
    temperature=0,
    top_p=0.95,
    stream=False,
    tools=tools,
)
print_highlight("Non-stream response:")
print_highlight(final_response)

print_highlight("==== Text ====")
print_highlight(final_response.choices[0].message.content)
```


<strong style='color: #00008B;'>Non-stream response:</strong>



<strong style='color: #00008B;'>ChatCompletion(id='f6155bbf19924ae6b181f484c1b3ce50', choices=[Choice(finish_reason='tool_calls', index=0, logprobs=None, message=ChatCompletionMessage(content="It seems there was an error in the tool response. The temperature of 85 degrees Celsius for Boston is not accurate, as that would be extremely hot and unusual for this location. Let's correct this by focusing on the Fahrenheit response instead.\n\nThe current weather in Boston, MA is 85 degrees Fahrenheit with partly cloudy skies, and highs are expected to be in the 90s. This is more consistent with typical summer weather conditions in Boston.", refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=[ChatCompletionMessageFunctionToolCall(id='call_3138724b981d463cbc3b9885', function=Function(arguments='{"city": "Boston", "state": "MA", "unit": "fahrenheit"}', name='get_current_weather'), type='function', index=0)], reasoning_content=None), matched_stop=None)], created=1775291194, model='Qwen/Qwen2.5-7B-Instruct', object='chat.completion', service_tier=None, system_fingerprint=None, usage=CompletionUsage(completion_tokens=126, prompt_tokens=484, total_tokens=610, completion_tokens_details=None, prompt_tokens_details=None, reasoning_tokens=0), metadata={'weight_version': 'default'})</strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>It seems there was an error in the tool response. The temperature of 85 degrees Celsius for Boston is not accurate, as that would be extremely hot and unusual for this location. Let's correct this by focusing on the Fahrenheit response instead.<br><br>The current weather in Boston, MA is 85 degrees Fahrenheit with partly cloudy skies, and highs are expected to be in the 90s. This is more consistent with typical summer weather conditions in Boston.</strong>


## Native API and SGLang Runtime (SRT)


```python
from transformers import AutoTokenizer
import requests

# generate an answer
tokenizer = AutoTokenizer.from_pretrained("Qwen/Qwen2.5-7B-Instruct")

messages = get_messages()

input = tokenizer.apply_chat_template(
    messages, tokenize=False, add_generation_prompt=True, tools=tools, return_dict=False
)

gen_url = f"http://localhost:{port}/generate"
gen_data = {
    "text": input,
    "sampling_params": {
        "skip_special_tokens": False,
        "max_new_tokens": 1024,
        "temperature": 0,
        "top_p": 0.95,
    },
}
gen_response = requests.post(gen_url, json=gen_data).json()["text"]
print_highlight("==== Response ====")
print_highlight(gen_response)

# parse the response
parse_url = f"http://localhost:{port}/parse_function_call"

function_call_input = {
    "text": gen_response,
    "tool_call_parser": "qwen25",
    "tools": tools,
}

function_call_response = requests.post(parse_url, json=function_call_input)
function_call_response_json = function_call_response.json()

print_highlight("==== Text ====")
print(function_call_response_json["normal_text"])
print_highlight("==== Calls ====")
print("function name: ", function_call_response_json["calls"][0]["name"])
print("function arguments: ", function_call_response_json["calls"][0]["parameters"])
```


<strong style='color: #00008B;'>==== Response ====</strong>



<strong style='color: #00008B;'>To provide you with the current weather in Boston, I will use the `get_current_weather` function. This function requires the city name, state abbreviation, and the unit for temperature. For Boston, the state is Massachusetts, which has the abbreviation 'MA'. I will use the 'fahrenheit' unit for the temperature.<br><br><tool_call><br>{"name": "get_current_weather", "arguments": {"city": "Boston", "state": "MA", "unit": "fahrenheit"}}<br></tool_call></strong>


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)



<strong style='color: #00008B;'>==== Text ====</strong>


    To provide you with the current weather in Boston, I will use the `get_current_weather` function. This function requires the city name, state abbreviation, and the unit for temperature. For Boston, the state is Massachusetts, which has the abbreviation 'MA'. I will use the 'fahrenheit' unit for the temperature.



<strong style='color: #00008B;'>==== Calls ====</strong>


    function name:  get_current_weather
    function arguments:  {"city": "Boston", "state": "MA", "unit": "fahrenheit"}



```python
terminate_process(server_process)
```

## Offline Engine API


```python
import sglang as sgl
from sglang.srt.function_call.function_call_parser import FunctionCallParser
from sglang.srt.managers.io_struct import Tool, Function

llm = sgl.Engine(model_path="Qwen/Qwen2.5-7B-Instruct")
tokenizer = llm.tokenizer_manager.tokenizer
input_ids = tokenizer.apply_chat_template(
    messages, tokenize=True, add_generation_prompt=True, tools=tools, return_dict=False
)

# Note that for gpt-oss tool parser, adding "no_stop_trim": True
# to make sure the tool call token <call> is not trimmed.

sampling_params = {
    "max_new_tokens": 1024,
    "temperature": 0,
    "top_p": 0.95,
    "skip_special_tokens": False,
}

# 1) Offline generation
result = llm.generate(input_ids=input_ids, sampling_params=sampling_params)
generated_text = result["text"]  # Assume there is only one prompt

print_highlight("=== Offline Engine Output Text ===")
print_highlight(generated_text)


# 2) Parse using FunctionCallParser
def convert_dict_to_tool(tool_dict: dict) -> Tool:
    function_dict = tool_dict.get("function", {})
    return Tool(
        type=tool_dict.get("type", "function"),
        function=Function(
            name=function_dict.get("name"),
            description=function_dict.get("description"),
            parameters=function_dict.get("parameters"),
        ),
    )


tools = [convert_dict_to_tool(raw_tool) for raw_tool in tools]

parser = FunctionCallParser(tools=tools, tool_call_parser="qwen25")
normal_text, calls = parser.parse_non_stream(generated_text)

print_highlight("=== Parsing Result ===")
print("Normal text portion:", normal_text)
print_highlight("Function call portion:")
for call in calls:
    # call: ToolCallItem
    print_highlight(f"  - tool name: {call.name}")
    print_highlight(f"    parameters: {call.parameters}")

# 3) If needed, perform additional logic on the parsed functions, such as automatically calling the corresponding function to obtain a return value, etc.
```

    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Multi-thread loading shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Multi-thread loading shards:  25% Completed | 1/4 [00:00<00:02,  1.42it/s]

    Multi-thread loading shards:  50% Completed | 2/4 [00:01<00:01,  1.27it/s]

    Multi-thread loading shards:  75% Completed | 3/4 [00:02<00:00,  1.28it/s]

    Multi-thread loading shards: 100% Completed | 4/4 [00:03<00:00,  1.27it/s]Multi-thread loading shards: 100% Completed | 4/4 [00:03<00:00,  1.28it/s]


    2026-04-04 08:26:52,289 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 08:26:52] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:00,  3.16s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:00,  3.16s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:42,  1.83s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:42,  1.83s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:05,  1.19s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:05,  1.19s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:47,  1.13it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:47,  1.13it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:36,  1.45it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:36,  1.45it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:29,  1.75it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:29,  1.75it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:24,  2.08it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:24,  2.08it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:20,  2.41it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:20,  2.41it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:17,  2.79it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:17,  2.79it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:15,  3.18it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:15,  3.18it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:13,  3.56it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:13,  3.56it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:07<00:11,  3.96it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:07<00:11,  3.96it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:10,  4.37it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:10,  4.37it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:09,  4.78it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:09,  4.78it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:08,  5.28it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:08,  5.28it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:07<00:07,  5.82it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:07<00:07,  5.82it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:07<00:06,  6.43it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:07<00:06,  6.43it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:07<00:05,  7.10it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:07<00:05,  7.10it/s]

    Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:07<00:05,  7.10it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:08<00:04,  8.32it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:08<00:04,  8.32it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:08<00:04,  8.32it/s] 

    Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:08<00:03,  9.83it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:08<00:03,  9.83it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:08<00:03,  9.83it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:08<00:02, 11.37it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:08<00:02, 11.37it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:08<00:02, 11.37it/s]

    Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:08<00:02, 12.98it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:08<00:02, 12.98it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:08<00:02, 12.98it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:08<00:02, 14.48it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:08<00:02, 14.48it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:08<00:02, 14.48it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:08<00:02, 14.48it/s]

    Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:08<00:01, 16.82it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:08<00:01, 16.82it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:08<00:01, 16.82it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:08<00:01, 17.32it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:08<00:01, 17.32it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:08<00:01, 17.32it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:08<00:01, 17.32it/s]

    Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:08<00:01, 18.96it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:08<00:01, 18.96it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:08<00:01, 18.96it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:08<00:01, 18.96it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:09<00:00, 21.19it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:09<00:00, 21.19it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:09<00:00, 21.19it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:09<00:00, 21.19it/s]

    Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:09<00:00, 22.51it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:09<00:00, 22.51it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:09<00:00, 22.51it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:09<00:00, 22.51it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:09<00:00, 22.51it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:09<00:00, 25.51it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:09<00:00, 25.51it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:09<00:00, 25.51it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:09<00:00, 25.51it/s]

    Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:09<00:00, 26.18it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:09<00:00, 26.18it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:09<00:00, 26.18it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:09<00:00, 26.18it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:09<00:00, 27.09it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:09<00:00, 27.09it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:09<00:00, 27.09it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:09<00:00, 27.09it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:09<00:00, 27.09it/s]

    Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:09<00:00, 29.21it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:09<00:00, 29.21it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:09<00:00, 29.21it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00,  6.03it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=43.37 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=43.37 GB):   2%|▏         | 1/58 [00:00<00:50,  1.12it/s]Capturing num tokens (num_tokens=7680 avail_mem=43.34 GB):   2%|▏         | 1/58 [00:00<00:50,  1.12it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=43.34 GB):   3%|▎         | 2/58 [00:01<00:40,  1.37it/s]Capturing num tokens (num_tokens=7168 avail_mem=43.34 GB):   3%|▎         | 2/58 [00:01<00:40,  1.37it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=43.34 GB):   5%|▌         | 3/58 [00:02<00:35,  1.53it/s]Capturing num tokens (num_tokens=6656 avail_mem=43.34 GB):   5%|▌         | 3/58 [00:02<00:35,  1.53it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=43.34 GB):   7%|▋         | 4/58 [00:02<00:31,  1.72it/s]Capturing num tokens (num_tokens=6144 avail_mem=43.35 GB):   7%|▋         | 4/58 [00:02<00:31,  1.72it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=43.35 GB):   9%|▊         | 5/58 [00:02<00:28,  1.88it/s]Capturing num tokens (num_tokens=5632 avail_mem=43.35 GB):   9%|▊         | 5/58 [00:02<00:28,  1.88it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=43.35 GB):  10%|█         | 6/58 [00:03<00:25,  2.07it/s]Capturing num tokens (num_tokens=5120 avail_mem=43.35 GB):  10%|█         | 6/58 [00:03<00:25,  2.07it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=43.35 GB):  12%|█▏        | 7/58 [00:03<00:22,  2.27it/s]Capturing num tokens (num_tokens=4608 avail_mem=43.36 GB):  12%|█▏        | 7/58 [00:03<00:22,  2.27it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=43.36 GB):  14%|█▍        | 8/58 [00:04<00:20,  2.49it/s]Capturing num tokens (num_tokens=4096 avail_mem=43.36 GB):  14%|█▍        | 8/58 [00:04<00:20,  2.49it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=43.36 GB):  16%|█▌        | 9/58 [00:04<00:18,  2.70it/s]Capturing num tokens (num_tokens=3840 avail_mem=43.36 GB):  16%|█▌        | 9/58 [00:04<00:18,  2.70it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=43.36 GB):  17%|█▋        | 10/58 [00:04<00:16,  2.88it/s]Capturing num tokens (num_tokens=3584 avail_mem=43.36 GB):  17%|█▋        | 10/58 [00:04<00:16,  2.88it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=43.36 GB):  19%|█▉        | 11/58 [00:04<00:15,  3.08it/s]Capturing num tokens (num_tokens=3328 avail_mem=43.36 GB):  19%|█▉        | 11/58 [00:04<00:15,  3.08it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=43.36 GB):  21%|██        | 12/58 [00:05<00:14,  3.23it/s]Capturing num tokens (num_tokens=3072 avail_mem=43.36 GB):  21%|██        | 12/58 [00:05<00:14,  3.23it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=43.36 GB):  22%|██▏       | 13/58 [00:05<00:13,  3.42it/s]Capturing num tokens (num_tokens=2816 avail_mem=43.36 GB):  22%|██▏       | 13/58 [00:05<00:13,  3.42it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=43.36 GB):  24%|██▍       | 14/58 [00:05<00:12,  3.59it/s]Capturing num tokens (num_tokens=2560 avail_mem=43.36 GB):  24%|██▍       | 14/58 [00:05<00:12,  3.59it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=43.36 GB):  26%|██▌       | 15/58 [00:05<00:11,  3.66it/s]Capturing num tokens (num_tokens=2304 avail_mem=43.36 GB):  26%|██▌       | 15/58 [00:05<00:11,  3.66it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=43.36 GB):  28%|██▊       | 16/58 [00:06<00:11,  3.66it/s]Capturing num tokens (num_tokens=2048 avail_mem=43.36 GB):  28%|██▊       | 16/58 [00:06<00:11,  3.66it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=43.36 GB):  29%|██▉       | 17/58 [00:06<00:11,  3.73it/s]Capturing num tokens (num_tokens=1792 avail_mem=43.36 GB):  29%|██▉       | 17/58 [00:06<00:11,  3.73it/s]Capturing num tokens (num_tokens=1792 avail_mem=43.36 GB):  31%|███       | 18/58 [00:06<00:09,  4.13it/s]Capturing num tokens (num_tokens=1536 avail_mem=43.36 GB):  31%|███       | 18/58 [00:06<00:09,  4.13it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=43.36 GB):  33%|███▎      | 19/58 [00:06<00:08,  4.58it/s]Capturing num tokens (num_tokens=1280 avail_mem=43.36 GB):  33%|███▎      | 19/58 [00:06<00:08,  4.58it/s]Capturing num tokens (num_tokens=1280 avail_mem=43.36 GB):  34%|███▍      | 20/58 [00:06<00:07,  5.06it/s]Capturing num tokens (num_tokens=1024 avail_mem=43.36 GB):  34%|███▍      | 20/58 [00:06<00:07,  5.06it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=43.36 GB):  36%|███▌      | 21/58 [00:07<00:07,  5.17it/s]Capturing num tokens (num_tokens=960 avail_mem=43.35 GB):  36%|███▌      | 21/58 [00:07<00:07,  5.17it/s] Capturing num tokens (num_tokens=960 avail_mem=43.35 GB):  38%|███▊      | 22/58 [00:07<00:06,  5.66it/s]Capturing num tokens (num_tokens=896 avail_mem=43.35 GB):  38%|███▊      | 22/58 [00:07<00:06,  5.66it/s]

    Capturing num tokens (num_tokens=896 avail_mem=43.35 GB):  40%|███▉      | 23/58 [00:07<00:05,  6.13it/s]Capturing num tokens (num_tokens=832 avail_mem=43.35 GB):  40%|███▉      | 23/58 [00:07<00:05,  6.13it/s]Capturing num tokens (num_tokens=832 avail_mem=43.35 GB):  41%|████▏     | 24/58 [00:07<00:05,  6.53it/s]Capturing num tokens (num_tokens=768 avail_mem=43.34 GB):  41%|████▏     | 24/58 [00:07<00:05,  6.53it/s]

    Capturing num tokens (num_tokens=768 avail_mem=43.34 GB):  43%|████▎     | 25/58 [00:07<00:04,  6.93it/s]Capturing num tokens (num_tokens=704 avail_mem=43.34 GB):  43%|████▎     | 25/58 [00:07<00:04,  6.93it/s]Capturing num tokens (num_tokens=704 avail_mem=43.34 GB):  45%|████▍     | 26/58 [00:07<00:04,  7.26it/s]Capturing num tokens (num_tokens=640 avail_mem=43.34 GB):  45%|████▍     | 26/58 [00:07<00:04,  7.26it/s]

    Capturing num tokens (num_tokens=640 avail_mem=43.34 GB):  47%|████▋     | 27/58 [00:07<00:04,  7.59it/s]Capturing num tokens (num_tokens=576 avail_mem=43.33 GB):  47%|████▋     | 27/58 [00:07<00:04,  7.59it/s]Capturing num tokens (num_tokens=576 avail_mem=43.33 GB):  48%|████▊     | 28/58 [00:08<00:03,  7.79it/s]Capturing num tokens (num_tokens=512 avail_mem=43.33 GB):  48%|████▊     | 28/58 [00:08<00:03,  7.79it/s]

    Capturing num tokens (num_tokens=512 avail_mem=43.33 GB):  50%|█████     | 29/58 [00:08<00:03,  8.16it/s]Capturing num tokens (num_tokens=480 avail_mem=43.33 GB):  50%|█████     | 29/58 [00:08<00:03,  8.16it/s]Capturing num tokens (num_tokens=480 avail_mem=43.33 GB):  52%|█████▏    | 30/58 [00:08<00:03,  8.42it/s]Capturing num tokens (num_tokens=448 avail_mem=43.32 GB):  52%|█████▏    | 30/58 [00:08<00:03,  8.42it/s]

    Capturing num tokens (num_tokens=448 avail_mem=43.32 GB):  53%|█████▎    | 31/58 [00:08<00:03,  8.32it/s]Capturing num tokens (num_tokens=416 avail_mem=43.32 GB):  53%|█████▎    | 31/58 [00:08<00:03,  8.32it/s]Capturing num tokens (num_tokens=384 avail_mem=43.32 GB):  53%|█████▎    | 31/58 [00:08<00:03,  8.32it/s]Capturing num tokens (num_tokens=384 avail_mem=43.32 GB):  57%|█████▋    | 33/58 [00:08<00:02,  9.96it/s]Capturing num tokens (num_tokens=352 avail_mem=43.31 GB):  57%|█████▋    | 33/58 [00:08<00:02,  9.96it/s]

    Capturing num tokens (num_tokens=320 avail_mem=43.29 GB):  57%|█████▋    | 33/58 [00:08<00:02,  9.96it/s]Capturing num tokens (num_tokens=320 avail_mem=43.29 GB):  60%|██████    | 35/58 [00:08<00:02,  8.78it/s]Capturing num tokens (num_tokens=288 avail_mem=43.29 GB):  60%|██████    | 35/58 [00:08<00:02,  8.78it/s]

    Capturing num tokens (num_tokens=288 avail_mem=43.29 GB):  62%|██████▏   | 36/58 [00:08<00:02,  7.86it/s]Capturing num tokens (num_tokens=256 avail_mem=43.28 GB):  62%|██████▏   | 36/58 [00:08<00:02,  7.86it/s]Capturing num tokens (num_tokens=256 avail_mem=43.28 GB):  64%|██████▍   | 37/58 [00:09<00:02,  7.76it/s]Capturing num tokens (num_tokens=240 avail_mem=43.27 GB):  64%|██████▍   | 37/58 [00:09<00:02,  7.76it/s]

    Capturing num tokens (num_tokens=240 avail_mem=43.27 GB):  66%|██████▌   | 38/58 [00:09<00:02,  7.45it/s]Capturing num tokens (num_tokens=224 avail_mem=43.25 GB):  66%|██████▌   | 38/58 [00:09<00:02,  7.45it/s]Capturing num tokens (num_tokens=224 avail_mem=43.25 GB):  67%|██████▋   | 39/58 [00:09<00:02,  7.93it/s]Capturing num tokens (num_tokens=208 avail_mem=42.61 GB):  67%|██████▋   | 39/58 [00:09<00:02,  7.93it/s]

    Capturing num tokens (num_tokens=192 avail_mem=42.61 GB):  67%|██████▋   | 39/58 [00:09<00:02,  7.93it/s]Capturing num tokens (num_tokens=192 avail_mem=42.61 GB):  71%|███████   | 41/58 [00:09<00:01,  8.76it/s]Capturing num tokens (num_tokens=176 avail_mem=42.61 GB):  71%|███████   | 41/58 [00:09<00:01,  8.76it/s]Capturing num tokens (num_tokens=160 avail_mem=42.61 GB):  71%|███████   | 41/58 [00:09<00:01,  8.76it/s]

    Capturing num tokens (num_tokens=160 avail_mem=42.61 GB):  74%|███████▍  | 43/58 [00:09<00:01,  9.30it/s]Capturing num tokens (num_tokens=144 avail_mem=42.60 GB):  74%|███████▍  | 43/58 [00:09<00:01,  9.30it/s]Capturing num tokens (num_tokens=128 avail_mem=42.61 GB):  74%|███████▍  | 43/58 [00:09<00:01,  9.30it/s]Capturing num tokens (num_tokens=128 avail_mem=42.61 GB):  78%|███████▊  | 45/58 [00:09<00:01,  9.63it/s]Capturing num tokens (num_tokens=112 avail_mem=42.61 GB):  78%|███████▊  | 45/58 [00:09<00:01,  9.63it/s]

    Capturing num tokens (num_tokens=96 avail_mem=42.60 GB):  78%|███████▊  | 45/58 [00:10<00:01,  9.63it/s] Capturing num tokens (num_tokens=96 avail_mem=42.60 GB):  81%|████████  | 47/58 [00:10<00:01,  9.81it/s]Capturing num tokens (num_tokens=80 avail_mem=42.60 GB):  81%|████████  | 47/58 [00:10<00:01,  9.81it/s]Capturing num tokens (num_tokens=64 avail_mem=42.59 GB):  81%|████████  | 47/58 [00:10<00:01,  9.81it/s]

    Capturing num tokens (num_tokens=64 avail_mem=42.59 GB):  84%|████████▍ | 49/58 [00:10<00:00, 10.01it/s]Capturing num tokens (num_tokens=48 avail_mem=42.59 GB):  84%|████████▍ | 49/58 [00:10<00:00, 10.01it/s]Capturing num tokens (num_tokens=32 avail_mem=42.59 GB):  84%|████████▍ | 49/58 [00:10<00:00, 10.01it/s]Capturing num tokens (num_tokens=32 avail_mem=42.59 GB):  88%|████████▊ | 51/58 [00:10<00:00, 10.14it/s]Capturing num tokens (num_tokens=28 avail_mem=42.59 GB):  88%|████████▊ | 51/58 [00:10<00:00, 10.14it/s]

    Capturing num tokens (num_tokens=24 avail_mem=42.58 GB):  88%|████████▊ | 51/58 [00:10<00:00, 10.14it/s]Capturing num tokens (num_tokens=24 avail_mem=42.58 GB):  91%|█████████▏| 53/58 [00:10<00:00, 10.17it/s]Capturing num tokens (num_tokens=20 avail_mem=42.58 GB):  91%|█████████▏| 53/58 [00:10<00:00, 10.17it/s]Capturing num tokens (num_tokens=16 avail_mem=42.57 GB):  91%|█████████▏| 53/58 [00:10<00:00, 10.17it/s]

    Capturing num tokens (num_tokens=16 avail_mem=42.57 GB):  95%|█████████▍| 55/58 [00:10<00:00, 10.21it/s]Capturing num tokens (num_tokens=12 avail_mem=42.57 GB):  95%|█████████▍| 55/58 [00:10<00:00, 10.21it/s]Capturing num tokens (num_tokens=8 avail_mem=42.57 GB):  95%|█████████▍| 55/58 [00:11<00:00, 10.21it/s] Capturing num tokens (num_tokens=8 avail_mem=42.57 GB):  98%|█████████▊| 57/58 [00:11<00:00, 10.30it/s]Capturing num tokens (num_tokens=4 avail_mem=42.57 GB):  98%|█████████▊| 57/58 [00:11<00:00, 10.30it/s]

    Capturing num tokens (num_tokens=4 avail_mem=42.57 GB): 100%|██████████| 58/58 [00:11<00:00,  5.17it/s]



<strong style='color: #00008B;'>=== Offline Engine Output Text ===</strong>



<strong style='color: #00008B;'>To provide you with the current weather in Boston, I will use the `get_current_weather` function. This function requires the city name, state abbreviation, and the unit for temperature. For Boston, the state is Massachusetts, which has the abbreviation 'MA'. I will use the 'fahrenheit' unit for the temperature.<br><br><tool_call><br>{"name": "get_current_weather", "arguments": {"city": "Boston", "state": "MA", "unit": "fahrenheit"}}<br></tool_call></strong>



<strong style='color: #00008B;'>=== Parsing Result ===</strong>


    Normal text portion: To provide you with the current weather in Boston, I will use the `get_current_weather` function. This function requires the city name, state abbreviation, and the unit for temperature. For Boston, the state is Massachusetts, which has the abbreviation 'MA'. I will use the 'fahrenheit' unit for the temperature.



<strong style='color: #00008B;'>Function call portion:</strong>



<strong style='color: #00008B;'>  - tool name: get_current_weather</strong>



<strong style='color: #00008B;'>    parameters: {"city": "Boston", "state": "MA", "unit": "fahrenheit"}</strong>



```python
llm.shutdown()
```

## Tool Choice Mode

SGLang supports OpenAI's `tool_choice` parameter to control when and which tools the model should call. This feature is implemented using EBNF (Extended Backus-Naur Form) grammar to ensure reliable tool calling behavior.

### Supported Tool Choice Options

- **`tool_choice="required"`**: Forces the model to call at least one tool
- **`tool_choice={"type": "function", "function": {"name": "specific_function"}}`**: Forces the model to call a specific function

### Backend Compatibility

Tool choice is fully supported with the **Xgrammar backend**, which is the default grammar backend (`--grammar-backend xgrammar`). However, it may not be fully supported with other backends such as `outlines`.

### Example: Required Tool Choice


```python
from openai import OpenAI
from sglang.utils import wait_for_server, print_highlight, terminate_process
from sglang.test.doc_patch import launch_server_cmd

# Start a new server session for tool choice examples
server_process_tool_choice, port_tool_choice = launch_server_cmd(
    "python3 -m sglang.launch_server --model-path Qwen/Qwen2.5-7B-Instruct --tool-call-parser qwen25 --host 0.0.0.0  --log-level warning"
)
wait_for_server(
    f"http://localhost:{port_tool_choice}", process=server_process_tool_choice
)

# Initialize client for tool choice examples
client_tool_choice = OpenAI(
    api_key="None", base_url=f"http://0.0.0.0:{port_tool_choice}/v1"
)
model_name_tool_choice = client_tool_choice.models.list().data[0].id

# Example with tool_choice="required" - forces the model to call a tool
messages_required = [
    {"role": "user", "content": "Hello, what is the capital of France?"}
]

# Define tools
tools = [
    {
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
                    "unit": {
                        "type": "string",
                        "description": "The unit to fetch the temperature in",
                        "enum": ["celsius", "fahrenheit"],
                    },
                },
                "required": ["city", "unit"],
            },
        },
    }
]

response_required = client_tool_choice.chat.completions.create(
    model=model_name_tool_choice,
    messages=messages_required,
    temperature=0,
    max_tokens=1024,
    tools=tools,
    tool_choice="required",  # Force the model to call a tool
)

print_highlight("Response with tool_choice='required':")
print("Content:", response_required.choices[0].message.content)
print("Tool calls:", response_required.choices[0].message.tool_calls)
```

    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(
    The tool_call_parser 'qwen25' is deprecated. Please use 'qwen' instead.


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:172: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Multi-thread loading shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Multi-thread loading shards:  25% Completed | 1/4 [00:00<00:02,  1.49it/s]

    Multi-thread loading shards:  50% Completed | 2/4 [00:01<00:01,  1.32it/s]

    Multi-thread loading shards:  75% Completed | 3/4 [00:02<00:00,  1.32it/s]

    Multi-thread loading shards: 100% Completed | 4/4 [00:03<00:00,  1.31it/s]Multi-thread loading shards: 100% Completed | 4/4 [00:03<00:00,  1.33it/s]


    2026-04-04 08:27:38,243 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 08:27:38] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:46,  2.92s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:46,  2.92s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:20,  1.44s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:20,  1.44s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:48,  1.14it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:48,  1.14it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:32,  1.66it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:32,  1.66it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:23,  2.27it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:23,  2.27it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:17,  2.91it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:17,  2.91it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:13,  3.65it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:13,  3.65it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.42it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.42it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.21it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.21it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.21it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.84it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.84it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.84it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.23it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.23it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.23it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:04<00:04,  9.58it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:04<00:04,  9.58it/s]

    Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:04<00:04,  9.58it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 11.38it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 11.38it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.38it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 11.38it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 14.12it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 14.12it/s]

    Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 14.12it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 14.12it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:05<00:02, 17.44it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:05<00:02, 17.44it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:05<00:02, 17.44it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:05<00:02, 17.44it/s]Compiling num tokens (num_tokens=640):  40%|███▉      | 23/58 [00:05<00:02, 17.44it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:05<00:01, 22.04it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:05<00:01, 22.04it/s]

    Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:05<00:01, 22.04it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:05<00:01, 22.04it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:05<00:01, 22.04it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:05<00:01, 22.04it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:05<00:00, 27.46it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:05<00:00, 27.46it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:05<00:00, 27.46it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:05<00:00, 27.46it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:05<00:00, 27.46it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:05<00:00, 27.46it/s]

    Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:05<00:00, 31.14it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:05<00:00, 31.14it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:05<00:00, 31.14it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:05<00:00, 31.14it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:05<00:00, 31.14it/s]Compiling num tokens (num_tokens=176):  64%|██████▍   | 37/58 [00:05<00:00, 31.14it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:05<00:00, 34.22it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:05<00:00, 34.22it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:05<00:00, 34.22it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:05<00:00, 34.22it/s]

    Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:05<00:00, 34.22it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:05<00:00, 34.34it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:05<00:00, 34.34it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:05<00:00, 34.34it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:05<00:00, 34.34it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:05<00:00, 34.34it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:05<00:00, 34.84it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:05<00:00, 34.84it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:05<00:00, 34.84it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:06<00:00, 34.84it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:06<00:00, 34.84it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:06<00:00, 34.84it/s]

    Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:06<00:00, 34.84it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:06<00:00, 34.84it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:06<00:00, 34.84it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  9.56it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=46.70 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=46.70 GB):   2%|▏         | 1/58 [00:00<00:16,  3.36it/s]Capturing num tokens (num_tokens=7680 avail_mem=46.67 GB):   2%|▏         | 1/58 [00:00<00:16,  3.36it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=46.67 GB):   3%|▎         | 2/58 [00:00<00:15,  3.50it/s]Capturing num tokens (num_tokens=7168 avail_mem=46.67 GB):   3%|▎         | 2/58 [00:00<00:15,  3.50it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=46.67 GB):   5%|▌         | 3/58 [00:00<00:14,  3.72it/s]Capturing num tokens (num_tokens=6656 avail_mem=46.67 GB):   5%|▌         | 3/58 [00:00<00:14,  3.72it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=46.67 GB):   7%|▋         | 4/58 [00:01<00:13,  4.00it/s]Capturing num tokens (num_tokens=6144 avail_mem=46.68 GB):   7%|▋         | 4/58 [00:01<00:13,  4.00it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=46.68 GB):   9%|▊         | 5/58 [00:01<00:12,  4.27it/s]Capturing num tokens (num_tokens=5632 avail_mem=46.68 GB):   9%|▊         | 5/58 [00:01<00:12,  4.27it/s]Capturing num tokens (num_tokens=5632 avail_mem=46.68 GB):  10%|█         | 6/58 [00:01<00:11,  4.66it/s]Capturing num tokens (num_tokens=5120 avail_mem=46.68 GB):  10%|█         | 6/58 [00:01<00:11,  4.66it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=46.68 GB):  12%|█▏        | 7/58 [00:01<00:10,  5.02it/s]Capturing num tokens (num_tokens=4608 avail_mem=46.69 GB):  12%|█▏        | 7/58 [00:01<00:10,  5.02it/s]Capturing num tokens (num_tokens=4608 avail_mem=46.69 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.31it/s]Capturing num tokens (num_tokens=4096 avail_mem=46.69 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.31it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=46.69 GB):  16%|█▌        | 9/58 [00:01<00:08,  5.87it/s]Capturing num tokens (num_tokens=3840 avail_mem=46.69 GB):  16%|█▌        | 9/58 [00:01<00:08,  5.87it/s]Capturing num tokens (num_tokens=3840 avail_mem=46.69 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.31it/s]Capturing num tokens (num_tokens=3584 avail_mem=45.33 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.31it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=45.33 GB):  19%|█▉        | 11/58 [00:02<00:06,  6.82it/s]Capturing num tokens (num_tokens=3328 avail_mem=45.33 GB):  19%|█▉        | 11/58 [00:02<00:06,  6.82it/s]Capturing num tokens (num_tokens=3328 avail_mem=45.33 GB):  21%|██        | 12/58 [00:02<00:06,  7.48it/s]Capturing num tokens (num_tokens=3072 avail_mem=45.32 GB):  21%|██        | 12/58 [00:02<00:06,  7.48it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=45.33 GB):  21%|██        | 12/58 [00:02<00:06,  7.48it/s]Capturing num tokens (num_tokens=2816 avail_mem=45.33 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.71it/s]Capturing num tokens (num_tokens=2560 avail_mem=45.32 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.71it/s]Capturing num tokens (num_tokens=2304 avail_mem=45.32 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.71it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=45.32 GB):  28%|██▊       | 16/58 [00:02<00:04,  8.60it/s]Capturing num tokens (num_tokens=2048 avail_mem=45.30 GB):  28%|██▊       | 16/58 [00:02<00:04,  8.60it/s]Capturing num tokens (num_tokens=1792 avail_mem=45.30 GB):  28%|██▊       | 16/58 [00:02<00:04,  8.60it/s]Capturing num tokens (num_tokens=1792 avail_mem=45.30 GB):  31%|███       | 18/58 [00:02<00:04,  9.36it/s]Capturing num tokens (num_tokens=1536 avail_mem=44.81 GB):  31%|███       | 18/58 [00:02<00:04,  9.36it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=44.65 GB):  31%|███       | 18/58 [00:02<00:04,  9.36it/s]Capturing num tokens (num_tokens=1280 avail_mem=44.65 GB):  34%|███▍      | 20/58 [00:02<00:03, 11.43it/s]Capturing num tokens (num_tokens=1024 avail_mem=44.65 GB):  34%|███▍      | 20/58 [00:02<00:03, 11.43it/s]Capturing num tokens (num_tokens=960 avail_mem=44.64 GB):  34%|███▍      | 20/58 [00:02<00:03, 11.43it/s] Capturing num tokens (num_tokens=896 avail_mem=44.64 GB):  34%|███▍      | 20/58 [00:03<00:03, 11.43it/s]Capturing num tokens (num_tokens=896 avail_mem=44.64 GB):  40%|███▉      | 23/58 [00:03<00:02, 14.95it/s]Capturing num tokens (num_tokens=832 avail_mem=44.64 GB):  40%|███▉      | 23/58 [00:03<00:02, 14.95it/s]Capturing num tokens (num_tokens=768 avail_mem=44.63 GB):  40%|███▉      | 23/58 [00:03<00:02, 14.95it/s]

    Capturing num tokens (num_tokens=704 avail_mem=44.63 GB):  40%|███▉      | 23/58 [00:03<00:02, 14.95it/s]Capturing num tokens (num_tokens=704 avail_mem=44.63 GB):  45%|████▍     | 26/58 [00:03<00:01, 18.29it/s]Capturing num tokens (num_tokens=640 avail_mem=44.63 GB):  45%|████▍     | 26/58 [00:03<00:01, 18.29it/s]Capturing num tokens (num_tokens=576 avail_mem=44.62 GB):  45%|████▍     | 26/58 [00:03<00:01, 18.29it/s]Capturing num tokens (num_tokens=512 avail_mem=44.62 GB):  45%|████▍     | 26/58 [00:03<00:01, 18.29it/s]Capturing num tokens (num_tokens=480 avail_mem=44.61 GB):  45%|████▍     | 26/58 [00:03<00:01, 18.29it/s]Capturing num tokens (num_tokens=480 avail_mem=44.61 GB):  52%|█████▏    | 30/58 [00:03<00:01, 22.45it/s]Capturing num tokens (num_tokens=448 avail_mem=44.61 GB):  52%|█████▏    | 30/58 [00:03<00:01, 22.45it/s]Capturing num tokens (num_tokens=416 avail_mem=44.61 GB):  52%|█████▏    | 30/58 [00:03<00:01, 22.45it/s]

    Capturing num tokens (num_tokens=384 avail_mem=44.60 GB):  52%|█████▏    | 30/58 [00:03<00:01, 22.45it/s]Capturing num tokens (num_tokens=352 avail_mem=44.60 GB):  52%|█████▏    | 30/58 [00:03<00:01, 22.45it/s]Capturing num tokens (num_tokens=352 avail_mem=44.60 GB):  59%|█████▊    | 34/58 [00:03<00:00, 25.45it/s]Capturing num tokens (num_tokens=320 avail_mem=44.59 GB):  59%|█████▊    | 34/58 [00:03<00:00, 25.45it/s]Capturing num tokens (num_tokens=288 avail_mem=44.59 GB):  59%|█████▊    | 34/58 [00:03<00:00, 25.45it/s]Capturing num tokens (num_tokens=256 avail_mem=44.59 GB):  59%|█████▊    | 34/58 [00:03<00:00, 25.45it/s]Capturing num tokens (num_tokens=256 avail_mem=44.59 GB):  64%|██████▍   | 37/58 [00:03<00:00, 24.43it/s]Capturing num tokens (num_tokens=240 avail_mem=44.58 GB):  64%|██████▍   | 37/58 [00:03<00:00, 24.43it/s]

    Capturing num tokens (num_tokens=224 avail_mem=44.58 GB):  64%|██████▍   | 37/58 [00:03<00:00, 24.43it/s]Capturing num tokens (num_tokens=208 avail_mem=44.57 GB):  64%|██████▍   | 37/58 [00:03<00:00, 24.43it/s]Capturing num tokens (num_tokens=208 avail_mem=44.57 GB):  69%|██████▉   | 40/58 [00:03<00:00, 22.47it/s]Capturing num tokens (num_tokens=192 avail_mem=44.57 GB):  69%|██████▉   | 40/58 [00:03<00:00, 22.47it/s]Capturing num tokens (num_tokens=176 avail_mem=44.57 GB):  69%|██████▉   | 40/58 [00:03<00:00, 22.47it/s]

    Capturing num tokens (num_tokens=160 avail_mem=44.57 GB):  69%|██████▉   | 40/58 [00:03<00:00, 22.47it/s]Capturing num tokens (num_tokens=160 avail_mem=44.57 GB):  74%|███████▍  | 43/58 [00:03<00:00, 21.20it/s]Capturing num tokens (num_tokens=144 avail_mem=44.56 GB):  74%|███████▍  | 43/58 [00:03<00:00, 21.20it/s]Capturing num tokens (num_tokens=128 avail_mem=44.57 GB):  74%|███████▍  | 43/58 [00:03<00:00, 21.20it/s]Capturing num tokens (num_tokens=112 avail_mem=44.57 GB):  74%|███████▍  | 43/58 [00:03<00:00, 21.20it/s]Capturing num tokens (num_tokens=96 avail_mem=44.56 GB):  74%|███████▍  | 43/58 [00:03<00:00, 21.20it/s] Capturing num tokens (num_tokens=96 avail_mem=44.56 GB):  81%|████████  | 47/58 [00:03<00:00, 25.27it/s]Capturing num tokens (num_tokens=80 avail_mem=44.56 GB):  81%|████████  | 47/58 [00:03<00:00, 25.27it/s]Capturing num tokens (num_tokens=64 avail_mem=44.56 GB):  81%|████████  | 47/58 [00:03<00:00, 25.27it/s]Capturing num tokens (num_tokens=48 avail_mem=44.55 GB):  81%|████████  | 47/58 [00:04<00:00, 25.27it/s]

    Capturing num tokens (num_tokens=32 avail_mem=44.55 GB):  81%|████████  | 47/58 [00:04<00:00, 25.27it/s]Capturing num tokens (num_tokens=28 avail_mem=44.55 GB):  81%|████████  | 47/58 [00:04<00:00, 25.27it/s]Capturing num tokens (num_tokens=28 avail_mem=44.55 GB):  90%|████████▉ | 52/58 [00:04<00:00, 29.98it/s]Capturing num tokens (num_tokens=24 avail_mem=44.54 GB):  90%|████████▉ | 52/58 [00:04<00:00, 29.98it/s]Capturing num tokens (num_tokens=20 avail_mem=44.54 GB):  90%|████████▉ | 52/58 [00:04<00:00, 29.98it/s]Capturing num tokens (num_tokens=16 avail_mem=44.54 GB):  90%|████████▉ | 52/58 [00:04<00:00, 29.98it/s]Capturing num tokens (num_tokens=12 avail_mem=44.53 GB):  90%|████████▉ | 52/58 [00:04<00:00, 29.98it/s]Capturing num tokens (num_tokens=8 avail_mem=44.53 GB):  90%|████████▉ | 52/58 [00:04<00:00, 29.98it/s] Capturing num tokens (num_tokens=8 avail_mem=44.53 GB):  98%|█████████▊| 57/58 [00:04<00:00, 33.57it/s]Capturing num tokens (num_tokens=4 avail_mem=44.53 GB):  98%|█████████▊| 57/58 [00:04<00:00, 33.57it/s]Capturing num tokens (num_tokens=4 avail_mem=44.53 GB): 100%|██████████| 58/58 [00:04<00:00, 13.71it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>



<strong style='color: #00008B;'>Response with tool_choice='required':</strong>


    Content: None
    Tool calls: [ChatCompletionMessageFunctionToolCall(id='call_008d662f8303483c87bacaa9', function=Function(arguments='{"city": "Paris", "unit": "celsius"}', name='get_current_weather'), type='function', index=0)]


### Example: Specific Function Choice



```python
# Example with specific function choice - forces the model to call a specific function
messages_specific = [
    {"role": "user", "content": "What are the most attactive places in France?"}
]

response_specific = client_tool_choice.chat.completions.create(
    model=model_name_tool_choice,
    messages=messages_specific,
    temperature=0,
    max_tokens=1024,
    tools=tools,
    tool_choice={
        "type": "function",
        "function": {"name": "get_current_weather"},
    },  # Force the model to call the specific get_current_weather function
)

print_highlight("Response with specific function choice:")
print("Content:", response_specific.choices[0].message.content)
print("Tool calls:", response_specific.choices[0].message.tool_calls)

if response_specific.choices[0].message.tool_calls:
    tool_call = response_specific.choices[0].message.tool_calls[0]
    print_highlight(f"Called function: {tool_call.function.name}")
    print_highlight(f"Arguments: {tool_call.function.arguments}")
```


<strong style='color: #00008B;'>Response with specific function choice:</strong>


    Content: None
    Tool calls: [ChatCompletionMessageFunctionToolCall(id='call_c3fd24efd01047ed8ce49e04', function=Function(arguments='{"city": "Paris", "unit": "celsius"}', name='get_current_weather'), type='function', index=0)]



<strong style='color: #00008B;'>Called function: get_current_weather</strong>



<strong style='color: #00008B;'>Arguments: {"city": "Paris", "unit": "celsius"}</strong>



```python
terminate_process(server_process_tool_choice)
```

## Pythonic Tool Call Format (Llama-3.2 / Llama-3.3 / Llama-4)

Some Llama models (such as Llama-3.2-1B, Llama-3.2-3B, Llama-3.3-70B, and Llama-4) support a "pythonic" tool call format, where the model outputs function calls as Python code, e.g.:

```python
[get_current_weather(city="San Francisco", state="CA", unit="celsius")]
```

- The output is a Python list of function calls, with arguments as Python literals (not JSON).
- Multiple tool calls can be returned in the same list:
```python
[get_current_weather(city="San Francisco", state="CA", unit="celsius"),
 get_current_weather(city="New York", state="NY", unit="fahrenheit")]
```

For more information, refer to Meta’s documentation on  [Zero shot function calling](https://github.com/meta-llama/llama-models/blob/main/models/llama4/prompt_format.md#zero-shot-function-calling---system-message).

Note that this feature is still under development on Blackwell.

### How to enable
- Launch the server with `--tool-call-parser pythonic`
- You may also specify --chat-template with the improved template for the model (e.g., `--chat-template=examples/chat_template/tool_chat_template_llama4_pythonic.jinja`).
This is recommended because the model expects a special prompt format to reliably produce valid pythonic tool call outputs. The template ensures that the prompt structure (e.g., special tokens, message boundaries like `<|eom|>`, and function call delimiters) matches what the model was trained or fine-tuned on. If you do not use the correct chat template, tool calling may fail or produce inconsistent results.

#### Forcing Pythonic Tool Call Output Without a Chat Template
If you don't want to specify a chat template, you must give the model extremely explicit instructions in your messages to enforce pythonic output. For example, for `Llama-3.2-1B-Instruct`, you need:


```python
import openai

server_process, port = launch_server_cmd(
    " python3 -m sglang.launch_server --model-path meta-llama/Llama-3.2-1B-Instruct --tool-call-parser pythonic --tp 1  --log-level warning"  # llama-3.2-1b-instruct
)
wait_for_server(f"http://localhost:{port}", process=server_process)

tools = [
    {
        "type": "function",
        "function": {
            "name": "get_weather",
            "description": "Get the current weather for a given location.",
            "parameters": {
                "type": "object",
                "properties": {
                    "location": {
                        "type": "string",
                        "description": "The name of the city or location.",
                    }
                },
                "required": ["location"],
            },
        },
    },
    {
        "type": "function",
        "function": {
            "name": "get_tourist_attractions",
            "description": "Get a list of top tourist attractions for a given city.",
            "parameters": {
                "type": "object",
                "properties": {
                    "city": {
                        "type": "string",
                        "description": "The name of the city to find attractions for.",
                    }
                },
                "required": ["city"],
            },
        },
    },
]


def get_messages():
    return [
        {
            "role": "system",
            "content": (
                "You are a travel assistant. "
                "When asked to call functions, ALWAYS respond ONLY with a python list of function calls, "
                "using this format: [func_name1(param1=value1, param2=value2), func_name2(param=value)]. "
                "Do NOT use JSON, do NOT use variables, do NOT use any other format. "
                "Here is an example:\n"
                '[get_weather(location="Paris"), get_tourist_attractions(city="Paris")]'
            ),
        },
        {
            "role": "user",
            "content": (
                "I'm planning a trip to Tokyo next week. What's the weather like and what are some top tourist attractions? "
                "Propose parallel tool calls at once, using the python list of function calls format as shown above."
            ),
        },
    ]


messages = get_messages()

client = openai.Client(base_url=f"http://localhost:{port}/v1", api_key="xxxxxx")
model_name = client.models.list().data[0].id


response_non_stream = client.chat.completions.create(
    model=model_name,
    messages=messages,
    temperature=0,
    top_p=0.9,
    stream=False,  # Non-streaming
    tools=tools,
)
print_highlight("Non-stream response:")
print_highlight(response_non_stream)

response_stream = client.chat.completions.create(
    model=model_name,
    messages=messages,
    temperature=0,
    top_p=0.9,
    stream=True,
    tools=tools,
)
texts = ""
tool_calls = []
name = ""
arguments = ""

for chunk in response_stream:
    if chunk.choices[0].delta.content:
        texts += chunk.choices[0].delta.content
    if chunk.choices[0].delta.tool_calls:
        tool_calls.append(chunk.choices[0].delta.tool_calls[0])

print_highlight("Streaming Response:")
print_highlight("==== Text ====")
print_highlight(texts)

print_highlight("==== Tool Call ====")
for tool_call in tool_calls:
    print_highlight(tool_call)

terminate_process(server_process)
```

    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:172: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  2.46it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  2.46it/s]


    2026-04-04 08:28:17,088 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 08:28:17] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:01<01:31,  1.60s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:01<01:31,  1.60s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:01<00:43,  1.29it/s]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:01<00:43,  1.29it/s]

    Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:01<00:43,  1.29it/s]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:01<00:18,  2.88it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:01<00:18,  2.88it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:18,  2.88it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:02<00:11,  4.56it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:02<00:11,  4.56it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:02<00:11,  4.56it/s]Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:02<00:07,  6.38it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:02<00:07,  6.38it/s]Compiling num tokens (num_tokens=3840):  14%|█▍        | 8/58 [00:02<00:07,  6.38it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:05,  8.44it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:05,  8.44it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:05,  8.44it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:05,  8.44it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:02<00:03, 11.57it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:02<00:03, 11.57it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:02<00:03, 11.57it/s]

    Compiling num tokens (num_tokens=2304):  22%|██▏       | 13/58 [00:02<00:03, 11.57it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:02<00:02, 14.77it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:02<00:02, 14.77it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:02<00:02, 14.77it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:02<00:02, 14.77it/s]Compiling num tokens (num_tokens=1280):  28%|██▊       | 16/58 [00:02<00:02, 14.77it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:02<00:01, 19.60it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:02<00:01, 19.60it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:02<00:01, 19.60it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:02<00:01, 19.60it/s]

    Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:02<00:01, 19.60it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:02<00:01, 24.12it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:02<00:01, 24.12it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:02<00:01, 24.12it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:02<00:01, 24.12it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:02<00:01, 24.12it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:02<00:01, 26.80it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:02<00:01, 26.80it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:02<00:01, 26.80it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:03<00:01, 26.80it/s]

    Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:03<00:01, 26.80it/s]Compiling num tokens (num_tokens=384):  48%|████▊     | 28/58 [00:03<00:01, 26.80it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:00, 30.57it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:00, 30.57it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:00, 30.57it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:00, 30.57it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:00, 30.57it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:00, 30.57it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 33.82it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 33.82it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 33.82it/s]

    Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 33.82it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 33.82it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:03<00:00, 35.19it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:03<00:00, 35.19it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:03<00:00, 35.19it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:03<00:00, 35.19it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:03<00:00, 35.19it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:03<00:00, 36.25it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:03<00:00, 36.25it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:03<00:00, 36.25it/s]

    Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:03<00:00, 36.25it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:03<00:00, 36.25it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 37.14it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 37.14it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 37.14it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 37.14it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 37.14it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 37.14it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:03<00:00, 39.61it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:03<00:00, 39.61it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:03<00:00, 39.61it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:03<00:00, 39.61it/s]

    Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.83it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=56.39 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=56.39 GB):   2%|▏         | 1/58 [00:00<00:09,  6.28it/s]Capturing num tokens (num_tokens=7680 avail_mem=56.36 GB):   2%|▏         | 1/58 [00:00<00:09,  6.28it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=56.36 GB):   3%|▎         | 2/58 [00:00<00:08,  6.53it/s]Capturing num tokens (num_tokens=7168 avail_mem=56.37 GB):   3%|▎         | 2/58 [00:00<00:08,  6.53it/s]Capturing num tokens (num_tokens=7168 avail_mem=56.37 GB):   5%|▌         | 3/58 [00:00<00:08,  6.70it/s]Capturing num tokens (num_tokens=6656 avail_mem=56.37 GB):   5%|▌         | 3/58 [00:00<00:08,  6.70it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=56.37 GB):   7%|▋         | 4/58 [00:00<00:07,  7.07it/s]Capturing num tokens (num_tokens=6144 avail_mem=56.37 GB):   7%|▋         | 4/58 [00:00<00:07,  7.07it/s]Capturing num tokens (num_tokens=6144 avail_mem=56.37 GB):   9%|▊         | 5/58 [00:00<00:07,  7.46it/s]Capturing num tokens (num_tokens=5632 avail_mem=56.37 GB):   9%|▊         | 5/58 [00:00<00:07,  7.46it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=56.37 GB):  10%|█         | 6/58 [00:00<00:06,  7.85it/s]Capturing num tokens (num_tokens=5120 avail_mem=56.37 GB):  10%|█         | 6/58 [00:00<00:06,  7.85it/s]Capturing num tokens (num_tokens=5120 avail_mem=56.37 GB):  12%|█▏        | 7/58 [00:00<00:06,  8.23it/s]Capturing num tokens (num_tokens=4608 avail_mem=56.37 GB):  12%|█▏        | 7/58 [00:00<00:06,  8.23it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=56.37 GB):  14%|█▍        | 8/58 [00:01<00:05,  8.35it/s]Capturing num tokens (num_tokens=4096 avail_mem=56.38 GB):  14%|█▍        | 8/58 [00:01<00:05,  8.35it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=56.38 GB):  16%|█▌        | 9/58 [00:01<00:08,  5.90it/s]Capturing num tokens (num_tokens=3840 avail_mem=56.38 GB):  16%|█▌        | 9/58 [00:01<00:08,  5.90it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=56.38 GB):  17%|█▋        | 10/58 [00:01<00:09,  4.81it/s]Capturing num tokens (num_tokens=3584 avail_mem=56.37 GB):  17%|█▋        | 10/58 [00:01<00:09,  4.81it/s]Capturing num tokens (num_tokens=3328 avail_mem=56.38 GB):  17%|█▋        | 10/58 [00:01<00:09,  4.81it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=56.38 GB):  21%|██        | 12/58 [00:01<00:07,  6.10it/s]Capturing num tokens (num_tokens=3072 avail_mem=56.38 GB):  21%|██        | 12/58 [00:01<00:07,  6.10it/s]Capturing num tokens (num_tokens=3072 avail_mem=56.38 GB):  22%|██▏       | 13/58 [00:01<00:07,  6.34it/s]Capturing num tokens (num_tokens=2816 avail_mem=56.38 GB):  22%|██▏       | 13/58 [00:01<00:07,  6.34it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=56.37 GB):  22%|██▏       | 13/58 [00:02<00:07,  6.34it/s]Capturing num tokens (num_tokens=2560 avail_mem=56.37 GB):  26%|██▌       | 15/58 [00:02<00:06,  7.09it/s]Capturing num tokens (num_tokens=2304 avail_mem=56.36 GB):  26%|██▌       | 15/58 [00:02<00:06,  7.09it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=56.36 GB):  28%|██▊       | 16/58 [00:02<00:06,  6.70it/s]Capturing num tokens (num_tokens=2048 avail_mem=56.35 GB):  28%|██▊       | 16/58 [00:02<00:06,  6.70it/s]Capturing num tokens (num_tokens=2048 avail_mem=56.35 GB):  29%|██▉       | 17/58 [00:02<00:05,  6.99it/s]Capturing num tokens (num_tokens=1792 avail_mem=56.35 GB):  29%|██▉       | 17/58 [00:02<00:05,  6.99it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=56.35 GB):  31%|███       | 18/58 [00:02<00:05,  7.43it/s]Capturing num tokens (num_tokens=1536 avail_mem=56.33 GB):  31%|███       | 18/58 [00:02<00:05,  7.43it/s]Capturing num tokens (num_tokens=1280 avail_mem=55.86 GB):  31%|███       | 18/58 [00:02<00:05,  7.43it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=55.86 GB):  34%|███▍      | 20/58 [00:02<00:05,  7.44it/s]Capturing num tokens (num_tokens=1024 avail_mem=55.74 GB):  34%|███▍      | 20/58 [00:02<00:05,  7.44it/s]Capturing num tokens (num_tokens=960 avail_mem=55.69 GB):  34%|███▍      | 20/58 [00:02<00:05,  7.44it/s] Capturing num tokens (num_tokens=960 avail_mem=55.69 GB):  38%|███▊      | 22/58 [00:03<00:04,  8.91it/s]Capturing num tokens (num_tokens=896 avail_mem=55.69 GB):  38%|███▊      | 22/58 [00:03<00:04,  8.91it/s]

    Capturing num tokens (num_tokens=832 avail_mem=55.69 GB):  38%|███▊      | 22/58 [00:03<00:04,  8.91it/s]Capturing num tokens (num_tokens=832 avail_mem=55.69 GB):  41%|████▏     | 24/58 [00:03<00:03,  8.91it/s]Capturing num tokens (num_tokens=768 avail_mem=55.69 GB):  41%|████▏     | 24/58 [00:03<00:03,  8.91it/s]Capturing num tokens (num_tokens=768 avail_mem=55.69 GB):  43%|████▎     | 25/58 [00:03<00:03,  8.92it/s]Capturing num tokens (num_tokens=704 avail_mem=55.68 GB):  43%|████▎     | 25/58 [00:03<00:03,  8.92it/s]

    Capturing num tokens (num_tokens=640 avail_mem=55.68 GB):  43%|████▎     | 25/58 [00:03<00:03,  8.92it/s]Capturing num tokens (num_tokens=640 avail_mem=55.68 GB):  47%|████▋     | 27/58 [00:03<00:03, 10.26it/s]Capturing num tokens (num_tokens=576 avail_mem=55.68 GB):  47%|████▋     | 27/58 [00:03<00:03, 10.26it/s]Capturing num tokens (num_tokens=512 avail_mem=55.66 GB):  47%|████▋     | 27/58 [00:03<00:03, 10.26it/s]Capturing num tokens (num_tokens=512 avail_mem=55.66 GB):  50%|█████     | 29/58 [00:03<00:02, 11.70it/s]Capturing num tokens (num_tokens=480 avail_mem=55.67 GB):  50%|█████     | 29/58 [00:03<00:02, 11.70it/s]

    Capturing num tokens (num_tokens=448 avail_mem=55.67 GB):  50%|█████     | 29/58 [00:03<00:02, 11.70it/s]Capturing num tokens (num_tokens=448 avail_mem=55.67 GB):  53%|█████▎    | 31/58 [00:03<00:02, 12.88it/s]Capturing num tokens (num_tokens=416 avail_mem=55.67 GB):  53%|█████▎    | 31/58 [00:03<00:02, 12.88it/s]Capturing num tokens (num_tokens=384 avail_mem=55.67 GB):  53%|█████▎    | 31/58 [00:03<00:02, 12.88it/s]Capturing num tokens (num_tokens=384 avail_mem=55.67 GB):  57%|█████▋    | 33/58 [00:03<00:01, 13.95it/s]Capturing num tokens (num_tokens=352 avail_mem=55.66 GB):  57%|█████▋    | 33/58 [00:03<00:01, 13.95it/s]

    Capturing num tokens (num_tokens=320 avail_mem=55.66 GB):  57%|█████▋    | 33/58 [00:03<00:01, 13.95it/s]Capturing num tokens (num_tokens=320 avail_mem=55.66 GB):  60%|██████    | 35/58 [00:04<00:01, 14.75it/s]Capturing num tokens (num_tokens=288 avail_mem=55.66 GB):  60%|██████    | 35/58 [00:04<00:01, 14.75it/s]Capturing num tokens (num_tokens=256 avail_mem=55.66 GB):  60%|██████    | 35/58 [00:04<00:01, 14.75it/s]Capturing num tokens (num_tokens=256 avail_mem=55.66 GB):  64%|██████▍   | 37/58 [00:04<00:01, 15.61it/s]Capturing num tokens (num_tokens=240 avail_mem=55.67 GB):  64%|██████▍   | 37/58 [00:04<00:01, 15.61it/s]

    Capturing num tokens (num_tokens=224 avail_mem=55.67 GB):  64%|██████▍   | 37/58 [00:04<00:01, 15.61it/s]Capturing num tokens (num_tokens=224 avail_mem=55.67 GB):  67%|██████▋   | 39/58 [00:04<00:01, 16.13it/s]Capturing num tokens (num_tokens=208 avail_mem=55.67 GB):  67%|██████▋   | 39/58 [00:04<00:01, 16.13it/s]Capturing num tokens (num_tokens=192 avail_mem=55.67 GB):  67%|██████▋   | 39/58 [00:04<00:01, 16.13it/s]Capturing num tokens (num_tokens=192 avail_mem=55.67 GB):  71%|███████   | 41/58 [00:04<00:01, 16.41it/s]Capturing num tokens (num_tokens=176 avail_mem=55.66 GB):  71%|███████   | 41/58 [00:04<00:01, 16.41it/s]

    Capturing num tokens (num_tokens=160 avail_mem=55.66 GB):  71%|███████   | 41/58 [00:04<00:01, 16.41it/s]Capturing num tokens (num_tokens=160 avail_mem=55.66 GB):  74%|███████▍  | 43/58 [00:04<00:00, 16.64it/s]Capturing num tokens (num_tokens=144 avail_mem=55.66 GB):  74%|███████▍  | 43/58 [00:04<00:00, 16.64it/s]Capturing num tokens (num_tokens=128 avail_mem=55.66 GB):  74%|███████▍  | 43/58 [00:04<00:00, 16.64it/s]

    Capturing num tokens (num_tokens=128 avail_mem=55.66 GB):  78%|███████▊  | 45/58 [00:04<00:00, 15.30it/s]Capturing num tokens (num_tokens=112 avail_mem=55.65 GB):  78%|███████▊  | 45/58 [00:04<00:00, 15.30it/s]Capturing num tokens (num_tokens=96 avail_mem=55.65 GB):  78%|███████▊  | 45/58 [00:04<00:00, 15.30it/s] Capturing num tokens (num_tokens=96 avail_mem=55.65 GB):  81%|████████  | 47/58 [00:04<00:00, 15.67it/s]Capturing num tokens (num_tokens=80 avail_mem=55.65 GB):  81%|████████  | 47/58 [00:04<00:00, 15.67it/s]Capturing num tokens (num_tokens=64 avail_mem=55.65 GB):  81%|████████  | 47/58 [00:04<00:00, 15.67it/s]Capturing num tokens (num_tokens=48 avail_mem=55.65 GB):  81%|████████  | 47/58 [00:04<00:00, 15.67it/s]

    Capturing num tokens (num_tokens=32 avail_mem=55.64 GB):  81%|████████  | 47/58 [00:04<00:00, 15.67it/s]Capturing num tokens (num_tokens=32 avail_mem=55.64 GB):  88%|████████▊ | 51/58 [00:04<00:00, 20.53it/s]Capturing num tokens (num_tokens=28 avail_mem=55.64 GB):  88%|████████▊ | 51/58 [00:04<00:00, 20.53it/s]Capturing num tokens (num_tokens=24 avail_mem=54.58 GB):  88%|████████▊ | 51/58 [00:04<00:00, 20.53it/s]Capturing num tokens (num_tokens=20 avail_mem=50.46 GB):  88%|████████▊ | 51/58 [00:04<00:00, 20.53it/s]

    Capturing num tokens (num_tokens=20 avail_mem=50.46 GB):  93%|█████████▎| 54/58 [00:05<00:00, 19.44it/s]Capturing num tokens (num_tokens=16 avail_mem=40.62 GB):  93%|█████████▎| 54/58 [00:05<00:00, 19.44it/s]Capturing num tokens (num_tokens=12 avail_mem=40.61 GB):  93%|█████████▎| 54/58 [00:05<00:00, 19.44it/s]Capturing num tokens (num_tokens=12 avail_mem=40.61 GB):  97%|█████████▋| 56/58 [00:05<00:00, 18.85it/s]Capturing num tokens (num_tokens=8 avail_mem=40.61 GB):  97%|█████████▋| 56/58 [00:05<00:00, 18.85it/s] Capturing num tokens (num_tokens=4 avail_mem=40.61 GB):  97%|█████████▋| 56/58 [00:05<00:00, 18.85it/s]

    Capturing num tokens (num_tokens=4 avail_mem=40.61 GB): 100%|██████████| 58/58 [00:05<00:00, 11.06it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>



<strong style='color: #00008B;'>Non-stream response:</strong>



<strong style='color: #00008B;'>ChatCompletion(id='ee0801f88da546789bf31ba44667050b', choices=[Choice(finish_reason='tool_calls', index=0, logprobs=None, message=ChatCompletionMessage(content=None, refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=[ChatCompletionMessageFunctionToolCall(id='call_af8b8ae2e46b4f499cc2c800', function=Function(arguments='{"location": "Tokyo"}', name='get_weather'), type='function', index=0), ChatCompletionMessageFunctionToolCall(id='call_17be3200ec0e45709544994f', function=Function(arguments='{"city": "Tokyo"}', name='get_tourist_attractions'), type='function', index=1)], reasoning_content=None), matched_stop=None)], created=1775291315, model='meta-llama/Llama-3.2-1B-Instruct', object='chat.completion', service_tier=None, system_fingerprint=None, usage=CompletionUsage(completion_tokens=20, prompt_tokens=435, total_tokens=455, completion_tokens_details=None, prompt_tokens_details=None, reasoning_tokens=0), metadata={'weight_version': 'default'})</strong>



<strong style='color: #00008B;'>Streaming Response:</strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'></strong>



<strong style='color: #00008B;'>==== Tool Call ====</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=0, id='call_4022d85623d04415838acb14', function=ChoiceDeltaToolCallFunction(arguments='{"location": "Tokyo"}', name='get_weather'), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=1, id='call_f0a98e966f7043b4835fddfb', function=ChoiceDeltaToolCallFunction(arguments='{"city": "Tokyo"}', name='get_tourist_attractions'), type='function')</strong>


> **Note:**  
> The model may still default to JSON if it was heavily finetuned on that format. Prompt engineering (including examples) is the only way to increase the chance of pythonic output if you are not using a chat template.

## How to support a new model?
1. Update the TOOLS_TAG_LIST in sglang/srt/function_call_parser.py with the model’s tool tags. Currently supported tags include:
```
	TOOLS_TAG_LIST = [
	    “<|plugin|>“,
	    “<function=“,
	    “<tool_call>“,
	    “<|python_tag|>“,
	    “[TOOL_CALLS]”
	]
```
2. Create a new detector class in sglang/srt/function_call_parser.py that inherits from BaseFormatDetector. The detector should handle the model’s specific function call format. For example:
```
    class NewModelDetector(BaseFormatDetector):
```
3. Add the new detector to the MultiFormatParser class that manages all the format detectors.
