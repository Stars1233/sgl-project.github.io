# Structured Outputs

You can specify a JSON schema, [regular expression](https://en.wikipedia.org/wiki/Regular_expression) or [EBNF](https://en.wikipedia.org/wiki/Extended_Backus%E2%80%93Naur_form) to constrain the model output. The model output will be guaranteed to follow the given constraints. Only one constraint parameter (`json_schema`, `regex`, or `ebnf`) can be specified for a request.

SGLang supports three grammar backends:

- [XGrammar](https://github.com/mlc-ai/xgrammar)(default): Supports JSON schema, regular expression, and EBNF constraints.
- [Outlines](https://github.com/dottxt-ai/outlines): Supports JSON schema and regular expression constraints.
- [Llguidance](https://github.com/guidance-ai/llguidance): Supports JSON schema, regular expression, and EBNF constraints.

We suggest using XGrammar for its better performance and utility. XGrammar currently uses the [GGML BNF format](https://github.com/ggerganov/llama.cpp/blob/master/grammars/README.md). For more details, see [XGrammar technical overview](https://blog.mlc.ai/2024/11/22/achieving-efficient-flexible-portable-structured-generation-with-xgrammar).

To use Outlines, simply add `--grammar-backend outlines` when launching the server.
To use llguidance, add `--grammar-backend llguidance`  when launching the server.
If no backend is specified, XGrammar will be used as the default.

For better output quality, **It's advisable to explicitly include instructions in the prompt to guide the model to generate the desired format.** For example, you can specify, 'Please generate the output in the following JSON format: ...'.


## OpenAI Compatible API


```python
import openai
import os

from sglang.test.doc_patch import launch_server_cmd
from sglang.utils import wait_for_server, print_highlight, terminate_process

os.environ["TOKENIZERS_PARALLELISM"] = "false"


server_process, port = launch_server_cmd(
    "python -m sglang.launch_server --model-path meta-llama/Meta-Llama-3.1-8B-Instruct --host 0.0.0.0 --log-level warning"
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


    2026-04-01 19:56:37.853 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 19:56:37] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 19:56:37.853 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 19:56:37] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 19:56:37.853 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 19:56:37] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 19:56:37.853 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 19:56:37] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 19:56:37.853 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 19:56:37] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  25% Completed | 1/4 [00:00<00:02,  1.16it/s]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:01<00:01,  1.04it/s]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:02<00:00,  1.46it/s]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:03<00:00,  1.33it/s]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:03<00:00,  1.29it/s]


    2026-04-01 19:56:43,763 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 19:56:43] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:58,  3.13s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:58,  3.13s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:33,  1.68s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:33,  1.68s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<00:55,  1.01s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<00:55,  1.01s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:38,  1.40it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:38,  1.40it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:33,  1.59it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:33,  1.59it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:27,  1.91it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:27,  1.91it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:23,  2.19it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:23,  2.19it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:20,  2.48it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:20,  2.48it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:17,  2.78it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:17,  2.78it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:15,  3.17it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:15,  3.17it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:13,  3.50it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:13,  3.50it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:06<00:11,  3.90it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:06<00:11,  3.90it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:06<00:10,  4.20it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:06<00:10,  4.20it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:06<00:09,  4.76it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:06<00:09,  4.76it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:07,  5.62it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:07,  5.62it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:07<00:07,  5.99it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:07<00:07,  5.99it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:07<00:06,  6.57it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:07<00:06,  6.57it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:07<00:06,  6.57it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:07<00:04,  8.43it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:07<00:04,  8.43it/s]

    Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:07<00:04,  8.43it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:07<00:03,  9.53it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:07<00:03,  9.53it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:07<00:03,  9.53it/s]Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:07<00:03,  9.53it/s]

    Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:07<00:02, 12.45it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:07<00:02, 12.45it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:07<00:02, 12.45it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:07<00:02, 13.71it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:07<00:02, 13.71it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:07<00:02, 13.71it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:07<00:02, 13.71it/s]

    Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:07<00:02, 13.71it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:07<00:01, 19.05it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:07<00:01, 19.05it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:07<00:01, 19.05it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:08<00:01, 19.05it/s]Compiling num tokens (num_tokens=352):  52%|█████▏    | 30/58 [00:08<00:01, 19.05it/s]Compiling num tokens (num_tokens=320):  52%|█████▏    | 30/58 [00:08<00:01, 19.05it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:08<00:00, 25.45it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:08<00:00, 25.45it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:08<00:00, 25.45it/s]

    Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:08<00:00, 25.45it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:08<00:00, 24.56it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:08<00:00, 24.56it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:08<00:00, 24.56it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:08<00:00, 24.56it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:08<00:00, 25.02it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:08<00:00, 25.02it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:08<00:00, 25.02it/s]

    Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:08<00:00, 25.02it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:08<00:00, 25.71it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:08<00:00, 25.71it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:08<00:00, 25.71it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:08<00:00, 25.71it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:08<00:00, 25.33it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:08<00:00, 25.33it/s]

    Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:08<00:00, 25.33it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:08<00:00, 25.33it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:08<00:00, 22.43it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:08<00:00, 22.43it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:08<00:00, 22.43it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:08<00:00, 22.43it/s]

    Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:08<00:00, 22.73it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:08<00:00, 22.73it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:08<00:00, 22.73it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:08<00:00, 22.73it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:09<00:00, 22.35it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:09<00:00, 22.35it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:09<00:00, 22.35it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00,  6.40it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=41.81 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=41.81 GB):   2%|▏         | 1/58 [00:00<00:32,  1.77it/s]Capturing num tokens (num_tokens=7680 avail_mem=42.09 GB):   2%|▏         | 1/58 [00:00<00:32,  1.77it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=42.09 GB):   3%|▎         | 2/58 [00:01<00:28,  1.93it/s]Capturing num tokens (num_tokens=7168 avail_mem=42.09 GB):   3%|▎         | 2/58 [00:01<00:28,  1.93it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=42.09 GB):   5%|▌         | 3/58 [00:01<00:25,  2.20it/s]Capturing num tokens (num_tokens=6656 avail_mem=42.08 GB):   5%|▌         | 3/58 [00:01<00:25,  2.20it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=42.08 GB):   7%|▋         | 4/58 [00:01<00:23,  2.25it/s]Capturing num tokens (num_tokens=6144 avail_mem=42.01 GB):   7%|▋         | 4/58 [00:01<00:23,  2.25it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=42.01 GB):   9%|▊         | 5/58 [00:02<00:21,  2.47it/s]Capturing num tokens (num_tokens=5632 avail_mem=42.07 GB):   9%|▊         | 5/58 [00:02<00:21,  2.47it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=42.07 GB):  10%|█         | 6/58 [00:02<00:18,  2.78it/s]Capturing num tokens (num_tokens=5120 avail_mem=42.06 GB):  10%|█         | 6/58 [00:02<00:18,  2.78it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=42.06 GB):  12%|█▏        | 7/58 [00:02<00:16,  3.11it/s]Capturing num tokens (num_tokens=4608 avail_mem=42.05 GB):  12%|█▏        | 7/58 [00:02<00:16,  3.11it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=42.05 GB):  14%|█▍        | 8/58 [00:02<00:14,  3.45it/s]Capturing num tokens (num_tokens=4096 avail_mem=42.05 GB):  14%|█▍        | 8/58 [00:02<00:14,  3.45it/s]Capturing num tokens (num_tokens=4096 avail_mem=42.05 GB):  16%|█▌        | 9/58 [00:03<00:12,  3.87it/s]Capturing num tokens (num_tokens=3840 avail_mem=42.04 GB):  16%|█▌        | 9/58 [00:03<00:12,  3.87it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=42.04 GB):  17%|█▋        | 10/58 [00:03<00:11,  4.28it/s]Capturing num tokens (num_tokens=3584 avail_mem=42.05 GB):  17%|█▋        | 10/58 [00:03<00:11,  4.28it/s]Capturing num tokens (num_tokens=3584 avail_mem=42.05 GB):  19%|█▉        | 11/58 [00:03<00:10,  4.68it/s]Capturing num tokens (num_tokens=3328 avail_mem=42.01 GB):  19%|█▉        | 11/58 [00:03<00:10,  4.68it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=42.01 GB):  21%|██        | 12/58 [00:03<00:09,  5.04it/s]Capturing num tokens (num_tokens=3072 avail_mem=42.03 GB):  21%|██        | 12/58 [00:03<00:09,  5.04it/s]Capturing num tokens (num_tokens=3072 avail_mem=42.03 GB):  22%|██▏       | 13/58 [00:03<00:08,  5.49it/s]Capturing num tokens (num_tokens=2816 avail_mem=42.02 GB):  22%|██▏       | 13/58 [00:03<00:08,  5.49it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=42.02 GB):  24%|██▍       | 14/58 [00:03<00:07,  5.99it/s]Capturing num tokens (num_tokens=2560 avail_mem=42.02 GB):  24%|██▍       | 14/58 [00:03<00:07,  5.99it/s]Capturing num tokens (num_tokens=2560 avail_mem=42.02 GB):  26%|██▌       | 15/58 [00:04<00:06,  6.67it/s]Capturing num tokens (num_tokens=2304 avail_mem=42.01 GB):  26%|██▌       | 15/58 [00:04<00:06,  6.67it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=42.01 GB):  28%|██▊       | 16/58 [00:04<00:06,  6.05it/s]Capturing num tokens (num_tokens=2048 avail_mem=42.00 GB):  28%|██▊       | 16/58 [00:04<00:06,  6.05it/s]Capturing num tokens (num_tokens=1792 avail_mem=41.99 GB):  28%|██▊       | 16/58 [00:04<00:06,  6.05it/s]Capturing num tokens (num_tokens=1792 avail_mem=41.99 GB):  31%|███       | 18/58 [00:04<00:05,  7.98it/s]Capturing num tokens (num_tokens=1536 avail_mem=41.99 GB):  31%|███       | 18/58 [00:04<00:05,  7.98it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=41.99 GB):  31%|███       | 18/58 [00:04<00:05,  7.98it/s]Capturing num tokens (num_tokens=1024 avail_mem=41.99 GB):  31%|███       | 18/58 [00:04<00:05,  7.98it/s]Capturing num tokens (num_tokens=1024 avail_mem=41.99 GB):  36%|███▌      | 21/58 [00:04<00:03, 11.59it/s]Capturing num tokens (num_tokens=960 avail_mem=41.99 GB):  36%|███▌      | 21/58 [00:04<00:03, 11.59it/s] Capturing num tokens (num_tokens=896 avail_mem=41.98 GB):  36%|███▌      | 21/58 [00:04<00:03, 11.59it/s]Capturing num tokens (num_tokens=832 avail_mem=41.98 GB):  36%|███▌      | 21/58 [00:04<00:03, 11.59it/s]

    Capturing num tokens (num_tokens=832 avail_mem=41.98 GB):  41%|████▏     | 24/58 [00:04<00:02, 14.78it/s]Capturing num tokens (num_tokens=768 avail_mem=41.98 GB):  41%|████▏     | 24/58 [00:04<00:02, 14.78it/s]Capturing num tokens (num_tokens=704 avail_mem=41.97 GB):  41%|████▏     | 24/58 [00:04<00:02, 14.78it/s]Capturing num tokens (num_tokens=640 avail_mem=41.97 GB):  41%|████▏     | 24/58 [00:04<00:02, 14.78it/s]Capturing num tokens (num_tokens=640 avail_mem=41.97 GB):  47%|████▋     | 27/58 [00:04<00:01, 18.00it/s]Capturing num tokens (num_tokens=576 avail_mem=41.96 GB):  47%|████▋     | 27/58 [00:04<00:01, 18.00it/s]Capturing num tokens (num_tokens=512 avail_mem=41.96 GB):  47%|████▋     | 27/58 [00:04<00:01, 18.00it/s]Capturing num tokens (num_tokens=480 avail_mem=41.95 GB):  47%|████▋     | 27/58 [00:04<00:01, 18.00it/s]Capturing num tokens (num_tokens=448 avail_mem=41.95 GB):  47%|████▋     | 27/58 [00:04<00:01, 18.00it/s]

    Capturing num tokens (num_tokens=448 avail_mem=41.95 GB):  53%|█████▎    | 31/58 [00:04<00:01, 21.84it/s]Capturing num tokens (num_tokens=416 avail_mem=41.94 GB):  53%|█████▎    | 31/58 [00:04<00:01, 21.84it/s]Capturing num tokens (num_tokens=384 avail_mem=41.94 GB):  53%|█████▎    | 31/58 [00:04<00:01, 21.84it/s]Capturing num tokens (num_tokens=352 avail_mem=41.94 GB):  53%|█████▎    | 31/58 [00:04<00:01, 21.84it/s]Capturing num tokens (num_tokens=320 avail_mem=41.93 GB):  53%|█████▎    | 31/58 [00:04<00:01, 21.84it/s]Capturing num tokens (num_tokens=320 avail_mem=41.93 GB):  60%|██████    | 35/58 [00:04<00:00, 25.19it/s]Capturing num tokens (num_tokens=288 avail_mem=41.93 GB):  60%|██████    | 35/58 [00:04<00:00, 25.19it/s]Capturing num tokens (num_tokens=256 avail_mem=41.92 GB):  60%|██████    | 35/58 [00:05<00:00, 25.19it/s]Capturing num tokens (num_tokens=240 avail_mem=41.92 GB):  60%|██████    | 35/58 [00:05<00:00, 25.19it/s]Capturing num tokens (num_tokens=224 avail_mem=41.92 GB):  60%|██████    | 35/58 [00:05<00:00, 25.19it/s]

    Capturing num tokens (num_tokens=224 avail_mem=41.92 GB):  67%|██████▋   | 39/58 [00:05<00:00, 28.29it/s]Capturing num tokens (num_tokens=208 avail_mem=41.91 GB):  67%|██████▋   | 39/58 [00:05<00:00, 28.29it/s]Capturing num tokens (num_tokens=192 avail_mem=41.91 GB):  67%|██████▋   | 39/58 [00:05<00:00, 28.29it/s]Capturing num tokens (num_tokens=176 avail_mem=41.90 GB):  67%|██████▋   | 39/58 [00:05<00:00, 28.29it/s]Capturing num tokens (num_tokens=160 avail_mem=41.90 GB):  67%|██████▋   | 39/58 [00:05<00:00, 28.29it/s]Capturing num tokens (num_tokens=160 avail_mem=41.90 GB):  74%|███████▍  | 43/58 [00:05<00:00, 30.90it/s]Capturing num tokens (num_tokens=144 avail_mem=41.89 GB):  74%|███████▍  | 43/58 [00:05<00:00, 30.90it/s]Capturing num tokens (num_tokens=128 avail_mem=41.89 GB):  74%|███████▍  | 43/58 [00:05<00:00, 30.90it/s]Capturing num tokens (num_tokens=112 avail_mem=41.90 GB):  74%|███████▍  | 43/58 [00:05<00:00, 30.90it/s]Capturing num tokens (num_tokens=96 avail_mem=41.90 GB):  74%|███████▍  | 43/58 [00:05<00:00, 30.90it/s] 

    Capturing num tokens (num_tokens=96 avail_mem=41.90 GB):  81%|████████  | 47/58 [00:05<00:00, 33.04it/s]Capturing num tokens (num_tokens=80 avail_mem=41.89 GB):  81%|████████  | 47/58 [00:05<00:00, 33.04it/s]Capturing num tokens (num_tokens=64 avail_mem=41.89 GB):  81%|████████  | 47/58 [00:05<00:00, 33.04it/s]Capturing num tokens (num_tokens=48 avail_mem=41.88 GB):  81%|████████  | 47/58 [00:05<00:00, 33.04it/s]Capturing num tokens (num_tokens=32 avail_mem=41.88 GB):  81%|████████  | 47/58 [00:05<00:00, 33.04it/s]Capturing num tokens (num_tokens=32 avail_mem=41.88 GB):  88%|████████▊ | 51/58 [00:05<00:00, 34.57it/s]Capturing num tokens (num_tokens=28 avail_mem=41.88 GB):  88%|████████▊ | 51/58 [00:05<00:00, 34.57it/s]Capturing num tokens (num_tokens=24 avail_mem=41.87 GB):  88%|████████▊ | 51/58 [00:05<00:00, 34.57it/s]Capturing num tokens (num_tokens=20 avail_mem=41.86 GB):  88%|████████▊ | 51/58 [00:05<00:00, 34.57it/s]Capturing num tokens (num_tokens=16 avail_mem=41.86 GB):  88%|████████▊ | 51/58 [00:05<00:00, 34.57it/s]

    Capturing num tokens (num_tokens=16 avail_mem=41.86 GB):  95%|█████████▍| 55/58 [00:05<00:00, 35.79it/s]Capturing num tokens (num_tokens=12 avail_mem=41.86 GB):  95%|█████████▍| 55/58 [00:05<00:00, 35.79it/s]Capturing num tokens (num_tokens=8 avail_mem=41.85 GB):  95%|█████████▍| 55/58 [00:05<00:00, 35.79it/s] Capturing num tokens (num_tokens=4 avail_mem=41.85 GB):  95%|█████████▍| 55/58 [00:05<00:00, 35.79it/s]Capturing num tokens (num_tokens=4 avail_mem=41.85 GB): 100%|██████████| 58/58 [00:05<00:00, 10.37it/s]


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
    model="meta-llama/Meta-Llama-3.1-8B-Instruct",
    messages=[
        {
            "role": "user",
            "content": "Please generate the information of the capital of France in the JSON format.",
        },
    ],
    temperature=0,
    max_tokens=128,
    response_format={
        "type": "json_schema",
        "json_schema": {
            "name": "foo",
            # convert the pydantic model to json schema
            "schema": CapitalInfo.model_json_schema(),
        },
    },
)

response_content = response.choices[0].message.content
# validate the JSON response by the pydantic model
capital_info = CapitalInfo.model_validate_json(response_content)
print_highlight(f"Validated response: {capital_info.model_dump_json()}")
```


<strong style='color: #00008B;'>Validated response: {"name":"Paris","population":2147000}</strong>


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
    model="meta-llama/Meta-Llama-3.1-8B-Instruct",
    messages=[
        {
            "role": "user",
            "content": "Give me the information of the capital of France in the JSON format.",
        },
    ],
    temperature=0,
    max_tokens=128,
    response_format={
        "type": "json_schema",
        "json_schema": {"name": "foo", "schema": json.loads(json_schema)},
    },
)

print_highlight(response.choices[0].message.content)
```


<strong style='color: #00008B;'>{"name": "Paris", "population": 2147000}</strong>


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
    model="meta-llama/Meta-Llama-3.1-8B-Instruct",
    messages=[
        {"role": "system", "content": "You are a helpful geography bot."},
        {
            "role": "user",
            "content": "Give me the information of the capital of France.",
        },
    ],
    temperature=0,
    max_tokens=32,
    extra_body={"ebnf": ebnf_grammar},
)

print_highlight(response.choices[0].message.content)
```


<strong style='color: #00008B;'>Paris is the capital of France</strong>


### Regular expression


```python
response = client.chat.completions.create(
    model="meta-llama/Meta-Llama-3.1-8B-Instruct",
    messages=[
        {"role": "user", "content": "What is the capital of France?"},
    ],
    temperature=0,
    max_tokens=128,
    extra_body={"regex": "(Paris|London)"},
)

print_highlight(response.choices[0].message.content)
```


<strong style='color: #00008B;'>Paris</strong>


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
            "role": "user",
            "content": "You are in New York. Please get the current date and time, and the weather.",
        },
    ]


messages = get_messages()

response = client.chat.completions.create(
    model="meta-llama/Meta-Llama-3.1-8B-Instruct",
    messages=messages,
    response_format={
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
    },
)

print_highlight(response.choices[0].message.content)
```


<strong style='color: #00008B;'><function=get_current_date>{"timezone": "America/New_York"}</function><br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "fahrenheit"}</function><br><br>Sources: <br>1. get_current_date function <br>2. get_current_weather function</strong>



```python
# Support for XGrammar latest structural tag format
# <https://xgrammar.mlc.ai/docs/tutorials/structural_tag.html>
response = client.chat.completions.create(
    model="meta-llama/Meta-Llama-3.1-8B-Instruct",
    messages=messages,
    response_format={
        "type": "structural_tag",
        "format": {
            "type": "triggered_tags",
            "triggers": ["<function="],
            "tags": [
                {
                    "begin": "<function=get_current_weather>",
                    "content": {
                        "type": "json_schema",
                        "json_schema": schema_get_current_weather,
                    },
                    "end": "</function>",
                },
                {
                    "begin": "<function=get_current_date>",
                    "content": {
                        "type": "json_schema",
                        "json_schema": schema_get_current_date,
                    },
                    "end": "</function>",
                },
            ],
            "at_least_one": False,
            "stop_after_first": False,
        },
    },
)

print_highlight(response.choices[0].message.content)
```


<strong style='color: #00008B;'><function=get_current_date>{"timezone": "America/New_York"}</function><br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "fahrenheit"}</function><br><br>Sources:<br>- get_current_date function<br>- get_current_weather function</strong>


## Native API and SGLang Runtime (SRT)

### JSON

**Using Pydantic**


```python
import requests
import json
from pydantic import BaseModel, Field

from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("meta-llama/Meta-Llama-3.1-8B-Instruct")


# Define the schema using Pydantic
class CapitalInfo(BaseModel):
    name: str = Field(..., pattern=r"^\w+$", description="Name of the capital city")
    population: int = Field(..., description="Population of the capital city")


# Make API request
messages = [
    {
        "role": "user",
        "content": "Here is the information of the capital of France in the JSON format.\n",
    }
]
text = tokenizer.apply_chat_template(
    messages, tokenize=False, add_generation_prompt=True, return_dict=False
)
response = requests.post(
    f"http://localhost:{port}/generate",
    json={
        "text": text,
        "sampling_params": {
            "temperature": 0,
            "max_new_tokens": 64,
            "json_schema": json.dumps(CapitalInfo.model_json_schema()),
        },
    },
)
print_highlight(response.json())


response_data = json.loads(response.json()["text"])
# validate the response by the pydantic model
capital_info = CapitalInfo.model_validate(response_data)
print_highlight(f"Validated response: {capital_info.model_dump_json()}")
```


<strong style='color: #00008B;'>{'text': '{"name": "Paris", "population": 2147000}', 'output_ids': [5018, 609, 794, 330, 60704, 498, 330, 45541, 794, 220, 11584, 7007, 15, 92, 128009], 'meta_info': {'id': '5d8a440b18f24a8b9e051ff78ec56e28', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 50, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 15, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.12640758510679007, 'response_sent_to_client_ts': 1775073432.514235}}</strong>



<strong style='color: #00008B;'>Validated response: {"name":"Paris","population":2147000}</strong>


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
response = requests.post(
    f"http://localhost:{port}/generate",
    json={
        "text": text,
        "sampling_params": {
            "temperature": 0,
            "max_new_tokens": 64,
            "json_schema": json_schema,
        },
    },
)

print_highlight(response.json())
```


<strong style='color: #00008B;'>{'text': '{"name": "Paris", "population": 2147000}', 'output_ids': [5018, 609, 794, 330, 60704, 498, 330, 45541, 794, 220, 11584, 7007, 15, 92, 128009], 'meta_info': {'id': 'e4f32b527eee4da2a152c3933ee2056e', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 50, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 15, 'cached_tokens': 49, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.12296724505722523, 'response_sent_to_client_ts': 1775073432.646068}}</strong>


### EBNF


```python
messages = [
    {
        "role": "user",
        "content": "Give me the information of the capital of France.",
    }
]
text = tokenizer.apply_chat_template(
    messages, tokenize=False, add_generation_prompt=True, return_dict=False
)
response = requests.post(
    f"http://localhost:{port}/generate",
    json={
        "text": text,
        "sampling_params": {
            "max_new_tokens": 128,
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

print_highlight(response.json())
```


<strong style='color: #00008B;'>[{'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': 'd92c00e4e07140909b7acbedf7701538', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.07599607005249709, 'response_sent_to_client_ts': 1775073432.7446954}}, {'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': '37bf40edd3d7446b853de1c48584580c', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.07594659400638193, 'response_sent_to_client_ts': 1775073432.7447007}}, {'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': 'f2e28c4a8b0b4b9e936fab8025f561cc', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.07590773806441575, 'response_sent_to_client_ts': 1775073432.7447033}}]</strong>


