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

    [2026-03-25 11:08:11] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.


    [2026-03-25 11:08:11] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.


    [2026-03-25 11:08:11] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-25 11:08:15] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-25 11:08:15] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-25 11:08:15] INFO utils.py:164: NumExpr defaulting to 16 threads.
    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(
    [2026-03-25 11:08:16] WARNING server_args.py:908: The tool_call_parser 'qwen25' is deprecated. Please use 'qwen' instead.


    [2026-03-25 11:08:17] INFO server_args.py:2246: Attention backend not specified. Use fa3 backend by default.
    [2026-03-25 11:08:17] INFO server_args.py:3544: Set soft_watchdog_timeout since in CI


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [2026-03-25 11:08:23] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-25 11:08:23] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-25 11:08:23] INFO utils.py:164: NumExpr defaulting to 16 threads.
    [2026-03-25 11:08:23] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-25 11:08:23] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-25 11:08:23] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-03-25 11:08:27.539 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:08:27] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 11:08:27.539 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:08:27] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 11:08:27.539 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:08:27] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 11:08:27.539 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:08:27] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 11:08:27.539 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:08:27] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  25% Completed | 1/4 [00:00<00:01,  1.96it/s]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:01<00:01,  1.56it/s]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:01<00:00,  1.51it/s]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.46it/s]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.51it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:00,  3.16s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:00,  3.16s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:40,  1.79s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:40,  1.79s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:07,  1.22s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:07,  1.22s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:49,  1.09it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:49,  1.09it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:38,  1.39it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:38,  1.39it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:30,  1.69it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:30,  1.69it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:25,  2.03it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:25,  2.03it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:21,  2.37it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:21,  2.37it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:17,  2.76it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:17,  2.76it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:15,  3.17it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:15,  3.17it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:12,  3.62it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:12,  3.62it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:07<00:11,  4.09it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:07<00:11,  4.09it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:10,  4.38it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:10,  4.38it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:09,  4.63it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:09,  4.63it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:08,  5.00it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:08,  5.00it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:07<00:07,  5.53it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:07<00:07,  5.53it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:07<00:06,  5.99it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:07<00:06,  5.99it/s]

    Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:07<00:06,  6.52it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:07<00:06,  6.52it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:08<00:06,  6.52it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:08<00:04,  7.81it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:08<00:04,  7.81it/s]

    Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:08<00:04,  7.81it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:08<00:03,  9.49it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:08<00:03,  9.49it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:08<00:03,  9.49it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:08<00:03, 10.92it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:08<00:03, 10.92it/s]

    Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:08<00:03, 10.92it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:08<00:02, 12.49it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:08<00:02, 12.49it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:08<00:02, 12.49it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:08<00:02, 13.82it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:08<00:02, 13.82it/s]

    Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:08<00:02, 13.82it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:08<00:02, 13.82it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:08<00:01, 16.26it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:08<00:01, 16.26it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:08<00:01, 16.26it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:08<00:01, 16.26it/s]

    Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:08<00:01, 18.27it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:08<00:01, 18.27it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:09<00:01, 18.27it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:09<00:01, 18.27it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:09<00:01, 19.75it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:09<00:01, 19.75it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:09<00:01, 19.75it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:09<00:01, 19.75it/s]

    Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:09<00:00, 21.56it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:09<00:00, 21.56it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:09<00:00, 21.56it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:09<00:00, 21.56it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:09<00:00, 23.04it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:09<00:00, 23.04it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:09<00:00, 23.04it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:09<00:00, 23.04it/s]

    Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:09<00:00, 24.84it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:09<00:00, 24.84it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:09<00:00, 24.84it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:09<00:00, 24.84it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:09<00:00, 26.10it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:09<00:00, 26.10it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:09<00:00, 26.10it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:09<00:00, 26.10it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:09<00:00, 26.10it/s]

    Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:09<00:00, 28.40it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:09<00:00, 28.40it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:09<00:00, 28.40it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:09<00:00, 28.40it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:09<00:00, 28.40it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:09<00:00, 28.40it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00, 32.49it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00,  5.94it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=101.48 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=101.48 GB):   2%|▏         | 1/58 [00:00<00:37,  1.53it/s]Capturing num tokens (num_tokens=7680 avail_mem=101.43 GB):   2%|▏         | 1/58 [00:00<00:37,  1.53it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=101.43 GB):   3%|▎         | 2/58 [00:01<00:33,  1.66it/s]Capturing num tokens (num_tokens=7168 avail_mem=101.44 GB):   3%|▎         | 2/58 [00:01<00:33,  1.66it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=101.44 GB):   5%|▌         | 3/58 [00:01<00:30,  1.78it/s]Capturing num tokens (num_tokens=6656 avail_mem=101.43 GB):   5%|▌         | 3/58 [00:01<00:30,  1.78it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=101.43 GB):   7%|▋         | 4/58 [00:02<00:28,  1.91it/s]Capturing num tokens (num_tokens=6144 avail_mem=101.42 GB):   7%|▋         | 4/58 [00:02<00:28,  1.91it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=101.42 GB):   9%|▊         | 5/58 [00:02<00:26,  2.02it/s]Capturing num tokens (num_tokens=5632 avail_mem=101.42 GB):   9%|▊         | 5/58 [00:02<00:26,  2.02it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=101.42 GB):  10%|█         | 6/58 [00:03<00:23,  2.19it/s]Capturing num tokens (num_tokens=5120 avail_mem=101.41 GB):  10%|█         | 6/58 [00:03<00:23,  2.19it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=101.41 GB):  12%|█▏        | 7/58 [00:03<00:21,  2.38it/s]Capturing num tokens (num_tokens=4608 avail_mem=101.41 GB):  12%|█▏        | 7/58 [00:03<00:21,  2.38it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=101.41 GB):  14%|█▍        | 8/58 [00:03<00:18,  2.64it/s]Capturing num tokens (num_tokens=4096 avail_mem=101.42 GB):  14%|█▍        | 8/58 [00:03<00:18,  2.64it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=101.42 GB):  16%|█▌        | 9/58 [00:03<00:16,  2.90it/s]Capturing num tokens (num_tokens=3840 avail_mem=101.42 GB):  16%|█▌        | 9/58 [00:03<00:16,  2.90it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=101.42 GB):  17%|█▋        | 10/58 [00:04<00:16,  2.99it/s]Capturing num tokens (num_tokens=3584 avail_mem=101.42 GB):  17%|█▋        | 10/58 [00:04<00:16,  2.99it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=101.42 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.29it/s]Capturing num tokens (num_tokens=3328 avail_mem=101.42 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.29it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=101.42 GB):  21%|██        | 12/58 [00:04<00:12,  3.62it/s]Capturing num tokens (num_tokens=3072 avail_mem=101.42 GB):  21%|██        | 12/58 [00:04<00:12,  3.62it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=101.42 GB):  22%|██▏       | 13/58 [00:04<00:11,  3.93it/s]Capturing num tokens (num_tokens=2816 avail_mem=101.42 GB):  22%|██▏       | 13/58 [00:04<00:11,  3.93it/s]Capturing num tokens (num_tokens=2816 avail_mem=101.42 GB):  24%|██▍       | 14/58 [00:05<00:10,  4.33it/s]Capturing num tokens (num_tokens=2560 avail_mem=101.42 GB):  24%|██▍       | 14/58 [00:05<00:10,  4.33it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=101.42 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.73it/s]Capturing num tokens (num_tokens=2304 avail_mem=101.42 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.73it/s]Capturing num tokens (num_tokens=2304 avail_mem=101.42 GB):  28%|██▊       | 16/58 [00:05<00:08,  5.16it/s]Capturing num tokens (num_tokens=2048 avail_mem=101.42 GB):  28%|██▊       | 16/58 [00:05<00:08,  5.16it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=101.42 GB):  29%|██▉       | 17/58 [00:05<00:07,  5.69it/s]Capturing num tokens (num_tokens=1792 avail_mem=101.42 GB):  29%|██▉       | 17/58 [00:05<00:07,  5.69it/s]Capturing num tokens (num_tokens=1792 avail_mem=101.42 GB):  31%|███       | 18/58 [00:05<00:06,  6.23it/s]Capturing num tokens (num_tokens=1536 avail_mem=101.41 GB):  31%|███       | 18/58 [00:05<00:06,  6.23it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=101.41 GB):  33%|███▎      | 19/58 [00:05<00:05,  6.90it/s]Capturing num tokens (num_tokens=1280 avail_mem=101.41 GB):  33%|███▎      | 19/58 [00:05<00:05,  6.90it/s]Capturing num tokens (num_tokens=1024 avail_mem=101.41 GB):  33%|███▎      | 19/58 [00:05<00:05,  6.90it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=101.41 GB):  36%|███▌      | 21/58 [00:06<00:05,  7.28it/s]Capturing num tokens (num_tokens=960 avail_mem=101.38 GB):  36%|███▌      | 21/58 [00:06<00:05,  7.28it/s] Capturing num tokens (num_tokens=896 avail_mem=101.37 GB):  36%|███▌      | 21/58 [00:06<00:05,  7.28it/s]Capturing num tokens (num_tokens=896 avail_mem=101.37 GB):  40%|███▉      | 23/58 [00:06<00:04,  8.49it/s]Capturing num tokens (num_tokens=832 avail_mem=101.37 GB):  40%|███▉      | 23/58 [00:06<00:04,  8.49it/s]

    Capturing num tokens (num_tokens=768 avail_mem=101.36 GB):  40%|███▉      | 23/58 [00:06<00:04,  8.49it/s]Capturing num tokens (num_tokens=768 avail_mem=101.36 GB):  43%|████▎     | 25/58 [00:06<00:03,  9.87it/s]Capturing num tokens (num_tokens=704 avail_mem=101.36 GB):  43%|████▎     | 25/58 [00:06<00:03,  9.87it/s]Capturing num tokens (num_tokens=640 avail_mem=101.36 GB):  43%|████▎     | 25/58 [00:06<00:03,  9.87it/s]Capturing num tokens (num_tokens=640 avail_mem=101.36 GB):  47%|████▋     | 27/58 [00:06<00:02, 11.30it/s]Capturing num tokens (num_tokens=576 avail_mem=101.35 GB):  47%|████▋     | 27/58 [00:06<00:02, 11.30it/s]

    Capturing num tokens (num_tokens=512 avail_mem=101.35 GB):  47%|████▋     | 27/58 [00:06<00:02, 11.30it/s]Capturing num tokens (num_tokens=512 avail_mem=101.35 GB):  50%|█████     | 29/58 [00:06<00:02, 12.69it/s]Capturing num tokens (num_tokens=480 avail_mem=101.35 GB):  50%|█████     | 29/58 [00:06<00:02, 12.69it/s]Capturing num tokens (num_tokens=448 avail_mem=101.34 GB):  50%|█████     | 29/58 [00:06<00:02, 12.69it/s]Capturing num tokens (num_tokens=448 avail_mem=101.34 GB):  53%|█████▎    | 31/58 [00:06<00:01, 14.09it/s]Capturing num tokens (num_tokens=416 avail_mem=101.34 GB):  53%|█████▎    | 31/58 [00:06<00:01, 14.09it/s]

    Capturing num tokens (num_tokens=384 avail_mem=101.34 GB):  53%|█████▎    | 31/58 [00:06<00:01, 14.09it/s]Capturing num tokens (num_tokens=384 avail_mem=101.34 GB):  57%|█████▋    | 33/58 [00:06<00:01, 15.38it/s]Capturing num tokens (num_tokens=352 avail_mem=101.33 GB):  57%|█████▋    | 33/58 [00:06<00:01, 15.38it/s]Capturing num tokens (num_tokens=320 avail_mem=101.33 GB):  57%|█████▋    | 33/58 [00:06<00:01, 15.38it/s]Capturing num tokens (num_tokens=288 avail_mem=101.32 GB):  57%|█████▋    | 33/58 [00:06<00:01, 15.38it/s]Capturing num tokens (num_tokens=288 avail_mem=101.32 GB):  62%|██████▏   | 36/58 [00:06<00:01, 17.21it/s]Capturing num tokens (num_tokens=256 avail_mem=101.32 GB):  62%|██████▏   | 36/58 [00:06<00:01, 17.21it/s]

    Capturing num tokens (num_tokens=240 avail_mem=101.31 GB):  62%|██████▏   | 36/58 [00:06<00:01, 17.21it/s]Capturing num tokens (num_tokens=224 avail_mem=101.31 GB):  62%|██████▏   | 36/58 [00:07<00:01, 17.21it/s]Capturing num tokens (num_tokens=224 avail_mem=101.31 GB):  67%|██████▋   | 39/58 [00:07<00:01, 19.00it/s]Capturing num tokens (num_tokens=208 avail_mem=101.31 GB):  67%|██████▋   | 39/58 [00:07<00:01, 19.00it/s]Capturing num tokens (num_tokens=192 avail_mem=101.30 GB):  67%|██████▋   | 39/58 [00:07<00:01, 19.00it/s]Capturing num tokens (num_tokens=176 avail_mem=101.30 GB):  67%|██████▋   | 39/58 [00:07<00:01, 19.00it/s]

    Capturing num tokens (num_tokens=176 avail_mem=101.30 GB):  72%|███████▏  | 42/58 [00:07<00:00, 20.54it/s]Capturing num tokens (num_tokens=160 avail_mem=101.30 GB):  72%|███████▏  | 42/58 [00:07<00:00, 20.54it/s]Capturing num tokens (num_tokens=144 avail_mem=101.29 GB):  72%|███████▏  | 42/58 [00:07<00:00, 20.54it/s]Capturing num tokens (num_tokens=128 avail_mem=101.30 GB):  72%|███████▏  | 42/58 [00:07<00:00, 20.54it/s]Capturing num tokens (num_tokens=128 avail_mem=101.30 GB):  78%|███████▊  | 45/58 [00:07<00:00, 21.66it/s]Capturing num tokens (num_tokens=112 avail_mem=101.30 GB):  78%|███████▊  | 45/58 [00:07<00:00, 21.66it/s]Capturing num tokens (num_tokens=96 avail_mem=101.29 GB):  78%|███████▊  | 45/58 [00:07<00:00, 21.66it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=101.29 GB):  78%|███████▊  | 45/58 [00:07<00:00, 21.66it/s]Capturing num tokens (num_tokens=80 avail_mem=101.29 GB):  83%|████████▎ | 48/58 [00:07<00:00, 22.70it/s]Capturing num tokens (num_tokens=64 avail_mem=101.29 GB):  83%|████████▎ | 48/58 [00:07<00:00, 22.70it/s]Capturing num tokens (num_tokens=48 avail_mem=101.28 GB):  83%|████████▎ | 48/58 [00:07<00:00, 22.70it/s]Capturing num tokens (num_tokens=32 avail_mem=101.28 GB):  83%|████████▎ | 48/58 [00:07<00:00, 22.70it/s]Capturing num tokens (num_tokens=32 avail_mem=101.28 GB):  88%|████████▊ | 51/58 [00:07<00:00, 23.57it/s]Capturing num tokens (num_tokens=28 avail_mem=101.28 GB):  88%|████████▊ | 51/58 [00:07<00:00, 23.57it/s]Capturing num tokens (num_tokens=24 avail_mem=101.27 GB):  88%|████████▊ | 51/58 [00:07<00:00, 23.57it/s]

    Capturing num tokens (num_tokens=20 avail_mem=101.27 GB):  88%|████████▊ | 51/58 [00:07<00:00, 23.57it/s]Capturing num tokens (num_tokens=20 avail_mem=101.27 GB):  93%|█████████▎| 54/58 [00:07<00:00, 24.27it/s]Capturing num tokens (num_tokens=16 avail_mem=101.27 GB):  93%|█████████▎| 54/58 [00:07<00:00, 24.27it/s]Capturing num tokens (num_tokens=12 avail_mem=101.26 GB):  93%|█████████▎| 54/58 [00:07<00:00, 24.27it/s]Capturing num tokens (num_tokens=8 avail_mem=101.26 GB):  93%|█████████▎| 54/58 [00:07<00:00, 24.27it/s] Capturing num tokens (num_tokens=8 avail_mem=101.26 GB):  98%|█████████▊| 57/58 [00:07<00:00, 24.67it/s]Capturing num tokens (num_tokens=4 avail_mem=101.26 GB):  98%|█████████▊| 57/58 [00:07<00:00, 24.67it/s]Capturing num tokens (num_tokens=4 avail_mem=101.26 GB): 100%|██████████| 58/58 [00:07<00:00,  7.42it/s]


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



