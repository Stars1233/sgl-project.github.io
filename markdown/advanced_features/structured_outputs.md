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


    2026-04-01 15:59:02.742 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 15:59:02] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 15:59:02.742 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 15:59:02] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 15:59:02.742 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 15:59:02] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 15:59:02.742 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 15:59:02] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 15:59:02.742 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 15:59:02] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  25% Completed | 1/4 [00:00<00:00,  4.73it/s]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:01<00:01,  1.42it/s]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:02<00:00,  1.03it/s]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:03<00:00,  1.09s/it]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:03<00:00,  1.05it/s]


    2026-04-01 15:59:09,099 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 15:59:09] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:16,  3.44s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:16,  3.44s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:41,  1.82s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:41,  1.82s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:04,  1.18s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:04,  1.18s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:42,  1.27it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:42,  1.27it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:30,  1.75it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:30,  1.75it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:22,  2.31it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:22,  2.31it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:17,  2.96it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:17,  2.96it/s]Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:13,  3.66it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:13,  3.66it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:10,  4.47it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:10,  4.47it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:08,  5.37it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:08,  5.37it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:07,  6.24it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:07,  6.24it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:05<00:07,  6.24it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:05<00:05,  7.97it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:05<00:05,  7.97it/s]

    Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:05<00:05,  7.97it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:04,  9.59it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:04,  9.59it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:06<00:04,  9.59it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:06<00:03, 11.46it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:06<00:03, 11.46it/s]

    Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:06<00:03, 11.46it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:06<00:03, 11.46it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:06<00:02, 14.79it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:06<00:02, 14.79it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:06<00:02, 14.79it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:06<00:02, 14.79it/s]Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:06<00:02, 14.79it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:06<00:01, 20.34it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:06<00:01, 20.34it/s]

    Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:06<00:01, 20.34it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:06<00:01, 20.34it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:06<00:01, 20.34it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:06<00:01, 20.34it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:06<00:01, 27.10it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:06<00:01, 27.10it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:06<00:01, 27.10it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:06<00:01, 27.10it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:06<00:01, 27.10it/s]Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:06<00:01, 27.10it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:06<00:01, 27.10it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:06<00:00, 34.67it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:06<00:00, 34.67it/s]

    Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:06<00:00, 34.67it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:06<00:00, 34.67it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:06<00:00, 34.67it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:06<00:00, 34.67it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:06<00:00, 34.67it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:06<00:00, 41.08it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:06<00:00, 41.08it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:06<00:00, 41.08it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:06<00:00, 41.08it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:06<00:00, 41.08it/s]

    Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:07<00:00, 41.08it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:08<00:01,  9.81it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:08<00:01,  9.81it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:08<00:01,  9.81it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:08<00:01,  9.81it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:08<00:01,  9.81it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:08<00:00, 11.78it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:08<00:00, 11.78it/s]

    Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:08<00:00, 11.78it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:08<00:00, 11.78it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:08<00:00, 11.78it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:08<00:00, 14.30it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:08<00:00, 14.30it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:08<00:00, 14.30it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:08<00:00, 14.30it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:08<00:00, 14.30it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00,  6.91it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=100.17 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=100.17 GB):   2%|▏         | 1/58 [00:00<00:40,  1.39it/s]Capturing num tokens (num_tokens=7680 avail_mem=100.18 GB):   2%|▏         | 1/58 [00:00<00:40,  1.39it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=100.18 GB):   3%|▎         | 2/58 [00:01<00:38,  1.45it/s]Capturing num tokens (num_tokens=7168 avail_mem=100.15 GB):   3%|▎         | 2/58 [00:01<00:38,  1.45it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=100.15 GB):   5%|▌         | 3/58 [00:01<00:33,  1.62it/s]Capturing num tokens (num_tokens=6656 avail_mem=99.66 GB):   5%|▌         | 3/58 [00:01<00:33,  1.62it/s] 

    Capturing num tokens (num_tokens=6656 avail_mem=99.66 GB):   7%|▋         | 4/58 [00:02<00:30,  1.79it/s]Capturing num tokens (num_tokens=6144 avail_mem=99.73 GB):   7%|▋         | 4/58 [00:02<00:30,  1.79it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=99.73 GB):   9%|▊         | 5/58 [00:02<00:27,  1.94it/s]Capturing num tokens (num_tokens=5632 avail_mem=99.76 GB):   9%|▊         | 5/58 [00:02<00:27,  1.94it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=99.76 GB):  10%|█         | 6/58 [00:03<00:23,  2.18it/s]Capturing num tokens (num_tokens=5120 avail_mem=100.13 GB):  10%|█         | 6/58 [00:03<00:23,  2.18it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=100.13 GB):  12%|█▏        | 7/58 [00:03<00:21,  2.42it/s]Capturing num tokens (num_tokens=4608 avail_mem=100.14 GB):  12%|█▏        | 7/58 [00:03<00:21,  2.42it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=100.14 GB):  14%|█▍        | 8/58 [00:03<00:18,  2.73it/s]Capturing num tokens (num_tokens=4096 avail_mem=100.14 GB):  14%|█▍        | 8/58 [00:03<00:18,  2.73it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=100.14 GB):  16%|█▌        | 9/58 [00:03<00:15,  3.11it/s]Capturing num tokens (num_tokens=3840 avail_mem=100.13 GB):  16%|█▌        | 9/58 [00:03<00:15,  3.11it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=100.13 GB):  17%|█▋        | 10/58 [00:04<00:13,  3.48it/s]Capturing num tokens (num_tokens=3584 avail_mem=100.13 GB):  17%|█▋        | 10/58 [00:04<00:13,  3.48it/s]Capturing num tokens (num_tokens=3584 avail_mem=100.13 GB):  19%|█▉        | 11/58 [00:04<00:12,  3.88it/s]Capturing num tokens (num_tokens=3328 avail_mem=100.12 GB):  19%|█▉        | 11/58 [00:04<00:12,  3.88it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=100.12 GB):  21%|██        | 12/58 [00:04<00:10,  4.31it/s]Capturing num tokens (num_tokens=3072 avail_mem=100.11 GB):  21%|██        | 12/58 [00:04<00:10,  4.31it/s]Capturing num tokens (num_tokens=3072 avail_mem=100.11 GB):  22%|██▏       | 13/58 [00:04<00:09,  4.77it/s]Capturing num tokens (num_tokens=2816 avail_mem=100.10 GB):  22%|██▏       | 13/58 [00:04<00:09,  4.77it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=100.10 GB):  24%|██▍       | 14/58 [00:04<00:08,  5.30it/s]Capturing num tokens (num_tokens=2560 avail_mem=100.10 GB):  24%|██▍       | 14/58 [00:04<00:08,  5.30it/s]Capturing num tokens (num_tokens=2560 avail_mem=100.10 GB):  26%|██▌       | 15/58 [00:04<00:07,  5.89it/s]Capturing num tokens (num_tokens=2304 avail_mem=100.09 GB):  26%|██▌       | 15/58 [00:04<00:07,  5.89it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=100.09 GB):  28%|██▊       | 16/58 [00:05<00:06,  6.45it/s]Capturing num tokens (num_tokens=2048 avail_mem=100.08 GB):  28%|██▊       | 16/58 [00:05<00:06,  6.45it/s]Capturing num tokens (num_tokens=1792 avail_mem=100.07 GB):  28%|██▊       | 16/58 [00:05<00:06,  6.45it/s]Capturing num tokens (num_tokens=1792 avail_mem=100.07 GB):  31%|███       | 18/58 [00:05<00:05,  7.98it/s]Capturing num tokens (num_tokens=1536 avail_mem=100.07 GB):  31%|███       | 18/58 [00:05<00:05,  7.98it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=100.06 GB):  31%|███       | 18/58 [00:05<00:05,  7.98it/s]Capturing num tokens (num_tokens=1280 avail_mem=100.06 GB):  34%|███▍      | 20/58 [00:05<00:03,  9.50it/s]Capturing num tokens (num_tokens=1024 avail_mem=100.06 GB):  34%|███▍      | 20/58 [00:05<00:03,  9.50it/s]Capturing num tokens (num_tokens=960 avail_mem=100.05 GB):  34%|███▍      | 20/58 [00:05<00:03,  9.50it/s] Capturing num tokens (num_tokens=960 avail_mem=100.05 GB):  38%|███▊      | 22/58 [00:05<00:03, 11.23it/s]Capturing num tokens (num_tokens=896 avail_mem=100.05 GB):  38%|███▊      | 22/58 [00:05<00:03, 11.23it/s]

    Capturing num tokens (num_tokens=832 avail_mem=100.04 GB):  38%|███▊      | 22/58 [00:05<00:03, 11.23it/s]Capturing num tokens (num_tokens=832 avail_mem=100.04 GB):  41%|████▏     | 24/58 [00:05<00:02, 12.97it/s]Capturing num tokens (num_tokens=768 avail_mem=100.03 GB):  41%|████▏     | 24/58 [00:05<00:02, 12.97it/s]Capturing num tokens (num_tokens=704 avail_mem=100.02 GB):  41%|████▏     | 24/58 [00:05<00:02, 12.97it/s]Capturing num tokens (num_tokens=704 avail_mem=100.02 GB):  45%|████▍     | 26/58 [00:05<00:02, 14.46it/s]Capturing num tokens (num_tokens=640 avail_mem=100.02 GB):  45%|████▍     | 26/58 [00:05<00:02, 14.46it/s]

    Capturing num tokens (num_tokens=576 avail_mem=100.01 GB):  45%|████▍     | 26/58 [00:05<00:02, 14.46it/s]Capturing num tokens (num_tokens=512 avail_mem=100.00 GB):  45%|████▍     | 26/58 [00:05<00:02, 14.46it/s]Capturing num tokens (num_tokens=512 avail_mem=100.00 GB):  50%|█████     | 29/58 [00:05<00:01, 17.01it/s]Capturing num tokens (num_tokens=480 avail_mem=99.99 GB):  50%|█████     | 29/58 [00:05<00:01, 17.01it/s] Capturing num tokens (num_tokens=448 avail_mem=99.99 GB):  50%|█████     | 29/58 [00:05<00:01, 17.01it/s]Capturing num tokens (num_tokens=416 avail_mem=99.98 GB):  50%|█████     | 29/58 [00:05<00:01, 17.01it/s]

    Capturing num tokens (num_tokens=416 avail_mem=99.98 GB):  55%|█████▌    | 32/58 [00:06<00:01, 18.92it/s]Capturing num tokens (num_tokens=384 avail_mem=99.97 GB):  55%|█████▌    | 32/58 [00:06<00:01, 18.92it/s]Capturing num tokens (num_tokens=352 avail_mem=99.96 GB):  55%|█████▌    | 32/58 [00:06<00:01, 18.92it/s]Capturing num tokens (num_tokens=320 avail_mem=99.95 GB):  55%|█████▌    | 32/58 [00:06<00:01, 18.92it/s]Capturing num tokens (num_tokens=320 avail_mem=99.95 GB):  60%|██████    | 35/58 [00:06<00:01, 20.86it/s]Capturing num tokens (num_tokens=288 avail_mem=99.94 GB):  60%|██████    | 35/58 [00:06<00:01, 20.86it/s]Capturing num tokens (num_tokens=256 avail_mem=99.94 GB):  60%|██████    | 35/58 [00:06<00:01, 20.86it/s]Capturing num tokens (num_tokens=240 avail_mem=99.94 GB):  60%|██████    | 35/58 [00:06<00:01, 20.86it/s]

    Capturing num tokens (num_tokens=224 avail_mem=99.93 GB):  60%|██████    | 35/58 [00:06<00:01, 20.86it/s]Capturing num tokens (num_tokens=224 avail_mem=99.93 GB):  67%|██████▋   | 39/58 [00:06<00:00, 24.55it/s]Capturing num tokens (num_tokens=208 avail_mem=99.93 GB):  67%|██████▋   | 39/58 [00:06<00:00, 24.55it/s]Capturing num tokens (num_tokens=192 avail_mem=99.90 GB):  67%|██████▋   | 39/58 [00:06<00:00, 24.55it/s]Capturing num tokens (num_tokens=176 avail_mem=99.89 GB):  67%|██████▋   | 39/58 [00:06<00:00, 24.55it/s]Capturing num tokens (num_tokens=160 avail_mem=99.89 GB):  67%|██████▋   | 39/58 [00:06<00:00, 24.55it/s]Capturing num tokens (num_tokens=160 avail_mem=99.89 GB):  74%|███████▍  | 43/58 [00:06<00:00, 27.23it/s]Capturing num tokens (num_tokens=144 avail_mem=99.89 GB):  74%|███████▍  | 43/58 [00:06<00:00, 27.23it/s]Capturing num tokens (num_tokens=128 avail_mem=99.88 GB):  74%|███████▍  | 43/58 [00:06<00:00, 27.23it/s]

    Capturing num tokens (num_tokens=112 avail_mem=99.89 GB):  74%|███████▍  | 43/58 [00:06<00:00, 27.23it/s]Capturing num tokens (num_tokens=96 avail_mem=99.89 GB):  74%|███████▍  | 43/58 [00:06<00:00, 27.23it/s] Capturing num tokens (num_tokens=96 avail_mem=99.89 GB):  81%|████████  | 47/58 [00:06<00:00, 28.86it/s]Capturing num tokens (num_tokens=80 avail_mem=99.89 GB):  81%|████████  | 47/58 [00:06<00:00, 28.86it/s]Capturing num tokens (num_tokens=64 avail_mem=99.88 GB):  81%|████████  | 47/58 [00:06<00:00, 28.86it/s]

    Capturing num tokens (num_tokens=48 avail_mem=102.50 GB):  81%|████████  | 47/58 [00:06<00:00, 28.86it/s]Capturing num tokens (num_tokens=48 avail_mem=102.50 GB):  86%|████████▌ | 50/58 [00:06<00:00, 21.42it/s]Capturing num tokens (num_tokens=32 avail_mem=102.50 GB):  86%|████████▌ | 50/58 [00:06<00:00, 21.42it/s]Capturing num tokens (num_tokens=28 avail_mem=102.49 GB):  86%|████████▌ | 50/58 [00:06<00:00, 21.42it/s]Capturing num tokens (num_tokens=24 avail_mem=102.49 GB):  86%|████████▌ | 50/58 [00:06<00:00, 21.42it/s]Capturing num tokens (num_tokens=20 avail_mem=102.48 GB):  86%|████████▌ | 50/58 [00:06<00:00, 21.42it/s]Capturing num tokens (num_tokens=20 avail_mem=102.48 GB):  93%|█████████▎| 54/58 [00:06<00:00, 24.36it/s]Capturing num tokens (num_tokens=16 avail_mem=102.48 GB):  93%|█████████▎| 54/58 [00:06<00:00, 24.36it/s]Capturing num tokens (num_tokens=12 avail_mem=102.48 GB):  93%|█████████▎| 54/58 [00:06<00:00, 24.36it/s]

    Capturing num tokens (num_tokens=8 avail_mem=102.47 GB):  93%|█████████▎| 54/58 [00:06<00:00, 24.36it/s] Capturing num tokens (num_tokens=4 avail_mem=102.47 GB):  93%|█████████▎| 54/58 [00:06<00:00, 24.36it/s]Capturing num tokens (num_tokens=4 avail_mem=102.47 GB): 100%|██████████| 58/58 [00:06<00:00, 26.97it/s]Capturing num tokens (num_tokens=4 avail_mem=102.47 GB): 100%|██████████| 58/58 [00:06<00:00,  8.31it/s]


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


