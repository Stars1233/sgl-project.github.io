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


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:172: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-04-02 09:20:26.045 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:20:26] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:20:26.046 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:20:26] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:20:26.046 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:20:26] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:20:26.046 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:20:26] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:20:26.046 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:20:26] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/4 [00:00<?, ?it/s]Multi-thread loading shards:  25% Completed | 1/4 [00:00<00:00,  6.54it/s]

    Multi-thread loading shards:  50% Completed | 2/4 [00:00<00:00,  2.07it/s]

    Multi-thread loading shards:  75% Completed | 3/4 [00:01<00:00,  1.50it/s]

    Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.32it/s]Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.51it/s]


    2026-04-02 09:20:34,128 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 09:20:34] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:04<04:40,  4.91s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:04<04:40,  4.91s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:06<02:37,  2.81s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:06<02:37,  2.81s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:06<01:37,  1.77s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:06<01:37,  1.77s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:07<01:06,  1.24s/it]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:07<01:06,  1.24s/it]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:07<00:49,  1.06it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:07<00:49,  1.06it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:07<00:38,  1.36it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:07<00:38,  1.36it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:08<00:30,  1.67it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:08<00:30,  1.67it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:08<00:24,  2.00it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:08<00:24,  2.00it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:08<00:20,  2.39it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:08<00:20,  2.39it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:09<00:17,  2.79it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:09<00:17,  2.79it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:09<00:14,  3.19it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:09<00:14,  3.19it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:09<00:12,  3.64it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:09<00:12,  3.64it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:09<00:10,  4.09it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:09<00:10,  4.09it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:09<00:09,  4.60it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:09<00:09,  4.60it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:09<00:08,  5.12it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:09<00:08,  5.12it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:10<00:07,  5.94it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:10<00:07,  5.94it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:10<00:06,  6.60it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:10<00:06,  6.60it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:10<00:06,  6.60it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:10<00:04,  8.34it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:10<00:04,  8.34it/s]

    Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:10<00:04,  8.34it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:10<00:03, 10.31it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:10<00:03, 10.31it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:10<00:03, 10.31it/s]Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:10<00:03, 10.31it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:10<00:02, 13.92it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:10<00:02, 13.92it/s]

    Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:10<00:02, 13.92it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:10<00:02, 13.92it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:10<00:01, 17.28it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:10<00:01, 17.28it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:10<00:01, 17.28it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:10<00:01, 17.28it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:10<00:01, 17.28it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:10<00:01, 21.63it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:10<00:01, 21.63it/s]

    Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:10<00:01, 21.63it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:10<00:01, 21.63it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:10<00:01, 21.63it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:10<00:00, 25.07it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:10<00:00, 25.07it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:10<00:00, 25.07it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:10<00:00, 25.07it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:10<00:00, 25.07it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:10<00:00, 28.86it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:10<00:00, 28.86it/s]

    Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:11<00:00, 28.86it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:11<00:00, 28.86it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:11<00:00, 28.86it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:11<00:00, 31.16it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:11<00:00, 31.16it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:11<00:00, 31.16it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:11<00:00, 31.16it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:11<00:00, 31.16it/s] Compiling num tokens (num_tokens=80):  74%|███████▍  | 43/58 [00:11<00:00, 31.16it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:11<00:00, 35.21it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:11<00:00, 35.21it/s]

    Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:11<00:00, 35.21it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:11<00:00, 35.21it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:11<00:00, 35.21it/s]Compiling num tokens (num_tokens=24):  83%|████████▎ | 48/58 [00:11<00:00, 35.21it/s]Compiling num tokens (num_tokens=20):  83%|████████▎ | 48/58 [00:11<00:00, 35.21it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:11<00:00, 41.88it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:11<00:00, 41.88it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:11<00:00, 41.88it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:11<00:00, 41.88it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:11<00:00, 41.88it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:11<00:00,  5.11it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=87.75 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=87.75 GB):   2%|▏         | 1/58 [00:00<00:38,  1.49it/s]Capturing num tokens (num_tokens=7680 avail_mem=88.11 GB):   2%|▏         | 1/58 [00:00<00:38,  1.49it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=88.11 GB):   3%|▎         | 2/58 [00:01<00:34,  1.61it/s]Capturing num tokens (num_tokens=7168 avail_mem=88.89 GB):   3%|▎         | 2/58 [00:01<00:34,  1.61it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=88.89 GB):   5%|▌         | 3/58 [00:01<00:32,  1.71it/s]Capturing num tokens (num_tokens=6656 avail_mem=88.89 GB):   5%|▌         | 3/58 [00:01<00:32,  1.71it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=88.89 GB):   7%|▋         | 4/58 [00:02<00:30,  1.77it/s]Capturing num tokens (num_tokens=6144 avail_mem=88.18 GB):   7%|▋         | 4/58 [00:02<00:30,  1.77it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=88.18 GB):   9%|▊         | 5/58 [00:02<00:27,  1.92it/s]Capturing num tokens (num_tokens=5632 avail_mem=88.25 GB):   9%|▊         | 5/58 [00:02<00:27,  1.92it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=88.25 GB):  10%|█         | 6/58 [00:03<00:24,  2.09it/s]Capturing num tokens (num_tokens=5120 avail_mem=88.92 GB):  10%|█         | 6/58 [00:03<00:24,  2.09it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=88.92 GB):  12%|█▏        | 7/58 [00:03<00:24,  2.11it/s]Capturing num tokens (num_tokens=4608 avail_mem=88.93 GB):  12%|█▏        | 7/58 [00:03<00:24,  2.11it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=88.93 GB):  14%|█▍        | 8/58 [00:04<00:23,  2.17it/s]Capturing num tokens (num_tokens=4096 avail_mem=88.46 GB):  14%|█▍        | 8/58 [00:04<00:23,  2.17it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=88.46 GB):  16%|█▌        | 9/58 [00:04<00:20,  2.34it/s]Capturing num tokens (num_tokens=3840 avail_mem=88.50 GB):  16%|█▌        | 9/58 [00:04<00:20,  2.34it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=88.50 GB):  17%|█▋        | 10/58 [00:04<00:19,  2.48it/s]Capturing num tokens (num_tokens=3584 avail_mem=97.95 GB):  17%|█▋        | 10/58 [00:04<00:19,  2.48it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=97.95 GB):  19%|█▉        | 11/58 [00:04<00:16,  2.87it/s]Capturing num tokens (num_tokens=3328 avail_mem=98.26 GB):  19%|█▉        | 11/58 [00:04<00:16,  2.87it/s]Capturing num tokens (num_tokens=3328 avail_mem=98.26 GB):  21%|██        | 12/58 [00:05<00:13,  3.31it/s]Capturing num tokens (num_tokens=3072 avail_mem=98.26 GB):  21%|██        | 12/58 [00:05<00:13,  3.31it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=98.26 GB):  22%|██▏       | 13/58 [00:05<00:12,  3.68it/s]Capturing num tokens (num_tokens=2816 avail_mem=97.99 GB):  22%|██▏       | 13/58 [00:05<00:12,  3.68it/s]Capturing num tokens (num_tokens=2816 avail_mem=97.99 GB):  24%|██▍       | 14/58 [00:05<00:10,  4.13it/s]Capturing num tokens (num_tokens=2560 avail_mem=98.26 GB):  24%|██▍       | 14/58 [00:05<00:10,  4.13it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=98.26 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.77it/s]Capturing num tokens (num_tokens=2304 avail_mem=98.25 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.77it/s]Capturing num tokens (num_tokens=2304 avail_mem=98.25 GB):  28%|██▊       | 16/58 [00:05<00:08,  5.24it/s]Capturing num tokens (num_tokens=2048 avail_mem=98.14 GB):  28%|██▊       | 16/58 [00:05<00:08,  5.24it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=98.14 GB):  29%|██▉       | 17/58 [00:05<00:06,  6.08it/s]Capturing num tokens (num_tokens=1792 avail_mem=98.24 GB):  29%|██▉       | 17/58 [00:05<00:06,  6.08it/s]Capturing num tokens (num_tokens=1536 avail_mem=98.24 GB):  29%|██▉       | 17/58 [00:06<00:06,  6.08it/s]Capturing num tokens (num_tokens=1536 avail_mem=98.24 GB):  33%|███▎      | 19/58 [00:06<00:05,  7.64it/s]Capturing num tokens (num_tokens=1280 avail_mem=98.23 GB):  33%|███▎      | 19/58 [00:06<00:05,  7.64it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=98.14 GB):  33%|███▎      | 19/58 [00:06<00:05,  7.64it/s]Capturing num tokens (num_tokens=1024 avail_mem=98.14 GB):  36%|███▌      | 21/58 [00:06<00:04,  9.07it/s]Capturing num tokens (num_tokens=960 avail_mem=98.24 GB):  36%|███▌      | 21/58 [00:06<00:04,  9.07it/s] Capturing num tokens (num_tokens=896 avail_mem=98.22 GB):  36%|███▌      | 21/58 [00:06<00:04,  9.07it/s]

    Capturing num tokens (num_tokens=896 avail_mem=98.22 GB):  40%|███▉      | 23/58 [00:06<00:03, 10.75it/s]Capturing num tokens (num_tokens=832 avail_mem=98.21 GB):  40%|███▉      | 23/58 [00:06<00:03, 10.75it/s]Capturing num tokens (num_tokens=768 avail_mem=98.20 GB):  40%|███▉      | 23/58 [00:06<00:03, 10.75it/s]Capturing num tokens (num_tokens=768 avail_mem=98.20 GB):  43%|████▎     | 25/58 [00:06<00:02, 12.29it/s]Capturing num tokens (num_tokens=704 avail_mem=98.19 GB):  43%|████▎     | 25/58 [00:06<00:02, 12.29it/s]Capturing num tokens (num_tokens=640 avail_mem=98.18 GB):  43%|████▎     | 25/58 [00:06<00:02, 12.29it/s]

    Capturing num tokens (num_tokens=640 avail_mem=98.18 GB):  47%|████▋     | 27/58 [00:06<00:02, 13.76it/s]Capturing num tokens (num_tokens=576 avail_mem=98.17 GB):  47%|████▋     | 27/58 [00:06<00:02, 13.76it/s]Capturing num tokens (num_tokens=512 avail_mem=98.14 GB):  47%|████▋     | 27/58 [00:06<00:02, 13.76it/s]Capturing num tokens (num_tokens=480 avail_mem=98.14 GB):  47%|████▋     | 27/58 [00:06<00:02, 13.76it/s]Capturing num tokens (num_tokens=480 avail_mem=98.14 GB):  52%|█████▏    | 30/58 [00:06<00:01, 15.88it/s]Capturing num tokens (num_tokens=448 avail_mem=98.13 GB):  52%|█████▏    | 30/58 [00:06<00:01, 15.88it/s]

    Capturing num tokens (num_tokens=416 avail_mem=98.12 GB):  52%|█████▏    | 30/58 [00:06<00:01, 15.88it/s]Capturing num tokens (num_tokens=384 avail_mem=98.13 GB):  52%|█████▏    | 30/58 [00:06<00:01, 15.88it/s]Capturing num tokens (num_tokens=384 avail_mem=98.13 GB):  57%|█████▋    | 33/58 [00:06<00:01, 17.51it/s]Capturing num tokens (num_tokens=352 avail_mem=98.12 GB):  57%|█████▋    | 33/58 [00:06<00:01, 17.51it/s]Capturing num tokens (num_tokens=320 avail_mem=98.12 GB):  57%|█████▋    | 33/58 [00:06<00:01, 17.51it/s]Capturing num tokens (num_tokens=288 avail_mem=98.11 GB):  57%|█████▋    | 33/58 [00:07<00:01, 17.51it/s]

    Capturing num tokens (num_tokens=288 avail_mem=98.11 GB):  62%|██████▏   | 36/58 [00:07<00:01, 19.44it/s]Capturing num tokens (num_tokens=256 avail_mem=98.10 GB):  62%|██████▏   | 36/58 [00:07<00:01, 19.44it/s]Capturing num tokens (num_tokens=240 avail_mem=102.58 GB):  62%|██████▏   | 36/58 [00:07<00:01, 19.44it/s]Capturing num tokens (num_tokens=240 avail_mem=102.58 GB):  66%|██████▌   | 38/58 [00:07<00:01, 16.44it/s]Capturing num tokens (num_tokens=224 avail_mem=102.59 GB):  66%|██████▌   | 38/58 [00:07<00:01, 16.44it/s]

    Capturing num tokens (num_tokens=208 avail_mem=102.58 GB):  66%|██████▌   | 38/58 [00:07<00:01, 16.44it/s]Capturing num tokens (num_tokens=208 avail_mem=102.58 GB):  69%|██████▉   | 40/58 [00:07<00:01, 16.66it/s]Capturing num tokens (num_tokens=192 avail_mem=102.57 GB):  69%|██████▉   | 40/58 [00:07<00:01, 16.66it/s]

    Capturing num tokens (num_tokens=176 avail_mem=102.56 GB):  69%|██████▉   | 40/58 [00:07<00:01, 16.66it/s]Capturing num tokens (num_tokens=176 avail_mem=102.56 GB):  72%|███████▏  | 42/58 [00:07<00:01, 13.23it/s]Capturing num tokens (num_tokens=160 avail_mem=102.53 GB):  72%|███████▏  | 42/58 [00:07<00:01, 13.23it/s]Capturing num tokens (num_tokens=144 avail_mem=102.53 GB):  72%|███████▏  | 42/58 [00:07<00:01, 13.23it/s]Capturing num tokens (num_tokens=128 avail_mem=102.52 GB):  72%|███████▏  | 42/58 [00:07<00:01, 13.23it/s]Capturing num tokens (num_tokens=128 avail_mem=102.52 GB):  78%|███████▊  | 45/58 [00:07<00:00, 16.41it/s]Capturing num tokens (num_tokens=112 avail_mem=102.55 GB):  78%|███████▊  | 45/58 [00:07<00:00, 16.41it/s]Capturing num tokens (num_tokens=96 avail_mem=102.54 GB):  78%|███████▊  | 45/58 [00:07<00:00, 16.41it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=102.53 GB):  78%|███████▊  | 45/58 [00:07<00:00, 16.41it/s]Capturing num tokens (num_tokens=80 avail_mem=102.53 GB):  83%|████████▎ | 48/58 [00:07<00:00, 18.78it/s]Capturing num tokens (num_tokens=64 avail_mem=102.52 GB):  83%|████████▎ | 48/58 [00:07<00:00, 18.78it/s]Capturing num tokens (num_tokens=48 avail_mem=102.51 GB):  83%|████████▎ | 48/58 [00:07<00:00, 18.78it/s]Capturing num tokens (num_tokens=32 avail_mem=102.51 GB):  83%|████████▎ | 48/58 [00:07<00:00, 18.78it/s]Capturing num tokens (num_tokens=32 avail_mem=102.51 GB):  88%|████████▊ | 51/58 [00:07<00:00, 21.16it/s]Capturing num tokens (num_tokens=28 avail_mem=102.49 GB):  88%|████████▊ | 51/58 [00:07<00:00, 21.16it/s]Capturing num tokens (num_tokens=24 avail_mem=102.49 GB):  88%|████████▊ | 51/58 [00:07<00:00, 21.16it/s]

    Capturing num tokens (num_tokens=20 avail_mem=102.48 GB):  88%|████████▊ | 51/58 [00:07<00:00, 21.16it/s]Capturing num tokens (num_tokens=16 avail_mem=102.48 GB):  88%|████████▊ | 51/58 [00:08<00:00, 21.16it/s]Capturing num tokens (num_tokens=16 avail_mem=102.48 GB):  95%|█████████▍| 55/58 [00:08<00:00, 24.71it/s]Capturing num tokens (num_tokens=12 avail_mem=102.48 GB):  95%|█████████▍| 55/58 [00:08<00:00, 24.71it/s]Capturing num tokens (num_tokens=8 avail_mem=102.47 GB):  95%|█████████▍| 55/58 [00:08<00:00, 24.71it/s] Capturing num tokens (num_tokens=4 avail_mem=102.47 GB):  95%|█████████▍| 55/58 [00:08<00:00, 24.71it/s]Capturing num tokens (num_tokens=4 avail_mem=102.47 GB): 100%|██████████| 58/58 [00:08<00:00,  7.14it/s]


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


