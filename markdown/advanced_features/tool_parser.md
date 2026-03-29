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


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:176: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (
    [2026-03-29 08:08:42] numactl command not found, skipping NUMA node configuration for GPU. Install numactl (e.g., apt-get install numactl) to enable automatic NUMA binding.


    [2026-03-29 08:08:49] numactl command not found, skipping NUMA node configuration for GPU. Install numactl (e.g., apt-get install numactl) to enable automatic NUMA binding.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-03-29 08:08:51.284 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:08:51] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 08:08:51.284 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:08:51] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 08:08:51.284 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:08:51] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 08:08:51.284 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:08:51] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 08:08:51.284 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:08:51] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  25% Completed | 1/4 [00:00<00:01,  2.04it/s]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:01<00:01,  1.63it/s]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:01<00:00,  1.47it/s]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.42it/s]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.49it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:05<05:10,  5.45s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:05<05:10,  5.45s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:06<02:31,  2.71s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:06<02:31,  2.71s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:06<01:30,  1.65s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:06<01:30,  1.65s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:06<01:01,  1.14s/it]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:06<01:01,  1.14s/it]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:07<00:44,  1.19it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:07<00:44,  1.19it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:07<00:34,  1.51it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:07<00:34,  1.51it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:07<00:27,  1.89it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:07<00:27,  1.89it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:08<00:22,  2.27it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:08<00:22,  2.27it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:08<00:18,  2.71it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:08<00:18,  2.71it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:08<00:15,  3.19it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:08<00:15,  3.19it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:08<00:12,  3.65it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:08<00:12,  3.65it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:08<00:11,  4.13it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:08<00:11,  4.13it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:09<00:09,  4.63it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:09<00:09,  4.63it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:09<00:08,  5.13it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:09<00:08,  5.13it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:09<00:07,  5.74it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:09<00:07,  5.74it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:09<00:06,  6.35it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:09<00:06,  6.35it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:09<00:05,  7.05it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:09<00:05,  7.05it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:09<00:05,  7.05it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:09<00:04,  8.43it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:09<00:04,  8.43it/s]

    Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:09<00:04,  8.43it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:09<00:03, 10.00it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:09<00:03, 10.00it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:09<00:03, 10.00it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:09<00:02, 11.72it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:09<00:02, 11.72it/s]

    Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:10<00:02, 11.72it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:10<00:02, 13.57it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:10<00:02, 13.57it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:10<00:02, 13.57it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:10<00:02, 13.57it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:10<00:01, 16.15it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:10<00:01, 16.15it/s]

    Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:10<00:01, 16.15it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:10<00:01, 16.15it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:10<00:01, 18.90it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:10<00:01, 18.90it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:10<00:01, 18.90it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:10<00:01, 18.90it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:10<00:01, 21.37it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:10<00:01, 21.37it/s]

    Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:10<00:01, 21.37it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:10<00:01, 21.37it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:10<00:00, 23.35it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:10<00:00, 23.35it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:10<00:00, 23.35it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:10<00:00, 23.35it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:10<00:00, 23.35it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:10<00:00, 26.74it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:10<00:00, 26.74it/s]

    Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:10<00:00, 26.74it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:10<00:00, 26.74it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:10<00:00, 26.74it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:10<00:00, 28.68it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:10<00:00, 28.68it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:10<00:00, 28.68it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:10<00:00, 28.68it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:10<00:00, 28.68it/s]

    Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:10<00:00, 30.69it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:10<00:00, 30.69it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:10<00:00, 30.69it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:10<00:00, 30.69it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:10<00:00, 30.69it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:10<00:00, 30.69it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:11<00:00, 34.45it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:11<00:00, 34.45it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:11<00:00, 34.45it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:11<00:00, 34.45it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:11<00:00, 34.45it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:11<00:00,  5.24it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=104.00 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=104.00 GB):   2%|▏         | 1/58 [00:00<00:33,  1.68it/s]Capturing num tokens (num_tokens=7680 avail_mem=103.29 GB):   2%|▏         | 1/58 [00:00<00:33,  1.68it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=103.29 GB):   3%|▎         | 2/58 [00:01<00:31,  1.80it/s]Capturing num tokens (num_tokens=7168 avail_mem=103.29 GB):   3%|▎         | 2/58 [00:01<00:31,  1.80it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=103.29 GB):   5%|▌         | 3/58 [00:01<00:28,  1.92it/s]Capturing num tokens (num_tokens=6656 avail_mem=103.30 GB):   5%|▌         | 3/58 [00:01<00:28,  1.92it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=103.30 GB):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]Capturing num tokens (num_tokens=6144 avail_mem=103.30 GB):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=103.30 GB):   9%|▊         | 5/58 [00:02<00:26,  2.03it/s]Capturing num tokens (num_tokens=5632 avail_mem=103.30 GB):   9%|▊         | 5/58 [00:02<00:26,  2.03it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=103.30 GB):  10%|█         | 6/58 [00:02<00:23,  2.25it/s]Capturing num tokens (num_tokens=5120 avail_mem=102.33 GB):  10%|█         | 6/58 [00:02<00:23,  2.25it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=102.33 GB):  12%|█▏        | 7/58 [00:03<00:21,  2.41it/s]Capturing num tokens (num_tokens=4608 avail_mem=102.33 GB):  12%|█▏        | 7/58 [00:03<00:21,  2.41it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=102.33 GB):  14%|█▍        | 8/58 [00:03<00:18,  2.68it/s]Capturing num tokens (num_tokens=4096 avail_mem=102.34 GB):  14%|█▍        | 8/58 [00:03<00:18,  2.68it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=102.34 GB):  16%|█▌        | 9/58 [00:03<00:16,  2.92it/s]Capturing num tokens (num_tokens=3840 avail_mem=101.92 GB):  16%|█▌        | 9/58 [00:03<00:16,  2.92it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=101.92 GB):  17%|█▋        | 10/58 [00:04<00:15,  3.15it/s]Capturing num tokens (num_tokens=3584 avail_mem=101.91 GB):  17%|█▋        | 10/58 [00:04<00:15,  3.15it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=101.91 GB):  19%|█▉        | 11/58 [00:04<00:13,  3.43it/s]Capturing num tokens (num_tokens=3328 avail_mem=101.91 GB):  19%|█▉        | 11/58 [00:04<00:13,  3.43it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=101.91 GB):  21%|██        | 12/58 [00:04<00:12,  3.76it/s]Capturing num tokens (num_tokens=3072 avail_mem=101.91 GB):  21%|██        | 12/58 [00:04<00:12,  3.76it/s]Capturing num tokens (num_tokens=3072 avail_mem=101.91 GB):  22%|██▏       | 13/58 [00:04<00:11,  4.07it/s]Capturing num tokens (num_tokens=2816 avail_mem=101.91 GB):  22%|██▏       | 13/58 [00:04<00:11,  4.07it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=101.91 GB):  24%|██▍       | 14/58 [00:04<00:09,  4.46it/s]Capturing num tokens (num_tokens=2560 avail_mem=101.91 GB):  24%|██▍       | 14/58 [00:04<00:09,  4.46it/s]Capturing num tokens (num_tokens=2560 avail_mem=101.91 GB):  26%|██▌       | 15/58 [00:05<00:08,  4.89it/s]Capturing num tokens (num_tokens=2304 avail_mem=101.91 GB):  26%|██▌       | 15/58 [00:05<00:08,  4.89it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=101.91 GB):  28%|██▊       | 16/58 [00:05<00:07,  5.33it/s]Capturing num tokens (num_tokens=2048 avail_mem=101.91 GB):  28%|██▊       | 16/58 [00:05<00:07,  5.33it/s]Capturing num tokens (num_tokens=2048 avail_mem=101.91 GB):  29%|██▉       | 17/58 [00:05<00:07,  5.83it/s]Capturing num tokens (num_tokens=1792 avail_mem=101.91 GB):  29%|██▉       | 17/58 [00:05<00:07,  5.83it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=101.91 GB):  31%|███       | 18/58 [00:05<00:06,  6.42it/s]Capturing num tokens (num_tokens=1536 avail_mem=101.90 GB):  31%|███       | 18/58 [00:05<00:06,  6.42it/s]Capturing num tokens (num_tokens=1536 avail_mem=101.90 GB):  33%|███▎      | 19/58 [00:05<00:05,  7.07it/s]Capturing num tokens (num_tokens=1280 avail_mem=101.91 GB):  33%|███▎      | 19/58 [00:05<00:05,  7.07it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=101.91 GB):  33%|███▎      | 19/58 [00:05<00:05,  7.07it/s]Capturing num tokens (num_tokens=1024 avail_mem=101.91 GB):  36%|███▌      | 21/58 [00:05<00:04,  8.59it/s]Capturing num tokens (num_tokens=960 avail_mem=101.90 GB):  36%|███▌      | 21/58 [00:05<00:04,  8.59it/s] Capturing num tokens (num_tokens=896 avail_mem=101.90 GB):  36%|███▌      | 21/58 [00:05<00:04,  8.59it/s]

    Capturing num tokens (num_tokens=896 avail_mem=101.90 GB):  40%|███▉      | 23/58 [00:05<00:03,  9.89it/s]Capturing num tokens (num_tokens=832 avail_mem=101.90 GB):  40%|███▉      | 23/58 [00:05<00:03,  9.89it/s]Capturing num tokens (num_tokens=768 avail_mem=101.89 GB):  40%|███▉      | 23/58 [00:05<00:03,  9.89it/s]Capturing num tokens (num_tokens=768 avail_mem=101.89 GB):  43%|████▎     | 25/58 [00:06<00:02, 11.15it/s]Capturing num tokens (num_tokens=704 avail_mem=101.89 GB):  43%|████▎     | 25/58 [00:06<00:02, 11.15it/s]

    Capturing num tokens (num_tokens=640 avail_mem=101.88 GB):  43%|████▎     | 25/58 [00:06<00:02, 11.15it/s]Capturing num tokens (num_tokens=640 avail_mem=101.88 GB):  47%|████▋     | 27/58 [00:06<00:02, 12.39it/s]Capturing num tokens (num_tokens=576 avail_mem=101.88 GB):  47%|████▋     | 27/58 [00:06<00:02, 12.39it/s]Capturing num tokens (num_tokens=512 avail_mem=101.87 GB):  47%|████▋     | 27/58 [00:06<00:02, 12.39it/s]Capturing num tokens (num_tokens=512 avail_mem=101.87 GB):  50%|█████     | 29/58 [00:06<00:02, 13.66it/s]Capturing num tokens (num_tokens=480 avail_mem=101.87 GB):  50%|█████     | 29/58 [00:06<00:02, 13.66it/s]

    Capturing num tokens (num_tokens=448 avail_mem=101.87 GB):  50%|█████     | 29/58 [00:06<00:02, 13.66it/s]Capturing num tokens (num_tokens=448 avail_mem=101.87 GB):  53%|█████▎    | 31/58 [00:06<00:01, 14.96it/s]Capturing num tokens (num_tokens=416 avail_mem=101.87 GB):  53%|█████▎    | 31/58 [00:06<00:01, 14.96it/s]Capturing num tokens (num_tokens=384 avail_mem=101.57 GB):  53%|█████▎    | 31/58 [00:06<00:01, 14.96it/s]

    Capturing num tokens (num_tokens=384 avail_mem=101.57 GB):  57%|█████▋    | 33/58 [00:06<00:02, 10.86it/s]Capturing num tokens (num_tokens=352 avail_mem=101.56 GB):  57%|█████▋    | 33/58 [00:06<00:02, 10.86it/s]

    Capturing num tokens (num_tokens=320 avail_mem=101.56 GB):  57%|█████▋    | 33/58 [00:06<00:02, 10.86it/s]Capturing num tokens (num_tokens=320 avail_mem=101.56 GB):  60%|██████    | 35/58 [00:06<00:02,  8.68it/s]Capturing num tokens (num_tokens=288 avail_mem=101.56 GB):  60%|██████    | 35/58 [00:06<00:02,  8.68it/s]

    Capturing num tokens (num_tokens=256 avail_mem=101.55 GB):  60%|██████    | 35/58 [00:07<00:02,  8.68it/s]Capturing num tokens (num_tokens=256 avail_mem=101.55 GB):  64%|██████▍   | 37/58 [00:07<00:02,  9.20it/s]Capturing num tokens (num_tokens=240 avail_mem=101.55 GB):  64%|██████▍   | 37/58 [00:07<00:02,  9.20it/s]Capturing num tokens (num_tokens=224 avail_mem=101.54 GB):  64%|██████▍   | 37/58 [00:07<00:02,  9.20it/s]

    Capturing num tokens (num_tokens=224 avail_mem=101.54 GB):  67%|██████▋   | 39/58 [00:07<00:01, 10.51it/s]Capturing num tokens (num_tokens=208 avail_mem=101.54 GB):  67%|██████▋   | 39/58 [00:07<00:01, 10.51it/s]Capturing num tokens (num_tokens=192 avail_mem=101.54 GB):  67%|██████▋   | 39/58 [00:07<00:01, 10.51it/s]Capturing num tokens (num_tokens=192 avail_mem=101.54 GB):  71%|███████   | 41/58 [00:07<00:01, 11.73it/s]Capturing num tokens (num_tokens=176 avail_mem=101.53 GB):  71%|███████   | 41/58 [00:07<00:01, 11.73it/s]Capturing num tokens (num_tokens=160 avail_mem=101.53 GB):  71%|███████   | 41/58 [00:07<00:01, 11.73it/s]

    Capturing num tokens (num_tokens=160 avail_mem=101.53 GB):  74%|███████▍  | 43/58 [00:07<00:01, 12.85it/s]Capturing num tokens (num_tokens=144 avail_mem=101.52 GB):  74%|███████▍  | 43/58 [00:07<00:01, 12.85it/s]Capturing num tokens (num_tokens=128 avail_mem=101.53 GB):  74%|███████▍  | 43/58 [00:07<00:01, 12.85it/s]Capturing num tokens (num_tokens=128 avail_mem=101.53 GB):  78%|███████▊  | 45/58 [00:07<00:00, 14.15it/s]Capturing num tokens (num_tokens=112 avail_mem=101.53 GB):  78%|███████▊  | 45/58 [00:07<00:00, 14.15it/s]Capturing num tokens (num_tokens=96 avail_mem=101.52 GB):  78%|███████▊  | 45/58 [00:07<00:00, 14.15it/s] 

    Capturing num tokens (num_tokens=96 avail_mem=101.52 GB):  81%|████████  | 47/58 [00:07<00:00, 15.24it/s]Capturing num tokens (num_tokens=80 avail_mem=101.52 GB):  81%|████████  | 47/58 [00:07<00:00, 15.24it/s]Capturing num tokens (num_tokens=64 avail_mem=101.51 GB):  81%|████████  | 47/58 [00:07<00:00, 15.24it/s]Capturing num tokens (num_tokens=64 avail_mem=101.51 GB):  84%|████████▍ | 49/58 [00:07<00:00, 16.24it/s]Capturing num tokens (num_tokens=48 avail_mem=101.51 GB):  84%|████████▍ | 49/58 [00:07<00:00, 16.24it/s]Capturing num tokens (num_tokens=32 avail_mem=101.51 GB):  84%|████████▍ | 49/58 [00:07<00:00, 16.24it/s]

    Capturing num tokens (num_tokens=28 avail_mem=101.51 GB):  84%|████████▍ | 49/58 [00:07<00:00, 16.24it/s]Capturing num tokens (num_tokens=28 avail_mem=101.51 GB):  90%|████████▉ | 52/58 [00:08<00:00, 17.43it/s]Capturing num tokens (num_tokens=24 avail_mem=101.50 GB):  90%|████████▉ | 52/58 [00:08<00:00, 17.43it/s]Capturing num tokens (num_tokens=20 avail_mem=101.50 GB):  90%|████████▉ | 52/58 [00:08<00:00, 17.43it/s]Capturing num tokens (num_tokens=20 avail_mem=101.50 GB):  93%|█████████▎| 54/58 [00:08<00:00, 17.94it/s]Capturing num tokens (num_tokens=16 avail_mem=101.49 GB):  93%|█████████▎| 54/58 [00:08<00:00, 17.94it/s]

    Capturing num tokens (num_tokens=12 avail_mem=101.49 GB):  93%|█████████▎| 54/58 [00:08<00:00, 17.94it/s]Capturing num tokens (num_tokens=12 avail_mem=101.49 GB):  97%|█████████▋| 56/58 [00:08<00:00, 18.42it/s]Capturing num tokens (num_tokens=8 avail_mem=101.49 GB):  97%|█████████▋| 56/58 [00:08<00:00, 18.42it/s] Capturing num tokens (num_tokens=4 avail_mem=101.48 GB):  97%|█████████▋| 56/58 [00:08<00:00, 18.42it/s]Capturing num tokens (num_tokens=4 avail_mem=101.48 GB): 100%|██████████| 58/58 [00:08<00:00,  6.98it/s]


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



