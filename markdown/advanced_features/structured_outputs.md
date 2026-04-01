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
    2026-04-01 01:09:35.068 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:09:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 01:09:35.068 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:09:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 01:09:35.068 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:09:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 01:09:35.068 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:09:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 01:09:35.068 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:09:35] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]Loading safetensors checkpoint shards:  25% Completed | 1/4 [00:00<00:00,  5.68it/s]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:01<00:01,  1.78it/s]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:02<00:00,  1.31it/s]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:03<00:00,  1.16it/s]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:03<00:00,  1.32it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:21,  3.53s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:21,  3.53s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:40,  1.79s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:40,  1.79s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:00,  1.10s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:00,  1.10s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:41,  1.31it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:41,  1.31it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:30,  1.74it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:30,  1.74it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:23,  2.26it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:23,  2.26it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:18,  2.81it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:18,  2.81it/s]Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:14,  3.37it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:14,  3.37it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:12,  4.00it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:12,  4.00it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:10,  4.72it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:10,  4.72it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:08,  5.39it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:08,  5.39it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:05<00:07,  6.08it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:05<00:07,  6.08it/s]

    Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:06<00:07,  6.08it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:06<00:05,  7.74it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:06<00:05,  7.74it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:06<00:05,  7.74it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:06<00:04,  9.43it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:06<00:04,  9.43it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:06<00:04,  9.43it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:06<00:03, 11.51it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:06<00:03, 11.51it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:06<00:03, 11.51it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:06<00:03, 11.51it/s]

    Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:06<00:02, 15.45it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:06<00:02, 15.45it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:06<00:02, 15.45it/s]Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:06<00:02, 15.45it/s]Compiling num tokens (num_tokens=768):  36%|███▌      | 21/58 [00:06<00:02, 15.45it/s]Compiling num tokens (num_tokens=704):  36%|███▌      | 21/58 [00:06<00:02, 15.45it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:06<00:01, 22.83it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:06<00:01, 22.83it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:06<00:01, 22.83it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:06<00:01, 22.83it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:06<00:01, 22.83it/s]

    Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:06<00:01, 22.83it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:06<00:00, 29.58it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:06<00:00, 29.58it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:06<00:00, 29.58it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:06<00:00, 29.58it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:06<00:00, 29.58it/s]Compiling num tokens (num_tokens=288):  53%|█████▎    | 31/58 [00:06<00:00, 29.58it/s]Compiling num tokens (num_tokens=256):  53%|█████▎    | 31/58 [00:06<00:00, 29.58it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:06<00:00, 36.58it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:06<00:00, 36.58it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:06<00:00, 36.58it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:06<00:00, 36.58it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:06<00:00, 36.58it/s]Compiling num tokens (num_tokens=176):  64%|██████▍   | 37/58 [00:06<00:00, 36.58it/s]

    Compiling num tokens (num_tokens=160):  64%|██████▍   | 37/58 [00:06<00:00, 36.58it/s]Compiling num tokens (num_tokens=144):  64%|██████▍   | 37/58 [00:06<00:00, 36.58it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:06<00:00, 43.63it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:06<00:00, 43.63it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:06<00:00, 43.63it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:06<00:00, 43.63it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:06<00:00, 43.63it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:06<00:00, 43.63it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:06<00:00, 43.63it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:07<00:00, 47.51it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:07<00:00, 47.51it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:07<00:00, 47.51it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:07<00:00, 47.51it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:07<00:00, 47.51it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:07<00:00, 47.51it/s]

    Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:07<00:00, 47.51it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:07<00:00, 47.51it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:07<00:00, 53.55it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:07<00:00, 53.55it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:07<00:00,  8.15it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=100.01 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=100.01 GB):   2%|▏         | 1/58 [00:00<00:23,  2.39it/s]Capturing num tokens (num_tokens=7680 avail_mem=99.98 GB):   2%|▏         | 1/58 [00:00<00:23,  2.39it/s] 

    Capturing num tokens (num_tokens=7680 avail_mem=99.98 GB):   3%|▎         | 2/58 [00:00<00:20,  2.77it/s]Capturing num tokens (num_tokens=7168 avail_mem=99.98 GB):   3%|▎         | 2/58 [00:00<00:20,  2.77it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=99.98 GB):   5%|▌         | 3/58 [00:01<00:17,  3.08it/s]Capturing num tokens (num_tokens=6656 avail_mem=99.99 GB):   5%|▌         | 3/58 [00:01<00:17,  3.08it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=99.99 GB):   7%|▋         | 4/58 [00:01<00:16,  3.29it/s]Capturing num tokens (num_tokens=6144 avail_mem=99.99 GB):   7%|▋         | 4/58 [00:01<00:16,  3.29it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=99.99 GB):   9%|▊         | 5/58 [00:01<00:14,  3.61it/s]Capturing num tokens (num_tokens=5632 avail_mem=99.99 GB):   9%|▊         | 5/58 [00:01<00:14,  3.61it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=99.99 GB):  10%|█         | 6/58 [00:01<00:13,  3.95it/s]Capturing num tokens (num_tokens=5120 avail_mem=100.00 GB):  10%|█         | 6/58 [00:01<00:13,  3.95it/s]Capturing num tokens (num_tokens=5120 avail_mem=100.00 GB):  12%|█▏        | 7/58 [00:01<00:11,  4.30it/s]Capturing num tokens (num_tokens=4608 avail_mem=100.00 GB):  12%|█▏        | 7/58 [00:01<00:11,  4.30it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=100.00 GB):  14%|█▍        | 8/58 [00:02<00:10,  4.75it/s]Capturing num tokens (num_tokens=4096 avail_mem=100.01 GB):  14%|█▍        | 8/58 [00:02<00:10,  4.75it/s]Capturing num tokens (num_tokens=4096 avail_mem=100.01 GB):  16%|█▌        | 9/58 [00:02<00:09,  5.21it/s]Capturing num tokens (num_tokens=3840 avail_mem=100.01 GB):  16%|█▌        | 9/58 [00:02<00:09,  5.21it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=100.01 GB):  17%|█▋        | 10/58 [00:02<00:08,  5.65it/s]Capturing num tokens (num_tokens=3584 avail_mem=100.01 GB):  17%|█▋        | 10/58 [00:02<00:08,  5.65it/s]Capturing num tokens (num_tokens=3584 avail_mem=100.01 GB):  19%|█▉        | 11/58 [00:02<00:07,  6.14it/s]Capturing num tokens (num_tokens=3328 avail_mem=100.00 GB):  19%|█▉        | 11/58 [00:02<00:07,  6.14it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=100.00 GB):  21%|██        | 12/58 [00:02<00:07,  5.91it/s]Capturing num tokens (num_tokens=3072 avail_mem=99.95 GB):  21%|██        | 12/58 [00:02<00:07,  5.91it/s] Capturing num tokens (num_tokens=3072 avail_mem=99.95 GB):  22%|██▏       | 13/58 [00:02<00:07,  6.33it/s]Capturing num tokens (num_tokens=2816 avail_mem=99.95 GB):  22%|██▏       | 13/58 [00:02<00:07,  6.33it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=99.95 GB):  24%|██▍       | 14/58 [00:02<00:06,  7.05it/s]Capturing num tokens (num_tokens=2560 avail_mem=99.95 GB):  24%|██▍       | 14/58 [00:02<00:06,  7.05it/s]Capturing num tokens (num_tokens=2560 avail_mem=99.95 GB):  26%|██▌       | 15/58 [00:03<00:06,  6.83it/s]Capturing num tokens (num_tokens=2304 avail_mem=102.58 GB):  26%|██▌       | 15/58 [00:03<00:06,  6.83it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=102.57 GB):  26%|██▌       | 15/58 [00:03<00:06,  6.83it/s]Capturing num tokens (num_tokens=2048 avail_mem=102.57 GB):  29%|██▉       | 17/58 [00:03<00:04,  8.76it/s]Capturing num tokens (num_tokens=1792 avail_mem=102.57 GB):  29%|██▉       | 17/58 [00:03<00:04,  8.76it/s]Capturing num tokens (num_tokens=1536 avail_mem=102.57 GB):  29%|██▉       | 17/58 [00:03<00:04,  8.76it/s]Capturing num tokens (num_tokens=1536 avail_mem=102.57 GB):  33%|███▎      | 19/58 [00:03<00:03, 10.83it/s]Capturing num tokens (num_tokens=1280 avail_mem=102.57 GB):  33%|███▎      | 19/58 [00:03<00:03, 10.83it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=102.57 GB):  33%|███▎      | 19/58 [00:03<00:03, 10.83it/s]Capturing num tokens (num_tokens=960 avail_mem=102.57 GB):  33%|███▎      | 19/58 [00:03<00:03, 10.83it/s] Capturing num tokens (num_tokens=960 avail_mem=102.57 GB):  38%|███▊      | 22/58 [00:03<00:02, 14.12it/s]Capturing num tokens (num_tokens=896 avail_mem=102.56 GB):  38%|███▊      | 22/58 [00:03<00:02, 14.12it/s]Capturing num tokens (num_tokens=832 avail_mem=102.56 GB):  38%|███▊      | 22/58 [00:03<00:02, 14.12it/s]Capturing num tokens (num_tokens=768 avail_mem=102.56 GB):  38%|███▊      | 22/58 [00:03<00:02, 14.12it/s]

    Capturing num tokens (num_tokens=768 avail_mem=102.56 GB):  43%|████▎     | 25/58 [00:03<00:01, 17.06it/s]Capturing num tokens (num_tokens=704 avail_mem=102.55 GB):  43%|████▎     | 25/58 [00:03<00:01, 17.06it/s]Capturing num tokens (num_tokens=640 avail_mem=102.55 GB):  43%|████▎     | 25/58 [00:03<00:01, 17.06it/s]Capturing num tokens (num_tokens=576 avail_mem=102.54 GB):  43%|████▎     | 25/58 [00:03<00:01, 17.06it/s]Capturing num tokens (num_tokens=576 avail_mem=102.54 GB):  48%|████▊     | 28/58 [00:03<00:01, 19.89it/s]Capturing num tokens (num_tokens=512 avail_mem=102.54 GB):  48%|████▊     | 28/58 [00:03<00:01, 19.89it/s]Capturing num tokens (num_tokens=480 avail_mem=102.53 GB):  48%|████▊     | 28/58 [00:03<00:01, 19.89it/s]Capturing num tokens (num_tokens=448 avail_mem=102.53 GB):  48%|████▊     | 28/58 [00:03<00:01, 19.89it/s]

    Capturing num tokens (num_tokens=448 avail_mem=102.53 GB):  53%|█████▎    | 31/58 [00:03<00:01, 22.44it/s]Capturing num tokens (num_tokens=416 avail_mem=102.53 GB):  53%|█████▎    | 31/58 [00:03<00:01, 22.44it/s]Capturing num tokens (num_tokens=384 avail_mem=102.52 GB):  53%|█████▎    | 31/58 [00:03<00:01, 22.44it/s]Capturing num tokens (num_tokens=352 avail_mem=102.52 GB):  53%|█████▎    | 31/58 [00:03<00:01, 22.44it/s]Capturing num tokens (num_tokens=320 avail_mem=102.52 GB):  53%|█████▎    | 31/58 [00:03<00:01, 22.44it/s]Capturing num tokens (num_tokens=320 avail_mem=102.52 GB):  60%|██████    | 35/58 [00:03<00:00, 25.62it/s]Capturing num tokens (num_tokens=288 avail_mem=102.51 GB):  60%|██████    | 35/58 [00:03<00:00, 25.62it/s]Capturing num tokens (num_tokens=256 avail_mem=102.51 GB):  60%|██████    | 35/58 [00:03<00:00, 25.62it/s]Capturing num tokens (num_tokens=240 avail_mem=102.50 GB):  60%|██████    | 35/58 [00:04<00:00, 25.62it/s]

    Capturing num tokens (num_tokens=224 avail_mem=102.50 GB):  60%|██████    | 35/58 [00:04<00:00, 25.62it/s]Capturing num tokens (num_tokens=224 avail_mem=102.50 GB):  67%|██████▋   | 39/58 [00:04<00:00, 28.25it/s]Capturing num tokens (num_tokens=208 avail_mem=102.49 GB):  67%|██████▋   | 39/58 [00:04<00:00, 28.25it/s]Capturing num tokens (num_tokens=192 avail_mem=102.49 GB):  67%|██████▋   | 39/58 [00:04<00:00, 28.25it/s]Capturing num tokens (num_tokens=176 avail_mem=102.48 GB):  67%|██████▋   | 39/58 [00:04<00:00, 28.25it/s]Capturing num tokens (num_tokens=160 avail_mem=102.48 GB):  67%|██████▋   | 39/58 [00:04<00:00, 28.25it/s]Capturing num tokens (num_tokens=160 avail_mem=102.48 GB):  74%|███████▍  | 43/58 [00:04<00:00, 29.66it/s]Capturing num tokens (num_tokens=144 avail_mem=102.47 GB):  74%|███████▍  | 43/58 [00:04<00:00, 29.66it/s]Capturing num tokens (num_tokens=128 avail_mem=102.47 GB):  74%|███████▍  | 43/58 [00:04<00:00, 29.66it/s]

    Capturing num tokens (num_tokens=112 avail_mem=102.48 GB):  74%|███████▍  | 43/58 [00:04<00:00, 29.66it/s]Capturing num tokens (num_tokens=96 avail_mem=102.48 GB):  74%|███████▍  | 43/58 [00:04<00:00, 29.66it/s] Capturing num tokens (num_tokens=96 avail_mem=102.48 GB):  81%|████████  | 47/58 [00:04<00:00, 31.23it/s]Capturing num tokens (num_tokens=80 avail_mem=102.47 GB):  81%|████████  | 47/58 [00:04<00:00, 31.23it/s]Capturing num tokens (num_tokens=64 avail_mem=102.47 GB):  81%|████████  | 47/58 [00:04<00:00, 31.23it/s]Capturing num tokens (num_tokens=48 avail_mem=102.46 GB):  81%|████████  | 47/58 [00:04<00:00, 31.23it/s]Capturing num tokens (num_tokens=32 avail_mem=102.46 GB):  81%|████████  | 47/58 [00:04<00:00, 31.23it/s]Capturing num tokens (num_tokens=32 avail_mem=102.46 GB):  88%|████████▊ | 51/58 [00:04<00:00, 32.22it/s]Capturing num tokens (num_tokens=28 avail_mem=102.46 GB):  88%|████████▊ | 51/58 [00:04<00:00, 32.22it/s]

    Capturing num tokens (num_tokens=24 avail_mem=102.45 GB):  88%|████████▊ | 51/58 [00:04<00:00, 32.22it/s]Capturing num tokens (num_tokens=20 avail_mem=102.45 GB):  88%|████████▊ | 51/58 [00:04<00:00, 32.22it/s]Capturing num tokens (num_tokens=16 avail_mem=102.44 GB):  88%|████████▊ | 51/58 [00:04<00:00, 32.22it/s]Capturing num tokens (num_tokens=16 avail_mem=102.44 GB):  95%|█████████▍| 55/58 [00:04<00:00, 32.94it/s]Capturing num tokens (num_tokens=12 avail_mem=102.44 GB):  95%|█████████▍| 55/58 [00:04<00:00, 32.94it/s]Capturing num tokens (num_tokens=8 avail_mem=102.44 GB):  95%|█████████▍| 55/58 [00:04<00:00, 32.94it/s] Capturing num tokens (num_tokens=4 avail_mem=102.43 GB):  95%|█████████▍| 55/58 [00:04<00:00, 32.94it/s]Capturing num tokens (num_tokens=4 avail_mem=102.43 GB): 100%|██████████| 58/58 [00:04<00:00, 12.58it/s]


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