<strong style='color: #00008B;'><function=get_current_date>{"timezone": "America/New_York"}</function><br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "fahrenheit"}</function></strong>


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


<strong style='color: #00008B;'>{'text': '{"name": "Paris", "population": 2147000}', 'output_ids': [5018, 609, 794, 330, 60704, 498, 330, 45541, 794, 220, 11584, 7007, 15, 92, 128009], 'meta_info': {'id': '010253a0483249dba55fe87e9879b0fc', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 50, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 15, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.17562203016132116, 'response_sent_to_client_ts': 1775059178.559764}}</strong>



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


<strong style='color: #00008B;'>{'text': '{"name": "Paris", "population": 2147000}', 'output_ids': [5018, 609, 794, 330, 60704, 498, 330, 45541, 794, 220, 11584, 7007, 15, 92, 128009], 'meta_info': {'id': '87e4bc3ff6c5477c911c89d6f8b6ff9e', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 50, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 15, 'cached_tokens': 49, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.1792942830361426, 'response_sent_to_client_ts': 1775059178.7497506}}</strong>


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


<strong style='color: #00008B;'>[{'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': '093dcc35f7924cd3a338da69af3bbfd2', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.09381657838821411, 'response_sent_to_client_ts': 1775059178.879731}}, {'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': 'fb3219cbc8a14dca87a9aecb7256e418', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.09370895894244313, 'response_sent_to_client_ts': 1775059178.879747}}, {'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': '2fd076b3667a45eea4f3f02962ea29ec', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.0936511131003499, 'response_sent_to_client_ts': 1775059178.879754}}]</strong>


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