<strong style='color: #00008B;'><function=get_current_date>{"timezone": "America/New_York"}</function><br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "fahrenheit"}</function></strong>



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


<strong style='color: #00008B;'><function=get_current_date>{"timezone": "America/New_York"}</function><br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "fahrenheit"}</function><br><br>Please note that the current date and time will be displayed in the New York timezone, and the current weather will be displayed in Fahrenheit.</strong>


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


<strong style='color: #00008B;'>{'text': '{"name": "Paris", "population": 2147000}', 'output_ids': [5018, 609, 794, 330, 60704, 498, 330, 45541, 794, 220, 11584, 7007, 15, 92, 128009], 'meta_info': {'id': '5a6d4d47c7d8440db8f409fa7eefbd46', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 50, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 15, 'cached_tokens': 1, 'cached_tokens_details': {'device': 1, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.17596728214994073, 'response_sent_to_client_ts': 1775121668.2483354}}</strong>



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


<strong style='color: #00008B;'>{'text': '{"name": "Paris", "population": 2147000}', 'output_ids': [5018, 609, 794, 330, 60704, 498, 330, 45541, 794, 220, 11584, 7007, 15, 92, 128009], 'meta_info': {'id': '1439e33325fb4dfeb6ed12167a5d1671', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 50, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 15, 'cached_tokens': 49, 'cached_tokens_details': {'device': 49, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.15993860829621553, 'response_sent_to_client_ts': 1775121668.4188082}}</strong>


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


<strong style='color: #00008B;'>[{'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': 'e363ce5f5f0a4656bc798f58b70885e2', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': {'device': 45, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.09491024306043983, 'response_sent_to_client_ts': 1775121668.5501482}}, {'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': '0226877d3e554208b617a48ab7acfe86', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': {'device': 45, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.09482698002830148, 'response_sent_to_client_ts': 1775121668.55016}}, {'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': 'cc9973beaf5a41ac92a6b83765fe2107', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': {'device': 45, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.09477976895868778, 'response_sent_to_client_ts': 1775121668.5501645}}]</strong>


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


<strong style='color: #00008B;'>{'text': 'France', 'output_ids': [50100, 128009], 'meta_info': {'id': '69e3f52e856f434bb0838dbd6e03b19c', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2, 'cached_tokens': 31, 'cached_tokens_details': {'device': 31, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.0368576580658555, 'response_sent_to_client_ts': 1775121668.595543}}</strong>


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


<strong style='color: #00008B;'>{'text': 'France.', 'output_ids': [50100, 13, 128009], 'meta_info': {'id': '94f2c59695ba4882bf25d23310993c37', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 3, 'cached_tokens': 40, 'cached_tokens_details': {'device': 40, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.12163611361756921, 'response_sent_to_client_ts': 1775121670.146849}}</strong>



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


<strong style='color: #00008B;'>{'text': 'France.', 'output_ids': [50100, 13, 128009], 'meta_info': {'id': 'aa6bfb55f9794cf0a7aef76ee63aca91', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 3, 'cached_tokens': 40, 'cached_tokens_details': {'device': 40, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.04644093010574579, 'response_sent_to_client_ts': 1775121670.2056878}}</strong>



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


    2026-04-02 09:21:24.321 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:21:24] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:21:24.321 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:21:24] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:21:24.321 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:21:24] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:21:24.321 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:21:24] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:21:24.321 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:21:24] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/4 [00:00<?, ?it/s]Multi-thread loading shards:  25% Completed | 1/4 [00:00<00:00,  6.47it/s]

    Multi-thread loading shards:  50% Completed | 2/4 [00:00<00:00,  2.10it/s]

    Multi-thread loading shards:  75% Completed | 3/4 [00:01<00:00,  1.50it/s]

    Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.30it/s]Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.50it/s]


    2026-04-02 09:21:33,346 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 09:21:33] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:17,  3.47s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:17,  3.47s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:55,  2.06s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:55,  2.06s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:05<01:14,  1.36s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:05<01:14,  1.36s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:05<00:56,  1.05s/it]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:05<00:56,  1.05s/it]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:06<00:46,  1.15it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:06<00:46,  1.15it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:06<00:38,  1.36it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:06<00:38,  1.36it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:07<00:32,  1.57it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:07<00:32,  1.57it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:07<00:28,  1.78it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:07<00:28,  1.78it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:07<00:23,  2.06it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:07<00:23,  2.06it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:08<00:20,  2.30it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:08<00:20,  2.30it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:08<00:17,  2.62it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:08<00:17,  2.62it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:08<00:15,  2.92it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:08<00:15,  2.92it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:08<00:13,  3.39it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:08<00:13,  3.39it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:09<00:12,  3.60it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:09<00:12,  3.60it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:09<00:10,  4.12it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:09<00:10,  4.12it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:09<00:09,  4.65it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:09<00:09,  4.65it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:09<00:08,  4.96it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:09<00:08,  4.96it/s]

    Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:09<00:08,  4.96it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:09<00:06,  6.46it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:09<00:06,  6.46it/s]

    Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:09<00:06,  6.46it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:09<00:04,  8.08it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:09<00:04,  8.08it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:09<00:04,  8.08it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:10<00:03,  9.90it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:10<00:03,  9.90it/s]

    Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:10<00:03,  9.90it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:10<00:03, 11.00it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:10<00:03, 11.00it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:10<00:03, 11.00it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:10<00:03, 11.00it/s]

    Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:10<00:02, 14.21it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:10<00:02, 14.21it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:10<00:02, 14.21it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:10<00:02, 14.21it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:10<00:01, 15.97it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:10<00:01, 15.97it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:10<00:01, 15.97it/s]

    Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:10<00:01, 15.97it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:10<00:01, 17.85it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:10<00:01, 17.85it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:10<00:01, 17.85it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:10<00:01, 17.85it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:10<00:01, 19.39it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:10<00:01, 19.39it/s]

    Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:10<00:01, 19.39it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:10<00:01, 19.39it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:10<00:00, 20.68it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:10<00:00, 20.68it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:10<00:00, 20.68it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:10<00:00, 20.68it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:10<00:00, 20.68it/s]

    Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:10<00:00, 23.76it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:10<00:00, 23.76it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:11<00:00, 23.76it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:11<00:00, 23.76it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:11<00:00, 23.76it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:11<00:00, 25.01it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:11<00:00, 25.01it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:11<00:00, 25.01it/s]

    Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:11<00:00, 25.01it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:11<00:00, 25.01it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:11<00:00, 28.39it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:11<00:00, 28.39it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:11<00:00, 28.39it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:11<00:00, 28.39it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:11<00:00, 28.39it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:11<00:00, 30.33it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:11<00:00, 30.33it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:11<00:00, 30.33it/s]

    Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:11<00:00,  5.10it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=84.96 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=84.96 GB):   2%|▏         | 1/58 [00:00<00:31,  1.81it/s]Capturing num tokens (num_tokens=7680 avail_mem=85.05 GB):   2%|▏         | 1/58 [00:00<00:31,  1.81it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=85.05 GB):   3%|▎         | 2/58 [00:01<00:28,  1.95it/s]Capturing num tokens (num_tokens=7168 avail_mem=84.97 GB):   3%|▎         | 2/58 [00:01<00:28,  1.95it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=84.97 GB):   5%|▌         | 3/58 [00:01<00:25,  2.17it/s]Capturing num tokens (num_tokens=6656 avail_mem=85.00 GB):   5%|▌         | 3/58 [00:01<00:25,  2.17it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=85.00 GB):   7%|▋         | 4/58 [00:01<00:23,  2.33it/s]Capturing num tokens (num_tokens=6144 avail_mem=84.97 GB):   7%|▋         | 4/58 [00:01<00:23,  2.33it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=84.97 GB):   9%|▊         | 5/58 [00:02<00:20,  2.57it/s]Capturing num tokens (num_tokens=5632 avail_mem=84.94 GB):   9%|▊         | 5/58 [00:02<00:20,  2.57it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=84.94 GB):  10%|█         | 6/58 [00:02<00:17,  2.90it/s]Capturing num tokens (num_tokens=5120 avail_mem=84.94 GB):  10%|█         | 6/58 [00:02<00:17,  2.90it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=84.94 GB):  12%|█▏        | 7/58 [00:02<00:15,  3.24it/s]Capturing num tokens (num_tokens=4608 avail_mem=84.95 GB):  12%|█▏        | 7/58 [00:02<00:15,  3.24it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=84.95 GB):  14%|█▍        | 8/58 [00:02<00:13,  3.61it/s]Capturing num tokens (num_tokens=4096 avail_mem=84.95 GB):  14%|█▍        | 8/58 [00:02<00:13,  3.61it/s]Capturing num tokens (num_tokens=4096 avail_mem=84.95 GB):  16%|█▌        | 9/58 [00:03<00:12,  4.02it/s]Capturing num tokens (num_tokens=3840 avail_mem=84.94 GB):  16%|█▌        | 9/58 [00:03<00:12,  4.02it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=84.94 GB):  17%|█▋        | 10/58 [00:03<00:10,  4.37it/s]Capturing num tokens (num_tokens=3584 avail_mem=84.94 GB):  17%|█▋        | 10/58 [00:03<00:10,  4.37it/s]Capturing num tokens (num_tokens=3584 avail_mem=84.94 GB):  19%|█▉        | 11/58 [00:03<00:09,  4.78it/s]Capturing num tokens (num_tokens=3328 avail_mem=84.92 GB):  19%|█▉        | 11/58 [00:03<00:09,  4.78it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=84.92 GB):  21%|██        | 12/58 [00:03<00:08,  5.46it/s]Capturing num tokens (num_tokens=3072 avail_mem=84.92 GB):  21%|██        | 12/58 [00:03<00:08,  5.46it/s]Capturing num tokens (num_tokens=3072 avail_mem=84.92 GB):  22%|██▏       | 13/58 [00:03<00:07,  6.18it/s]Capturing num tokens (num_tokens=2816 avail_mem=84.92 GB):  22%|██▏       | 13/58 [00:03<00:07,  6.18it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=84.92 GB):  24%|██▍       | 14/58 [00:03<00:06,  6.98it/s]Capturing num tokens (num_tokens=2560 avail_mem=84.92 GB):  24%|██▍       | 14/58 [00:03<00:06,  6.98it/s]Capturing num tokens (num_tokens=2304 avail_mem=84.92 GB):  24%|██▍       | 14/58 [00:03<00:06,  6.98it/s]Capturing num tokens (num_tokens=2304 avail_mem=84.92 GB):  28%|██▊       | 16/58 [00:03<00:04,  8.51it/s]Capturing num tokens (num_tokens=2048 avail_mem=84.92 GB):  28%|██▊       | 16/58 [00:03<00:04,  8.51it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=84.92 GB):  28%|██▊       | 16/58 [00:03<00:04,  8.51it/s]Capturing num tokens (num_tokens=1792 avail_mem=84.92 GB):  31%|███       | 18/58 [00:04<00:03, 10.32it/s]Capturing num tokens (num_tokens=1536 avail_mem=84.92 GB):  31%|███       | 18/58 [00:04<00:03, 10.32it/s]Capturing num tokens (num_tokens=1280 avail_mem=84.92 GB):  31%|███       | 18/58 [00:04<00:03, 10.32it/s]Capturing num tokens (num_tokens=1280 avail_mem=84.92 GB):  34%|███▍      | 20/58 [00:04<00:03, 12.38it/s]Capturing num tokens (num_tokens=1024 avail_mem=84.92 GB):  34%|███▍      | 20/58 [00:04<00:03, 12.38it/s]

    Capturing num tokens (num_tokens=960 avail_mem=84.91 GB):  34%|███▍      | 20/58 [00:04<00:03, 12.38it/s] Capturing num tokens (num_tokens=896 avail_mem=84.91 GB):  34%|███▍      | 20/58 [00:04<00:03, 12.38it/s]Capturing num tokens (num_tokens=896 avail_mem=84.91 GB):  40%|███▉      | 23/58 [00:04<00:02, 15.48it/s]Capturing num tokens (num_tokens=832 avail_mem=84.91 GB):  40%|███▉      | 23/58 [00:04<00:02, 15.48it/s]Capturing num tokens (num_tokens=768 avail_mem=84.90 GB):  40%|███▉      | 23/58 [00:04<00:02, 15.48it/s]Capturing num tokens (num_tokens=704 avail_mem=84.90 GB):  40%|███▉      | 23/58 [00:04<00:02, 15.48it/s]Capturing num tokens (num_tokens=704 avail_mem=84.90 GB):  45%|████▍     | 26/58 [00:04<00:01, 18.26it/s]

    Capturing num tokens (num_tokens=640 avail_mem=84.89 GB):  45%|████▍     | 26/58 [00:04<00:01, 18.26it/s]Capturing num tokens (num_tokens=576 avail_mem=84.89 GB):  45%|████▍     | 26/58 [00:04<00:01, 18.26it/s]Capturing num tokens (num_tokens=512 avail_mem=84.88 GB):  45%|████▍     | 26/58 [00:04<00:01, 18.26it/s]Capturing num tokens (num_tokens=512 avail_mem=84.88 GB):  50%|█████     | 29/58 [00:04<00:01, 21.06it/s]Capturing num tokens (num_tokens=480 avail_mem=84.88 GB):  50%|█████     | 29/58 [00:04<00:01, 21.06it/s]Capturing num tokens (num_tokens=448 avail_mem=84.87 GB):  50%|█████     | 29/58 [00:04<00:01, 21.06it/s]Capturing num tokens (num_tokens=416 avail_mem=84.87 GB):  50%|█████     | 29/58 [00:04<00:01, 21.06it/s]

    Capturing num tokens (num_tokens=384 avail_mem=84.87 GB):  50%|█████     | 29/58 [00:04<00:01, 21.06it/s]Capturing num tokens (num_tokens=384 avail_mem=84.87 GB):  57%|█████▋    | 33/58 [00:04<00:01, 24.14it/s]Capturing num tokens (num_tokens=352 avail_mem=84.86 GB):  57%|█████▋    | 33/58 [00:04<00:01, 24.14it/s]Capturing num tokens (num_tokens=320 avail_mem=84.86 GB):  57%|█████▋    | 33/58 [00:04<00:01, 24.14it/s]Capturing num tokens (num_tokens=288 avail_mem=84.85 GB):  57%|█████▋    | 33/58 [00:04<00:01, 24.14it/s]Capturing num tokens (num_tokens=256 avail_mem=84.85 GB):  57%|█████▋    | 33/58 [00:04<00:01, 24.14it/s]Capturing num tokens (num_tokens=256 avail_mem=84.85 GB):  64%|██████▍   | 37/58 [00:04<00:00, 27.12it/s]Capturing num tokens (num_tokens=240 avail_mem=84.85 GB):  64%|██████▍   | 37/58 [00:04<00:00, 27.12it/s]Capturing num tokens (num_tokens=224 avail_mem=84.84 GB):  64%|██████▍   | 37/58 [00:04<00:00, 27.12it/s]

    Capturing num tokens (num_tokens=208 avail_mem=84.84 GB):  64%|██████▍   | 37/58 [00:04<00:00, 27.12it/s]Capturing num tokens (num_tokens=192 avail_mem=84.83 GB):  64%|██████▍   | 37/58 [00:04<00:00, 27.12it/s]Capturing num tokens (num_tokens=192 avail_mem=84.83 GB):  71%|███████   | 41/58 [00:04<00:00, 29.18it/s]Capturing num tokens (num_tokens=176 avail_mem=84.83 GB):  71%|███████   | 41/58 [00:04<00:00, 29.18it/s]Capturing num tokens (num_tokens=160 avail_mem=84.82 GB):  71%|███████   | 41/58 [00:04<00:00, 29.18it/s]Capturing num tokens (num_tokens=144 avail_mem=84.82 GB):  71%|███████   | 41/58 [00:04<00:00, 29.18it/s]Capturing num tokens (num_tokens=128 avail_mem=84.81 GB):  71%|███████   | 41/58 [00:04<00:00, 29.18it/s]Capturing num tokens (num_tokens=128 avail_mem=84.81 GB):  78%|███████▊  | 45/58 [00:04<00:00, 30.83it/s]Capturing num tokens (num_tokens=112 avail_mem=84.82 GB):  78%|███████▊  | 45/58 [00:04<00:00, 30.83it/s]

    Capturing num tokens (num_tokens=96 avail_mem=84.82 GB):  78%|███████▊  | 45/58 [00:04<00:00, 30.83it/s] Capturing num tokens (num_tokens=80 avail_mem=84.82 GB):  78%|███████▊  | 45/58 [00:05<00:00, 30.83it/s]Capturing num tokens (num_tokens=64 avail_mem=84.81 GB):  78%|███████▊  | 45/58 [00:05<00:00, 30.83it/s]Capturing num tokens (num_tokens=64 avail_mem=84.81 GB):  84%|████████▍ | 49/58 [00:05<00:00, 32.00it/s]Capturing num tokens (num_tokens=48 avail_mem=84.81 GB):  84%|████████▍ | 49/58 [00:05<00:00, 32.00it/s]Capturing num tokens (num_tokens=32 avail_mem=84.81 GB):  84%|████████▍ | 49/58 [00:05<00:00, 32.00it/s]Capturing num tokens (num_tokens=28 avail_mem=84.80 GB):  84%|████████▍ | 49/58 [00:05<00:00, 32.00it/s]Capturing num tokens (num_tokens=24 avail_mem=84.79 GB):  84%|████████▍ | 49/58 [00:05<00:00, 32.00it/s]

    Capturing num tokens (num_tokens=24 avail_mem=84.79 GB):  91%|█████████▏| 53/58 [00:05<00:00, 32.16it/s]Capturing num tokens (num_tokens=20 avail_mem=84.79 GB):  91%|█████████▏| 53/58 [00:05<00:00, 32.16it/s]Capturing num tokens (num_tokens=16 avail_mem=84.79 GB):  91%|█████████▏| 53/58 [00:05<00:00, 32.16it/s]Capturing num tokens (num_tokens=12 avail_mem=84.78 GB):  91%|█████████▏| 53/58 [00:05<00:00, 32.16it/s]Capturing num tokens (num_tokens=8 avail_mem=84.78 GB):  91%|█████████▏| 53/58 [00:05<00:00, 32.16it/s] Capturing num tokens (num_tokens=8 avail_mem=84.78 GB):  98%|█████████▊| 57/58 [00:05<00:00, 32.98it/s]Capturing num tokens (num_tokens=4 avail_mem=84.77 GB):  98%|█████████▊| 57/58 [00:05<00:00, 32.98it/s]Capturing num tokens (num_tokens=4 avail_mem=84.77 GB): 100%|██████████| 58/58 [00:05<00:00, 10.88it/s]


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



<strong style='color: #00008B;'>Prompt: Give me the information of the capital of Italy.<br>Generated text: Paris is the capital of Italy</strong>


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
