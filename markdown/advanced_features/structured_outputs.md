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


    Multi-thread loading shards:   0% Completed | 0/4 [00:00<?, ?it/s]Multi-thread loading shards:  25% Completed | 1/4 [00:00<00:00,  5.80it/s]

    Multi-thread loading shards:  50% Completed | 2/4 [00:00<00:01,  1.85it/s]

    Multi-thread loading shards:  75% Completed | 3/4 [00:01<00:00,  1.36it/s]

    Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.19it/s]Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.36it/s]


    2026-04-04 09:20:45,056 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 09:20:45] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:22,  3.54s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:22,  3.54s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:59,  2.14s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:59,  2.14s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:05<01:15,  1.37s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:05<01:15,  1.37s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:05<00:53,  1.01it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:05<00:53,  1.01it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:41,  1.28it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:41,  1.28it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:06<00:32,  1.60it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:06<00:32,  1.60it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:06<00:26,  1.95it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:06<00:26,  1.95it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:22,  2.26it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:22,  2.26it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:07<00:18,  2.64it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:07<00:18,  2.64it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:07<00:15,  3.14it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:07<00:15,  3.14it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:07<00:13,  3.57it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:07<00:13,  3.57it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:07<00:13,  3.57it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:08,  5.14it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:08,  5.14it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:08,  5.30it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:08,  5.30it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:07,  5.83it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:07,  5.83it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:08<00:06,  6.24it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:08<00:06,  6.24it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:08<00:05,  6.87it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:08<00:05,  6.87it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:08<00:05,  6.87it/s]

    Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:08<00:04,  8.12it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:08<00:04,  8.12it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:08<00:04,  8.12it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:08<00:04,  8.12it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:08<00:03, 11.35it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:08<00:03, 11.35it/s]

    Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:08<00:03, 11.35it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:08<00:03, 11.35it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:08<00:02, 14.37it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:08<00:02, 14.37it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:08<00:02, 14.37it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:08<00:02, 14.37it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:08<00:01, 17.28it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:08<00:01, 17.28it/s]

    Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:08<00:01, 17.28it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:08<00:01, 17.28it/s]Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:08<00:01, 17.28it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:08<00:01, 22.07it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:08<00:01, 22.07it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:08<00:01, 22.07it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:08<00:01, 22.07it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:08<00:01, 22.07it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:08<00:01, 22.07it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:09<00:00, 27.01it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:09<00:00, 27.01it/s]

    Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:09<00:00, 27.01it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:09<00:00, 27.01it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:09<00:00, 27.01it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:09<00:00, 27.99it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:09<00:00, 27.99it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:09<00:00, 27.99it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:09<00:00, 27.99it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:09<00:00, 27.99it/s]

    Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:09<00:00, 29.68it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:09<00:00, 29.68it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:09<00:00, 29.68it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:09<00:00, 29.68it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:09<00:00, 29.68it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:09<00:00, 31.44it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:09<00:00, 31.44it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:09<00:00, 31.44it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:09<00:00, 31.44it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:09<00:00, 31.44it/s]

    Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:09<00:00, 31.44it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:09<00:00, 35.49it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:09<00:00, 35.49it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:09<00:00, 35.49it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:09<00:00, 35.49it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:09<00:00, 35.49it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00,  6.05it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=89.15 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=89.15 GB):   2%|▏         | 1/58 [00:00<00:27,  2.10it/s]Capturing num tokens (num_tokens=7680 avail_mem=89.10 GB):   2%|▏         | 1/58 [00:00<00:27,  2.10it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=89.10 GB):   3%|▎         | 2/58 [00:00<00:24,  2.25it/s]Capturing num tokens (num_tokens=7168 avail_mem=89.09 GB):   3%|▎         | 2/58 [00:00<00:24,  2.25it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=89.09 GB):   5%|▌         | 3/58 [00:01<00:22,  2.46it/s]Capturing num tokens (num_tokens=6656 avail_mem=89.07 GB):   5%|▌         | 3/58 [00:01<00:22,  2.46it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=89.07 GB):   7%|▋         | 4/58 [00:01<00:20,  2.63it/s]Capturing num tokens (num_tokens=6144 avail_mem=89.08 GB):   7%|▋         | 4/58 [00:01<00:20,  2.63it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=89.08 GB):   9%|▊         | 5/58 [00:01<00:18,  2.88it/s]Capturing num tokens (num_tokens=5632 avail_mem=89.07 GB):   9%|▊         | 5/58 [00:01<00:18,  2.88it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=89.07 GB):  10%|█         | 6/58 [00:02<00:16,  3.15it/s]Capturing num tokens (num_tokens=5120 avail_mem=89.06 GB):  10%|█         | 6/58 [00:02<00:16,  3.15it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=89.06 GB):  12%|█▏        | 7/58 [00:02<00:14,  3.41it/s]Capturing num tokens (num_tokens=4608 avail_mem=89.05 GB):  12%|█▏        | 7/58 [00:02<00:14,  3.41it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=89.05 GB):  14%|█▍        | 8/58 [00:02<00:13,  3.79it/s]Capturing num tokens (num_tokens=4096 avail_mem=89.05 GB):  14%|█▍        | 8/58 [00:02<00:13,  3.79it/s]Capturing num tokens (num_tokens=4096 avail_mem=89.05 GB):  16%|█▌        | 9/58 [00:02<00:11,  4.36it/s]Capturing num tokens (num_tokens=3840 avail_mem=89.05 GB):  16%|█▌        | 9/58 [00:02<00:11,  4.36it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=89.05 GB):  17%|█▋        | 10/58 [00:02<00:09,  4.91it/s]Capturing num tokens (num_tokens=3584 avail_mem=89.05 GB):  17%|█▋        | 10/58 [00:02<00:09,  4.91it/s]Capturing num tokens (num_tokens=3584 avail_mem=89.05 GB):  19%|█▉        | 11/58 [00:03<00:08,  5.29it/s]Capturing num tokens (num_tokens=3328 avail_mem=89.05 GB):  19%|█▉        | 11/58 [00:03<00:08,  5.29it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=89.05 GB):  21%|██        | 12/58 [00:03<00:08,  5.31it/s]Capturing num tokens (num_tokens=3072 avail_mem=89.05 GB):  21%|██        | 12/58 [00:03<00:08,  5.31it/s]Capturing num tokens (num_tokens=3072 avail_mem=89.05 GB):  22%|██▏       | 13/58 [00:03<00:08,  5.46it/s]Capturing num tokens (num_tokens=2816 avail_mem=89.05 GB):  22%|██▏       | 13/58 [00:03<00:08,  5.46it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=89.05 GB):  24%|██▍       | 14/58 [00:03<00:07,  5.77it/s]Capturing num tokens (num_tokens=2560 avail_mem=89.05 GB):  24%|██▍       | 14/58 [00:03<00:07,  5.77it/s]Capturing num tokens (num_tokens=2560 avail_mem=89.05 GB):  26%|██▌       | 15/58 [00:03<00:06,  6.20it/s]Capturing num tokens (num_tokens=2304 avail_mem=89.05 GB):  26%|██▌       | 15/58 [00:03<00:06,  6.20it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=89.05 GB):  28%|██▊       | 16/58 [00:03<00:06,  6.61it/s]Capturing num tokens (num_tokens=2048 avail_mem=89.05 GB):  28%|██▊       | 16/58 [00:03<00:06,  6.61it/s]Capturing num tokens (num_tokens=2048 avail_mem=89.05 GB):  29%|██▉       | 17/58 [00:03<00:05,  7.33it/s]Capturing num tokens (num_tokens=1792 avail_mem=89.05 GB):  29%|██▉       | 17/58 [00:03<00:05,  7.33it/s]Capturing num tokens (num_tokens=1536 avail_mem=89.05 GB):  29%|██▉       | 17/58 [00:04<00:05,  7.33it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=89.05 GB):  33%|███▎      | 19/58 [00:04<00:04,  9.19it/s]Capturing num tokens (num_tokens=1280 avail_mem=89.05 GB):  33%|███▎      | 19/58 [00:04<00:04,  9.19it/s]Capturing num tokens (num_tokens=1024 avail_mem=89.05 GB):  33%|███▎      | 19/58 [00:04<00:04,  9.19it/s]Capturing num tokens (num_tokens=1024 avail_mem=89.05 GB):  36%|███▌      | 21/58 [00:04<00:03, 11.69it/s]Capturing num tokens (num_tokens=960 avail_mem=89.04 GB):  36%|███▌      | 21/58 [00:04<00:03, 11.69it/s] Capturing num tokens (num_tokens=896 avail_mem=89.04 GB):  36%|███▌      | 21/58 [00:04<00:03, 11.69it/s]

    Capturing num tokens (num_tokens=896 avail_mem=89.04 GB):  40%|███▉      | 23/58 [00:04<00:02, 13.74it/s]Capturing num tokens (num_tokens=832 avail_mem=89.04 GB):  40%|███▉      | 23/58 [00:04<00:02, 13.74it/s]Capturing num tokens (num_tokens=768 avail_mem=98.11 GB):  40%|███▉      | 23/58 [00:04<00:02, 13.74it/s]

    Capturing num tokens (num_tokens=768 avail_mem=98.11 GB):  43%|████▎     | 25/58 [00:04<00:02, 12.03it/s]Capturing num tokens (num_tokens=704 avail_mem=98.10 GB):  43%|████▎     | 25/58 [00:04<00:02, 12.03it/s]Capturing num tokens (num_tokens=640 avail_mem=98.10 GB):  43%|████▎     | 25/58 [00:04<00:02, 12.03it/s]Capturing num tokens (num_tokens=576 avail_mem=98.09 GB):  43%|████▎     | 25/58 [00:04<00:02, 12.03it/s]Capturing num tokens (num_tokens=576 avail_mem=98.09 GB):  48%|████▊     | 28/58 [00:04<00:01, 15.82it/s]Capturing num tokens (num_tokens=512 avail_mem=98.09 GB):  48%|████▊     | 28/58 [00:04<00:01, 15.82it/s]Capturing num tokens (num_tokens=480 avail_mem=98.08 GB):  48%|████▊     | 28/58 [00:04<00:01, 15.82it/s]

    Capturing num tokens (num_tokens=480 avail_mem=98.08 GB):  52%|█████▏    | 30/58 [00:04<00:01, 16.11it/s]Capturing num tokens (num_tokens=448 avail_mem=98.08 GB):  52%|█████▏    | 30/58 [00:04<00:01, 16.11it/s]Capturing num tokens (num_tokens=416 avail_mem=98.07 GB):  52%|█████▏    | 30/58 [00:04<00:01, 16.11it/s]Capturing num tokens (num_tokens=384 avail_mem=98.07 GB):  52%|█████▏    | 30/58 [00:04<00:01, 16.11it/s]Capturing num tokens (num_tokens=384 avail_mem=98.07 GB):  57%|█████▋    | 33/58 [00:04<00:01, 19.07it/s]Capturing num tokens (num_tokens=352 avail_mem=98.07 GB):  57%|█████▋    | 33/58 [00:04<00:01, 19.07it/s]Capturing num tokens (num_tokens=320 avail_mem=98.06 GB):  57%|█████▋    | 33/58 [00:04<00:01, 19.07it/s]Capturing num tokens (num_tokens=288 avail_mem=98.06 GB):  57%|█████▋    | 33/58 [00:04<00:01, 19.07it/s]Capturing num tokens (num_tokens=256 avail_mem=98.05 GB):  57%|█████▋    | 33/58 [00:04<00:01, 19.07it/s]

    Capturing num tokens (num_tokens=256 avail_mem=98.05 GB):  64%|██████▍   | 37/58 [00:04<00:00, 23.47it/s]Capturing num tokens (num_tokens=240 avail_mem=98.05 GB):  64%|██████▍   | 37/58 [00:04<00:00, 23.47it/s]Capturing num tokens (num_tokens=224 avail_mem=98.05 GB):  64%|██████▍   | 37/58 [00:04<00:00, 23.47it/s]Capturing num tokens (num_tokens=208 avail_mem=98.04 GB):  64%|██████▍   | 37/58 [00:04<00:00, 23.47it/s]Capturing num tokens (num_tokens=192 avail_mem=98.04 GB):  64%|██████▍   | 37/58 [00:05<00:00, 23.47it/s]Capturing num tokens (num_tokens=192 avail_mem=98.04 GB):  71%|███████   | 41/58 [00:05<00:00, 26.65it/s]Capturing num tokens (num_tokens=176 avail_mem=98.03 GB):  71%|███████   | 41/58 [00:05<00:00, 26.65it/s]Capturing num tokens (num_tokens=160 avail_mem=98.03 GB):  71%|███████   | 41/58 [00:05<00:00, 26.65it/s]Capturing num tokens (num_tokens=144 avail_mem=98.02 GB):  71%|███████   | 41/58 [00:05<00:00, 26.65it/s]

    Capturing num tokens (num_tokens=128 avail_mem=98.02 GB):  71%|███████   | 41/58 [00:05<00:00, 26.65it/s]Capturing num tokens (num_tokens=128 avail_mem=98.02 GB):  78%|███████▊  | 45/58 [00:05<00:00, 29.11it/s]Capturing num tokens (num_tokens=112 avail_mem=98.03 GB):  78%|███████▊  | 45/58 [00:05<00:00, 29.11it/s]Capturing num tokens (num_tokens=96 avail_mem=98.03 GB):  78%|███████▊  | 45/58 [00:05<00:00, 29.11it/s] Capturing num tokens (num_tokens=80 avail_mem=98.02 GB):  78%|███████▊  | 45/58 [00:05<00:00, 29.11it/s]Capturing num tokens (num_tokens=64 avail_mem=98.02 GB):  78%|███████▊  | 45/58 [00:05<00:00, 29.11it/s]Capturing num tokens (num_tokens=64 avail_mem=98.02 GB):  84%|████████▍ | 49/58 [00:05<00:00, 30.70it/s]Capturing num tokens (num_tokens=48 avail_mem=98.01 GB):  84%|████████▍ | 49/58 [00:05<00:00, 30.70it/s]Capturing num tokens (num_tokens=32 avail_mem=98.01 GB):  84%|████████▍ | 49/58 [00:05<00:00, 30.70it/s]

    Capturing num tokens (num_tokens=28 avail_mem=98.00 GB):  84%|████████▍ | 49/58 [00:05<00:00, 30.70it/s]Capturing num tokens (num_tokens=24 avail_mem=98.00 GB):  84%|████████▍ | 49/58 [00:05<00:00, 30.70it/s]Capturing num tokens (num_tokens=24 avail_mem=98.00 GB):  91%|█████████▏| 53/58 [00:05<00:00, 31.78it/s]Capturing num tokens (num_tokens=20 avail_mem=97.99 GB):  91%|█████████▏| 53/58 [00:05<00:00, 31.78it/s]Capturing num tokens (num_tokens=16 avail_mem=97.99 GB):  91%|█████████▏| 53/58 [00:05<00:00, 31.78it/s]Capturing num tokens (num_tokens=12 avail_mem=97.99 GB):  91%|█████████▏| 53/58 [00:05<00:00, 31.78it/s]Capturing num tokens (num_tokens=8 avail_mem=97.98 GB):  91%|█████████▏| 53/58 [00:05<00:00, 31.78it/s] Capturing num tokens (num_tokens=8 avail_mem=97.98 GB):  98%|█████████▊| 57/58 [00:05<00:00, 32.86it/s]Capturing num tokens (num_tokens=4 avail_mem=97.98 GB):  98%|█████████▊| 57/58 [00:05<00:00, 32.86it/s]Capturing num tokens (num_tokens=4 avail_mem=97.98 GB): 100%|██████████| 58/58 [00:05<00:00, 10.48it/s]


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