<strong style='color: #00008B;'>{'text': 'France', 'output_ids': [50100, 128009], 'meta_info': {'id': '759f8c625551449db7db017a92e58b7b', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2, 'cached_tokens': 31, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.03656957997009158, 'response_sent_to_client_ts': 1775059178.9253993}}</strong>


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


<strong style='color: #00008B;'>{'text': 'France.', 'output_ids': [50100, 13, 128009], 'meta_info': {'id': 'a6549847ce3343ac813e2cf529a4ba10', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 3, 'cached_tokens': 40, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.1064315028488636, 'response_sent_to_client_ts': 1775059180.329255}}</strong>



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


<strong style='color: #00008B;'>{'text': 'France', 'output_ids': [50100, 128009], 'meta_info': {'id': '4ab1d812951149a8a945fe376218f2e8', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2, 'cached_tokens': 40, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.041418292094022036, 'response_sent_to_client_ts': 1775059180.3827767}}</strong>



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


    2026-04-01 15:59:54.545 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 15:59:54] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 15:59:54.545 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 15:59:54] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 15:59:54.545 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 15:59:54] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 15:59:54.545 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 15:59:54] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 15:59:54.545 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 15:59:54] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]Loading safetensors checkpoint shards:  25% Completed | 1/4 [00:00<00:00,  5.99it/s]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:00<00:01,  1.99it/s]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:01<00:00,  1.43it/s]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.31it/s]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.48it/s]


    2026-04-01 15:59:59,759 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 15:59:59] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:18,  3.48s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:18,  3.48s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<02:02,  2.18s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<02:02,  2.18s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:05<01:21,  1.48s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:05<01:21,  1.48s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:05<01:00,  1.13s/it]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:05<01:00,  1.13s/it]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:06<00:48,  1.08it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:06<00:48,  1.08it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:07<00:40,  1.29it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:07<00:40,  1.29it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:07<00:34,  1.50it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:07<00:34,  1.50it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:07<00:28,  1.74it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:07<00:28,  1.74it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:08<00:24,  2.03it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:08<00:24,  2.03it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:08<00:20,  2.31it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:08<00:20,  2.31it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:08<00:17,  2.61it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:08<00:17,  2.61it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:08<00:15,  3.00it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:08<00:15,  3.00it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:09<00:13,  3.23it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:09<00:13,  3.23it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:09<00:12,  3.58it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:09<00:12,  3.58it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:09<00:11,  3.86it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:09<00:11,  3.86it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:09<00:09,  4.43it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:09<00:09,  4.43it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:09<00:08,  4.65it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:09<00:08,  4.65it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:10<00:07,  5.29it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:10<00:07,  5.29it/s]

    Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:10<00:06,  6.08it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:10<00:06,  6.08it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:10<00:05,  6.54it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:10<00:05,  6.54it/s]

    Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:10<00:05,  6.54it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:10<00:04,  8.47it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:10<00:04,  8.47it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:10<00:04,  8.47it/s]

    Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:10<00:03,  9.80it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:10<00:03,  9.80it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:10<00:03,  9.80it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:10<00:02, 11.10it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:10<00:02, 11.10it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:10<00:02, 11.10it/s]

    Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:10<00:02, 12.88it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:10<00:02, 12.88it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:10<00:02, 12.88it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:11<00:02, 13.51it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:11<00:02, 13.51it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:11<00:02, 13.51it/s]

    Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:11<00:01, 14.99it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:11<00:01, 14.99it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:11<00:01, 14.99it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:11<00:01, 16.01it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:11<00:01, 16.01it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:11<00:01, 16.01it/s]

    Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:11<00:01, 16.85it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:11<00:01, 16.85it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:11<00:01, 16.85it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:11<00:01, 16.85it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:11<00:00, 19.03it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:11<00:00, 19.03it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:11<00:00, 19.03it/s]

    Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:11<00:00, 19.03it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:11<00:00, 20.13it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:11<00:00, 20.13it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:11<00:00, 20.13it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:11<00:00, 20.13it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:11<00:00, 20.55it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:11<00:00, 20.55it/s]

    Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:11<00:00, 20.55it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:11<00:00, 20.55it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:11<00:00, 21.84it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:11<00:00, 21.84it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:11<00:00, 21.84it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:11<00:00, 21.84it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:11<00:00, 23.15it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:11<00:00, 23.15it/s]

    Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:12<00:00, 23.15it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:12<00:00, 23.15it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:12<00:00, 24.18it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:12<00:00, 24.18it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:12<00:00, 24.18it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:12<00:00, 24.18it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:12<00:00, 24.18it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:12<00:00, 27.99it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:12<00:00,  4.76it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=83.44 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=83.44 GB):   2%|▏         | 1/58 [00:00<00:47,  1.20it/s]Capturing num tokens (num_tokens=7680 avail_mem=83.40 GB):   2%|▏         | 1/58 [00:00<00:47,  1.20it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=83.40 GB):   3%|▎         | 2/58 [00:01<00:43,  1.28it/s]Capturing num tokens (num_tokens=7168 avail_mem=83.34 GB):   3%|▎         | 2/58 [00:01<00:43,  1.28it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=83.34 GB):   5%|▌         | 3/58 [00:02<00:39,  1.40it/s]Capturing num tokens (num_tokens=6656 avail_mem=83.37 GB):   5%|▌         | 3/58 [00:02<00:39,  1.40it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=83.37 GB):   7%|▋         | 4/58 [00:02<00:35,  1.53it/s]Capturing num tokens (num_tokens=6144 avail_mem=83.36 GB):   7%|▋         | 4/58 [00:02<00:35,  1.53it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=83.36 GB):   9%|▊         | 5/58 [00:03<00:32,  1.65it/s]Capturing num tokens (num_tokens=5632 avail_mem=83.36 GB):   9%|▊         | 5/58 [00:03<00:32,  1.65it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=83.36 GB):  10%|█         | 6/58 [00:03<00:28,  1.81it/s]Capturing num tokens (num_tokens=5120 avail_mem=83.35 GB):  10%|█         | 6/58 [00:03<00:28,  1.81it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=83.35 GB):  12%|█▏        | 7/58 [00:04<00:25,  1.97it/s]Capturing num tokens (num_tokens=4608 avail_mem=83.34 GB):  12%|█▏        | 7/58 [00:04<00:25,  1.97it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=83.34 GB):  14%|█▍        | 8/58 [00:04<00:23,  2.16it/s]Capturing num tokens (num_tokens=4096 avail_mem=83.35 GB):  14%|█▍        | 8/58 [00:04<00:23,  2.16it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=83.35 GB):  16%|█▌        | 9/58 [00:04<00:20,  2.35it/s]Capturing num tokens (num_tokens=3840 avail_mem=83.32 GB):  16%|█▌        | 9/58 [00:04<00:20,  2.35it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=83.32 GB):  17%|█▋        | 10/58 [00:05<00:18,  2.54it/s]Capturing num tokens (num_tokens=3584 avail_mem=83.34 GB):  17%|█▋        | 10/58 [00:05<00:18,  2.54it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=83.34 GB):  19%|█▉        | 11/58 [00:05<00:17,  2.73it/s]Capturing num tokens (num_tokens=3328 avail_mem=83.33 GB):  19%|█▉        | 11/58 [00:05<00:17,  2.73it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=83.33 GB):  21%|██        | 12/58 [00:05<00:15,  2.93it/s]Capturing num tokens (num_tokens=3072 avail_mem=83.30 GB):  21%|██        | 12/58 [00:05<00:15,  2.93it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=83.30 GB):  22%|██▏       | 13/58 [00:06<00:14,  3.15it/s]Capturing num tokens (num_tokens=2816 avail_mem=83.31 GB):  22%|██▏       | 13/58 [00:06<00:14,  3.15it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=83.31 GB):  24%|██▍       | 14/58 [00:06<00:12,  3.40it/s]Capturing num tokens (num_tokens=2560 avail_mem=83.30 GB):  24%|██▍       | 14/58 [00:06<00:12,  3.40it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=83.30 GB):  26%|██▌       | 15/58 [00:06<00:11,  3.67it/s]Capturing num tokens (num_tokens=2304 avail_mem=83.30 GB):  26%|██▌       | 15/58 [00:06<00:11,  3.67it/s]Capturing num tokens (num_tokens=2304 avail_mem=83.30 GB):  28%|██▊       | 16/58 [00:06<00:10,  4.06it/s]Capturing num tokens (num_tokens=2048 avail_mem=83.29 GB):  28%|██▊       | 16/58 [00:06<00:10,  4.06it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=83.29 GB):  29%|██▉       | 17/58 [00:06<00:08,  4.65it/s]Capturing num tokens (num_tokens=1792 avail_mem=83.29 GB):  29%|██▉       | 17/58 [00:06<00:08,  4.65it/s]Capturing num tokens (num_tokens=1792 avail_mem=83.29 GB):  31%|███       | 18/58 [00:06<00:07,  5.19it/s]Capturing num tokens (num_tokens=1536 avail_mem=83.29 GB):  31%|███       | 18/58 [00:06<00:07,  5.19it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=83.29 GB):  33%|███▎      | 19/58 [00:07<00:06,  5.86it/s]Capturing num tokens (num_tokens=1280 avail_mem=83.29 GB):  33%|███▎      | 19/58 [00:07<00:06,  5.86it/s]Capturing num tokens (num_tokens=1280 avail_mem=83.29 GB):  34%|███▍      | 20/58 [00:07<00:05,  6.65it/s]Capturing num tokens (num_tokens=1024 avail_mem=83.29 GB):  34%|███▍      | 20/58 [00:07<00:05,  6.65it/s]Capturing num tokens (num_tokens=960 avail_mem=83.29 GB):  34%|███▍      | 20/58 [00:07<00:05,  6.65it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=83.29 GB):  38%|███▊      | 22/58 [00:07<00:04,  8.17it/s]Capturing num tokens (num_tokens=896 avail_mem=83.28 GB):  38%|███▊      | 22/58 [00:07<00:04,  8.17it/s]Capturing num tokens (num_tokens=832 avail_mem=83.28 GB):  38%|███▊      | 22/58 [00:07<00:04,  8.17it/s]Capturing num tokens (num_tokens=832 avail_mem=83.28 GB):  41%|████▏     | 24/58 [00:07<00:03,  9.26it/s]Capturing num tokens (num_tokens=768 avail_mem=83.28 GB):  41%|████▏     | 24/58 [00:07<00:03,  9.26it/s]

    Capturing num tokens (num_tokens=704 avail_mem=83.27 GB):  41%|████▏     | 24/58 [00:07<00:03,  9.26it/s]Capturing num tokens (num_tokens=704 avail_mem=83.27 GB):  45%|████▍     | 26/58 [00:07<00:03, 10.20it/s]Capturing num tokens (num_tokens=640 avail_mem=83.27 GB):  45%|████▍     | 26/58 [00:07<00:03, 10.20it/s]Capturing num tokens (num_tokens=576 avail_mem=83.27 GB):  45%|████▍     | 26/58 [00:07<00:03, 10.20it/s]

    Capturing num tokens (num_tokens=576 avail_mem=83.27 GB):  48%|████▊     | 28/58 [00:07<00:02, 11.25it/s]Capturing num tokens (num_tokens=512 avail_mem=83.26 GB):  48%|████▊     | 28/58 [00:07<00:02, 11.25it/s]Capturing num tokens (num_tokens=480 avail_mem=83.26 GB):  48%|████▊     | 28/58 [00:07<00:02, 11.25it/s]Capturing num tokens (num_tokens=480 avail_mem=83.26 GB):  52%|█████▏    | 30/58 [00:07<00:02, 12.27it/s]Capturing num tokens (num_tokens=448 avail_mem=83.25 GB):  52%|█████▏    | 30/58 [00:07<00:02, 12.27it/s]

    Capturing num tokens (num_tokens=416 avail_mem=83.25 GB):  52%|█████▏    | 30/58 [00:08<00:02, 12.27it/s]Capturing num tokens (num_tokens=416 avail_mem=83.25 GB):  55%|█████▌    | 32/58 [00:08<00:02, 12.77it/s]Capturing num tokens (num_tokens=384 avail_mem=83.24 GB):  55%|█████▌    | 32/58 [00:08<00:02, 12.77it/s]Capturing num tokens (num_tokens=352 avail_mem=83.24 GB):  55%|█████▌    | 32/58 [00:08<00:02, 12.77it/s]Capturing num tokens (num_tokens=352 avail_mem=83.24 GB):  59%|█████▊    | 34/58 [00:08<00:01, 13.38it/s]Capturing num tokens (num_tokens=320 avail_mem=83.24 GB):  59%|█████▊    | 34/58 [00:08<00:01, 13.38it/s]

    Capturing num tokens (num_tokens=288 avail_mem=83.23 GB):  59%|█████▊    | 34/58 [00:08<00:01, 13.38it/s]Capturing num tokens (num_tokens=288 avail_mem=83.23 GB):  62%|██████▏   | 36/58 [00:08<00:01, 14.42it/s]Capturing num tokens (num_tokens=256 avail_mem=83.23 GB):  62%|██████▏   | 36/58 [00:08<00:01, 14.42it/s]Capturing num tokens (num_tokens=240 avail_mem=83.22 GB):  62%|██████▏   | 36/58 [00:08<00:01, 14.42it/s]Capturing num tokens (num_tokens=240 avail_mem=83.22 GB):  66%|██████▌   | 38/58 [00:08<00:01, 15.57it/s]Capturing num tokens (num_tokens=224 avail_mem=83.22 GB):  66%|██████▌   | 38/58 [00:08<00:01, 15.57it/s]

    Capturing num tokens (num_tokens=208 avail_mem=83.21 GB):  66%|██████▌   | 38/58 [00:08<00:01, 15.57it/s]Capturing num tokens (num_tokens=208 avail_mem=83.21 GB):  69%|██████▉   | 40/58 [00:08<00:01, 16.31it/s]Capturing num tokens (num_tokens=192 avail_mem=83.21 GB):  69%|██████▉   | 40/58 [00:08<00:01, 16.31it/s]Capturing num tokens (num_tokens=176 avail_mem=83.20 GB):  69%|██████▉   | 40/58 [00:08<00:01, 16.31it/s]Capturing num tokens (num_tokens=176 avail_mem=83.20 GB):  72%|███████▏  | 42/58 [00:08<00:00, 17.19it/s]Capturing num tokens (num_tokens=160 avail_mem=83.20 GB):  72%|███████▏  | 42/58 [00:08<00:00, 17.19it/s]

    Capturing num tokens (num_tokens=144 avail_mem=83.19 GB):  72%|███████▏  | 42/58 [00:08<00:00, 17.19it/s]Capturing num tokens (num_tokens=144 avail_mem=83.19 GB):  76%|███████▌  | 44/58 [00:08<00:00, 17.76it/s]Capturing num tokens (num_tokens=128 avail_mem=83.19 GB):  76%|███████▌  | 44/58 [00:08<00:00, 17.76it/s]Capturing num tokens (num_tokens=112 avail_mem=83.20 GB):  76%|███████▌  | 44/58 [00:08<00:00, 17.76it/s]Capturing num tokens (num_tokens=96 avail_mem=83.20 GB):  76%|███████▌  | 44/58 [00:08<00:00, 17.76it/s] Capturing num tokens (num_tokens=96 avail_mem=83.20 GB):  81%|████████  | 47/58 [00:08<00:00, 18.82it/s]Capturing num tokens (num_tokens=80 avail_mem=83.19 GB):  81%|████████  | 47/58 [00:08<00:00, 18.82it/s]

    Capturing num tokens (num_tokens=64 avail_mem=83.19 GB):  81%|████████  | 47/58 [00:09<00:00, 18.82it/s]Capturing num tokens (num_tokens=64 avail_mem=83.19 GB):  84%|████████▍ | 49/58 [00:09<00:00, 18.81it/s]Capturing num tokens (num_tokens=48 avail_mem=83.19 GB):  84%|████████▍ | 49/58 [00:09<00:00, 18.81it/s]Capturing num tokens (num_tokens=32 avail_mem=83.18 GB):  84%|████████▍ | 49/58 [00:09<00:00, 18.81it/s]Capturing num tokens (num_tokens=28 avail_mem=83.18 GB):  84%|████████▍ | 49/58 [00:09<00:00, 18.81it/s]Capturing num tokens (num_tokens=28 avail_mem=83.18 GB):  90%|████████▉ | 52/58 [00:09<00:00, 20.19it/s]Capturing num tokens (num_tokens=24 avail_mem=83.17 GB):  90%|████████▉ | 52/58 [00:09<00:00, 20.19it/s]

    Capturing num tokens (num_tokens=20 avail_mem=83.17 GB):  90%|████████▉ | 52/58 [00:09<00:00, 20.19it/s]Capturing num tokens (num_tokens=16 avail_mem=83.16 GB):  90%|████████▉ | 52/58 [00:09<00:00, 20.19it/s]Capturing num tokens (num_tokens=16 avail_mem=83.16 GB):  95%|█████████▍| 55/58 [00:09<00:00, 21.15it/s]Capturing num tokens (num_tokens=12 avail_mem=83.16 GB):  95%|█████████▍| 55/58 [00:09<00:00, 21.15it/s]Capturing num tokens (num_tokens=8 avail_mem=83.16 GB):  95%|█████████▍| 55/58 [00:09<00:00, 21.15it/s] Capturing num tokens (num_tokens=4 avail_mem=83.15 GB):  95%|█████████▍| 55/58 [00:09<00:00, 21.15it/s]

    Capturing num tokens (num_tokens=4 avail_mem=83.15 GB): 100%|██████████| 58/58 [00:09<00:00, 21.83it/s]Capturing num tokens (num_tokens=4 avail_mem=83.15 GB): 100%|██████████| 58/58 [00:09<00:00,  6.15it/s]


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



<strong style='color: #00008B;'>Prompt: <|begin_of_text|><|start_header_id|>system<|end_header_id|><br><br>Cutting Knowledge Date: December 2023<br>Today Date: 26 Jul 2024<br><br><|eot_id|><|start_header_id|>user<|end_header_id|><br><br>Paris is the capital of<|eot_id|><|start_header_id|>assistant<|end_header_id|><br><br><br>Generated text: Paris is the capital of France.</strong>



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