<strong style='color: #00008B;'>ChatCompletion(id='466c9fcc34344d26b6e3567d3e58c312', choices=[Choice(finish_reason='tool_calls', index=0, logprobs=None, message=ChatCompletionMessage(content="To determine the current weather in Boston, I will use the `get_current_weather` function by providing the city name, state abbreviation, and the temperature unit in which you prefer the information. Since the user did not specify the unit, I will assume Celsius for the response.\n\nReasoning: The `get_current_weather` function requires the city name, state abbreviation, and the temperature unit. Boston is the city, and Massachusetts' state abbreviation is MA. I will use Celsius as the default unit for the temperature.", refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=[ChatCompletionMessageFunctionToolCall(id='call_deff2bf0757f47f194ac4a24', function=Function(arguments='{"city": "Boston", "state": "MA", "unit": "celsius"}', name='get_current_weather'), type='function', index=0)], reasoning_content=None), matched_stop=None)], created=1774436939, model='Qwen/Qwen2.5-7B-Instruct', object='chat.completion', service_tier=None, system_fingerprint=None, usage=CompletionUsage(completion_tokens=137, prompt_tokens=290, total_tokens=427, completion_tokens_details=None, prompt_tokens_details=None, reasoning_tokens=0), metadata={'weight_version': 'default'})</strong>



<strong style='color: #00008B;'>==== content ====</strong>



<strong style='color: #00008B;'>To determine the current weather in Boston, I will use the `get_current_weather` function by providing the city name, state abbreviation, and the temperature unit in which you prefer the information. Since the user did not specify the unit, I will assume Celsius for the response.<br><br>Reasoning: The `get_current_weather` function requires the city name, state abbreviation, and the temperature unit. Boston is the city, and Massachusetts' state abbreviation is MA. I will use Celsius as the default unit for the temperature.</strong>



<strong style='color: #00008B;'>==== tool_calls ====</strong>



<strong style='color: #00008B;'>[ChatCompletionMessageFunctionToolCall(id='call_deff2bf0757f47f194ac4a24', function=Function(arguments='{"city": "Boston", "state": "MA", "unit": "celsius"}', name='get_current_weather'), type='function', index=0)]</strong>


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



<strong style='color: #00008B;'>To determine the current weather in Boston, I will use the `get_current_weather` function by providing the city name, state abbreviation, and the temperature unit in which you prefer the information to be presented. Since Boston is in Massachusetts, the state abbreviation is 'MA'. For the temperature unit, I will use Fahrenheit as it is commonly used in the United States.<br><br>Now, I will call the function with these details.<br></strong>



<strong style='color: #00008B;'>==== Tool Call ====</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=0, id='call_c5f0496c8fba4f15b05cd91f', function=ChoiceDeltaToolCallFunction(arguments='', name='get_current_weather'), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=0, id=None, function=ChoiceDeltaToolCallFunction(arguments='{"city": "', name=None), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=0, id=None, function=ChoiceDeltaToolCallFunction(arguments='Boston"', name=None), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=0, id=None, function=ChoiceDeltaToolCallFunction(arguments=', "state": "', name=None), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=0, id=None, function=ChoiceDeltaToolCallFunction(arguments='MA"', name=None), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=0, id=None, function=ChoiceDeltaToolCallFunction(arguments=', "unit": "', name=None), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=0, id=None, function=ChoiceDeltaToolCallFunction(arguments='f', name=None), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=0, id=None, function=ChoiceDeltaToolCallFunction(arguments='ahrenheit"}', name=None), type='function')</strong>


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



<strong style='color: #00008B;'>streamed function call arguments: {"city": "Boston", "state": "MA", "unit": "fahrenheit"}</strong>


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



<strong style='color: #00008B;'>Updated message history: [{'role': 'user', 'content': "What's the weather like in Boston today? Output a reasoning before act, then use the tools to help you."}, ChatCompletionMessage(content="To determine the current weather in Boston, I will use the `get_current_weather` function by providing the city name, state abbreviation, and the temperature unit in which you prefer the information. Since the user did not specify the unit, I will assume Celsius for the response.\n\nReasoning: The `get_current_weather` function requires the city name, state abbreviation, and the temperature unit. Boston is the city, and Massachusetts' state abbreviation is MA. I will use Celsius as the default unit for the temperature.", refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=[ChatCompletionMessageFunctionToolCall(id='call_deff2bf0757f47f194ac4a24', function=Function(arguments='{"city": "Boston", "state": "MA", "unit": "celsius"}', name='get_current_weather'), type='function', index=0)], reasoning_content=None), {'role': 'tool', 'tool_call_id': 'call_deff2bf0757f47f194ac4a24', 'content': "The weather in Boston, MA is 85 degrees celsius. It is partly cloudly, with highs in the 90's.", 'name': 'get_current_weather'}]</strong>


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