### Regular expression


```python
messages = [
    {
        "role": "user",
        "content": "Paris is the capital of",
    }
]
text = tokenizer.apply_chat_template(
    messages, tokenize=False, add_generation_prompt=True, return_dict=False
)
response = requests.post(
    f"http://localhost:{port}/generate",
    json={
        "text": text,
        "sampling_params": {
            "temperature": 0,
            "max_new_tokens": 64,
            "regex": "(France|England)",
        },
    },
)
print_highlight(response.json())
```


<strong style='color: #00008B;'>{'text': 'France', 'output_ids': [50100, 128009], 'meta_info': {'id': 'ac7d2df1be4a4a77835e5b20c3251fe6', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2, 'cached_tokens': 31, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.026133453007787466, 'response_sent_to_client_ts': 1775073432.7775607}}</strong>


### Structural Tag


```python
from transformers import AutoTokenizer

# generate an answer
tokenizer = AutoTokenizer.from_pretrained("meta-llama/Meta-Llama-3.1-8B-Instruct")

text = tokenizer.apply_chat_template(
    messages, tokenize=False, add_generation_prompt=True, return_dict=False
)
payload = {
    "text": text,
    "sampling_params": {
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
        )
    },
}


# Send POST request to the API endpoint
response = requests.post(f"http://localhost:{port}/generate", json=payload)
print_highlight(response.json())
```