<strong style='color: #00008B;'>ChatCompletion(id='ddea12b89a2d4d5d8f1ebaabec02117e', choices=[Choice(finish_reason='tool_calls', index=0, logprobs=None, message=ChatCompletionMessage(content="To determine the current weather in Boston, I will use the `get_current_weather` function by providing the city name, state abbreviation, and the temperature unit in which you prefer the information. Since the user did not specify the unit, I will assume Celsius for the response.\n\nReasoning: The `get_current_weather` function requires the city name, state abbreviation, and the temperature unit. Boston is the city, and Massachusetts' state abbreviation is MA. I will use Celsius as the default unit for the temperature.", refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=[ChatCompletionMessageFunctionToolCall(id='call_484cdda0d07848f4915030d9', function=Function(arguments='{"city": "Boston", "state": "MA", "unit": "celsius"}', name='get_current_weather'), type='function', index=0)], reasoning_content=None), matched_stop=None)], created=1774771765, model='Qwen/Qwen2.5-7B-Instruct', object='chat.completion', service_tier=None, system_fingerprint=None, usage=CompletionUsage(completion_tokens=137, prompt_tokens=290, total_tokens=427, completion_tokens_details=None, prompt_tokens_details=None, reasoning_tokens=0), metadata={'weight_version': 'default'})</strong>