<strong style='color: #00008B;'>ChatCompletion(id='d6efab2c5c094e1b9994a3574aa2155c', choices=[Choice(finish_reason='tool_calls', index=0, logprobs=None, message=ChatCompletionMessage(content='There seems to be an error in the response as the temperature in Celsius for Boston should not exceed 35-40 degrees, given the typical climate. Let me correct this by rechecking with the function using the correct unit, which is likely Fahrenheit.\n\nReasoning: The response indicates an unusually high temperature in Celsius, which is not plausible for Boston. I will recheck the weather using the correct unit, which is likely Fahrenheit.', refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=[ChatCompletionMessageFunctionToolCall(id='call_76d237d5c54f4deea933f7d2', function=Function(arguments='{"city": "Boston", "state": "MA", "unit": "fahrenheit"}', name='get_current_weather'), type='function', index=0)], reasoning_content=None), matched_stop=None)], created=1774436942, model='Qwen/Qwen2.5-7B-Instruct', object='chat.completion', service_tier=None, system_fingerprint=None, usage=CompletionUsage(completion_tokens=122, prompt_tokens=473, total_tokens=595, completion_tokens_details=None, prompt_tokens_details=None, reasoning_tokens=0), metadata={'weight_version': 'default'})</strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>There seems to be an error in the response as the temperature in Celsius for Boston should not exceed 35-40 degrees, given the typical climate. Let me correct this by rechecking with the function using the correct unit, which is likely Fahrenheit.<br><br>Reasoning: The response indicates an unusually high temperature in Celsius, which is not plausible for Boston. I will recheck the weather using the correct unit, which is likely Fahrenheit.</strong>


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



<strong style='color: #00008B;'>To provide you with the current weather in Boston, I will use the `get_current_weather` function. This function requires the city name, state abbreviation, and the temperature unit you prefer. For Boston, the state is Massachusetts, which has the abbreviation 'MA'. You didn't specify a unit, so I'll provide the temperature in both Celsius and Fahrenheit for your convenience.<br><br>Let's proceed with fetching the weather data.<br><tool_call><br>{"name": "get_current_weather", "arguments": {"city": "Boston", "state": "MA", "unit": "celsius"}}<br></tool_call><br><tool_call><br>{"name": "get_current_weather", "arguments": {"city": "Boston", "state": "MA", "unit": "fahrenheit"}}<br></tool_call></strong>


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)



<strong style='color: #00008B;'>==== Text ====</strong>


    To provide you with the current weather in Boston, I will use the `get_current_weather` function. This function requires the city name, state abbreviation, and the temperature unit you prefer. For Boston, the state is Massachusetts, which has the abbreviation 'MA'. You didn't specify a unit, so I'll provide the temperature in both Celsius and Fahrenheit for your convenience.
    
    Let's proceed with fetching the weather data.