<strong style='color: #00008B;'><function=get_current_date>{"timezone": "America/New_York"}</function><br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "fahrenheit"}</function><br><br>Note: I used New York City and New York State for the weather function call.</strong>



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


<strong style='color: #00008B;'><function=get_current_date>{"timezone": "America/New_York"}</function><br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "fahrenheit"}</function><br><br>Note: The weather and date may be different when you execute the code as it's a real-time function.</strong>


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


<strong style='color: #00008B;'>{'text': '{"name": "Paris", "population": 2147000}', 'output_ids': [5018, 609, 794, 330, 60704, 498, 330, 45541, 794, 220, 11584, 7007, 15, 92, 128009], 'meta_info': {'id': 'f2968c969c114433b1c489127df5bdbb', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 50, 'weight_version': 'default', 'total_retractions': 0, 'reasoning_tokens': 0, 'completion_tokens': 15, 'cached_tokens': 1, 'cached_tokens_details': {'device': 1, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.15580809209495783, 'response_sent_to_client_ts': 1775294475.180089}}</strong>



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


<strong style='color: #00008B;'>{'text': '{"name": "Paris", "population": 2147000}', 'output_ids': [5018, 609, 794, 330, 60704, 498, 330, 45541, 794, 220, 11584, 7007, 15, 92, 128009], 'meta_info': {'id': '9e4166d318264f5bb36ed44565eb2a53', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 50, 'weight_version': 'default', 'total_retractions': 0, 'reasoning_tokens': 0, 'completion_tokens': 15, 'cached_tokens': 49, 'cached_tokens_details': {'device': 49, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.15212754206731915, 'response_sent_to_client_ts': 1775294475.3413343}}</strong>


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


<strong style='color: #00008B;'>[{'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': 'a9ce38f7381a46cba23c74572b18fa01', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'reasoning_tokens': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': {'device': 45, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.08401619596406817, 'response_sent_to_client_ts': 1775294475.4480612}}, {'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': '0a0dd8039e224775940e89c8d9a28db4', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'reasoning_tokens': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': {'device': 45, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.0839550350792706, 'response_sent_to_client_ts': 1775294475.4480753}}, {'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': 'a5f184708ecb4d09be69ffc8aec34d2f', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'reasoning_tokens': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': {'device': 45, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.08391140028834343, 'response_sent_to_client_ts': 1775294475.44808}}]</strong>


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


<strong style='color: #00008B;'>{'text': 'France', 'output_ids': [50100, 128009], 'meta_info': {'id': 'cb5da2616e3149648e0bde9aa6bb8919', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'reasoning_tokens': 0, 'completion_tokens': 2, 'cached_tokens': 31, 'cached_tokens_details': {'device': 31, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.03193457471206784, 'response_sent_to_client_ts': 1775294475.488178}}</strong>


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


<strong style='color: #00008B;'>{'text': 'France.', 'output_ids': [50100, 13, 128009], 'meta_info': {'id': 'd3491b2b1328426ba4a87ccec5d61c89', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'reasoning_tokens': 0, 'completion_tokens': 3, 'cached_tokens': 40, 'cached_tokens_details': {'device': 40, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.12099724169820547, 'response_sent_to_client_ts': 1775294476.9863505}}</strong>



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


<strong style='color: #00008B;'>{'text': 'France.', 'output_ids': [50100, 13, 128009], 'meta_info': {'id': '0bb8c4d1100641e88ad745ee05f71a7f', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'reasoning_tokens': 0, 'completion_tokens': 3, 'cached_tokens': 40, 'cached_tokens_details': {'device': 40, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.04219266213476658, 'response_sent_to_client_ts': 1775294477.045794}}</strong>



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


    Multi-thread loading shards:   0% Completed | 0/4 [00:00<?, ?it/s]Multi-thread loading shards:  25% Completed | 1/4 [00:00<00:00,  6.18it/s]

    Multi-thread loading shards:  50% Completed | 2/4 [00:00<00:01,  1.97it/s]

    Multi-thread loading shards:  75% Completed | 3/4 [00:01<00:00,  1.45it/s]

    Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.28it/s]Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.46it/s]


    2026-04-04 09:21:36,889 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 09:21:36] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:20,  3.52s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:20,  3.52s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:53,  2.03s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:53,  2.03s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:10,  1.28s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:10,  1.28s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:05<00:48,  1.11it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:05<00:48,  1.11it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:35,  1.48it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:35,  1.48it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:27,  1.89it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:27,  1.89it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:06<00:23,  2.16it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:06<00:23,  2.16it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:21,  2.30it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:21,  2.30it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:19,  2.46it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:19,  2.46it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:07<00:18,  2.62it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:07<00:18,  2.62it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:07<00:17,  2.72it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:07<00:17,  2.72it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:07<00:15,  2.90it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:07<00:15,  2.90it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:14,  3.15it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:14,  3.15it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:08<00:13,  3.29it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:08<00:13,  3.29it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:08<00:11,  3.60it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:08<00:11,  3.60it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:08<00:10,  4.10it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:08<00:10,  4.10it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:08<00:09,  4.32it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:08<00:09,  4.32it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:09<00:08,  4.79it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:09<00:08,  4.79it/s]

    Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:09<00:07,  5.54it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:09<00:07,  5.54it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:09<00:07,  5.54it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:09<00:05,  7.34it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:09<00:05,  7.34it/s] 

    Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:09<00:05,  7.34it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:09<00:03,  8.77it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:09<00:03,  8.77it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:09<00:03,  8.77it/s]

    Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:09<00:03,  9.65it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:09<00:03,  9.65it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:09<00:03,  9.65it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:09<00:02, 11.24it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:09<00:02, 11.24it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:09<00:02, 11.24it/s]

    Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:09<00:02, 12.42it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:09<00:02, 12.42it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:09<00:02, 12.42it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:10<00:02, 13.06it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:10<00:02, 13.06it/s]

    Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:10<00:02, 13.06it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:10<00:01, 13.30it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:10<00:01, 13.30it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:10<00:01, 13.30it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:10<00:01, 13.30it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:10<00:01, 15.84it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:10<00:01, 15.84it/s]

    Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:10<00:01, 15.84it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:10<00:01, 15.89it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:10<00:01, 15.89it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:10<00:01, 15.89it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:10<00:01, 16.31it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:10<00:01, 16.31it/s]

    Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:10<00:01, 16.31it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:10<00:01, 16.31it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:10<00:00, 19.23it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:10<00:00, 19.23it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:10<00:00, 19.23it/s]

    Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:10<00:00, 18.23it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:10<00:00, 18.23it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:10<00:00, 18.23it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:10<00:00, 18.23it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:10<00:00, 19.64it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:10<00:00, 19.64it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:10<00:00, 19.64it/s]

    Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:10<00:00, 19.64it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:11<00:00, 20.53it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:11<00:00, 20.53it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:11<00:00, 20.53it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:11<00:00, 20.53it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:11<00:00, 21.07it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:11<00:00, 21.07it/s]

    Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:11<00:00, 21.07it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:11<00:00, 21.07it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:11<00:00, 21.07it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:11<00:00,  5.16it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=84.36 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=84.36 GB):   2%|▏         | 1/58 [00:00<00:47,  1.20it/s]Capturing num tokens (num_tokens=7680 avail_mem=84.86 GB):   2%|▏         | 1/58 [00:00<00:47,  1.20it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=84.86 GB):   3%|▎         | 2/58 [00:01<00:42,  1.31it/s]Capturing num tokens (num_tokens=7168 avail_mem=85.16 GB):   3%|▎         | 2/58 [00:01<00:42,  1.31it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=85.16 GB):   5%|▌         | 3/58 [00:02<00:36,  1.52it/s]Capturing num tokens (num_tokens=6656 avail_mem=85.15 GB):   5%|▌         | 3/58 [00:02<00:36,  1.52it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=85.15 GB):   7%|▋         | 4/58 [00:02<00:31,  1.73it/s]Capturing num tokens (num_tokens=6144 avail_mem=85.13 GB):   7%|▋         | 4/58 [00:02<00:31,  1.73it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=85.13 GB):   9%|▊         | 5/58 [00:02<00:26,  1.97it/s]Capturing num tokens (num_tokens=5632 avail_mem=85.11 GB):   9%|▊         | 5/58 [00:02<00:26,  1.97it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=85.11 GB):  10%|█         | 6/58 [00:03<00:23,  2.20it/s]Capturing num tokens (num_tokens=5120 avail_mem=85.10 GB):  10%|█         | 6/58 [00:03<00:23,  2.20it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=85.10 GB):  12%|█▏        | 7/58 [00:03<00:20,  2.45it/s]Capturing num tokens (num_tokens=4608 avail_mem=85.04 GB):  12%|█▏        | 7/58 [00:03<00:20,  2.45it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=85.04 GB):  14%|█▍        | 8/58 [00:03<00:17,  2.79it/s]Capturing num tokens (num_tokens=4096 avail_mem=85.06 GB):  14%|█▍        | 8/58 [00:03<00:17,  2.79it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=85.06 GB):  16%|█▌        | 9/58 [00:04<00:15,  3.20it/s]Capturing num tokens (num_tokens=3840 avail_mem=85.05 GB):  16%|█▌        | 9/58 [00:04<00:15,  3.20it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=85.05 GB):  17%|█▋        | 10/58 [00:04<00:13,  3.55it/s]Capturing num tokens (num_tokens=3584 avail_mem=85.03 GB):  17%|█▋        | 10/58 [00:04<00:13,  3.55it/s]Capturing num tokens (num_tokens=3584 avail_mem=85.03 GB):  19%|█▉        | 11/58 [00:04<00:11,  3.95it/s]Capturing num tokens (num_tokens=3328 avail_mem=84.99 GB):  19%|█▉        | 11/58 [00:04<00:11,  3.95it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=84.99 GB):  21%|██        | 12/58 [00:04<00:10,  4.34it/s]Capturing num tokens (num_tokens=3072 avail_mem=85.03 GB):  21%|██        | 12/58 [00:04<00:10,  4.34it/s]Capturing num tokens (num_tokens=3072 avail_mem=85.03 GB):  22%|██▏       | 13/58 [00:04<00:09,  4.78it/s]Capturing num tokens (num_tokens=2816 avail_mem=85.01 GB):  22%|██▏       | 13/58 [00:04<00:09,  4.78it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=85.01 GB):  24%|██▍       | 14/58 [00:04<00:08,  5.27it/s]Capturing num tokens (num_tokens=2560 avail_mem=84.99 GB):  24%|██▍       | 14/58 [00:04<00:08,  5.27it/s]Capturing num tokens (num_tokens=2560 avail_mem=84.99 GB):  26%|██▌       | 15/58 [00:05<00:07,  5.81it/s]Capturing num tokens (num_tokens=2304 avail_mem=84.98 GB):  26%|██▌       | 15/58 [00:05<00:07,  5.81it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=84.98 GB):  28%|██▊       | 16/58 [00:05<00:06,  6.40it/s]Capturing num tokens (num_tokens=2048 avail_mem=84.97 GB):  28%|██▊       | 16/58 [00:05<00:06,  6.40it/s]Capturing num tokens (num_tokens=2048 avail_mem=84.97 GB):  29%|██▉       | 17/58 [00:05<00:05,  7.09it/s]Capturing num tokens (num_tokens=1792 avail_mem=84.96 GB):  29%|██▉       | 17/58 [00:05<00:05,  7.09it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=84.94 GB):  29%|██▉       | 17/58 [00:05<00:05,  7.09it/s]Capturing num tokens (num_tokens=1536 avail_mem=84.94 GB):  33%|███▎      | 19/58 [00:05<00:04,  8.29it/s]Capturing num tokens (num_tokens=1280 avail_mem=84.93 GB):  33%|███▎      | 19/58 [00:05<00:04,  8.29it/s]Capturing num tokens (num_tokens=1024 avail_mem=84.92 GB):  33%|███▎      | 19/58 [00:05<00:04,  8.29it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=84.92 GB):  36%|███▌      | 21/58 [00:05<00:03, 10.27it/s]Capturing num tokens (num_tokens=960 avail_mem=84.91 GB):  36%|███▌      | 21/58 [00:05<00:03, 10.27it/s] Capturing num tokens (num_tokens=896 avail_mem=84.91 GB):  36%|███▌      | 21/58 [00:05<00:03, 10.27it/s]Capturing num tokens (num_tokens=832 avail_mem=84.91 GB):  36%|███▌      | 21/58 [00:05<00:03, 10.27it/s]Capturing num tokens (num_tokens=832 avail_mem=84.91 GB):  41%|████▏     | 24/58 [00:05<00:02, 13.96it/s]Capturing num tokens (num_tokens=768 avail_mem=84.90 GB):  41%|████▏     | 24/58 [00:05<00:02, 13.96it/s]Capturing num tokens (num_tokens=704 avail_mem=84.90 GB):  41%|████▏     | 24/58 [00:05<00:02, 13.96it/s]

    Capturing num tokens (num_tokens=640 avail_mem=84.89 GB):  41%|████▏     | 24/58 [00:05<00:02, 13.96it/s]Capturing num tokens (num_tokens=640 avail_mem=84.89 GB):  47%|████▋     | 27/58 [00:05<00:01, 17.26it/s]Capturing num tokens (num_tokens=576 avail_mem=84.89 GB):  47%|████▋     | 27/58 [00:05<00:01, 17.26it/s]Capturing num tokens (num_tokens=512 avail_mem=84.88 GB):  47%|████▋     | 27/58 [00:05<00:01, 17.26it/s]Capturing num tokens (num_tokens=480 avail_mem=84.88 GB):  47%|████▋     | 27/58 [00:05<00:01, 17.26it/s]Capturing num tokens (num_tokens=480 avail_mem=84.88 GB):  52%|█████▏    | 30/58 [00:05<00:01, 20.35it/s]Capturing num tokens (num_tokens=448 avail_mem=84.87 GB):  52%|█████▏    | 30/58 [00:05<00:01, 20.35it/s]Capturing num tokens (num_tokens=416 avail_mem=84.87 GB):  52%|█████▏    | 30/58 [00:05<00:01, 20.35it/s]

    Capturing num tokens (num_tokens=384 avail_mem=84.87 GB):  52%|█████▏    | 30/58 [00:06<00:01, 20.35it/s]Capturing num tokens (num_tokens=352 avail_mem=84.86 GB):  52%|█████▏    | 30/58 [00:06<00:01, 20.35it/s]Capturing num tokens (num_tokens=352 avail_mem=84.86 GB):  59%|█████▊    | 34/58 [00:06<00:01, 23.51it/s]Capturing num tokens (num_tokens=320 avail_mem=84.86 GB):  59%|█████▊    | 34/58 [00:06<00:01, 23.51it/s]Capturing num tokens (num_tokens=288 avail_mem=84.85 GB):  59%|█████▊    | 34/58 [00:06<00:01, 23.51it/s]Capturing num tokens (num_tokens=256 avail_mem=84.85 GB):  59%|█████▊    | 34/58 [00:06<00:01, 23.51it/s]Capturing num tokens (num_tokens=256 avail_mem=84.85 GB):  64%|██████▍   | 37/58 [00:06<00:00, 24.33it/s]Capturing num tokens (num_tokens=240 avail_mem=84.85 GB):  64%|██████▍   | 37/58 [00:06<00:00, 24.33it/s]

    Capturing num tokens (num_tokens=224 avail_mem=84.84 GB):  64%|██████▍   | 37/58 [00:06<00:00, 24.33it/s]Capturing num tokens (num_tokens=208 avail_mem=84.84 GB):  64%|██████▍   | 37/58 [00:06<00:00, 24.33it/s]Capturing num tokens (num_tokens=208 avail_mem=84.84 GB):  69%|██████▉   | 40/58 [00:06<00:00, 25.57it/s]Capturing num tokens (num_tokens=192 avail_mem=84.83 GB):  69%|██████▉   | 40/58 [00:06<00:00, 25.57it/s]

    Capturing num tokens (num_tokens=176 avail_mem=84.83 GB):  69%|██████▉   | 40/58 [00:06<00:00, 25.57it/s]Capturing num tokens (num_tokens=160 avail_mem=84.82 GB):  69%|██████▉   | 40/58 [00:06<00:00, 25.57it/s]Capturing num tokens (num_tokens=160 avail_mem=84.82 GB):  74%|███████▍  | 43/58 [00:06<00:00, 18.95it/s]Capturing num tokens (num_tokens=144 avail_mem=84.82 GB):  74%|███████▍  | 43/58 [00:06<00:00, 18.95it/s]Capturing num tokens (num_tokens=128 avail_mem=84.81 GB):  74%|███████▍  | 43/58 [00:06<00:00, 18.95it/s]Capturing num tokens (num_tokens=112 avail_mem=84.82 GB):  74%|███████▍  | 43/58 [00:06<00:00, 18.95it/s]Capturing num tokens (num_tokens=96 avail_mem=84.82 GB):  74%|███████▍  | 43/58 [00:06<00:00, 18.95it/s] Capturing num tokens (num_tokens=96 avail_mem=84.82 GB):  81%|████████  | 47/58 [00:06<00:00, 22.78it/s]Capturing num tokens (num_tokens=80 avail_mem=84.82 GB):  81%|████████  | 47/58 [00:06<00:00, 22.78it/s]

    Capturing num tokens (num_tokens=64 avail_mem=84.81 GB):  81%|████████  | 47/58 [00:06<00:00, 22.78it/s]Capturing num tokens (num_tokens=48 avail_mem=84.81 GB):  81%|████████  | 47/58 [00:06<00:00, 22.78it/s]Capturing num tokens (num_tokens=32 avail_mem=84.81 GB):  81%|████████  | 47/58 [00:06<00:00, 22.78it/s]Capturing num tokens (num_tokens=32 avail_mem=84.81 GB):  88%|████████▊ | 51/58 [00:06<00:00, 25.82it/s]Capturing num tokens (num_tokens=28 avail_mem=84.80 GB):  88%|████████▊ | 51/58 [00:06<00:00, 25.82it/s]Capturing num tokens (num_tokens=24 avail_mem=84.79 GB):  88%|████████▊ | 51/58 [00:06<00:00, 25.82it/s]Capturing num tokens (num_tokens=20 avail_mem=84.79 GB):  88%|████████▊ | 51/58 [00:06<00:00, 25.82it/s]Capturing num tokens (num_tokens=16 avail_mem=84.79 GB):  88%|████████▊ | 51/58 [00:06<00:00, 25.82it/s]

    Capturing num tokens (num_tokens=16 avail_mem=84.79 GB):  95%|█████████▍| 55/58 [00:06<00:00, 28.15it/s]Capturing num tokens (num_tokens=12 avail_mem=84.78 GB):  95%|█████████▍| 55/58 [00:06<00:00, 28.15it/s]Capturing num tokens (num_tokens=8 avail_mem=84.78 GB):  95%|█████████▍| 55/58 [00:06<00:00, 28.15it/s] Capturing num tokens (num_tokens=4 avail_mem=84.77 GB):  95%|█████████▍| 55/58 [00:06<00:00, 28.15it/s]Capturing num tokens (num_tokens=4 avail_mem=84.77 GB): 100%|██████████| 58/58 [00:06<00:00,  8.32it/s]


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



<strong style='color: #00008B;'>Prompt: Give me the information of the capital of Italy.<br>Generated text: Paris is the capital of France</strong>


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