<strong style='color: #00008B;'>{'text': 'Paris is the capital of France.', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 13, 128009], 'meta_info': {'id': '80e9f86bc8e2429c822e4286a100c755', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 8, 'cached_tokens': 40, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.11297937494236976, 'response_sent_to_client_ts': 1775073434.0430434}}</strong>



```python
# Support for XGrammar latest structural tag format
# <https://xgrammar.mlc.ai/docs/tutorials/structural_tag.html>
payload = {
    "text": text,
    "sampling_params": {
        "structural_tag": json.dumps(
            {
                "type": "structural_tag",
                "format": {
                    "type": "triggered_tags",
                    "triggers": ["<function="],
                    "tags": [
                        {
                            "begin": "<function=get_current_weather>",
                            "content": {
                                "type": "json_schema",
                                "json_schema": schema_get_current_weather,
                            },
                            "end": "</function>",
                        },
                        {
                            "begin": "<function=get_current_date>",
                            "content": {
                                "type": "json_schema",
                                "json_schema": schema_get_current_date,
                            },
                            "end": "</function>",
                        },
                    ],
                    "at_least_one": False,
                    "stop_after_first": False,
                },
            }
        )
    },
}


# Send POST request to the API endpoint
response = requests.post(f"http://localhost:{port}/generate", json=payload)
print_highlight(response.json())
```