<strong style='color: #00008B;'>==== Calls ====</strong>


    function name:  get_current_weather
    function arguments:  {"city": "Boston", "state": "MA", "unit": "celsius"}



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

    [2026-03-25 11:09:09] INFO server_args.py:2246: Attention backend not specified. Use fa3 backend by default.


    [2026-03-25 11:09:09] INFO server_args.py:3544: Set soft_watchdog_timeout since in CI


    [2026-03-25 11:09:09] INFO engine.py:177: server_args=ServerArgs(model_path='Qwen/Qwen2.5-7B-Instruct', tokenizer_path='Qwen/Qwen2.5-7B-Instruct', tokenizer_mode='auto', tokenizer_worker_num=1, skip_tokenizer_init=False, load_format='auto', model_loader_extra_config='{}', trust_remote_code=False, context_length=None, is_embedding=False, enable_multimodal=None, revision=None, model_impl='auto', host='127.0.0.1', port=30000, fastapi_root_path='', grpc_mode=False, skip_server_warmup=False, warmups=None, nccl_port=None, checkpoint_engine_wait_weights_before_ready=False, ssl_keyfile=None, ssl_certfile=None, ssl_ca_certs=None, ssl_keyfile_password=None, enable_ssl_refresh=False, dtype='auto', quantization=None, quantization_param_path=None, kv_cache_dtype='auto', enable_fp32_lm_head=False, modelopt_quant=None, modelopt_checkpoint_restore_path=None, modelopt_checkpoint_save_path=None, modelopt_export_path=None, quantize_and_serve=False, rl_quant_profile=None, mem_fraction_static=0.903, max_running_requests=128, max_queued_requests=None, max_total_tokens=20480, chunked_prefill_size=8192, enable_dynamic_chunking=False, max_prefill_tokens=16384, prefill_max_requests=None, schedule_policy='fcfs', enable_priority_scheduling=False, disable_priority_preemption=False, default_priority_value=None, abort_on_priority_when_disabled=False, schedule_low_priority_values_first=False, priority_scheduling_preemption_threshold=10, schedule_conservativeness=1.0, page_size=1, swa_full_tokens_ratio=0.8, disable_hybrid_swa_memory=False, radix_eviction_policy='lru', enable_prefill_delayer=False, prefill_delayer_max_delay_passes=30, prefill_delayer_token_usage_low_watermark=None, prefill_delayer_forward_passes_buckets=None, prefill_delayer_wait_seconds_buckets=None, device='cuda', tp_size=1, pp_size=1, pp_max_micro_batch_size=None, pp_async_batch_depth=0, stream_interval=1, incremental_streaming_output=False, enable_streaming_session=False, random_seed=207593417, constrained_json_whitespace_pattern=None, constrained_json_disable_any_whitespace=False, watchdog_timeout=300, soft_watchdog_timeout=300, dist_timeout=None, download_dir=None, model_checksum=None, base_gpu_id=0, gpu_id_step=1, sleep_on_idle=False, use_ray=False, custom_sigquit_handler=None, log_level='error', log_level_http=None, log_requests=False, log_requests_level=2, log_requests_format='text', log_requests_target=None, uvicorn_access_log_exclude_prefixes=[], crash_dump_folder=None, show_time_cost=False, enable_metrics=False, enable_metrics_for_all_schedulers=False, tokenizer_metrics_custom_labels_header='x-custom-labels', tokenizer_metrics_allowed_custom_labels=None, extra_metric_labels=None, bucket_time_to_first_token=None, bucket_inter_token_latency=None, bucket_e2e_request_latency=None, collect_tokens_histogram=False, prompt_tokens_buckets=None, generation_tokens_buckets=None, gc_warning_threshold_secs=0.0, decode_log_interval=40, enable_request_time_stats_logging=False, kv_events_config=None, enable_trace=False, otlp_traces_endpoint='localhost:4317', export_metrics_to_file=False, export_metrics_to_file_dir=None, api_key=None, admin_api_key=None, served_model_name='Qwen/Qwen2.5-7B-Instruct', weight_version='default', chat_template=None, hf_chat_template_name=None, completion_template=None, file_storage_path='sglang_storage', enable_cache_report=False, reasoning_parser=None, tool_call_parser=None, tool_server=None, sampling_defaults='model', dp_size=1, load_balance_method='round_robin', attn_cp_size=1, moe_dp_size=1, dist_init_addr=None, nnodes=1, node_rank=0, json_model_override_args='{}', preferred_sampling_params=None, enable_lora=None, enable_lora_overlap_loading=None, max_lora_rank=None, lora_target_modules=None, lora_paths=None, max_loaded_loras=None, max_loras_per_batch=8, lora_eviction_policy='lru', lora_backend='csgmv', max_lora_chunk_size=16, attention_backend='fa3', decode_attention_backend=None, prefill_attention_backend=None, sampling_backend='flashinfer', grammar_backend='xgrammar', mm_attention_backend=None, fp8_gemm_runner_backend='auto', fp4_gemm_runner_backend='auto', nsa_prefill_backend=None, nsa_decode_backend=None, disable_flashinfer_autotune=False, mamba_backend='triton', speculative_algorithm=None, speculative_draft_model_path=None, speculative_draft_model_revision=None, speculative_draft_load_format=None, speculative_num_steps=None, speculative_eagle_topk=None, speculative_num_draft_tokens=None, speculative_accept_threshold_single=1.0, speculative_accept_threshold_acc=1.0, speculative_token_map=None, speculative_attention_mode='prefill', speculative_draft_attention_backend=None, speculative_moe_runner_backend='auto', speculative_moe_a2a_backend=None, speculative_draft_model_quantization=None, speculative_ngram_min_match_window_size=1, speculative_ngram_max_match_window_size=12, speculative_ngram_min_bfs_breadth=1, speculative_ngram_max_bfs_breadth=10, speculative_ngram_match_type='BFS', speculative_ngram_max_trie_depth=18, speculative_ngram_capacity=10000000, enable_multi_layer_eagle=False, ep_size=1, moe_a2a_backend='none', moe_runner_backend='auto', flashinfer_mxfp4_moe_precision='default', enable_flashinfer_allreduce_fusion=False, enable_aiter_allreduce_fusion=False, deepep_mode='auto', ep_num_redundant_experts=0, ep_dispatch_algorithm=None, init_expert_location='trivial', enable_eplb=False, eplb_algorithm='auto', eplb_rebalance_num_iterations=1000, eplb_rebalance_layers_per_chunk=None, eplb_min_rebalancing_utilization_threshold=1.0, expert_distribution_recorder_mode=None, expert_distribution_recorder_buffer_size=1000, enable_expert_distribution_metrics=False, deepep_config=None, moe_dense_tp_size=None, elastic_ep_backend=None, enable_elastic_expert_backup=False, mooncake_ib_device=None, max_mamba_cache_size=None, mamba_ssm_dtype=None, mamba_full_memory_ratio=0.9, mamba_scheduler_strategy='no_buffer', mamba_track_interval=256, linear_attn_backend='triton', linear_attn_decode_backend=None, linear_attn_prefill_backend=None, enable_hierarchical_cache=False, hicache_ratio=2.0, hicache_size=0, hicache_write_policy='write_through', hicache_io_backend='kernel', hicache_mem_layout='layer_first', disable_hicache_numa_detect=False, hicache_storage_backend=None, hicache_storage_prefetch_policy='best_effort', hicache_storage_backend_extra_config=None, enable_hisparse=False, hisparse_config=None, enable_lmcache=False, kt_weight_path=None, kt_method=None, kt_cpuinfer=None, kt_threadpool_count=None, kt_num_gpu_experts=None, kt_max_deferred_experts_per_token=None, dllm_algorithm=None, dllm_algorithm_config=None, enable_double_sparsity=False, ds_channel_config_path=None, ds_heavy_channel_num=32, ds_heavy_token_num=256, ds_heavy_channel_type='qk', ds_sparse_decode_threshold=4096, cpu_offload_gb=0, offload_group_size=-1, offload_num_in_group=1, offload_prefetch_step=1, offload_mode='cpu', multi_item_scoring_delimiter=None, disable_radix_cache=False, cuda_graph_max_bs=4, cuda_graph_bs=[1, 2, 4, 8, 12, 16, 24, 32, 40, 48, 56, 64, 72, 80, 88, 96, 104, 112, 120, 128, 136, 144, 152, 160, 168, 176, 184, 192, 200, 208, 216, 224, 232, 240, 248, 256], disable_cuda_graph=True, disable_cuda_graph_padding=False, enable_profile_cuda_graph=False, enable_cudagraph_gc=False, enable_layerwise_nvtx_marker=False, enable_nccl_nvls=False, enable_symm_mem=False, disable_flashinfer_cutlass_moe_fp4_allgather=False, enable_tokenizer_batch_encode=False, disable_tokenizer_batch_decode=False, disable_outlines_disk_cache=False, disable_custom_all_reduce=False, enable_mscclpp=False, enable_torch_symm_mem=False, pre_warm_nccl=False, disable_overlap_schedule=False, enable_mixed_chunk=False, enable_dp_attention=False, enable_dp_lm_head=False, enable_two_batch_overlap=False, enable_single_batch_overlap=False, tbo_token_distribution_threshold=0.48, enable_torch_compile=False, disable_piecewise_cuda_graph=False, enforce_piecewise_cuda_graph=False, enable_torch_compile_debug_mode=False, torch_compile_max_bs=32, piecewise_cuda_graph_max_tokens=8192, piecewise_cuda_graph_tokens=[4, 8, 12, 16, 20, 24, 28, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 352, 384, 416, 448, 480, 512, 576, 640, 704, 768, 832, 896, 960, 1024, 1280, 1536, 1792, 2048, 2304, 2560, 2816, 3072, 3328, 3584, 3840, 4096, 4608, 5120, 5632, 6144, 6656, 7168, 7680, 8192], piecewise_cuda_graph_compiler='eager', torchao_config='', enable_nan_detection=False, enable_p2p_check=False, triton_attention_reduce_in_fp32=False, triton_attention_num_kv_splits=8, triton_attention_split_tile_size=None, num_continuous_decode_steps=1, delete_ckpt_after_loading=False, enable_memory_saver=False, enable_weights_cpu_backup=False, enable_draft_weights_cpu_backup=False, allow_auto_truncate=False, enable_custom_logit_processor=False, flashinfer_mla_disable_ragged=False, disable_shared_experts_fusion=False, disable_chunked_prefix_cache=False, disable_fast_image_processor=False, keep_mm_feature_on_device=False, enable_return_hidden_states=False, enable_return_routed_experts=False, scheduler_recv_interval=1, numa_node=None, enable_deterministic_inference=False, rl_on_policy_target=None, enable_attn_tp_input_scattered=False, enable_nsa_prefill_context_parallel=False, nsa_prefill_cp_mode='round-robin-split', enable_fused_qk_norm_rope=False, enable_precise_embedding_interpolation=False, enable_fused_moe_sum_all_reduce=False, enable_prefill_context_parallel=False, prefill_cp_mode='in-seq-split', enable_dynamic_batch_tokenizer=False, dynamic_batch_tokenizer_batch_size=32, dynamic_batch_tokenizer_batch_timeout=0.002, debug_tensor_dump_output_folder=None, debug_tensor_dump_layers=None, debug_tensor_dump_input_file=None, debug_tensor_dump_inject=False, disaggregation_mode='null', disaggregation_transfer_backend='mooncake', disaggregation_bootstrap_port=8998, disaggregation_ib_device=None, disaggregation_decode_enable_offload_kvcache=False, num_reserved_decode_tokens=512, disaggregation_decode_polling_interval=1, encoder_only=False, language_only=False, encoder_transfer_backend='zmq_to_scheduler', encoder_urls=[], enable_adaptive_dispatch_to_encoder=False, custom_weight_loader=[], weight_loader_disable_mmap=False, remote_instance_weight_loader_seed_instance_ip=None, remote_instance_weight_loader_seed_instance_service_port=None, remote_instance_weight_loader_send_weights_group_ports=None, remote_instance_weight_loader_backend='nccl', remote_instance_weight_loader_start_seed_via_transfer_engine=False, modelexpress_config=None, enable_pdmux=False, pdmux_config_path=None, sm_group_num=8, mm_max_concurrent_calls=32, mm_per_request_timeout=10.0, enable_broadcast_mm_inputs_process=False, enable_prefix_mm_cache=False, mm_enable_dp_encoder=False, mm_process_config={}, limit_mm_data_per_request=None, enable_mm_global_cache=False, decrypted_config_file=None, decrypted_draft_config_file=None, forward_hooks=None)


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-25 11:09:18.685 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:09:18] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 11:09:18.685 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:09:18] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 11:09:18.685 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:09:18] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 11:09:18.685 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:09:18] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 11:09:18.685 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:09:18] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  25% Completed | 1/4 [00:00<00:01,  1.94it/s]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:01<00:01,  1.53it/s]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:01<00:00,  1.51it/s]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.48it/s]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.52it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:52,  3.03s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:52,  3.03s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:22,  1.48s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:22,  1.48s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.11it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.11it/s]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.61it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.61it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:24,  2.18it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:24,  2.18it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.74it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.74it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:15,  3.20it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:15,  3.20it/s]Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:13,  3.64it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:13,  3.64it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:11,  4.28it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:11,  4.28it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:11,  4.28it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:07,  5.98it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:07,  5.98it/s]

    Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:07,  5.98it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:05<00:05,  7.53it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:05<00:05,  7.53it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:05<00:05,  7.53it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:04,  9.18it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:04,  9.18it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04,  9.18it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 11.15it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 11.15it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.15it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 11.15it/s]

    Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 14.42it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 14.42it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 14.42it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 14.42it/s]Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 14.42it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 19.53it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 19.53it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 19.53it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 19.53it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 19.53it/s]

    Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 19.53it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 26.07it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 26.07it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 26.07it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 26.07it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 26.07it/s]Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:05<00:01, 26.07it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:05<00:01, 26.07it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:05<00:00, 33.58it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:05<00:00, 33.58it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:05<00:00, 33.58it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:05<00:00, 33.58it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:05<00:00, 33.58it/s]

    Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:05<00:00, 33.58it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:05<00:00, 33.58it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 39.11it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 39.11it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:05<00:00, 39.11it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:05<00:00, 39.11it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:05<00:00, 39.11it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:05<00:00, 39.11it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:05<00:00, 39.11it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:06<00:00, 42.49it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:06<00:00, 42.49it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:06<00:00, 42.49it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:06<00:00, 42.49it/s]

    Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:06<00:00, 42.49it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:06<00:00, 42.49it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:06<00:00, 42.49it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:06<00:00, 46.70it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:06<00:00, 46.70it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:06<00:00, 46.70it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:06<00:00, 46.70it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:06<00:00, 46.70it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:06<00:00, 46.70it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  9.40it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=112.64 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=112.64 GB):   2%|▏         | 1/58 [00:00<00:16,  3.48it/s]Capturing num tokens (num_tokens=7680 avail_mem=112.61 GB):   2%|▏         | 1/58 [00:00<00:16,  3.48it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=112.61 GB):   3%|▎         | 2/58 [00:00<00:15,  3.60it/s]Capturing num tokens (num_tokens=7168 avail_mem=112.61 GB):   3%|▎         | 2/58 [00:00<00:15,  3.60it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=112.61 GB):   5%|▌         | 3/58 [00:00<00:14,  3.81it/s]Capturing num tokens (num_tokens=6656 avail_mem=112.61 GB):   5%|▌         | 3/58 [00:00<00:14,  3.81it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=112.61 GB):   7%|▋         | 4/58 [00:01<00:13,  4.09it/s]Capturing num tokens (num_tokens=6144 avail_mem=112.62 GB):   7%|▋         | 4/58 [00:01<00:13,  4.09it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=112.62 GB):   9%|▊         | 5/58 [00:01<00:12,  4.20it/s]Capturing num tokens (num_tokens=5632 avail_mem=112.62 GB):   9%|▊         | 5/58 [00:01<00:12,  4.20it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=112.62 GB):  10%|█         | 6/58 [00:01<00:11,  4.37it/s]Capturing num tokens (num_tokens=5120 avail_mem=112.62 GB):  10%|█         | 6/58 [00:01<00:11,  4.37it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=112.62 GB):  12%|█▏        | 7/58 [00:01<00:12,  4.25it/s]Capturing num tokens (num_tokens=4608 avail_mem=112.63 GB):  12%|█▏        | 7/58 [00:01<00:12,  4.25it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=112.63 GB):  14%|█▍        | 8/58 [00:01<00:11,  4.39it/s]Capturing num tokens (num_tokens=4096 avail_mem=112.63 GB):  14%|█▍        | 8/58 [00:01<00:11,  4.39it/s]Capturing num tokens (num_tokens=4096 avail_mem=112.63 GB):  16%|█▌        | 9/58 [00:02<00:10,  4.58it/s]Capturing num tokens (num_tokens=3840 avail_mem=112.63 GB):  16%|█▌        | 9/58 [00:02<00:10,  4.58it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=112.63 GB):  17%|█▋        | 10/58 [00:02<00:10,  4.79it/s]Capturing num tokens (num_tokens=3584 avail_mem=112.63 GB):  17%|█▋        | 10/58 [00:02<00:10,  4.79it/s]Capturing num tokens (num_tokens=3584 avail_mem=112.63 GB):  19%|█▉        | 11/58 [00:02<00:08,  5.54it/s]Capturing num tokens (num_tokens=3328 avail_mem=112.63 GB):  19%|█▉        | 11/58 [00:02<00:08,  5.54it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=112.63 GB):  21%|██        | 12/58 [00:02<00:07,  6.34it/s]Capturing num tokens (num_tokens=3072 avail_mem=112.63 GB):  21%|██        | 12/58 [00:02<00:07,  6.34it/s]Capturing num tokens (num_tokens=3072 avail_mem=112.63 GB):  22%|██▏       | 13/58 [00:02<00:06,  7.11it/s]Capturing num tokens (num_tokens=2816 avail_mem=112.63 GB):  22%|██▏       | 13/58 [00:02<00:06,  7.11it/s]Capturing num tokens (num_tokens=2560 avail_mem=112.63 GB):  22%|██▏       | 13/58 [00:02<00:06,  7.11it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=112.63 GB):  26%|██▌       | 15/58 [00:02<00:04,  8.73it/s]Capturing num tokens (num_tokens=2304 avail_mem=112.63 GB):  26%|██▌       | 15/58 [00:02<00:04,  8.73it/s]Capturing num tokens (num_tokens=2048 avail_mem=112.63 GB):  26%|██▌       | 15/58 [00:02<00:04,  8.73it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=112.63 GB):  29%|██▉       | 17/58 [00:03<00:04,  9.09it/s]Capturing num tokens (num_tokens=1792 avail_mem=112.63 GB):  29%|██▉       | 17/58 [00:03<00:04,  9.09it/s]Capturing num tokens (num_tokens=1536 avail_mem=112.63 GB):  29%|██▉       | 17/58 [00:03<00:04,  9.09it/s]Capturing num tokens (num_tokens=1536 avail_mem=112.63 GB):  33%|███▎      | 19/58 [00:03<00:03, 10.87it/s]Capturing num tokens (num_tokens=1280 avail_mem=112.63 GB):  33%|███▎      | 19/58 [00:03<00:03, 10.87it/s]Capturing num tokens (num_tokens=1024 avail_mem=112.63 GB):  33%|███▎      | 19/58 [00:03<00:03, 10.87it/s]

    Capturing num tokens (num_tokens=960 avail_mem=112.62 GB):  33%|███▎      | 19/58 [00:03<00:03, 10.87it/s] Capturing num tokens (num_tokens=960 avail_mem=112.62 GB):  38%|███▊      | 22/58 [00:03<00:02, 14.29it/s]Capturing num tokens (num_tokens=896 avail_mem=112.62 GB):  38%|███▊      | 22/58 [00:03<00:02, 14.29it/s]Capturing num tokens (num_tokens=832 avail_mem=112.62 GB):  38%|███▊      | 22/58 [00:03<00:02, 14.29it/s]Capturing num tokens (num_tokens=768 avail_mem=112.61 GB):  38%|███▊      | 22/58 [00:03<00:02, 14.29it/s]Capturing num tokens (num_tokens=768 avail_mem=112.61 GB):  43%|████▎     | 25/58 [00:03<00:01, 17.47it/s]Capturing num tokens (num_tokens=704 avail_mem=112.61 GB):  43%|████▎     | 25/58 [00:03<00:01, 17.47it/s]Capturing num tokens (num_tokens=640 avail_mem=112.61 GB):  43%|████▎     | 25/58 [00:03<00:01, 17.47it/s]

    Capturing num tokens (num_tokens=576 avail_mem=112.60 GB):  43%|████▎     | 25/58 [00:03<00:01, 17.47it/s]Capturing num tokens (num_tokens=512 avail_mem=112.60 GB):  43%|████▎     | 25/58 [00:03<00:01, 17.47it/s]Capturing num tokens (num_tokens=512 avail_mem=112.60 GB):  50%|█████     | 29/58 [00:03<00:01, 21.44it/s]Capturing num tokens (num_tokens=480 avail_mem=112.59 GB):  50%|█████     | 29/58 [00:03<00:01, 21.44it/s]Capturing num tokens (num_tokens=448 avail_mem=112.59 GB):  50%|█████     | 29/58 [00:03<00:01, 21.44it/s]Capturing num tokens (num_tokens=416 avail_mem=112.59 GB):  50%|█████     | 29/58 [00:03<00:01, 21.44it/s]Capturing num tokens (num_tokens=384 avail_mem=112.58 GB):  50%|█████     | 29/58 [00:03<00:01, 21.44it/s]Capturing num tokens (num_tokens=384 avail_mem=112.58 GB):  57%|█████▋    | 33/58 [00:03<00:00, 25.03it/s]Capturing num tokens (num_tokens=352 avail_mem=112.58 GB):  57%|█████▋    | 33/58 [00:03<00:00, 25.03it/s]

    Capturing num tokens (num_tokens=320 avail_mem=112.57 GB):  57%|█████▋    | 33/58 [00:03<00:00, 25.03it/s]Capturing num tokens (num_tokens=288 avail_mem=112.57 GB):  57%|█████▋    | 33/58 [00:03<00:00, 25.03it/s]Capturing num tokens (num_tokens=256 avail_mem=112.57 GB):  57%|█████▋    | 33/58 [00:03<00:00, 25.03it/s]Capturing num tokens (num_tokens=256 avail_mem=112.57 GB):  64%|██████▍   | 37/58 [00:03<00:00, 28.25it/s]Capturing num tokens (num_tokens=240 avail_mem=112.56 GB):  64%|██████▍   | 37/58 [00:03<00:00, 28.25it/s]Capturing num tokens (num_tokens=224 avail_mem=112.56 GB):  64%|██████▍   | 37/58 [00:03<00:00, 28.25it/s]Capturing num tokens (num_tokens=208 avail_mem=112.55 GB):  64%|██████▍   | 37/58 [00:03<00:00, 28.25it/s]Capturing num tokens (num_tokens=192 avail_mem=112.55 GB):  64%|██████▍   | 37/58 [00:03<00:00, 28.25it/s]Capturing num tokens (num_tokens=192 avail_mem=112.55 GB):  71%|███████   | 41/58 [00:03<00:00, 30.87it/s]Capturing num tokens (num_tokens=176 avail_mem=112.55 GB):  71%|███████   | 41/58 [00:03<00:00, 30.87it/s]

    Capturing num tokens (num_tokens=160 avail_mem=112.55 GB):  71%|███████   | 41/58 [00:03<00:00, 30.87it/s]Capturing num tokens (num_tokens=144 avail_mem=112.54 GB):  71%|███████   | 41/58 [00:03<00:00, 30.87it/s]Capturing num tokens (num_tokens=128 avail_mem=112.55 GB):  71%|███████   | 41/58 [00:03<00:00, 30.87it/s]Capturing num tokens (num_tokens=128 avail_mem=112.55 GB):  78%|███████▊  | 45/58 [00:03<00:00, 32.79it/s]Capturing num tokens (num_tokens=112 avail_mem=112.55 GB):  78%|███████▊  | 45/58 [00:03<00:00, 32.79it/s]Capturing num tokens (num_tokens=96 avail_mem=112.54 GB):  78%|███████▊  | 45/58 [00:03<00:00, 32.79it/s] Capturing num tokens (num_tokens=80 avail_mem=112.54 GB):  78%|███████▊  | 45/58 [00:03<00:00, 32.79it/s]Capturing num tokens (num_tokens=64 avail_mem=112.54 GB):  78%|███████▊  | 45/58 [00:04<00:00, 32.79it/s]Capturing num tokens (num_tokens=64 avail_mem=112.54 GB):  84%|████████▍ | 49/58 [00:04<00:00, 34.20it/s]Capturing num tokens (num_tokens=48 avail_mem=112.53 GB):  84%|████████▍ | 49/58 [00:04<00:00, 34.20it/s]

    Capturing num tokens (num_tokens=32 avail_mem=112.53 GB):  84%|████████▍ | 49/58 [00:04<00:00, 34.20it/s]Capturing num tokens (num_tokens=28 avail_mem=112.53 GB):  84%|████████▍ | 49/58 [00:04<00:00, 34.20it/s]Capturing num tokens (num_tokens=24 avail_mem=112.52 GB):  84%|████████▍ | 49/58 [00:04<00:00, 34.20it/s]Capturing num tokens (num_tokens=24 avail_mem=112.52 GB):  91%|█████████▏| 53/58 [00:04<00:00, 35.26it/s]Capturing num tokens (num_tokens=20 avail_mem=112.52 GB):  91%|█████████▏| 53/58 [00:04<00:00, 35.26it/s]Capturing num tokens (num_tokens=16 avail_mem=112.52 GB):  91%|█████████▏| 53/58 [00:04<00:00, 35.26it/s]Capturing num tokens (num_tokens=12 avail_mem=112.51 GB):  91%|█████████▏| 53/58 [00:04<00:00, 35.26it/s]Capturing num tokens (num_tokens=8 avail_mem=112.51 GB):  91%|█████████▏| 53/58 [00:04<00:00, 35.26it/s] Capturing num tokens (num_tokens=8 avail_mem=112.51 GB):  98%|█████████▊| 57/58 [00:04<00:00, 36.13it/s]Capturing num tokens (num_tokens=4 avail_mem=112.50 GB):  98%|█████████▊| 57/58 [00:04<00:00, 36.13it/s]

    Capturing num tokens (num_tokens=4 avail_mem=112.50 GB): 100%|██████████| 58/58 [00:04<00:00, 13.60it/s]