<strong style='color: #00008B;'><function=get_current_date>{"timezone": "America/New_York"}</function><br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "fahrenheit"}</function><br><br>Sources: <br>- get_current_date function <br>- get_current_weather function</strong>


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


<strong style='color: #00008B;'>{'text': '{"name": "Paris", "population": 2147000}', 'output_ids': [5018, 609, 794, 330, 60704, 498, 330, 45541, 794, 220, 11584, 7007, 15, 92, 128009], 'meta_info': {'id': 'bb375244010143929c5697b6329aada3', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 50, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 15, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.17447136715054512, 'response_sent_to_client_ts': 1775005807.2005246}}</strong>



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


<strong style='color: #00008B;'>{'text': '{"name": "Paris", "population": 2147000}', 'output_ids': [5018, 609, 794, 330, 60704, 498, 330, 45541, 794, 220, 11584, 7007, 15, 92, 128009], 'meta_info': {'id': '27dcc784e5f24d2f96043c0059c65195', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 50, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 15, 'cached_tokens': 49, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.179715724196285, 'response_sent_to_client_ts': 1775005807.390022}}</strong>


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


<strong style='color: #00008B;'>[{'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': 'c7cde7a910454305ac6370fc10d84996', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.09301389986649156, 'response_sent_to_client_ts': 1775005807.5215323}}, {'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': 'aa0a8052394842c8a1f774b88d33f843', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.09293174790218472, 'response_sent_to_client_ts': 1775005807.5215433}}, {'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': 'e619e52adeb2448ba3936669b4580836', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.09289148729294538, 'response_sent_to_client_ts': 1775005807.5215483}}]</strong>


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