<strong style='color: #00008B;'>{'text': 'France.', 'output_ids': [50100, 13, 128009], 'meta_info': {'id': '35aeca51cf0642f1920314274ee6d9cc', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 3, 'cached_tokens': 40, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.03551741095725447, 'response_sent_to_client_ts': 1775073434.0868874}}</strong>



```python
terminate_process(server_process)
```

## Offline Engine API


```python
import sglang as sgl

llm = sgl.Engine(
    model_path="meta-llama/Meta-Llama-3.1-8B-Instruct", grammar_backend="xgrammar"
)
```

    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-04-01 19:57:26.570 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 19:57:26] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 19:57:26.570 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 19:57:26] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 19:57:26.570 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 19:57:26] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 19:57:26.570 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 19:57:26] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 19:57:26.571 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 19:57:26] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  25% Completed | 1/4 [00:00<00:02,  1.21it/s]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:01<00:01,  1.08it/s]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:02<00:00,  1.50it/s]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:03<00:00,  1.35it/s]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:03<00:00,  1.32it/s]


    2026-04-01 19:57:32,128 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 19:57:32] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:08,  3.31s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:08,  3.31s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:42,  1.83s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:42,  1.83s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:05,  1.19s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:05,  1.19s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:46,  1.16it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:46,  1.16it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:36,  1.46it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:36,  1.46it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:29,  1.77it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:29,  1.77it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:23,  2.13it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:23,  2.13it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:20,  2.48it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:20,  2.48it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:17,  2.82it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:17,  2.82it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:14,  3.27it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:14,  3.27it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:12,  3.82it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:12,  3.82it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:06<00:11,  4.16it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:06<00:11,  4.16it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:09,  4.70it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:09,  4.70it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:08,  5.24it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:08,  5.24it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:07,  5.64it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:07,  5.64it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:07<00:07,  5.64it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:07<00:05,  7.01it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:07<00:05,  7.01it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:07<00:05,  7.01it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:07<00:04,  8.40it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:07<00:04,  8.40it/s]

    Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:07<00:04,  8.40it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:07<00:03, 10.38it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:07<00:03, 10.38it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:07<00:03, 10.38it/s]Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:07<00:03, 10.38it/s]

    Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:07<00:02, 13.11it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:07<00:02, 13.11it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:08<00:02, 13.11it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:08<00:02, 13.11it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:08<00:01, 16.73it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:08<00:01, 16.73it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:08<00:01, 16.73it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:08<00:01, 16.73it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:08<00:01, 16.73it/s]

    Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:08<00:01, 20.64it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:08<00:01, 20.64it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:08<00:01, 20.64it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:08<00:01, 20.64it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:08<00:01, 20.64it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:08<00:00, 24.69it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:08<00:00, 24.69it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:08<00:00, 24.69it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:08<00:00, 24.69it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:08<00:00, 24.69it/s]

    Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:08<00:00, 27.05it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:08<00:00, 27.05it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:08<00:00, 27.05it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:08<00:00, 27.05it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:08<00:00, 27.05it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:08<00:00, 29.27it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:08<00:00, 29.27it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:08<00:00, 29.27it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:08<00:00, 29.27it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:08<00:00, 29.27it/s] 

    Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:08<00:00, 31.54it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:08<00:00, 31.54it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:08<00:00, 31.54it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:08<00:00, 31.54it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:08<00:00, 31.54it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:08<00:00, 31.54it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:08<00:00, 35.98it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:08<00:00, 35.98it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:08<00:00, 35.98it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:08<00:00, 35.98it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:08<00:00, 35.98it/s]Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:08<00:00, 35.98it/s] 

    Compiling num tokens (num_tokens=4):  90%|████████▉ | 52/58 [00:08<00:00, 35.98it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00, 41.79it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00,  6.52it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=42.04 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=42.04 GB):   2%|▏         | 1/58 [00:00<00:28,  2.00it/s]Capturing num tokens (num_tokens=7680 avail_mem=41.33 GB):   2%|▏         | 1/58 [00:00<00:28,  2.00it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=41.33 GB):   3%|▎         | 2/58 [00:00<00:24,  2.24it/s]Capturing num tokens (num_tokens=7168 avail_mem=41.30 GB):   3%|▎         | 2/58 [00:00<00:24,  2.24it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=41.30 GB):   5%|▌         | 3/58 [00:01<00:20,  2.71it/s]Capturing num tokens (num_tokens=6656 avail_mem=41.30 GB):   5%|▌         | 3/58 [00:01<00:20,  2.71it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=41.30 GB):   7%|▋         | 4/58 [00:01<00:17,  3.04it/s]Capturing num tokens (num_tokens=6144 avail_mem=41.30 GB):   7%|▋         | 4/58 [00:01<00:17,  3.04it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=41.30 GB):   9%|▊         | 5/58 [00:01<00:15,  3.40it/s]Capturing num tokens (num_tokens=5632 avail_mem=41.31 GB):   9%|▊         | 5/58 [00:01<00:15,  3.40it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=41.31 GB):  10%|█         | 6/58 [00:01<00:13,  3.80it/s]Capturing num tokens (num_tokens=5120 avail_mem=41.31 GB):  10%|█         | 6/58 [00:01<00:13,  3.80it/s]Capturing num tokens (num_tokens=5120 avail_mem=41.31 GB):  12%|█▏        | 7/58 [00:02<00:12,  4.20it/s]Capturing num tokens (num_tokens=4608 avail_mem=41.32 GB):  12%|█▏        | 7/58 [00:02<00:12,  4.20it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=41.32 GB):  14%|█▍        | 8/58 [00:02<00:10,  4.65it/s]Capturing num tokens (num_tokens=4096 avail_mem=41.32 GB):  14%|█▍        | 8/58 [00:02<00:10,  4.65it/s]Capturing num tokens (num_tokens=4096 avail_mem=41.32 GB):  16%|█▌        | 9/58 [00:02<00:09,  5.16it/s]Capturing num tokens (num_tokens=3840 avail_mem=41.32 GB):  16%|█▌        | 9/58 [00:02<00:09,  5.16it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=41.32 GB):  17%|█▋        | 10/58 [00:02<00:08,  5.64it/s]Capturing num tokens (num_tokens=3584 avail_mem=41.32 GB):  17%|█▋        | 10/58 [00:02<00:08,  5.64it/s]Capturing num tokens (num_tokens=3584 avail_mem=41.32 GB):  19%|█▉        | 11/58 [00:02<00:07,  6.06it/s]Capturing num tokens (num_tokens=3328 avail_mem=40.26 GB):  19%|█▉        | 11/58 [00:02<00:07,  6.06it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=40.26 GB):  21%|██        | 12/58 [00:02<00:07,  6.53it/s]Capturing num tokens (num_tokens=3072 avail_mem=30.53 GB):  21%|██        | 12/58 [00:02<00:07,  6.53it/s]Capturing num tokens (num_tokens=3072 avail_mem=30.53 GB):  22%|██▏       | 13/58 [00:02<00:06,  7.14it/s]Capturing num tokens (num_tokens=2816 avail_mem=26.30 GB):  22%|██▏       | 13/58 [00:02<00:06,  7.14it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=26.30 GB):  22%|██▏       | 13/58 [00:02<00:06,  7.14it/s]Capturing num tokens (num_tokens=2560 avail_mem=26.30 GB):  26%|██▌       | 15/58 [00:03<00:05,  8.51it/s]Capturing num tokens (num_tokens=2304 avail_mem=26.30 GB):  26%|██▌       | 15/58 [00:03<00:05,  8.51it/s]Capturing num tokens (num_tokens=2048 avail_mem=26.30 GB):  26%|██▌       | 15/58 [00:03<00:05,  8.51it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=26.30 GB):  29%|██▉       | 17/58 [00:03<00:04,  9.95it/s]Capturing num tokens (num_tokens=1792 avail_mem=26.30 GB):  29%|██▉       | 17/58 [00:03<00:04,  9.95it/s]Capturing num tokens (num_tokens=1536 avail_mem=26.30 GB):  29%|██▉       | 17/58 [00:03<00:04,  9.95it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=26.30 GB):  33%|███▎      | 19/58 [00:03<00:04,  9.27it/s]Capturing num tokens (num_tokens=1280 avail_mem=26.30 GB):  33%|███▎      | 19/58 [00:03<00:04,  9.27it/s]Capturing num tokens (num_tokens=1024 avail_mem=26.30 GB):  33%|███▎      | 19/58 [00:03<00:04,  9.27it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=26.30 GB):  36%|███▌      | 21/58 [00:03<00:04,  9.05it/s]Capturing num tokens (num_tokens=960 avail_mem=26.29 GB):  36%|███▌      | 21/58 [00:03<00:04,  9.05it/s] Capturing num tokens (num_tokens=896 avail_mem=26.29 GB):  36%|███▌      | 21/58 [00:03<00:04,  9.05it/s]Capturing num tokens (num_tokens=832 avail_mem=26.29 GB):  36%|███▌      | 21/58 [00:03<00:04,  9.05it/s]Capturing num tokens (num_tokens=832 avail_mem=26.29 GB):  41%|████▏     | 24/58 [00:03<00:02, 12.15it/s]Capturing num tokens (num_tokens=768 avail_mem=26.28 GB):  41%|████▏     | 24/58 [00:03<00:02, 12.15it/s]Capturing num tokens (num_tokens=704 avail_mem=26.28 GB):  41%|████▏     | 24/58 [00:03<00:02, 12.15it/s]

    Capturing num tokens (num_tokens=640 avail_mem=26.27 GB):  41%|████▏     | 24/58 [00:03<00:02, 12.15it/s]Capturing num tokens (num_tokens=640 avail_mem=26.27 GB):  47%|████▋     | 27/58 [00:03<00:02, 15.45it/s]Capturing num tokens (num_tokens=576 avail_mem=26.27 GB):  47%|████▋     | 27/58 [00:03<00:02, 15.45it/s]Capturing num tokens (num_tokens=512 avail_mem=26.27 GB):  47%|████▋     | 27/58 [00:03<00:02, 15.45it/s]Capturing num tokens (num_tokens=480 avail_mem=26.26 GB):  47%|████▋     | 27/58 [00:04<00:02, 15.45it/s]Capturing num tokens (num_tokens=448 avail_mem=26.26 GB):  47%|████▋     | 27/58 [00:04<00:02, 15.45it/s]Capturing num tokens (num_tokens=448 avail_mem=26.26 GB):  53%|█████▎    | 31/58 [00:04<00:01, 19.34it/s]Capturing num tokens (num_tokens=416 avail_mem=26.25 GB):  53%|█████▎    | 31/58 [00:04<00:01, 19.34it/s]

    Capturing num tokens (num_tokens=384 avail_mem=26.25 GB):  53%|█████▎    | 31/58 [00:04<00:01, 19.34it/s]Capturing num tokens (num_tokens=352 avail_mem=26.24 GB):  53%|█████▎    | 31/58 [00:04<00:01, 19.34it/s]Capturing num tokens (num_tokens=320 avail_mem=26.24 GB):  53%|█████▎    | 31/58 [00:04<00:01, 19.34it/s]Capturing num tokens (num_tokens=320 avail_mem=26.24 GB):  60%|██████    | 35/58 [00:04<00:01, 22.73it/s]Capturing num tokens (num_tokens=288 avail_mem=26.23 GB):  60%|██████    | 35/58 [00:04<00:01, 22.73it/s]Capturing num tokens (num_tokens=256 avail_mem=26.23 GB):  60%|██████    | 35/58 [00:04<00:01, 22.73it/s]Capturing num tokens (num_tokens=240 avail_mem=26.23 GB):  60%|██████    | 35/58 [00:04<00:01, 22.73it/s]Capturing num tokens (num_tokens=224 avail_mem=26.23 GB):  60%|██████    | 35/58 [00:04<00:01, 22.73it/s]

    Capturing num tokens (num_tokens=224 avail_mem=26.23 GB):  67%|██████▋   | 39/58 [00:04<00:00, 25.81it/s]Capturing num tokens (num_tokens=208 avail_mem=26.22 GB):  67%|██████▋   | 39/58 [00:04<00:00, 25.81it/s]Capturing num tokens (num_tokens=192 avail_mem=26.22 GB):  67%|██████▋   | 39/58 [00:04<00:00, 25.81it/s]Capturing num tokens (num_tokens=176 avail_mem=26.21 GB):  67%|██████▋   | 39/58 [00:04<00:00, 25.81it/s]Capturing num tokens (num_tokens=176 avail_mem=26.21 GB):  72%|███████▏  | 42/58 [00:04<00:00, 26.34it/s]Capturing num tokens (num_tokens=160 avail_mem=26.20 GB):  72%|███████▏  | 42/58 [00:04<00:00, 26.34it/s]Capturing num tokens (num_tokens=144 avail_mem=26.20 GB):  72%|███████▏  | 42/58 [00:04<00:00, 26.34it/s]Capturing num tokens (num_tokens=128 avail_mem=26.20 GB):  72%|███████▏  | 42/58 [00:04<00:00, 26.34it/s]Capturing num tokens (num_tokens=112 avail_mem=26.21 GB):  72%|███████▏  | 42/58 [00:04<00:00, 26.34it/s]

    Capturing num tokens (num_tokens=112 avail_mem=26.21 GB):  79%|███████▉  | 46/58 [00:04<00:00, 29.08it/s]Capturing num tokens (num_tokens=96 avail_mem=26.20 GB):  79%|███████▉  | 46/58 [00:04<00:00, 29.08it/s] Capturing num tokens (num_tokens=80 avail_mem=26.20 GB):  79%|███████▉  | 46/58 [00:04<00:00, 29.08it/s]Capturing num tokens (num_tokens=64 avail_mem=26.19 GB):  79%|███████▉  | 46/58 [00:04<00:00, 29.08it/s]Capturing num tokens (num_tokens=48 avail_mem=26.19 GB):  79%|███████▉  | 46/58 [00:04<00:00, 29.08it/s]Capturing num tokens (num_tokens=48 avail_mem=26.19 GB):  86%|████████▌ | 50/58 [00:04<00:00, 28.30it/s]Capturing num tokens (num_tokens=32 avail_mem=26.19 GB):  86%|████████▌ | 50/58 [00:04<00:00, 28.30it/s]Capturing num tokens (num_tokens=28 avail_mem=26.18 GB):  86%|████████▌ | 50/58 [00:04<00:00, 28.30it/s]

    Capturing num tokens (num_tokens=24 avail_mem=26.18 GB):  86%|████████▌ | 50/58 [00:04<00:00, 28.30it/s]Capturing num tokens (num_tokens=20 avail_mem=26.17 GB):  86%|████████▌ | 50/58 [00:04<00:00, 28.30it/s]Capturing num tokens (num_tokens=20 avail_mem=26.17 GB):  93%|█████████▎| 54/58 [00:04<00:00, 30.73it/s]Capturing num tokens (num_tokens=16 avail_mem=26.17 GB):  93%|█████████▎| 54/58 [00:04<00:00, 30.73it/s]Capturing num tokens (num_tokens=12 avail_mem=26.16 GB):  93%|█████████▎| 54/58 [00:04<00:00, 30.73it/s]Capturing num tokens (num_tokens=8 avail_mem=26.16 GB):  93%|█████████▎| 54/58 [00:04<00:00, 30.73it/s] Capturing num tokens (num_tokens=4 avail_mem=26.15 GB):  93%|█████████▎| 54/58 [00:04<00:00, 30.73it/s]Capturing num tokens (num_tokens=4 avail_mem=26.15 GB): 100%|██████████| 58/58 [00:04<00:00, 32.89it/s]Capturing num tokens (num_tokens=4 avail_mem=26.15 GB): 100%|██████████| 58/58 [00:04<00:00, 11.85it/s]


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
    "temperature": 0.1,
    "top_p": 0.95,
    "json_schema": json.dumps(CapitalInfo.model_json_schema()),
}