<strong style='color: #00008B;'>=== Offline Engine Output Text ===</strong>



<strong style='color: #00008B;'>To provide you with the current weather in Boston, I will use the `get_current_weather` function. Since you didn't specify the unit for temperature, I'll assume you prefer Fahrenheit, which is commonly used in the United States.<br><br>Reasoning: The user asked for the current weather in Boston, so we need to fetch the weather data for this specific location. Using the `get_current_weather` function with the city set to "Boston" and the state set to "MA" (the two-letter abbreviation for Massachusetts), and the unit set to "fahrenheit" will give us the required information.<br><br><tool_call><br>{"name": "get_current_weather", "arguments": {"city": "Boston", "state": "MA", "unit": "fahrenheit"}}<br></tool_call></strong>



<strong style='color: #00008B;'>=== Parsing Result ===</strong>


    Normal text portion: To provide you with the current weather in Boston, I will use the `get_current_weather` function. Since you didn't specify the unit for temperature, I'll assume you prefer Fahrenheit, which is commonly used in the United States.
    
    Reasoning: The user asked for the current weather in Boston, so we need to fetch the weather data for this specific location. Using the `get_current_weather` function with the city set to "Boston" and the state set to "MA" (the two-letter abbreviation for Massachusetts), and the unit set to "fahrenheit" will give us the required information.



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

    [2026-03-25 11:09:42] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-25 11:09:42] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-25 11:09:42] INFO utils.py:164: NumExpr defaulting to 16 threads.
    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(
    [2026-03-25 11:09:43] WARNING server_args.py:908: The tool_call_parser 'qwen25' is deprecated. Please use 'qwen' instead.


    [2026-03-25 11:09:44] INFO server_args.py:2246: Attention backend not specified. Use fa3 backend by default.


    [2026-03-25 11:09:45] INFO server_args.py:3544: Set soft_watchdog_timeout since in CI


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [2026-03-25 11:09:51] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-25 11:09:51] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-25 11:09:51] INFO utils.py:164: NumExpr defaulting to 16 threads.
    [2026-03-25 11:09:51] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-25 11:09:51] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-25 11:09:51] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-03-25 11:10:13.635 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:10:13] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 11:10:13.635 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:10:13] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 11:10:13.636 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:10:13] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 11:10:13.636 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:10:13] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 11:10:13.636 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:10:13] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  25% Completed | 1/4 [00:00<00:01,  1.99it/s]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:01<00:01,  1.52it/s]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:01<00:00,  1.46it/s]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.39it/s]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.45it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:57,  3.12s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:57,  3.12s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:32,  1.65s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:32,  1.65s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<00:56,  1.03s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<00:56,  1.03s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:37,  1.42it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:37,  1.42it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:26,  1.97it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:26,  1.97it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:20,  2.59it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:20,  2.59it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:16,  3.12it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:16,  3.12it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:13,  3.60it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:13,  3.60it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:11,  4.24it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:11,  4.24it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:09,  4.90it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:09,  4.90it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:08,  5.65it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:08,  5.65it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:05<00:07,  6.32it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:05<00:07,  6.32it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:05<00:07,  6.32it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:05<00:05,  7.80it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:05<00:05,  7.80it/s]

    Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:05<00:05,  7.80it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:05<00:04,  9.04it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:05<00:04,  9.04it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:05<00:04,  9.04it/s]

    Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:05<00:03, 10.71it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:05<00:03, 10.71it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:05<00:03, 10.71it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:06<00:03, 12.45it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:06<00:03, 12.45it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:06<00:03, 12.45it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:06<00:03, 12.45it/s]

    Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:06<00:02, 16.23it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:06<00:02, 16.23it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:06<00:02, 16.23it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:06<00:02, 16.23it/s]Compiling num tokens (num_tokens=640):  40%|███▉      | 23/58 [00:06<00:02, 16.23it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:06<00:01, 21.19it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:06<00:01, 21.19it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:06<00:01, 21.19it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:06<00:01, 21.19it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:06<00:01, 21.19it/s]

    Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:06<00:01, 21.19it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:06<00:00, 26.97it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:06<00:00, 26.97it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:06<00:00, 26.97it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:06<00:00, 26.97it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:06<00:00, 26.97it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:06<00:00, 26.97it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:06<00:00, 31.45it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:06<00:00, 31.45it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:06<00:00, 31.45it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:06<00:00, 31.45it/s]

    Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:06<00:00, 31.45it/s]Compiling num tokens (num_tokens=176):  64%|██████▍   | 37/58 [00:06<00:00, 31.45it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:06<00:00, 35.29it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:06<00:00, 35.29it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:06<00:00, 35.29it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:06<00:00, 35.29it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:06<00:00, 35.29it/s]Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:06<00:00, 35.29it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:06<00:00, 37.72it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:06<00:00, 37.72it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:06<00:00, 37.72it/s]

    Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:06<00:00, 37.72it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:06<00:00, 37.72it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:06<00:00, 37.72it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:06<00:00, 37.72it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:06<00:00, 42.10it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:06<00:00, 42.10it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:06<00:00, 42.10it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:06<00:00, 42.10it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:06<00:00, 42.10it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:06<00:00, 42.10it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  8.42it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=104.65 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=104.65 GB):   2%|▏         | 1/58 [00:00<00:16,  3.49it/s]Capturing num tokens (num_tokens=7680 avail_mem=104.62 GB):   2%|▏         | 1/58 [00:00<00:16,  3.49it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=104.62 GB):   3%|▎         | 2/58 [00:00<00:15,  3.60it/s]Capturing num tokens (num_tokens=7168 avail_mem=104.62 GB):   3%|▎         | 2/58 [00:00<00:15,  3.60it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=104.62 GB):   5%|▌         | 3/58 [00:00<00:14,  3.82it/s]Capturing num tokens (num_tokens=6656 avail_mem=104.62 GB):   5%|▌         | 3/58 [00:00<00:14,  3.82it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=104.62 GB):   7%|▋         | 4/58 [00:01<00:13,  4.10it/s]Capturing num tokens (num_tokens=6144 avail_mem=104.63 GB):   7%|▋         | 4/58 [00:01<00:13,  4.10it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=104.63 GB):   9%|▊         | 5/58 [00:01<00:12,  4.25it/s]Capturing num tokens (num_tokens=5632 avail_mem=104.61 GB):   9%|▊         | 5/58 [00:01<00:12,  4.25it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=104.61 GB):  10%|█         | 6/58 [00:01<00:11,  4.43it/s]Capturing num tokens (num_tokens=5120 avail_mem=104.12 GB):  10%|█         | 6/58 [00:01<00:11,  4.43it/s]Capturing num tokens (num_tokens=5120 avail_mem=104.12 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.84it/s]Capturing num tokens (num_tokens=4608 avail_mem=103.96 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.84it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=103.96 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.17it/s]Capturing num tokens (num_tokens=4096 avail_mem=103.97 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.17it/s]Capturing num tokens (num_tokens=4096 avail_mem=103.97 GB):  16%|█▌        | 9/58 [00:01<00:08,  5.73it/s]Capturing num tokens (num_tokens=3840 avail_mem=103.97 GB):  16%|█▌        | 9/58 [00:01<00:08,  5.73it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=103.97 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.26it/s]Capturing num tokens (num_tokens=3584 avail_mem=103.97 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.26it/s]Capturing num tokens (num_tokens=3584 avail_mem=103.97 GB):  19%|█▉        | 11/58 [00:02<00:06,  6.84it/s]Capturing num tokens (num_tokens=3328 avail_mem=103.97 GB):  19%|█▉        | 11/58 [00:02<00:06,  6.84it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=103.97 GB):  21%|██        | 12/58 [00:02<00:06,  7.46it/s]Capturing num tokens (num_tokens=3072 avail_mem=103.97 GB):  21%|██        | 12/58 [00:02<00:06,  7.46it/s]Capturing num tokens (num_tokens=2816 avail_mem=103.97 GB):  21%|██        | 12/58 [00:02<00:06,  7.46it/s]Capturing num tokens (num_tokens=2816 avail_mem=103.97 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.68it/s]Capturing num tokens (num_tokens=2560 avail_mem=103.97 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.68it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=103.97 GB):  26%|██▌       | 15/58 [00:02<00:04,  8.62it/s]Capturing num tokens (num_tokens=2304 avail_mem=103.97 GB):  26%|██▌       | 15/58 [00:02<00:04,  8.62it/s]Capturing num tokens (num_tokens=2048 avail_mem=103.97 GB):  26%|██▌       | 15/58 [00:02<00:04,  8.62it/s]Capturing num tokens (num_tokens=2048 avail_mem=103.97 GB):  29%|██▉       | 17/58 [00:02<00:03, 10.28it/s]Capturing num tokens (num_tokens=1792 avail_mem=103.97 GB):  29%|██▉       | 17/58 [00:02<00:03, 10.28it/s]Capturing num tokens (num_tokens=1536 avail_mem=103.96 GB):  29%|██▉       | 17/58 [00:02<00:03, 10.28it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=103.96 GB):  33%|███▎      | 19/58 [00:02<00:03, 12.22it/s]Capturing num tokens (num_tokens=1280 avail_mem=103.97 GB):  33%|███▎      | 19/58 [00:02<00:03, 12.22it/s]Capturing num tokens (num_tokens=1024 avail_mem=103.97 GB):  33%|███▎      | 19/58 [00:02<00:03, 12.22it/s]Capturing num tokens (num_tokens=960 avail_mem=103.96 GB):  33%|███▎      | 19/58 [00:02<00:03, 12.22it/s] Capturing num tokens (num_tokens=960 avail_mem=103.96 GB):  38%|███▊      | 22/58 [00:02<00:02, 15.55it/s]Capturing num tokens (num_tokens=896 avail_mem=103.96 GB):  38%|███▊      | 22/58 [00:02<00:02, 15.55it/s]Capturing num tokens (num_tokens=832 avail_mem=103.96 GB):  38%|███▊      | 22/58 [00:02<00:02, 15.55it/s]

    Capturing num tokens (num_tokens=768 avail_mem=103.95 GB):  38%|███▊      | 22/58 [00:03<00:02, 15.55it/s]Capturing num tokens (num_tokens=768 avail_mem=103.95 GB):  43%|████▎     | 25/58 [00:03<00:01, 18.71it/s]Capturing num tokens (num_tokens=704 avail_mem=103.95 GB):  43%|████▎     | 25/58 [00:03<00:01, 18.71it/s]Capturing num tokens (num_tokens=640 avail_mem=103.94 GB):  43%|████▎     | 25/58 [00:03<00:01, 18.71it/s]Capturing num tokens (num_tokens=576 avail_mem=103.94 GB):  43%|████▎     | 25/58 [00:03<00:01, 18.71it/s]Capturing num tokens (num_tokens=512 avail_mem=103.94 GB):  43%|████▎     | 25/58 [00:03<00:01, 18.71it/s]Capturing num tokens (num_tokens=512 avail_mem=103.94 GB):  50%|█████     | 29/58 [00:03<00:01, 22.60it/s]Capturing num tokens (num_tokens=480 avail_mem=103.93 GB):  50%|█████     | 29/58 [00:03<00:01, 22.60it/s]Capturing num tokens (num_tokens=448 avail_mem=103.93 GB):  50%|█████     | 29/58 [00:03<00:01, 22.60it/s]

    Capturing num tokens (num_tokens=416 avail_mem=103.93 GB):  50%|█████     | 29/58 [00:03<00:01, 22.60it/s]Capturing num tokens (num_tokens=384 avail_mem=103.92 GB):  50%|█████     | 29/58 [00:03<00:01, 22.60it/s]Capturing num tokens (num_tokens=384 avail_mem=103.92 GB):  57%|█████▋    | 33/58 [00:03<00:01, 24.49it/s]Capturing num tokens (num_tokens=352 avail_mem=103.92 GB):  57%|█████▋    | 33/58 [00:03<00:01, 24.49it/s]Capturing num tokens (num_tokens=320 avail_mem=103.91 GB):  57%|█████▋    | 33/58 [00:03<00:01, 24.49it/s]Capturing num tokens (num_tokens=288 avail_mem=103.91 GB):  57%|█████▋    | 33/58 [00:03<00:01, 24.49it/s]Capturing num tokens (num_tokens=256 avail_mem=103.91 GB):  57%|█████▋    | 33/58 [00:03<00:01, 24.49it/s]Capturing num tokens (num_tokens=256 avail_mem=103.91 GB):  64%|██████▍   | 37/58 [00:03<00:00, 26.92it/s]Capturing num tokens (num_tokens=240 avail_mem=103.90 GB):  64%|██████▍   | 37/58 [00:03<00:00, 26.92it/s]

    Capturing num tokens (num_tokens=224 avail_mem=103.90 GB):  64%|██████▍   | 37/58 [00:03<00:00, 26.92it/s]Capturing num tokens (num_tokens=208 avail_mem=103.89 GB):  64%|██████▍   | 37/58 [00:03<00:00, 26.92it/s]Capturing num tokens (num_tokens=192 avail_mem=103.89 GB):  64%|██████▍   | 37/58 [00:03<00:00, 26.92it/s]Capturing num tokens (num_tokens=192 avail_mem=103.89 GB):  71%|███████   | 41/58 [00:03<00:00, 29.87it/s]Capturing num tokens (num_tokens=176 avail_mem=103.89 GB):  71%|███████   | 41/58 [00:03<00:00, 29.87it/s]Capturing num tokens (num_tokens=160 avail_mem=103.89 GB):  71%|███████   | 41/58 [00:03<00:00, 29.87it/s]Capturing num tokens (num_tokens=144 avail_mem=103.88 GB):  71%|███████   | 41/58 [00:03<00:00, 29.87it/s]Capturing num tokens (num_tokens=128 avail_mem=102.83 GB):  71%|███████   | 41/58 [00:03<00:00, 29.87it/s]Capturing num tokens (num_tokens=128 avail_mem=102.83 GB):  78%|███████▊  | 45/58 [00:03<00:00, 32.00it/s]Capturing num tokens (num_tokens=112 avail_mem=102.83 GB):  78%|███████▊  | 45/58 [00:03<00:00, 32.00it/s]

    Capturing num tokens (num_tokens=96 avail_mem=102.83 GB):  78%|███████▊  | 45/58 [00:03<00:00, 32.00it/s] Capturing num tokens (num_tokens=80 avail_mem=102.82 GB):  78%|███████▊  | 45/58 [00:03<00:00, 32.00it/s]Capturing num tokens (num_tokens=64 avail_mem=102.82 GB):  78%|███████▊  | 45/58 [00:03<00:00, 32.00it/s]Capturing num tokens (num_tokens=64 avail_mem=102.82 GB):  84%|████████▍ | 49/58 [00:03<00:00, 33.62it/s]Capturing num tokens (num_tokens=48 avail_mem=102.81 GB):  84%|████████▍ | 49/58 [00:03<00:00, 33.62it/s]Capturing num tokens (num_tokens=32 avail_mem=102.28 GB):  84%|████████▍ | 49/58 [00:03<00:00, 33.62it/s]Capturing num tokens (num_tokens=28 avail_mem=93.07 GB):  84%|████████▍ | 49/58 [00:03<00:00, 33.62it/s] Capturing num tokens (num_tokens=24 avail_mem=88.84 GB):  84%|████████▍ | 49/58 [00:03<00:00, 33.62it/s]Capturing num tokens (num_tokens=24 avail_mem=88.84 GB):  91%|█████████▏| 53/58 [00:03<00:00, 34.54it/s]Capturing num tokens (num_tokens=20 avail_mem=88.84 GB):  91%|█████████▏| 53/58 [00:03<00:00, 34.54it/s]

    Capturing num tokens (num_tokens=16 avail_mem=88.83 GB):  91%|█████████▏| 53/58 [00:03<00:00, 34.54it/s]Capturing num tokens (num_tokens=12 avail_mem=88.83 GB):  91%|█████████▏| 53/58 [00:03<00:00, 34.54it/s]Capturing num tokens (num_tokens=8 avail_mem=88.83 GB):  91%|█████████▏| 53/58 [00:03<00:00, 34.54it/s] Capturing num tokens (num_tokens=8 avail_mem=88.83 GB):  98%|█████████▊| 57/58 [00:04<00:00, 30.22it/s]Capturing num tokens (num_tokens=4 avail_mem=88.82 GB):  98%|█████████▊| 57/58 [00:04<00:00, 30.22it/s]Capturing num tokens (num_tokens=4 avail_mem=88.82 GB): 100%|██████████| 58/58 [00:04<00:00, 14.25it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>



<strong style='color: #00008B;'>Response with tool_choice='required':</strong>


    Content: None
    Tool calls: [ChatCompletionMessageFunctionToolCall(id='call_47804a569ef649e9af1f774b', function=Function(arguments='{"city": "Paris", "unit": "celsius"}', name='get_current_weather'), type='function', index=0)]


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
    Tool calls: [ChatCompletionMessageFunctionToolCall(id='call_439aced49e1e4a2aa537d574', function=Function(arguments='{"city": "Paris", "unit": "celsius"}', name='get_current_weather'), type='function', index=0)]



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

    [2026-03-25 11:10:51] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-25 11:10:51] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-25 11:10:51] INFO utils.py:164: NumExpr defaulting to 16 threads.
    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    [2026-03-25 11:10:53] INFO server_args.py:2246: Attention backend not specified. Use fa3 backend by default.
    [2026-03-25 11:10:53] INFO server_args.py:3544: Set soft_watchdog_timeout since in CI


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [2026-03-25 11:11:00] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-25 11:11:00] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-25 11:11:00] INFO utils.py:164: NumExpr defaulting to 16 threads.
    [2026-03-25 11:11:00] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-25 11:11:00] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-25 11:11:00] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-25 11:11:06.944 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:11:06] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 11:11:06.944 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:11:06] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 11:11:06.944 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:11:06] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 11:11:06.944 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:11:06] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 11:11:06.944 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:11:06] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  3.11it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  3.10it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:01<01:37,  1.71s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:01<01:37,  1.71s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:01<01:37,  1.71s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:01<00:26,  2.04it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:01<00:26,  2.04it/s]Compiling num tokens (num_tokens=6144):   5%|▌         | 3/58 [00:01<00:26,  2.04it/s]Compiling num tokens (num_tokens=5632):   5%|▌         | 3/58 [00:01<00:26,  2.04it/s]

    Compiling num tokens (num_tokens=5120):   5%|▌         | 3/58 [00:01<00:26,  2.04it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:01<00:09,  5.59it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:01<00:09,  5.59it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:01<00:09,  5.59it/s]Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:02<00:09,  5.59it/s]Compiling num tokens (num_tokens=3584):  12%|█▏        | 7/58 [00:02<00:09,  5.59it/s]Compiling num tokens (num_tokens=3328):  12%|█▏        | 7/58 [00:02<00:09,  5.59it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:02<00:04, 10.88it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:02<00:04, 10.88it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:02<00:04, 10.88it/s]Compiling num tokens (num_tokens=2560):  21%|██        | 12/58 [00:02<00:04, 10.88it/s]Compiling num tokens (num_tokens=2304):  21%|██        | 12/58 [00:02<00:04, 10.88it/s]Compiling num tokens (num_tokens=2048):  21%|██        | 12/58 [00:02<00:04, 10.88it/s]Compiling num tokens (num_tokens=1792):  21%|██        | 12/58 [00:02<00:04, 10.88it/s]

    Compiling num tokens (num_tokens=1536):  21%|██        | 12/58 [00:02<00:04, 10.88it/s]Compiling num tokens (num_tokens=1280):  21%|██        | 12/58 [00:02<00:04, 10.88it/s]Compiling num tokens (num_tokens=1024):  21%|██        | 12/58 [00:02<00:04, 10.88it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:02<00:01, 22.26it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:02<00:01, 22.26it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:02<00:01, 22.26it/s]Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:02<00:01, 22.26it/s]Compiling num tokens (num_tokens=768):  36%|███▌      | 21/58 [00:02<00:01, 22.26it/s]Compiling num tokens (num_tokens=704):  36%|███▌      | 21/58 [00:02<00:01, 22.26it/s]Compiling num tokens (num_tokens=640):  36%|███▌      | 21/58 [00:02<00:01, 22.26it/s]Compiling num tokens (num_tokens=576):  36%|███▌      | 21/58 [00:02<00:01, 22.26it/s]Compiling num tokens (num_tokens=512):  36%|███▌      | 21/58 [00:02<00:01, 22.26it/s]Compiling num tokens (num_tokens=480):  36%|███▌      | 21/58 [00:02<00:01, 22.26it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:02<00:00, 33.86it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:02<00:00, 33.86it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:02<00:00, 33.86it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:02<00:00, 33.86it/s]Compiling num tokens (num_tokens=352):  52%|█████▏    | 30/58 [00:02<00:00, 33.86it/s]Compiling num tokens (num_tokens=320):  52%|█████▏    | 30/58 [00:02<00:00, 33.86it/s]Compiling num tokens (num_tokens=288):  52%|█████▏    | 30/58 [00:02<00:00, 33.86it/s]Compiling num tokens (num_tokens=256):  52%|█████▏    | 30/58 [00:02<00:00, 33.86it/s]

    Compiling num tokens (num_tokens=240):  52%|█████▏    | 30/58 [00:02<00:00, 33.86it/s]Compiling num tokens (num_tokens=224):  52%|█████▏    | 30/58 [00:02<00:00, 33.86it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:02<00:00, 44.19it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:02<00:00, 44.19it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:02<00:00, 44.19it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:02<00:00, 44.19it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:02<00:00, 44.19it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:02<00:00, 44.19it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:02<00:00, 44.19it/s]Compiling num tokens (num_tokens=112):  67%|██████▋   | 39/58 [00:02<00:00, 44.19it/s]Compiling num tokens (num_tokens=96):  67%|██████▋   | 39/58 [00:02<00:00, 44.19it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:02<00:00, 50.09it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:02<00:00, 50.09it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:02<00:00, 50.09it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:02<00:00, 50.09it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:02<00:00, 50.09it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:02<00:00, 50.09it/s]

    Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:02<00:00, 50.09it/s]Compiling num tokens (num_tokens=20):  81%|████████  | 47/58 [00:02<00:00, 50.09it/s]Compiling num tokens (num_tokens=16):  81%|████████  | 47/58 [00:02<00:00, 50.09it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:02<00:00, 56.75it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:02<00:00, 56.75it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:02<00:00, 56.75it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:02<00:00, 56.75it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:02<00:00, 22.15it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.98 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.95 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.95 GB):   3%|▎         | 2/58 [00:00<00:03, 14.15it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.95 GB):   3%|▎         | 2/58 [00:00<00:03, 14.15it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=118.95 GB):   3%|▎         | 2/58 [00:00<00:03, 14.15it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.95 GB):   7%|▋         | 4/58 [00:00<00:03, 15.77it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.95 GB):   7%|▋         | 4/58 [00:00<00:03, 15.77it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.95 GB):   7%|▋         | 4/58 [00:00<00:03, 15.77it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.96 GB):   7%|▋         | 4/58 [00:00<00:03, 15.77it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.96 GB):  12%|█▏        | 7/58 [00:00<00:02, 18.72it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.96 GB):  12%|█▏        | 7/58 [00:00<00:02, 18.72it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=118.96 GB):  12%|█▏        | 7/58 [00:00<00:02, 18.72it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.96 GB):  12%|█▏        | 7/58 [00:00<00:02, 18.72it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.96 GB):  17%|█▋        | 10/58 [00:00<00:02, 22.07it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.96 GB):  17%|█▋        | 10/58 [00:00<00:02, 22.07it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.96 GB):  17%|█▋        | 10/58 [00:00<00:02, 22.07it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.96 GB):  17%|█▋        | 10/58 [00:00<00:02, 22.07it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.96 GB):  17%|█▋        | 10/58 [00:00<00:02, 22.07it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.96 GB):  24%|██▍       | 14/58 [00:00<00:01, 26.67it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.96 GB):  24%|██▍       | 14/58 [00:00<00:01, 26.67it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=118.96 GB):  24%|██▍       | 14/58 [00:00<00:01, 26.67it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.96 GB):  24%|██▍       | 14/58 [00:00<00:01, 26.67it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.96 GB):  24%|██▍       | 14/58 [00:00<00:01, 26.67it/s]Capturing num tokens (num_tokens=1536 avail_mem=117.80 GB):  24%|██▍       | 14/58 [00:00<00:01, 26.67it/s]Capturing num tokens (num_tokens=1536 avail_mem=117.80 GB):  33%|███▎      | 19/58 [00:00<00:01, 28.39it/s]Capturing num tokens (num_tokens=1280 avail_mem=117.80 GB):  33%|███▎      | 19/58 [00:00<00:01, 28.39it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=117.79 GB):  33%|███▎      | 19/58 [00:00<00:01, 28.39it/s]Capturing num tokens (num_tokens=960 avail_mem=117.79 GB):  33%|███▎      | 19/58 [00:00<00:01, 28.39it/s] Capturing num tokens (num_tokens=960 avail_mem=117.79 GB):  38%|███▊      | 22/58 [00:01<00:01, 20.82it/s]Capturing num tokens (num_tokens=896 avail_mem=117.79 GB):  38%|███▊      | 22/58 [00:01<00:01, 20.82it/s]

    Capturing num tokens (num_tokens=832 avail_mem=117.79 GB):  38%|███▊      | 22/58 [00:01<00:01, 20.82it/s]Capturing num tokens (num_tokens=768 avail_mem=117.78 GB):  38%|███▊      | 22/58 [00:01<00:01, 20.82it/s]Capturing num tokens (num_tokens=768 avail_mem=117.78 GB):  43%|████▎     | 25/58 [00:01<00:01, 19.55it/s]Capturing num tokens (num_tokens=704 avail_mem=117.78 GB):  43%|████▎     | 25/58 [00:01<00:01, 19.55it/s]Capturing num tokens (num_tokens=640 avail_mem=117.78 GB):  43%|████▎     | 25/58 [00:01<00:01, 19.55it/s]

    Capturing num tokens (num_tokens=576 avail_mem=117.78 GB):  43%|████▎     | 25/58 [00:01<00:01, 19.55it/s]Capturing num tokens (num_tokens=576 avail_mem=117.78 GB):  48%|████▊     | 28/58 [00:01<00:01, 17.62it/s]Capturing num tokens (num_tokens=512 avail_mem=117.76 GB):  48%|████▊     | 28/58 [00:01<00:01, 17.62it/s]Capturing num tokens (num_tokens=480 avail_mem=117.77 GB):  48%|████▊     | 28/58 [00:01<00:01, 17.62it/s]

    Capturing num tokens (num_tokens=480 avail_mem=117.77 GB):  52%|█████▏    | 30/58 [00:01<00:01, 17.21it/s]Capturing num tokens (num_tokens=448 avail_mem=117.77 GB):  52%|█████▏    | 30/58 [00:01<00:01, 17.21it/s]Capturing num tokens (num_tokens=416 avail_mem=117.77 GB):  52%|█████▏    | 30/58 [00:01<00:01, 17.21it/s]Capturing num tokens (num_tokens=416 avail_mem=117.77 GB):  55%|█████▌    | 32/58 [00:01<00:01, 17.24it/s]Capturing num tokens (num_tokens=384 avail_mem=117.77 GB):  55%|█████▌    | 32/58 [00:01<00:01, 17.24it/s]Capturing num tokens (num_tokens=352 avail_mem=117.76 GB):  55%|█████▌    | 32/58 [00:01<00:01, 17.24it/s]

    Capturing num tokens (num_tokens=352 avail_mem=117.76 GB):  59%|█████▊    | 34/58 [00:02<00:03,  7.26it/s]Capturing num tokens (num_tokens=320 avail_mem=117.76 GB):  59%|█████▊    | 34/58 [00:02<00:03,  7.26it/s]Capturing num tokens (num_tokens=288 avail_mem=117.76 GB):  59%|█████▊    | 34/58 [00:02<00:03,  7.26it/s]

    Capturing num tokens (num_tokens=288 avail_mem=117.76 GB):  62%|██████▏   | 36/58 [00:02<00:02,  7.76it/s]Capturing num tokens (num_tokens=256 avail_mem=117.76 GB):  62%|██████▏   | 36/58 [00:02<00:02,  7.76it/s]Capturing num tokens (num_tokens=240 avail_mem=117.77 GB):  62%|██████▏   | 36/58 [00:02<00:02,  7.76it/s]Capturing num tokens (num_tokens=240 avail_mem=117.77 GB):  66%|██████▌   | 38/58 [00:02<00:02,  9.13it/s]Capturing num tokens (num_tokens=224 avail_mem=117.77 GB):  66%|██████▌   | 38/58 [00:02<00:02,  9.13it/s]Capturing num tokens (num_tokens=208 avail_mem=117.77 GB):  66%|██████▌   | 38/58 [00:02<00:02,  9.13it/s]

    Capturing num tokens (num_tokens=208 avail_mem=117.77 GB):  69%|██████▉   | 40/58 [00:02<00:01, 10.48it/s]Capturing num tokens (num_tokens=192 avail_mem=117.77 GB):  69%|██████▉   | 40/58 [00:02<00:01, 10.48it/s]Capturing num tokens (num_tokens=176 avail_mem=117.76 GB):  69%|██████▉   | 40/58 [00:02<00:01, 10.48it/s]Capturing num tokens (num_tokens=176 avail_mem=117.76 GB):  72%|███████▏  | 42/58 [00:02<00:01, 12.12it/s]Capturing num tokens (num_tokens=160 avail_mem=117.76 GB):  72%|███████▏  | 42/58 [00:02<00:01, 12.12it/s]Capturing num tokens (num_tokens=144 avail_mem=117.76 GB):  72%|███████▏  | 42/58 [00:02<00:01, 12.12it/s]

    Capturing num tokens (num_tokens=144 avail_mem=117.76 GB):  76%|███████▌  | 44/58 [00:03<00:01, 13.42it/s]Capturing num tokens (num_tokens=128 avail_mem=117.76 GB):  76%|███████▌  | 44/58 [00:03<00:01, 13.42it/s]Capturing num tokens (num_tokens=112 avail_mem=117.75 GB):  76%|███████▌  | 44/58 [00:03<00:01, 13.42it/s]Capturing num tokens (num_tokens=96 avail_mem=117.75 GB):  76%|███████▌  | 44/58 [00:03<00:01, 13.42it/s] Capturing num tokens (num_tokens=96 avail_mem=117.75 GB):  81%|████████  | 47/58 [00:03<00:00, 15.68it/s]Capturing num tokens (num_tokens=80 avail_mem=117.75 GB):  81%|████████  | 47/58 [00:03<00:00, 15.68it/s]Capturing num tokens (num_tokens=64 avail_mem=117.75 GB):  81%|████████  | 47/58 [00:03<00:00, 15.68it/s]

    Capturing num tokens (num_tokens=48 avail_mem=117.75 GB):  81%|████████  | 47/58 [00:03<00:00, 15.68it/s]Capturing num tokens (num_tokens=48 avail_mem=117.75 GB):  86%|████████▌ | 50/58 [00:03<00:00, 17.09it/s]Capturing num tokens (num_tokens=32 avail_mem=117.74 GB):  86%|████████▌ | 50/58 [00:03<00:00, 17.09it/s]Capturing num tokens (num_tokens=28 avail_mem=117.74 GB):  86%|████████▌ | 50/58 [00:03<00:00, 17.09it/s]Capturing num tokens (num_tokens=24 avail_mem=117.74 GB):  86%|████████▌ | 50/58 [00:03<00:00, 17.09it/s]Capturing num tokens (num_tokens=20 avail_mem=117.74 GB):  86%|████████▌ | 50/58 [00:03<00:00, 17.09it/s]Capturing num tokens (num_tokens=20 avail_mem=117.74 GB):  93%|█████████▎| 54/58 [00:03<00:00, 21.25it/s]

    Capturing num tokens (num_tokens=16 avail_mem=117.73 GB):  93%|█████████▎| 54/58 [00:03<00:00, 21.25it/s]Capturing num tokens (num_tokens=12 avail_mem=117.73 GB):  93%|█████████▎| 54/58 [00:03<00:00, 21.25it/s]Capturing num tokens (num_tokens=8 avail_mem=117.73 GB):  93%|█████████▎| 54/58 [00:03<00:00, 21.25it/s] Capturing num tokens (num_tokens=4 avail_mem=117.73 GB):  93%|█████████▎| 54/58 [00:03<00:00, 21.25it/s]Capturing num tokens (num_tokens=4 avail_mem=117.73 GB): 100%|██████████| 58/58 [00:03<00:00, 25.47it/s]Capturing num tokens (num_tokens=4 avail_mem=117.73 GB): 100%|██████████| 58/58 [00:03<00:00, 16.28it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>



<strong style='color: #00008B;'>Non-stream response:</strong>



<strong style='color: #00008B;'>ChatCompletion(id='aa7e69944cf5400291270201d01632d0', choices=[Choice(finish_reason='tool_calls', index=0, logprobs=None, message=ChatCompletionMessage(content=None, refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=[ChatCompletionMessageFunctionToolCall(id='call_1496b6665d034762b1fad605', function=Function(arguments='{"location": "Tokyo"}', name='get_weather'), type='function', index=0), ChatCompletionMessageFunctionToolCall(id='call_77b8313282d54ecdb2a634d9', function=Function(arguments='{"city": "Tokyo"}', name='get_tourist_attractions'), type='function', index=1)], reasoning_content=None), matched_stop=None)], created=1774437096, model='meta-llama/Llama-3.2-1B-Instruct', object='chat.completion', service_tier=None, system_fingerprint=None, usage=CompletionUsage(completion_tokens=20, prompt_tokens=435, total_tokens=455, completion_tokens_details=None, prompt_tokens_details=None, reasoning_tokens=0), metadata={'weight_version': 'default'})</strong>



<strong style='color: #00008B;'>Streaming Response:</strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'></strong>



<strong style='color: #00008B;'>==== Tool Call ====</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=0, id='call_66180394a82b41878e5f455a', function=ChoiceDeltaToolCallFunction(arguments='{"location": "Tokyo"}', name='get_weather'), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=1, id='call_ef92d41340be45299e13b9a6', function=ChoiceDeltaToolCallFunction(arguments='{"city": "Tokyo"}', name='get_tourist_attractions'), type='function')</strong>


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