<strong style='color: #00008B;'>{'text': 'France', 'output_ids': [50100, 128009], 'meta_info': {'id': '8da9636285ec4dc98f47c9be0e8ed071', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2, 'cached_tokens': 31, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.039633575826883316, 'response_sent_to_client_ts': 1775005807.568198}}</strong>


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


<strong style='color: #00008B;'>{'text': 'France.', 'output_ids': [50100, 13, 128009], 'meta_info': {'id': 'b14dbf3a322340b995ab206b4b09c0db', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 3, 'cached_tokens': 40, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.10117501299828291, 'response_sent_to_client_ts': 1775005809.0498543}}</strong>



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


<strong style='color: #00008B;'>{'text': 'France.', 'output_ids': [50100, 13, 128009], 'meta_info': {'id': 'e978e821a7eb452da36ec591e1a8eb26', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 3, 'cached_tokens': 40, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.05058536818251014, 'response_sent_to_client_ts': 1775005809.1104782}}</strong>



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


    2026-04-01 01:10:23.125 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:10:23] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 01:10:23.125 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:10:23] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 01:10:23.125 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:10:23] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 01:10:23.126 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:10:23] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 01:10:23.126 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:10:23] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]Loading safetensors checkpoint shards:  25% Completed | 1/4 [00:00<00:00,  5.79it/s]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:01<00:01,  1.72it/s]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:02<00:00,  1.27it/s]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:03<00:00,  1.13it/s]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:03<00:00,  1.29it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:15,  3.42s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:15,  3.42s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:52,  2.01s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:52,  2.01s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:12,  1.32s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:12,  1.32s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:05<00:52,  1.03it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:05<00:52,  1.03it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:40,  1.30it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:40,  1.30it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:06<00:32,  1.60it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:06<00:32,  1.60it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:06<00:26,  1.90it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:06<00:26,  1.90it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:22,  2.22it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:22,  2.22it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:07<00:18,  2.58it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:07<00:18,  2.58it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:07<00:16,  2.97it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:07<00:16,  2.97it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:07<00:14,  3.31it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:07<00:14,  3.31it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:07<00:12,  3.68it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:07<00:12,  3.68it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:11,  4.09it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:11,  4.09it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:08<00:09,  4.49it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:08<00:09,  4.49it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:08<00:08,  5.02it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:08<00:08,  5.02it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:08<00:07,  5.59it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:08<00:07,  5.59it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:08<00:06,  6.10it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:08<00:06,  6.10it/s]

    Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:08<00:05,  6.91it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:08<00:05,  6.91it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:08<00:05,  6.91it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:08<00:04,  8.39it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:08<00:04,  8.39it/s]

    Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:08<00:04,  8.39it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:08<00:03, 10.33it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:08<00:03, 10.33it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:08<00:03, 10.33it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:08<00:02, 11.94it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:08<00:02, 11.94it/s]

    Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:08<00:02, 11.94it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:09<00:02, 13.70it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:09<00:02, 13.70it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:09<00:02, 13.70it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:09<00:02, 13.70it/s]

    Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:09<00:01, 15.77it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:09<00:01, 15.77it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:09<00:01, 15.77it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:09<00:01, 15.77it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:09<00:01, 17.68it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:09<00:01, 17.68it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:09<00:01, 17.68it/s]

    Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:09<00:01, 17.68it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:09<00:01, 19.50it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:09<00:01, 19.50it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:09<00:01, 19.50it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:09<00:01, 19.50it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:09<00:00, 21.77it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:09<00:00, 21.77it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:09<00:00, 21.77it/s]

    Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:09<00:00, 21.77it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:09<00:00, 21.77it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:09<00:00, 25.02it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:09<00:00, 25.02it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:09<00:00, 25.02it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:09<00:00, 25.02it/s]

    Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:10<00:01, 11.33it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:10<00:01, 11.33it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:10<00:01, 11.33it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:10<00:01, 11.33it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:10<00:01, 11.33it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:10<00:00, 15.02it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:10<00:00, 15.02it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:10<00:00, 15.02it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:10<00:00, 15.02it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:10<00:00, 15.02it/s]

    Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:10<00:00, 18.68it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:10<00:00, 18.68it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:10<00:00, 18.68it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:10<00:00, 18.68it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:10<00:00, 18.68it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:10<00:00, 18.68it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:10<00:00, 24.08it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:10<00:00,  5.46it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=83.30 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=83.30 GB):   2%|▏         | 1/58 [00:01<01:34,  1.65s/it]Capturing num tokens (num_tokens=7680 avail_mem=83.27 GB):   2%|▏         | 1/58 [00:01<01:34,  1.65s/it]

    Capturing num tokens (num_tokens=7680 avail_mem=83.27 GB):   3%|▎         | 2/58 [00:02<01:00,  1.07s/it]Capturing num tokens (num_tokens=7168 avail_mem=83.27 GB):   3%|▎         | 2/58 [00:02<01:00,  1.07s/it]

    Capturing num tokens (num_tokens=7168 avail_mem=83.27 GB):   5%|▌         | 3/58 [00:02<00:46,  1.19it/s]Capturing num tokens (num_tokens=6656 avail_mem=83.27 GB):   5%|▌         | 3/58 [00:02<00:46,  1.19it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=83.27 GB):   7%|▋         | 4/58 [00:03<00:39,  1.38it/s]Capturing num tokens (num_tokens=6144 avail_mem=83.24 GB):   7%|▋         | 4/58 [00:03<00:39,  1.38it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=83.24 GB):   9%|▊         | 5/58 [00:03<00:33,  1.58it/s]Capturing num tokens (num_tokens=5632 avail_mem=83.25 GB):   9%|▊         | 5/58 [00:03<00:33,  1.58it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=83.25 GB):  10%|█         | 6/58 [00:04<00:29,  1.78it/s]Capturing num tokens (num_tokens=5120 avail_mem=83.25 GB):  10%|█         | 6/58 [00:04<00:29,  1.78it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=83.25 GB):  12%|█▏        | 7/58 [00:04<00:26,  1.93it/s]Capturing num tokens (num_tokens=4608 avail_mem=83.26 GB):  12%|█▏        | 7/58 [00:04<00:26,  1.93it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=83.26 GB):  14%|█▍        | 8/58 [00:05<00:23,  2.12it/s]Capturing num tokens (num_tokens=4096 avail_mem=83.26 GB):  14%|█▍        | 8/58 [00:05<00:23,  2.12it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=83.26 GB):  16%|█▌        | 9/58 [00:05<00:21,  2.33it/s]Capturing num tokens (num_tokens=3840 avail_mem=83.26 GB):  16%|█▌        | 9/58 [00:05<00:21,  2.33it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=83.26 GB):  17%|█▋        | 10/58 [00:05<00:18,  2.56it/s]Capturing num tokens (num_tokens=3584 avail_mem=83.26 GB):  17%|█▋        | 10/58 [00:05<00:18,  2.56it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=83.26 GB):  19%|█▉        | 11/58 [00:06<00:16,  2.80it/s]Capturing num tokens (num_tokens=3328 avail_mem=83.26 GB):  19%|█▉        | 11/58 [00:06<00:16,  2.80it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=83.26 GB):  21%|██        | 12/58 [00:06<00:14,  3.07it/s]Capturing num tokens (num_tokens=3072 avail_mem=83.26 GB):  21%|██        | 12/58 [00:06<00:14,  3.07it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=83.26 GB):  22%|██▏       | 13/58 [00:06<00:13,  3.37it/s]Capturing num tokens (num_tokens=2816 avail_mem=83.26 GB):  22%|██▏       | 13/58 [00:06<00:13,  3.37it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=83.26 GB):  24%|██▍       | 14/58 [00:06<00:11,  3.70it/s]Capturing num tokens (num_tokens=2560 avail_mem=83.26 GB):  24%|██▍       | 14/58 [00:06<00:11,  3.70it/s]Capturing num tokens (num_tokens=2560 avail_mem=83.26 GB):  26%|██▌       | 15/58 [00:06<00:10,  4.08it/s]Capturing num tokens (num_tokens=2304 avail_mem=83.26 GB):  26%|██▌       | 15/58 [00:06<00:10,  4.08it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=83.26 GB):  28%|██▊       | 16/58 [00:07<00:09,  4.43it/s]Capturing num tokens (num_tokens=2048 avail_mem=83.26 GB):  28%|██▊       | 16/58 [00:07<00:09,  4.43it/s]Capturing num tokens (num_tokens=2048 avail_mem=83.26 GB):  29%|██▉       | 17/58 [00:07<00:08,  5.00it/s]Capturing num tokens (num_tokens=1792 avail_mem=83.26 GB):  29%|██▉       | 17/58 [00:07<00:08,  5.00it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=83.26 GB):  31%|███       | 18/58 [00:07<00:07,  5.56it/s]Capturing num tokens (num_tokens=1536 avail_mem=83.26 GB):  31%|███       | 18/58 [00:07<00:07,  5.56it/s]Capturing num tokens (num_tokens=1536 avail_mem=83.26 GB):  33%|███▎      | 19/58 [00:07<00:06,  6.16it/s]Capturing num tokens (num_tokens=1280 avail_mem=83.26 GB):  33%|███▎      | 19/58 [00:07<00:06,  6.16it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=83.26 GB):  34%|███▍      | 20/58 [00:07<00:05,  6.92it/s]Capturing num tokens (num_tokens=1024 avail_mem=83.26 GB):  34%|███▍      | 20/58 [00:07<00:05,  6.92it/s]Capturing num tokens (num_tokens=960 avail_mem=83.25 GB):  34%|███▍      | 20/58 [00:07<00:05,  6.92it/s] Capturing num tokens (num_tokens=960 avail_mem=83.25 GB):  38%|███▊      | 22/58 [00:07<00:04,  8.35it/s]Capturing num tokens (num_tokens=896 avail_mem=83.25 GB):  38%|███▊      | 22/58 [00:07<00:04,  8.35it/s]

    Capturing num tokens (num_tokens=832 avail_mem=83.25 GB):  38%|███▊      | 22/58 [00:07<00:04,  8.35it/s]Capturing num tokens (num_tokens=832 avail_mem=83.25 GB):  41%|████▏     | 24/58 [00:07<00:03,  9.36it/s]Capturing num tokens (num_tokens=768 avail_mem=83.24 GB):  41%|████▏     | 24/58 [00:07<00:03,  9.36it/s]Capturing num tokens (num_tokens=704 avail_mem=83.24 GB):  41%|████▏     | 24/58 [00:08<00:03,  9.36it/s]

    Capturing num tokens (num_tokens=704 avail_mem=83.24 GB):  45%|████▍     | 26/58 [00:08<00:03, 10.28it/s]Capturing num tokens (num_tokens=640 avail_mem=83.23 GB):  45%|████▍     | 26/58 [00:08<00:03, 10.28it/s]Capturing num tokens (num_tokens=576 avail_mem=83.23 GB):  45%|████▍     | 26/58 [00:08<00:03, 10.28it/s]Capturing num tokens (num_tokens=576 avail_mem=83.23 GB):  48%|████▊     | 28/58 [00:08<00:02, 11.28it/s]Capturing num tokens (num_tokens=512 avail_mem=83.23 GB):  48%|████▊     | 28/58 [00:08<00:02, 11.28it/s]

    Capturing num tokens (num_tokens=480 avail_mem=83.20 GB):  48%|████▊     | 28/58 [00:08<00:02, 11.28it/s]Capturing num tokens (num_tokens=480 avail_mem=83.20 GB):  52%|█████▏    | 30/58 [00:08<00:02, 11.97it/s]Capturing num tokens (num_tokens=448 avail_mem=83.20 GB):  52%|█████▏    | 30/58 [00:08<00:02, 11.97it/s]Capturing num tokens (num_tokens=416 avail_mem=83.19 GB):  52%|█████▏    | 30/58 [00:08<00:02, 11.97it/s]

    Capturing num tokens (num_tokens=416 avail_mem=83.19 GB):  55%|█████▌    | 32/58 [00:08<00:02, 12.43it/s]Capturing num tokens (num_tokens=384 avail_mem=83.19 GB):  55%|█████▌    | 32/58 [00:08<00:02, 12.43it/s]

    Capturing num tokens (num_tokens=352 avail_mem=101.90 GB):  55%|█████▌    | 32/58 [00:08<00:02, 12.43it/s]Capturing num tokens (num_tokens=352 avail_mem=101.90 GB):  59%|█████▊    | 34/58 [00:08<00:02, 10.00it/s]Capturing num tokens (num_tokens=320 avail_mem=101.90 GB):  59%|█████▊    | 34/58 [00:08<00:02, 10.00it/s]Capturing num tokens (num_tokens=288 avail_mem=101.89 GB):  59%|█████▊    | 34/58 [00:08<00:02, 10.00it/s]Capturing num tokens (num_tokens=288 avail_mem=101.89 GB):  62%|██████▏   | 36/58 [00:08<00:01, 11.62it/s]Capturing num tokens (num_tokens=256 avail_mem=101.89 GB):  62%|██████▏   | 36/58 [00:08<00:01, 11.62it/s]

    Capturing num tokens (num_tokens=240 avail_mem=101.89 GB):  62%|██████▏   | 36/58 [00:09<00:01, 11.62it/s]Capturing num tokens (num_tokens=240 avail_mem=101.89 GB):  66%|██████▌   | 38/58 [00:09<00:01, 13.31it/s]Capturing num tokens (num_tokens=224 avail_mem=101.88 GB):  66%|██████▌   | 38/58 [00:09<00:01, 13.31it/s]Capturing num tokens (num_tokens=208 avail_mem=101.88 GB):  66%|██████▌   | 38/58 [00:09<00:01, 13.31it/s]Capturing num tokens (num_tokens=208 avail_mem=101.88 GB):  69%|██████▉   | 40/58 [00:09<00:01, 14.82it/s]Capturing num tokens (num_tokens=192 avail_mem=101.87 GB):  69%|██████▉   | 40/58 [00:09<00:01, 14.82it/s]Capturing num tokens (num_tokens=176 avail_mem=101.87 GB):  69%|██████▉   | 40/58 [00:09<00:01, 14.82it/s]

    Capturing num tokens (num_tokens=160 avail_mem=101.86 GB):  69%|██████▉   | 40/58 [00:09<00:01, 14.82it/s]Capturing num tokens (num_tokens=160 avail_mem=101.86 GB):  74%|███████▍  | 43/58 [00:09<00:00, 16.63it/s]Capturing num tokens (num_tokens=144 avail_mem=101.86 GB):  74%|███████▍  | 43/58 [00:09<00:00, 16.63it/s]Capturing num tokens (num_tokens=128 avail_mem=101.85 GB):  74%|███████▍  | 43/58 [00:09<00:00, 16.63it/s]Capturing num tokens (num_tokens=112 avail_mem=101.86 GB):  74%|███████▍  | 43/58 [00:09<00:00, 16.63it/s]Capturing num tokens (num_tokens=112 avail_mem=101.86 GB):  79%|███████▉  | 46/58 [00:09<00:00, 18.14it/s]Capturing num tokens (num_tokens=96 avail_mem=101.86 GB):  79%|███████▉  | 46/58 [00:09<00:00, 18.14it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=101.86 GB):  79%|███████▉  | 46/58 [00:09<00:00, 18.14it/s]Capturing num tokens (num_tokens=64 avail_mem=101.85 GB):  79%|███████▉  | 46/58 [00:09<00:00, 18.14it/s]Capturing num tokens (num_tokens=64 avail_mem=101.85 GB):  84%|████████▍ | 49/58 [00:09<00:00, 19.21it/s]Capturing num tokens (num_tokens=48 avail_mem=101.85 GB):  84%|████████▍ | 49/58 [00:09<00:00, 19.21it/s]Capturing num tokens (num_tokens=32 avail_mem=101.85 GB):  84%|████████▍ | 49/58 [00:09<00:00, 19.21it/s]Capturing num tokens (num_tokens=28 avail_mem=101.84 GB):  84%|████████▍ | 49/58 [00:09<00:00, 19.21it/s]

    Capturing num tokens (num_tokens=28 avail_mem=101.84 GB):  90%|████████▉ | 52/58 [00:09<00:00, 20.01it/s]Capturing num tokens (num_tokens=24 avail_mem=101.83 GB):  90%|████████▉ | 52/58 [00:09<00:00, 20.01it/s]Capturing num tokens (num_tokens=20 avail_mem=101.83 GB):  90%|████████▉ | 52/58 [00:09<00:00, 20.01it/s]Capturing num tokens (num_tokens=16 avail_mem=101.83 GB):  90%|████████▉ | 52/58 [00:09<00:00, 20.01it/s]Capturing num tokens (num_tokens=16 avail_mem=101.83 GB):  95%|█████████▍| 55/58 [00:09<00:00, 20.69it/s]Capturing num tokens (num_tokens=12 avail_mem=101.82 GB):  95%|█████████▍| 55/58 [00:09<00:00, 20.69it/s]Capturing num tokens (num_tokens=8 avail_mem=101.82 GB):  95%|█████████▍| 55/58 [00:09<00:00, 20.69it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=101.81 GB):  95%|█████████▍| 55/58 [00:09<00:00, 20.69it/s]Capturing num tokens (num_tokens=4 avail_mem=101.81 GB): 100%|██████████| 58/58 [00:09<00:00, 21.14it/s]Capturing num tokens (num_tokens=4 avail_mem=101.81 GB): 100%|██████████| 58/58 [00:09<00:00,  5.81it/s]


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