outputs = llm.generate(prompts, sampling_params)
for prompt, output in zip(prompts, outputs):
    print_highlight("===============================")
    print_highlight(f"Prompt: {prompt}")  # validate the output by the pydantic model
    capital_info = CapitalInfo.model_validate_json(output["text"])
    print_highlight(f"Validated output: {capital_info.model_dump_json()}")
```


<strong style='color: #00008B;'>===============================</strong>



<strong style='color: #00008B;'>Prompt: Give me the information of the capital of China in the JSON format.</strong>



<strong style='color: #00008B;'>Validated output: {"name":"Beijing","population":21500000}</strong>



<strong style='color: #00008B;'>===============================</strong>



<strong style='color: #00008B;'>Prompt: Give me the information of the capital of France in the JSON format.</strong>



<strong style='color: #00008B;'>Validated output: {"name":"Paris","population":2141000}</strong>



<strong style='color: #00008B;'>===============================</strong>



<strong style='color: #00008B;'>Prompt: Give me the information of the capital of Ireland in the JSON format.</strong>



<strong style='color: #00008B;'>Validated output: {"name":"Dublin","population":527617}</strong>


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

sampling_params = {"temperature": 0.1, "top_p": 0.95, "json_schema": json_schema}

outputs = llm.generate(prompts, sampling_params)
for prompt, output in zip(prompts, outputs):
    print_highlight("===============================")
    print_highlight(f"Prompt: {prompt}\nGenerated text: {output['text']}")
```