<strong style='color: #00008B;'>==== content ====</strong>



<strong style='color: #00008B;'>To determine the current weather in Boston, I will use the `get_current_weather` function by providing the city name, state abbreviation, and the temperature unit in which you prefer the information. Since the user did not specify the unit, I will assume Celsius for the response.<br><br>Reasoning: The `get_current_weather` function requires the city name, state abbreviation, and the temperature unit. Boston is the city, and Massachusetts' state abbreviation is MA. I will use Celsius as the default unit for the temperature.</strong>



<strong style='color: #00008B;'>==== tool_calls ====</strong>



<strong style='color: #00008B;'>[ChatCompletionMessageFunctionToolCall(id='call_484cdda0d07848f4915030d9', function=Function(arguments='{"city": "Boston", "state": "MA", "unit": "celsius"}', name='get_current_weather'), type='function', index=0)]</strong>


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



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=0, id='call_74bc0ece69d7426494dbf4b9', function=ChoiceDeltaToolCallFunction(arguments='', name='get_current_weather'), type='function')</strong>



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



<strong style='color: #00008B;'>Updated message history: [{'role': 'user', 'content': "What's the weather like in Boston today? Output a reasoning before act, then use the tools to help you."}, ChatCompletionMessage(content="To determine the current weather in Boston, I will use the `get_current_weather` function by providing the city name, state abbreviation, and the temperature unit in which you prefer the information. Since the user did not specify the unit, I will assume Celsius for the response.\n\nReasoning: The `get_current_weather` function requires the city name, state abbreviation, and the temperature unit. Boston is the city, and Massachusetts' state abbreviation is MA. I will use Celsius as the default unit for the temperature.", refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=[ChatCompletionMessageFunctionToolCall(id='call_484cdda0d07848f4915030d9', function=Function(arguments='{"city": "Boston", "state": "MA", "unit": "celsius"}', name='get_current_weather'), type='function', index=0)], reasoning_content=None), {'role': 'tool', 'tool_call_id': 'call_484cdda0d07848f4915030d9', 'content': "The weather in Boston, MA is 85 degrees celsius. It is partly cloudly, with highs in the 90's.", 'name': 'get_current_weather'}]</strong>


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