<strong style='color: #00008B;'>===============================</strong>



<strong style='color: #00008B;'>Prompt: Give me the information of the capital of China in the JSON format.<br>Generated text: {"name": "Beijing", "population": 21500000}</strong>



<strong style='color: #00008B;'>===============================</strong>



<strong style='color: #00008B;'>Prompt: Give me the information of the capital of France in the JSON format.<br>Generated text: {"name": "Paris", "population": 2141000}</strong>



<strong style='color: #00008B;'>===============================</strong>



<strong style='color: #00008B;'>Prompt: Give me the information of the capital of Ireland in the JSON format.<br>Generated text: {"name": "Dublin", "population": 527617}</strong>


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
    print_highlight("===============================")
    print_highlight(f"Prompt: {prompt}\nGenerated text: {output['text']}")
```


<strong style='color: #00008B;'>===============================</strong>



<strong style='color: #00008B;'>Prompt: Give me the information of the capital of France.<br>Generated text: Paris is the capital of France</strong>



<strong style='color: #00008B;'>===============================</strong>



<strong style='color: #00008B;'>Prompt: Give me the information of the capital of Germany.<br>Generated text: Berlin is the capital of Germany</strong>



<strong style='color: #00008B;'>===============================</strong>



<strong style='color: #00008B;'>Prompt: Give me the information of the capital of Italy.<br>Generated text: London is the capital of Italy</strong>


### Regular expression


```python
prompts = [
    "Please provide information about London as a major global city:",
    "Please provide information about Paris as a major global city:",
]

sampling_params = {"temperature": 0.8, "top_p": 0.95, "regex": "(France|England)"}

outputs = llm.generate(prompts, sampling_params)
for prompt, output in zip(prompts, outputs):
    print_highlight("===============================")
    print_highlight(f"Prompt: {prompt}\nGenerated text: {output['text']}")
```


<strong style='color: #00008B;'>===============================</strong>



<strong style='color: #00008B;'>Prompt: Please provide information about London as a major global city:<br>Generated text: England</strong>



<strong style='color: #00008B;'>===============================</strong>



<strong style='color: #00008B;'>Prompt: Please provide information about Paris as a major global city:<br>Generated text: France</strong>


### Structural Tag


```python
text = tokenizer.apply_chat_template(
    messages, tokenize=False, add_generation_prompt=True, return_dict=False
)
prompts = [text]


sampling_params = {
    "temperature": 0.8,
    "top_p": 0.95,
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
    print_highlight("===============================")
    print_highlight(f"Prompt: {prompt}\nGenerated text: {output['text']}")
```


<strong style='color: #00008B;'>===============================</strong>



<strong style='color: #00008B;'>Prompt: <|begin_of_text|><|start_header_id|>system<|end_header_id|><br><br>Cutting Knowledge Date: December 2023<br>Today Date: 26 Jul 2024<br><br><|eot_id|><|start_header_id|>user<|end_header_id|><br><br>Paris is the capital of<|eot_id|><|start_header_id|>assistant<|end_header_id|><br><br><br>Generated text: France.</strong>



```python
# Support for XGrammar latest structural tag format
# <https://xgrammar.mlc.ai/docs/tutorials/structural_tag.html>
sampling_params = {
    "temperature": 0.8,
    "top_p": 0.95,
    "structural_tag": json.dumps(
        {
            "type": "structural_tag",
            "format": {
                "type": "triggered_tags",
                "triggers": ["<function="],
                "tags": [
                    {
                        "begin": "<function=get_current_weather>",
                        "content": {
                            "type": "json_schema",
                            "json_schema": schema_get_current_weather,
                        },
                        "end": "</function>",
                    },
                    {
                        "begin": "<function=get_current_date>",
                        "content": {
                            "type": "json_schema",
                            "json_schema": schema_get_current_date,
                        },
                        "end": "</function>",
                    },
                ],
                "at_least_one": False,
                "stop_after_first": False,
            },
        }
    ),
}


# Send POST request to the API endpoint
outputs = llm.generate(prompts, sampling_params)
for prompt, output in zip(prompts, outputs):
    print_highlight("===============================")
    print_highlight(f"Prompt: {prompt}\nGenerated text: {output['text']}")
```


<strong style='color: #00008B;'>===============================</strong>



<strong style='color: #00008B;'>Prompt: <|begin_of_text|><|start_header_id|>system<|end_header_id|><br><br>Cutting Knowledge Date: December 2023<br>Today Date: 26 Jul 2024<br><br><|eot_id|><|start_header_id|>user<|end_header_id|><br><br>Paris is the capital of<|eot_id|><|start_header_id|>assistant<|end_header_id|><br><br><br>Generated text: France.</strong>



```python
llm.shutdown()
```