<strong style='color: #00008B;'>ChatCompletion(id='e7d2155ec20640879860af359a0c2a67', choices=[Choice(finish_reason='tool_calls', index=0, logprobs=None, message=ChatCompletionMessage(content='There seems to be an error in the response as the temperature in Celsius for Boston should not exceed 35-40 degrees, given the typical climate. Let me correct this by rechecking with the function using the correct unit, which is likely Fahrenheit.\n\nReasoning: The response indicates an unusually high temperature in Celsius, which is not plausible for Boston. I will recheck the weather using the correct unit, which is likely Fahrenheit.', refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=[ChatCompletionMessageFunctionToolCall(id='call_7e5e35bf2ea44b6aa5d23f00', function=Function(arguments='{"city": "Boston", "state": "MA", "unit": "fahrenheit"}', name='get_current_weather'), type='function', index=0)], reasoning_content=None), matched_stop=None)], created=1774771767, model='Qwen/Qwen2.5-7B-Instruct', object='chat.completion', service_tier=None, system_fingerprint=None, usage=CompletionUsage(completion_tokens=122, prompt_tokens=473, total_tokens=595, completion_tokens_details=None, prompt_tokens_details=None, reasoning_tokens=0), metadata={'weight_version': 'default'})</strong>



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

    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-29 08:09:42.243 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:09:42] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 08:09:42.243 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:09:42] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 08:09:42.243 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:09:42] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 08:09:42.243 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:09:42] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 08:09:42.243 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:09:42] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  25% Completed | 1/4 [00:00<00:01,  2.38it/s]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:01<00:01,  1.89it/s]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:01<00:00,  1.74it/s]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.69it/s]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.76it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:56,  3.10s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:56,  3.10s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:24,  1.51s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:24,  1.51s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.10it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.10it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.61it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.61it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:24,  2.21it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:24,  2.21it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.81it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.81it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.54it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.54it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.23it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.23it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.08it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.08it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.08it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.73it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.73it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.73it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.19it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.19it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.19it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:04,  9.76it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:04,  9.76it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04,  9.76it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 11.66it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 11.66it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.66it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 11.66it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 14.84it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 14.84it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 14.84it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 14.84it/s]

    Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 14.84it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 19.92it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 19.92it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 19.92it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 19.92it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 19.92it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 19.92it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 26.29it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 26.29it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 26.29it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 26.29it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 26.29it/s]

    Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:05<00:01, 26.29it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:05<00:01, 26.29it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:05<00:00, 33.60it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:05<00:00, 33.60it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:05<00:00, 33.60it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:05<00:00, 33.60it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:05<00:00, 33.60it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:05<00:00, 33.60it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:05<00:00, 33.60it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 38.98it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 38.98it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:05<00:00, 38.98it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:05<00:00, 38.98it/s]

    Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:05<00:00, 38.98it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:05<00:00, 38.98it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:05<00:00, 38.98it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:05<00:00, 42.28it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:05<00:00, 42.28it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:05<00:00, 42.28it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:05<00:00, 42.28it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:05<00:00, 42.28it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:05<00:00, 42.28it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:05<00:00, 42.28it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:05<00:00, 46.37it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:05<00:00, 46.37it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:05<00:00, 46.37it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:06<00:00, 46.37it/s]

    Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:06<00:00, 46.37it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:06<00:00, 46.37it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  9.61it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=114.43 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=114.43 GB):   2%|▏         | 1/58 [00:00<00:18,  3.09it/s]Capturing num tokens (num_tokens=7680 avail_mem=114.39 GB):   2%|▏         | 1/58 [00:00<00:18,  3.09it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=114.39 GB):   3%|▎         | 2/58 [00:00<00:16,  3.40it/s]Capturing num tokens (num_tokens=7168 avail_mem=114.39 GB):   3%|▎         | 2/58 [00:00<00:16,  3.40it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=114.39 GB):   5%|▌         | 3/58 [00:00<00:15,  3.65it/s]Capturing num tokens (num_tokens=6656 avail_mem=114.40 GB):   5%|▌         | 3/58 [00:00<00:15,  3.65it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=114.40 GB):   7%|▋         | 4/58 [00:01<00:13,  3.94it/s]Capturing num tokens (num_tokens=6144 avail_mem=114.40 GB):   7%|▋         | 4/58 [00:01<00:13,  3.94it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=114.40 GB):   9%|▊         | 5/58 [00:01<00:12,  4.19it/s]Capturing num tokens (num_tokens=5632 avail_mem=114.41 GB):   9%|▊         | 5/58 [00:01<00:12,  4.19it/s]Capturing num tokens (num_tokens=5632 avail_mem=114.41 GB):  10%|█         | 6/58 [00:01<00:11,  4.58it/s]Capturing num tokens (num_tokens=5120 avail_mem=114.41 GB):  10%|█         | 6/58 [00:01<00:11,  4.58it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=114.41 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.89it/s]Capturing num tokens (num_tokens=4608 avail_mem=114.41 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.89it/s]Capturing num tokens (num_tokens=4608 avail_mem=114.41 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.30it/s]Capturing num tokens (num_tokens=4096 avail_mem=114.42 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.30it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=114.42 GB):  16%|█▌        | 9/58 [00:01<00:08,  5.71it/s]Capturing num tokens (num_tokens=3840 avail_mem=114.42 GB):  16%|█▌        | 9/58 [00:01<00:08,  5.71it/s]Capturing num tokens (num_tokens=3840 avail_mem=114.42 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.11it/s]Capturing num tokens (num_tokens=3584 avail_mem=114.42 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.11it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=114.42 GB):  19%|█▉        | 11/58 [00:02<00:07,  6.58it/s]Capturing num tokens (num_tokens=3328 avail_mem=114.42 GB):  19%|█▉        | 11/58 [00:02<00:07,  6.58it/s]Capturing num tokens (num_tokens=3328 avail_mem=114.42 GB):  21%|██        | 12/58 [00:02<00:06,  7.13it/s]Capturing num tokens (num_tokens=3072 avail_mem=114.42 GB):  21%|██        | 12/58 [00:02<00:06,  7.13it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=114.42 GB):  22%|██▏       | 13/58 [00:02<00:05,  7.64it/s]Capturing num tokens (num_tokens=2816 avail_mem=114.42 GB):  22%|██▏       | 13/58 [00:02<00:05,  7.64it/s]Capturing num tokens (num_tokens=2560 avail_mem=114.42 GB):  22%|██▏       | 13/58 [00:02<00:05,  7.64it/s]Capturing num tokens (num_tokens=2560 avail_mem=114.42 GB):  26%|██▌       | 15/58 [00:02<00:04,  9.11it/s]Capturing num tokens (num_tokens=2304 avail_mem=114.42 GB):  26%|██▌       | 15/58 [00:02<00:04,  9.11it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=114.42 GB):  26%|██▌       | 15/58 [00:02<00:04,  9.11it/s]Capturing num tokens (num_tokens=2048 avail_mem=114.42 GB):  29%|██▉       | 17/58 [00:02<00:03, 10.69it/s]Capturing num tokens (num_tokens=1792 avail_mem=114.42 GB):  29%|██▉       | 17/58 [00:02<00:03, 10.69it/s]Capturing num tokens (num_tokens=1536 avail_mem=113.42 GB):  29%|██▉       | 17/58 [00:02<00:03, 10.69it/s]Capturing num tokens (num_tokens=1536 avail_mem=113.42 GB):  33%|███▎      | 19/58 [00:02<00:03, 12.38it/s]Capturing num tokens (num_tokens=1280 avail_mem=113.42 GB):  33%|███▎      | 19/58 [00:02<00:03, 12.38it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=113.42 GB):  33%|███▎      | 19/58 [00:02<00:03, 12.38it/s]Capturing num tokens (num_tokens=960 avail_mem=113.42 GB):  33%|███▎      | 19/58 [00:02<00:03, 12.38it/s] Capturing num tokens (num_tokens=960 avail_mem=113.42 GB):  38%|███▊      | 22/58 [00:02<00:02, 15.73it/s]Capturing num tokens (num_tokens=896 avail_mem=113.42 GB):  38%|███▊      | 22/58 [00:02<00:02, 15.73it/s]Capturing num tokens (num_tokens=832 avail_mem=113.41 GB):  38%|███▊      | 22/58 [00:03<00:02, 15.73it/s]Capturing num tokens (num_tokens=768 avail_mem=113.41 GB):  38%|███▊      | 22/58 [00:03<00:02, 15.73it/s]Capturing num tokens (num_tokens=768 avail_mem=113.41 GB):  43%|████▎     | 25/58 [00:03<00:01, 18.83it/s]Capturing num tokens (num_tokens=704 avail_mem=113.40 GB):  43%|████▎     | 25/58 [00:03<00:01, 18.83it/s]

    Capturing num tokens (num_tokens=640 avail_mem=113.40 GB):  43%|████▎     | 25/58 [00:03<00:01, 18.83it/s]Capturing num tokens (num_tokens=576 avail_mem=113.40 GB):  43%|████▎     | 25/58 [00:03<00:01, 18.83it/s]Capturing num tokens (num_tokens=576 avail_mem=113.40 GB):  48%|████▊     | 28/58 [00:03<00:01, 21.69it/s]Capturing num tokens (num_tokens=512 avail_mem=113.39 GB):  48%|████▊     | 28/58 [00:03<00:01, 21.69it/s]Capturing num tokens (num_tokens=480 avail_mem=113.39 GB):  48%|████▊     | 28/58 [00:03<00:01, 21.69it/s]Capturing num tokens (num_tokens=448 avail_mem=113.39 GB):  48%|████▊     | 28/58 [00:03<00:01, 21.69it/s]Capturing num tokens (num_tokens=416 avail_mem=113.38 GB):  48%|████▊     | 28/58 [00:03<00:01, 21.69it/s]Capturing num tokens (num_tokens=416 avail_mem=113.38 GB):  55%|█████▌    | 32/58 [00:03<00:01, 25.60it/s]Capturing num tokens (num_tokens=384 avail_mem=113.38 GB):  55%|█████▌    | 32/58 [00:03<00:01, 25.60it/s]

    Capturing num tokens (num_tokens=352 avail_mem=113.37 GB):  55%|█████▌    | 32/58 [00:03<00:01, 25.60it/s]Capturing num tokens (num_tokens=320 avail_mem=113.37 GB):  55%|█████▌    | 32/58 [00:03<00:01, 25.60it/s]Capturing num tokens (num_tokens=288 avail_mem=113.37 GB):  55%|█████▌    | 32/58 [00:03<00:01, 25.60it/s]Capturing num tokens (num_tokens=288 avail_mem=113.37 GB):  62%|██████▏   | 36/58 [00:03<00:00, 28.78it/s]Capturing num tokens (num_tokens=256 avail_mem=113.36 GB):  62%|██████▏   | 36/58 [00:03<00:00, 28.78it/s]Capturing num tokens (num_tokens=240 avail_mem=113.36 GB):  62%|██████▏   | 36/58 [00:03<00:00, 28.78it/s]Capturing num tokens (num_tokens=224 avail_mem=113.36 GB):  62%|██████▏   | 36/58 [00:03<00:00, 28.78it/s]Capturing num tokens (num_tokens=208 avail_mem=113.35 GB):  62%|██████▏   | 36/58 [00:03<00:00, 28.78it/s]Capturing num tokens (num_tokens=208 avail_mem=113.35 GB):  69%|██████▉   | 40/58 [00:03<00:00, 31.48it/s]Capturing num tokens (num_tokens=192 avail_mem=113.35 GB):  69%|██████▉   | 40/58 [00:03<00:00, 31.48it/s]

    Capturing num tokens (num_tokens=176 avail_mem=113.35 GB):  69%|██████▉   | 40/58 [00:03<00:00, 31.48it/s]Capturing num tokens (num_tokens=160 avail_mem=113.34 GB):  69%|██████▉   | 40/58 [00:03<00:00, 31.48it/s]Capturing num tokens (num_tokens=144 avail_mem=113.34 GB):  69%|██████▉   | 40/58 [00:03<00:00, 31.48it/s]Capturing num tokens (num_tokens=144 avail_mem=113.34 GB):  76%|███████▌  | 44/58 [00:03<00:00, 33.14it/s]Capturing num tokens (num_tokens=128 avail_mem=113.35 GB):  76%|███████▌  | 44/58 [00:03<00:00, 33.14it/s]Capturing num tokens (num_tokens=112 avail_mem=113.35 GB):  76%|███████▌  | 44/58 [00:03<00:00, 33.14it/s]Capturing num tokens (num_tokens=96 avail_mem=113.34 GB):  76%|███████▌  | 44/58 [00:03<00:00, 33.14it/s] Capturing num tokens (num_tokens=80 avail_mem=113.33 GB):  76%|███████▌  | 44/58 [00:03<00:00, 33.14it/s]Capturing num tokens (num_tokens=80 avail_mem=113.33 GB):  83%|████████▎ | 48/58 [00:03<00:00, 34.61it/s]Capturing num tokens (num_tokens=64 avail_mem=113.33 GB):  83%|████████▎ | 48/58 [00:03<00:00, 34.61it/s]

    Capturing num tokens (num_tokens=48 avail_mem=113.33 GB):  83%|████████▎ | 48/58 [00:03<00:00, 34.61it/s]Capturing num tokens (num_tokens=32 avail_mem=113.32 GB):  83%|████████▎ | 48/58 [00:03<00:00, 34.61it/s]Capturing num tokens (num_tokens=28 avail_mem=113.32 GB):  83%|████████▎ | 48/58 [00:03<00:00, 34.61it/s]Capturing num tokens (num_tokens=28 avail_mem=113.32 GB):  90%|████████▉ | 52/58 [00:03<00:00, 35.69it/s]Capturing num tokens (num_tokens=24 avail_mem=113.32 GB):  90%|████████▉ | 52/58 [00:03<00:00, 35.69it/s]Capturing num tokens (num_tokens=20 avail_mem=113.31 GB):  90%|████████▉ | 52/58 [00:03<00:00, 35.69it/s]Capturing num tokens (num_tokens=16 avail_mem=113.31 GB):  90%|████████▉ | 52/58 [00:03<00:00, 35.69it/s]Capturing num tokens (num_tokens=12 avail_mem=113.31 GB):  90%|████████▉ | 52/58 [00:03<00:00, 35.69it/s]Capturing num tokens (num_tokens=12 avail_mem=113.31 GB):  97%|█████████▋| 56/58 [00:03<00:00, 36.42it/s]Capturing num tokens (num_tokens=8 avail_mem=113.30 GB):  97%|█████████▋| 56/58 [00:03<00:00, 36.42it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=113.30 GB):  97%|█████████▋| 56/58 [00:03<00:00, 36.42it/s]Capturing num tokens (num_tokens=4 avail_mem=113.30 GB): 100%|██████████| 58/58 [00:03<00:00, 14.56it/s]



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

    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(
    The tool_call_parser 'qwen25' is deprecated. Please use 'qwen' instead.


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:176: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (
    [2026-03-29 08:10:08] numactl command not found, skipping NUMA node configuration for GPU. Install numactl (e.g., apt-get install numactl) to enable automatic NUMA binding.


    [2026-03-29 08:10:15] numactl command not found, skipping NUMA node configuration for GPU. Install numactl (e.g., apt-get install numactl) to enable automatic NUMA binding.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-03-29 08:10:17.368 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:10:17] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 08:10:17.368 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:10:17] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 08:10:17.368 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:10:17] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 08:10:17.368 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:10:17] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 08:10:17.368 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:10:17] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  25% Completed | 1/4 [00:00<00:01,  2.19it/s]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:01<00:01,  1.78it/s]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:01<00:00,  1.70it/s]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.60it/s]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.67it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:53,  3.04s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:53,  3.04s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:23,  1.48s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:23,  1.48s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.12it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.12it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.64it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.64it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:23,  2.24it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:23,  2.24it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.88it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.88it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.63it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.63it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.41it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.41it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.29it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.29it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.29it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.98it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.98it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.98it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.46it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.46it/s]

    Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.46it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:04<00:04, 10.05it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:04<00:04, 10.05it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04, 10.05it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 12.00it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 12.00it/s]

    Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 12.00it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 12.00it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 15.25it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 15.25it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 15.25it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 15.25it/s]Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 15.25it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 20.37it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 20.37it/s]

    Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 20.37it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 20.37it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 20.37it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 20.37it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 26.95it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 26.95it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 26.95it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 26.95it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 26.95it/s]Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:05<00:01, 26.95it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:05<00:01, 26.95it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:05<00:00, 34.43it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:05<00:00, 34.43it/s]

    Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:05<00:00, 34.43it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:05<00:00, 34.43it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:05<00:00, 34.43it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:05<00:00, 34.43it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:05<00:00, 34.43it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 39.84it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 39.84it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:05<00:00, 39.84it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:05<00:00, 39.84it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:05<00:00, 39.84it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:05<00:00, 39.84it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:05<00:00, 39.84it/s] 

    Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:05<00:00, 43.02it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:05<00:00, 43.02it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:05<00:00, 43.02it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:05<00:00, 43.02it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:05<00:00, 43.02it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:05<00:00, 43.02it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:05<00:00, 43.02it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:05<00:00, 47.13it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:05<00:00, 47.13it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:05<00:00, 47.13it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:05<00:00, 47.13it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:05<00:00, 47.13it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:05<00:00, 47.13it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:05<00:00,  9.81it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=104.65 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=104.65 GB):   2%|▏         | 1/58 [00:00<00:16,  3.51it/s]Capturing num tokens (num_tokens=7680 avail_mem=104.62 GB):   2%|▏         | 1/58 [00:00<00:16,  3.51it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=104.62 GB):   3%|▎         | 2/58 [00:00<00:15,  3.61it/s]Capturing num tokens (num_tokens=7168 avail_mem=104.62 GB):   3%|▎         | 2/58 [00:00<00:15,  3.61it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=104.62 GB):   5%|▌         | 3/58 [00:00<00:14,  3.78it/s]Capturing num tokens (num_tokens=6656 avail_mem=104.59 GB):   5%|▌         | 3/58 [00:00<00:14,  3.78it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=104.59 GB):   7%|▋         | 4/58 [00:01<00:14,  3.78it/s]Capturing num tokens (num_tokens=6144 avail_mem=104.59 GB):   7%|▋         | 4/58 [00:01<00:14,  3.78it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=104.59 GB):   9%|▊         | 5/58 [00:01<00:12,  4.11it/s]Capturing num tokens (num_tokens=5632 avail_mem=104.59 GB):   9%|▊         | 5/58 [00:01<00:12,  4.11it/s]Capturing num tokens (num_tokens=5632 avail_mem=104.59 GB):  10%|█         | 6/58 [00:01<00:11,  4.54it/s]Capturing num tokens (num_tokens=5120 avail_mem=104.59 GB):  10%|█         | 6/58 [00:01<00:11,  4.54it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=104.59 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.95it/s]Capturing num tokens (num_tokens=4608 avail_mem=104.60 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.95it/s]Capturing num tokens (num_tokens=4608 avail_mem=104.60 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.46it/s]Capturing num tokens (num_tokens=4096 avail_mem=104.60 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.46it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=104.60 GB):  16%|█▌        | 9/58 [00:01<00:08,  5.87it/s]Capturing num tokens (num_tokens=3840 avail_mem=104.61 GB):  16%|█▌        | 9/58 [00:01<00:08,  5.87it/s]Capturing num tokens (num_tokens=3840 avail_mem=104.61 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.14it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=104.61 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.14it/s]Capturing num tokens (num_tokens=3584 avail_mem=104.61 GB):  19%|█▉        | 11/58 [00:02<00:09,  5.14it/s]Capturing num tokens (num_tokens=3328 avail_mem=104.61 GB):  19%|█▉        | 11/58 [00:02<00:09,  5.14it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=104.61 GB):  21%|██        | 12/58 [00:02<00:07,  5.99it/s]Capturing num tokens (num_tokens=3072 avail_mem=104.60 GB):  21%|██        | 12/58 [00:02<00:07,  5.99it/s]Capturing num tokens (num_tokens=3072 avail_mem=104.60 GB):  22%|██▏       | 13/58 [00:02<00:06,  6.81it/s]Capturing num tokens (num_tokens=2816 avail_mem=104.61 GB):  22%|██▏       | 13/58 [00:02<00:06,  6.81it/s]Capturing num tokens (num_tokens=2560 avail_mem=104.61 GB):  22%|██▏       | 13/58 [00:02<00:06,  6.81it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=104.61 GB):  26%|██▌       | 15/58 [00:02<00:05,  8.52it/s]Capturing num tokens (num_tokens=2304 avail_mem=104.61 GB):  26%|██▌       | 15/58 [00:02<00:05,  8.52it/s]Capturing num tokens (num_tokens=2048 avail_mem=104.60 GB):  26%|██▌       | 15/58 [00:02<00:05,  8.52it/s]Capturing num tokens (num_tokens=2048 avail_mem=104.60 GB):  29%|██▉       | 17/58 [00:02<00:04, 10.18it/s]Capturing num tokens (num_tokens=1792 avail_mem=104.61 GB):  29%|██▉       | 17/58 [00:02<00:04, 10.18it/s]Capturing num tokens (num_tokens=1536 avail_mem=104.60 GB):  29%|██▉       | 17/58 [00:02<00:04, 10.18it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=104.60 GB):  33%|███▎      | 19/58 [00:02<00:03, 12.08it/s]Capturing num tokens (num_tokens=1280 avail_mem=104.60 GB):  33%|███▎      | 19/58 [00:02<00:03, 12.08it/s]Capturing num tokens (num_tokens=1024 avail_mem=104.60 GB):  33%|███▎      | 19/58 [00:02<00:03, 12.08it/s]Capturing num tokens (num_tokens=1024 avail_mem=104.60 GB):  36%|███▌      | 21/58 [00:03<00:02, 13.71it/s]Capturing num tokens (num_tokens=960 avail_mem=104.60 GB):  36%|███▌      | 21/58 [00:03<00:02, 13.71it/s] Capturing num tokens (num_tokens=896 avail_mem=104.59 GB):  36%|███▌      | 21/58 [00:03<00:02, 13.71it/s]Capturing num tokens (num_tokens=832 avail_mem=104.59 GB):  36%|███▌      | 21/58 [00:03<00:02, 13.71it/s]

    Capturing num tokens (num_tokens=832 avail_mem=104.59 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.17it/s]Capturing num tokens (num_tokens=768 avail_mem=104.59 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.17it/s]Capturing num tokens (num_tokens=704 avail_mem=104.58 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.17it/s]Capturing num tokens (num_tokens=640 avail_mem=104.58 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.17it/s]Capturing num tokens (num_tokens=640 avail_mem=104.58 GB):  47%|████▋     | 27/58 [00:03<00:01, 20.45it/s]Capturing num tokens (num_tokens=576 avail_mem=104.58 GB):  47%|████▋     | 27/58 [00:03<00:01, 20.45it/s]Capturing num tokens (num_tokens=512 avail_mem=104.57 GB):  47%|████▋     | 27/58 [00:03<00:01, 20.45it/s]Capturing num tokens (num_tokens=480 avail_mem=104.57 GB):  47%|████▋     | 27/58 [00:03<00:01, 20.45it/s]Capturing num tokens (num_tokens=448 avail_mem=104.57 GB):  47%|████▋     | 27/58 [00:03<00:01, 20.45it/s]

    Capturing num tokens (num_tokens=448 avail_mem=104.57 GB):  53%|█████▎    | 31/58 [00:03<00:01, 24.45it/s]Capturing num tokens (num_tokens=416 avail_mem=104.56 GB):  53%|█████▎    | 31/58 [00:03<00:01, 24.45it/s]Capturing num tokens (num_tokens=384 avail_mem=104.56 GB):  53%|█████▎    | 31/58 [00:03<00:01, 24.45it/s]Capturing num tokens (num_tokens=352 avail_mem=104.55 GB):  53%|█████▎    | 31/58 [00:03<00:01, 24.45it/s]Capturing num tokens (num_tokens=320 avail_mem=104.55 GB):  53%|█████▎    | 31/58 [00:03<00:01, 24.45it/s]Capturing num tokens (num_tokens=320 avail_mem=104.55 GB):  60%|██████    | 35/58 [00:03<00:00, 27.78it/s]Capturing num tokens (num_tokens=288 avail_mem=104.55 GB):  60%|██████    | 35/58 [00:03<00:00, 27.78it/s]Capturing num tokens (num_tokens=256 avail_mem=104.54 GB):  60%|██████    | 35/58 [00:03<00:00, 27.78it/s]Capturing num tokens (num_tokens=240 avail_mem=104.54 GB):  60%|██████    | 35/58 [00:03<00:00, 27.78it/s]Capturing num tokens (num_tokens=224 avail_mem=104.53 GB):  60%|██████    | 35/58 [00:03<00:00, 27.78it/s]

    Capturing num tokens (num_tokens=224 avail_mem=104.53 GB):  67%|██████▋   | 39/58 [00:03<00:00, 30.64it/s]Capturing num tokens (num_tokens=208 avail_mem=104.53 GB):  67%|██████▋   | 39/58 [00:03<00:00, 30.64it/s]Capturing num tokens (num_tokens=192 avail_mem=104.53 GB):  67%|██████▋   | 39/58 [00:03<00:00, 30.64it/s]Capturing num tokens (num_tokens=176 avail_mem=104.52 GB):  67%|██████▋   | 39/58 [00:03<00:00, 30.64it/s]Capturing num tokens (num_tokens=160 avail_mem=104.52 GB):  67%|██████▋   | 39/58 [00:03<00:00, 30.64it/s]Capturing num tokens (num_tokens=160 avail_mem=104.52 GB):  74%|███████▍  | 43/58 [00:03<00:00, 32.89it/s]Capturing num tokens (num_tokens=144 avail_mem=104.52 GB):  74%|███████▍  | 43/58 [00:03<00:00, 32.89it/s]Capturing num tokens (num_tokens=128 avail_mem=104.53 GB):  74%|███████▍  | 43/58 [00:03<00:00, 32.89it/s]Capturing num tokens (num_tokens=112 avail_mem=104.52 GB):  74%|███████▍  | 43/58 [00:03<00:00, 32.89it/s]Capturing num tokens (num_tokens=96 avail_mem=104.52 GB):  74%|███████▍  | 43/58 [00:03<00:00, 32.89it/s] 

    Capturing num tokens (num_tokens=96 avail_mem=104.52 GB):  81%|████████  | 47/58 [00:03<00:00, 34.11it/s]Capturing num tokens (num_tokens=80 avail_mem=104.51 GB):  81%|████████  | 47/58 [00:03<00:00, 34.11it/s]Capturing num tokens (num_tokens=64 avail_mem=104.51 GB):  81%|████████  | 47/58 [00:03<00:00, 34.11it/s]Capturing num tokens (num_tokens=48 avail_mem=104.51 GB):  81%|████████  | 47/58 [00:03<00:00, 34.11it/s]Capturing num tokens (num_tokens=32 avail_mem=104.50 GB):  81%|████████  | 47/58 [00:03<00:00, 34.11it/s]Capturing num tokens (num_tokens=32 avail_mem=104.50 GB):  88%|████████▊ | 51/58 [00:03<00:00, 35.39it/s]Capturing num tokens (num_tokens=28 avail_mem=104.50 GB):  88%|████████▊ | 51/58 [00:03<00:00, 35.39it/s]Capturing num tokens (num_tokens=24 avail_mem=104.50 GB):  88%|████████▊ | 51/58 [00:03<00:00, 35.39it/s]Capturing num tokens (num_tokens=20 avail_mem=104.49 GB):  88%|████████▊ | 51/58 [00:03<00:00, 35.39it/s]Capturing num tokens (num_tokens=16 avail_mem=104.49 GB):  88%|████████▊ | 51/58 [00:04<00:00, 35.39it/s]

    Capturing num tokens (num_tokens=16 avail_mem=104.49 GB):  95%|█████████▍| 55/58 [00:04<00:00, 28.41it/s]Capturing num tokens (num_tokens=12 avail_mem=104.49 GB):  95%|█████████▍| 55/58 [00:04<00:00, 28.41it/s]Capturing num tokens (num_tokens=8 avail_mem=104.48 GB):  95%|█████████▍| 55/58 [00:04<00:00, 28.41it/s] Capturing num tokens (num_tokens=4 avail_mem=104.48 GB):  95%|█████████▍| 55/58 [00:04<00:00, 28.41it/s]Capturing num tokens (num_tokens=4 avail_mem=104.48 GB): 100%|██████████| 58/58 [00:04<00:00, 13.78it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>



<strong style='color: #00008B;'>Response with tool_choice='required':</strong>


    Content: None
    Tool calls: [ChatCompletionMessageFunctionToolCall(id='call_d5fdd6de8f094d2b8f2bee7a', function=Function(arguments='{"city": "Paris", "unit": "celsius"}', name='get_current_weather'), type='function', index=0)]


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
    Tool calls: [ChatCompletionMessageFunctionToolCall(id='call_673198a0fb9d480886e8343c', function=Function(arguments='{"city": "Paris", "unit": "celsius"}', name='get_current_weather'), type='function', index=0)]



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


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:176: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (
    [2026-03-29 08:10:49] numactl command not found, skipping NUMA node configuration for GPU. Install numactl (e.g., apt-get install numactl) to enable automatic NUMA binding.


    [2026-03-29 08:10:55] numactl command not found, skipping NUMA node configuration for GPU. Install numactl (e.g., apt-get install numactl) to enable automatic NUMA binding.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-03-29 08:10:58.967 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:10:58] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 08:10:58.968 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:10:58] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 08:10:58.968 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:10:58] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 08:10:58.968 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:10:58] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 08:10:58.968 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:10:58] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:01<00:00,  1.16s/it]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:01<00:00,  1.16s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:01<01:34,  1.66s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:01<01:34,  1.66s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:01<00:42,  1.33it/s]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:01<00:42,  1.33it/s]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:01<00:42,  1.33it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:01<00:16,  3.18it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:01<00:16,  3.18it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:01<00:16,  3.18it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:01<00:16,  3.18it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:02<00:08,  6.29it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:02<00:08,  6.29it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:02<00:08,  6.29it/s]Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:02<00:08,  6.29it/s]

    Compiling num tokens (num_tokens=3584):  12%|█▏        | 7/58 [00:02<00:08,  6.29it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:04, 10.97it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:04, 10.97it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:04, 10.97it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:04, 10.97it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:04, 10.97it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:04, 10.97it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:02<00:02, 17.22it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:02<00:02, 17.22it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:02<00:02, 17.22it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:02<00:02, 17.22it/s]Compiling num tokens (num_tokens=1280):  28%|██▊       | 16/58 [00:02<00:02, 17.22it/s]Compiling num tokens (num_tokens=1024):  28%|██▊       | 16/58 [00:02<00:02, 17.22it/s]

    Compiling num tokens (num_tokens=960):  28%|██▊       | 16/58 [00:02<00:02, 17.22it/s] Compiling num tokens (num_tokens=896):  28%|██▊       | 16/58 [00:02<00:02, 17.22it/s]Compiling num tokens (num_tokens=832):  28%|██▊       | 16/58 [00:02<00:02, 17.22it/s]Compiling num tokens (num_tokens=768):  28%|██▊       | 16/58 [00:02<00:02, 17.22it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 30.86it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 30.86it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 30.86it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 30.86it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 30.86it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:02<00:01, 30.86it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:02<00:01, 30.86it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:02<00:01, 30.86it/s]Compiling num tokens (num_tokens=384):  43%|████▎     | 25/58 [00:02<00:01, 30.86it/s]Compiling num tokens (num_tokens=352):  43%|████▎     | 25/58 [00:02<00:01, 30.86it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:02<00:00, 43.20it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:02<00:00, 43.20it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:02<00:00, 43.20it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:02<00:00, 43.20it/s]

    Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:15<00:00, 43.20it/s]Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:15<00:00, 43.20it/s]Compiling num tokens (num_tokens=208):  59%|█████▊    | 34/58 [00:15<00:00, 43.20it/s]Compiling num tokens (num_tokens=192):  59%|█████▊    | 34/58 [00:15<00:00, 43.20it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:15<00:10,  1.61it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:15<00:10,  1.61it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:15<00:10,  1.61it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:15<00:10,  1.61it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:15<00:10,  1.61it/s]

    Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:15<00:06,  2.02it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:15<00:06,  2.02it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:15<00:06,  2.02it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:15<00:06,  2.02it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:15<00:06,  2.02it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:15<00:06,  2.02it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:15<00:06,  2.02it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:15<00:02,  2.86it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:15<00:02,  2.86it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:15<00:02,  2.86it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:15<00:02,  2.86it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:15<00:02,  2.86it/s]

    Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:15<00:02,  2.86it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:15<00:02,  2.86it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:15<00:02,  2.86it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:15<00:00,  3.69it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=116.86 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=116.86 GB):   2%|▏         | 1/58 [00:00<00:09,  6.04it/s]Capturing num tokens (num_tokens=7680 avail_mem=116.83 GB):   2%|▏         | 1/58 [00:00<00:09,  6.04it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=116.83 GB):   3%|▎         | 2/58 [00:00<00:08,  6.75it/s]Capturing num tokens (num_tokens=7168 avail_mem=116.90 GB):   3%|▎         | 2/58 [00:00<00:08,  6.75it/s]Capturing num tokens (num_tokens=7168 avail_mem=116.90 GB):   5%|▌         | 3/58 [00:00<00:08,  6.74it/s]Capturing num tokens (num_tokens=6656 avail_mem=116.90 GB):   5%|▌         | 3/58 [00:00<00:08,  6.74it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=116.90 GB):   7%|▋         | 4/58 [00:00<00:07,  7.05it/s]Capturing num tokens (num_tokens=6144 avail_mem=117.77 GB):   7%|▋         | 4/58 [00:00<00:07,  7.05it/s]Capturing num tokens (num_tokens=5632 avail_mem=116.98 GB):   7%|▋         | 4/58 [00:00<00:07,  7.05it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=116.98 GB):  10%|█         | 6/58 [00:00<00:06,  7.99it/s]Capturing num tokens (num_tokens=5120 avail_mem=116.99 GB):  10%|█         | 6/58 [00:00<00:06,  7.99it/s]Capturing num tokens (num_tokens=5120 avail_mem=116.99 GB):  12%|█▏        | 7/58 [00:00<00:06,  8.37it/s]Capturing num tokens (num_tokens=4608 avail_mem=117.82 GB):  12%|█▏        | 7/58 [00:00<00:06,  8.37it/s]Capturing num tokens (num_tokens=4096 avail_mem=117.78 GB):  12%|█▏        | 7/58 [00:00<00:06,  8.37it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=117.78 GB):  16%|█▌        | 9/58 [00:01<00:05,  9.66it/s]Capturing num tokens (num_tokens=3840 avail_mem=117.05 GB):  16%|█▌        | 9/58 [00:01<00:05,  9.66it/s]Capturing num tokens (num_tokens=3584 avail_mem=117.05 GB):  16%|█▌        | 9/58 [00:01<00:05,  9.66it/s]Capturing num tokens (num_tokens=3584 avail_mem=117.05 GB):  19%|█▉        | 11/58 [00:01<00:04, 10.56it/s]Capturing num tokens (num_tokens=3328 avail_mem=117.78 GB):  19%|█▉        | 11/58 [00:01<00:04, 10.56it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=117.12 GB):  19%|█▉        | 11/58 [00:01<00:04, 10.56it/s]Capturing num tokens (num_tokens=3072 avail_mem=117.12 GB):  22%|██▏       | 13/58 [00:01<00:04, 11.02it/s]Capturing num tokens (num_tokens=2816 avail_mem=117.12 GB):  22%|██▏       | 13/58 [00:01<00:04, 11.02it/s]Capturing num tokens (num_tokens=2560 avail_mem=117.78 GB):  22%|██▏       | 13/58 [00:01<00:04, 11.02it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=117.78 GB):  26%|██▌       | 15/58 [00:01<00:03, 11.90it/s]Capturing num tokens (num_tokens=2304 avail_mem=117.19 GB):  26%|██▌       | 15/58 [00:01<00:03, 11.90it/s]Capturing num tokens (num_tokens=2048 avail_mem=117.19 GB):  26%|██▌       | 15/58 [00:01<00:03, 11.90it/s]Capturing num tokens (num_tokens=2048 avail_mem=117.19 GB):  29%|██▉       | 17/58 [00:01<00:03, 12.89it/s]Capturing num tokens (num_tokens=1792 avail_mem=117.78 GB):  29%|██▉       | 17/58 [00:01<00:03, 12.89it/s]Capturing num tokens (num_tokens=1536 avail_mem=117.26 GB):  29%|██▉       | 17/58 [00:01<00:03, 12.89it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=117.26 GB):  33%|███▎      | 19/58 [00:01<00:02, 13.68it/s]Capturing num tokens (num_tokens=1280 avail_mem=117.26 GB):  33%|███▎      | 19/58 [00:01<00:02, 13.68it/s]Capturing num tokens (num_tokens=1024 avail_mem=117.78 GB):  33%|███▎      | 19/58 [00:01<00:02, 13.68it/s]Capturing num tokens (num_tokens=960 avail_mem=117.28 GB):  33%|███▎      | 19/58 [00:01<00:02, 13.68it/s] Capturing num tokens (num_tokens=960 avail_mem=117.28 GB):  38%|███▊      | 22/58 [00:01<00:02, 15.89it/s]Capturing num tokens (num_tokens=896 avail_mem=117.28 GB):  38%|███▊      | 22/58 [00:01<00:02, 15.89it/s]Capturing num tokens (num_tokens=832 avail_mem=117.77 GB):  38%|███▊      | 22/58 [00:01<00:02, 15.89it/s]

    Capturing num tokens (num_tokens=768 avail_mem=117.77 GB):  38%|███▊      | 22/58 [00:02<00:02, 15.89it/s]Capturing num tokens (num_tokens=768 avail_mem=117.77 GB):  43%|████▎     | 25/58 [00:02<00:01, 17.94it/s]Capturing num tokens (num_tokens=704 avail_mem=117.30 GB):  43%|████▎     | 25/58 [00:02<00:01, 17.94it/s]Capturing num tokens (num_tokens=640 avail_mem=117.82 GB):  43%|████▎     | 25/58 [00:02<00:01, 17.94it/s]Capturing num tokens (num_tokens=576 avail_mem=117.77 GB):  43%|████▎     | 25/58 [00:02<00:01, 17.94it/s]Capturing num tokens (num_tokens=576 avail_mem=117.77 GB):  48%|████▊     | 28/58 [00:02<00:01, 19.71it/s]Capturing num tokens (num_tokens=512 avail_mem=117.31 GB):  48%|████▊     | 28/58 [00:02<00:01, 19.71it/s]

    Capturing num tokens (num_tokens=480 avail_mem=117.33 GB):  48%|████▊     | 28/58 [00:02<00:01, 19.71it/s]Capturing num tokens (num_tokens=448 avail_mem=117.76 GB):  48%|████▊     | 28/58 [00:02<00:01, 19.71it/s]Capturing num tokens (num_tokens=448 avail_mem=117.76 GB):  53%|█████▎    | 31/58 [00:02<00:01, 20.88it/s]Capturing num tokens (num_tokens=416 avail_mem=117.36 GB):  53%|█████▎    | 31/58 [00:02<00:01, 20.88it/s]Capturing num tokens (num_tokens=384 avail_mem=117.36 GB):  53%|█████▎    | 31/58 [00:02<00:01, 20.88it/s]Capturing num tokens (num_tokens=352 avail_mem=117.75 GB):  53%|█████▎    | 31/58 [00:02<00:01, 20.88it/s]Capturing num tokens (num_tokens=352 avail_mem=117.75 GB):  59%|█████▊    | 34/58 [00:02<00:01, 21.51it/s]

    Capturing num tokens (num_tokens=320 avail_mem=117.39 GB):  59%|█████▊    | 34/58 [00:06<00:01, 21.51it/s]Capturing num tokens (num_tokens=288 avail_mem=117.72 GB):  59%|█████▊    | 34/58 [00:06<00:01, 21.51it/s]Capturing num tokens (num_tokens=256 avail_mem=117.41 GB):  59%|█████▊    | 34/58 [00:06<00:01, 21.51it/s]Capturing num tokens (num_tokens=256 avail_mem=117.41 GB):  64%|██████▍   | 37/58 [00:06<00:08,  2.34it/s]Capturing num tokens (num_tokens=240 avail_mem=117.46 GB):  64%|██████▍   | 37/58 [00:06<00:08,  2.34it/s]Capturing num tokens (num_tokens=224 avail_mem=117.76 GB):  64%|██████▍   | 37/58 [00:06<00:08,  2.34it/s]Capturing num tokens (num_tokens=208 avail_mem=117.73 GB):  64%|██████▍   | 37/58 [00:06<00:08,  2.34it/s]

    Capturing num tokens (num_tokens=208 avail_mem=117.73 GB):  69%|██████▉   | 40/58 [00:06<00:05,  3.26it/s]Capturing num tokens (num_tokens=192 avail_mem=117.45 GB):  69%|██████▉   | 40/58 [00:06<00:05,  3.26it/s]Capturing num tokens (num_tokens=176 avail_mem=117.75 GB):  69%|██████▉   | 40/58 [00:06<00:05,  3.26it/s]Capturing num tokens (num_tokens=176 avail_mem=117.75 GB):  72%|███████▏  | 42/58 [00:06<00:04,  3.86it/s]Capturing num tokens (num_tokens=160 avail_mem=117.74 GB):  72%|███████▏  | 42/58 [00:06<00:04,  3.86it/s]

    Capturing num tokens (num_tokens=144 avail_mem=117.48 GB):  72%|███████▏  | 42/58 [00:06<00:04,  3.86it/s]Capturing num tokens (num_tokens=128 avail_mem=117.74 GB):  72%|███████▏  | 42/58 [00:06<00:04,  3.86it/s]Capturing num tokens (num_tokens=128 avail_mem=117.74 GB):  78%|███████▊  | 45/58 [00:06<00:02,  5.32it/s]Capturing num tokens (num_tokens=112 avail_mem=117.70 GB):  78%|███████▊  | 45/58 [00:06<00:02,  5.32it/s]Capturing num tokens (num_tokens=96 avail_mem=117.50 GB):  78%|███████▊  | 45/58 [00:06<00:02,  5.32it/s] Capturing num tokens (num_tokens=80 avail_mem=117.73 GB):  78%|███████▊  | 45/58 [00:06<00:02,  5.32it/s]Capturing num tokens (num_tokens=80 avail_mem=117.73 GB):  83%|████████▎ | 48/58 [00:06<00:01,  7.14it/s]Capturing num tokens (num_tokens=64 avail_mem=117.53 GB):  83%|████████▎ | 48/58 [00:06<00:01,  7.14it/s]Capturing num tokens (num_tokens=48 avail_mem=117.73 GB):  83%|████████▎ | 48/58 [00:06<00:01,  7.14it/s]

    Capturing num tokens (num_tokens=32 avail_mem=117.58 GB):  83%|████████▎ | 48/58 [00:06<00:01,  7.14it/s]Capturing num tokens (num_tokens=28 avail_mem=117.72 GB):  83%|████████▎ | 48/58 [00:06<00:01,  7.14it/s]Capturing num tokens (num_tokens=28 avail_mem=117.72 GB):  90%|████████▉ | 52/58 [00:06<00:00, 10.21it/s]Capturing num tokens (num_tokens=24 avail_mem=117.72 GB):  90%|████████▉ | 52/58 [00:06<00:00, 10.21it/s]Capturing num tokens (num_tokens=20 avail_mem=117.71 GB):  90%|████████▉ | 52/58 [00:06<00:00, 10.21it/s]Capturing num tokens (num_tokens=16 avail_mem=117.71 GB):  90%|████████▉ | 52/58 [00:06<00:00, 10.21it/s]Capturing num tokens (num_tokens=12 avail_mem=117.59 GB):  90%|████████▉ | 52/58 [00:06<00:00, 10.21it/s]Capturing num tokens (num_tokens=12 avail_mem=117.59 GB):  97%|█████████▋| 56/58 [00:06<00:00, 13.81it/s]Capturing num tokens (num_tokens=8 avail_mem=117.70 GB):  97%|█████████▋| 56/58 [00:06<00:00, 13.81it/s] Capturing num tokens (num_tokens=4 avail_mem=117.59 GB):  97%|█████████▋| 56/58 [00:07<00:00, 13.81it/s]

    Capturing num tokens (num_tokens=4 avail_mem=117.59 GB): 100%|██████████| 58/58 [00:07<00:00,  8.23it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>



<strong style='color: #00008B;'>Non-stream response:</strong>



<strong style='color: #00008B;'>ChatCompletion(id='e8cb1dc2c6ba436c83dd5e71fd6e4020', choices=[Choice(finish_reason='tool_calls', index=0, logprobs=None, message=ChatCompletionMessage(content=None, refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=[ChatCompletionMessageFunctionToolCall(id='call_9516d7417abb4fbc9e184d58', function=Function(arguments='{"location": "Tokyo"}', name='get_weather'), type='function', index=0), ChatCompletionMessageFunctionToolCall(id='call_262063fe03b0496b832086e0', function=Function(arguments='{"city": "Tokyo"}', name='get_tourist_attractions'), type='function', index=1)], reasoning_content=None), matched_stop=None)], created=1774771893, model='meta-llama/Llama-3.2-1B-Instruct', object='chat.completion', service_tier=None, system_fingerprint=None, usage=CompletionUsage(completion_tokens=20, prompt_tokens=435, total_tokens=455, completion_tokens_details=None, prompt_tokens_details=None, reasoning_tokens=0), metadata={'weight_version': 'default'})</strong>



<strong style='color: #00008B;'>Streaming Response:</strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'></strong>



<strong style='color: #00008B;'>==== Tool Call ====</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=0, id='call_e120ff9f5d96401d95005c8a', function=ChoiceDeltaToolCallFunction(arguments='{"location": "Tokyo"}', name='get_weather'), type='function')</strong>



<strong style='color: #00008B;'>ChoiceDeltaToolCall(index=1, id='call_9eae8229ae3c4ab6ac144615', function=ChoiceDeltaToolCallFunction(arguments='{"city": "Tokyo"}', name='get_tourist_attractions'), type='function')</strong>


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
