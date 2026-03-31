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


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:176: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-03-31 07:59:54.583 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 07:59:54] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 07:59:54.584 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 07:59:54] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 07:59:54.584 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 07:59:54] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 07:59:54.584 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 07:59:54] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 07:59:54.584 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 07:59:54] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]Loading safetensors checkpoint shards:  25% Completed | 1/4 [00:00<00:00,  6.40it/s]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:00<00:00,  2.02it/s]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:01<00:00,  1.51it/s]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.39it/s]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.57it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:15,  3.43s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:15,  3.43s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:49,  1.96s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:49,  1.96s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:09,  1.26s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:09,  1.26s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:05<00:48,  1.12it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:05<00:48,  1.12it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:36,  1.47it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:36,  1.47it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:27,  1.88it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:27,  1.88it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:21,  2.35it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:21,  2.35it/s]Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:17,  2.86it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:17,  2.86it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:13,  3.57it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:13,  3.57it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:10,  4.44it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:10,  4.44it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:08,  5.33it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:08,  5.33it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:06<00:08,  5.33it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:06<00:07,  6.43it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:06<00:07,  6.43it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:06<00:07,  6.21it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:06<00:07,  6.21it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:06<00:06,  6.44it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:06<00:06,  6.44it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:07<00:05,  7.05it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:07<00:05,  7.05it/s]

    Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:07<00:05,  7.05it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:07<00:04,  8.02it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:07<00:04,  8.02it/s]

    Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:07<00:04,  8.41it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:07<00:04,  8.41it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:07<00:04,  8.41it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:07<00:03,  9.68it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:07<00:03,  9.68it/s] 

    Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:07<00:03,  9.68it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:07<00:02, 11.79it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:07<00:02, 11.79it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:07<00:02, 11.79it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:07<00:02, 13.68it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:07<00:02, 13.68it/s]

    Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:07<00:02, 13.68it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:07<00:02, 15.03it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:07<00:02, 15.03it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:07<00:02, 15.03it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:07<00:02, 15.03it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:07<00:01, 17.18it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:07<00:01, 17.18it/s]

    Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:08<00:01, 17.18it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:08<00:01, 17.18it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:08<00:01, 19.54it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:08<00:01, 19.54it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:08<00:01, 19.54it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:08<00:01, 19.54it/s]

    Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:08<00:01, 21.21it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:08<00:01, 21.21it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:08<00:01, 21.21it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:08<00:01, 21.21it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:08<00:00, 22.70it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:08<00:00, 22.70it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:08<00:00, 22.70it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:08<00:00, 22.70it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:08<00:00, 22.70it/s]

    Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:08<00:00, 25.74it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:08<00:00, 25.74it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:08<00:00, 25.74it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:08<00:00, 25.74it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:08<00:00, 26.09it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:08<00:00, 26.09it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:08<00:00, 26.09it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:08<00:00, 26.09it/s]

    Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:08<00:00, 26.09it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:08<00:00, 28.57it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:08<00:00, 28.57it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:08<00:00, 28.57it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:08<00:00, 28.57it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:08<00:00, 28.57it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:08<00:00, 29.61it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:08<00:00, 29.61it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:08<00:00, 29.61it/s]

    Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:08<00:00, 29.61it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:08<00:00, 29.61it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00,  6.52it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=99.62 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=99.62 GB):   2%|▏         | 1/58 [00:00<00:39,  1.46it/s]Capturing num tokens (num_tokens=7680 avail_mem=99.65 GB):   2%|▏         | 1/58 [00:00<00:39,  1.46it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=99.65 GB):   3%|▎         | 2/58 [00:01<00:34,  1.65it/s]Capturing num tokens (num_tokens=7168 avail_mem=100.12 GB):   3%|▎         | 2/58 [00:01<00:34,  1.65it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=100.12 GB):   5%|▌         | 3/58 [00:01<00:29,  1.85it/s]Capturing num tokens (num_tokens=6656 avail_mem=99.82 GB):   5%|▌         | 3/58 [00:01<00:29,  1.85it/s] 

    Capturing num tokens (num_tokens=6656 avail_mem=99.82 GB):   7%|▋         | 4/58 [00:02<00:26,  2.02it/s]Capturing num tokens (num_tokens=6144 avail_mem=100.12 GB):   7%|▋         | 4/58 [00:02<00:26,  2.02it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=100.12 GB):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]Capturing num tokens (num_tokens=5632 avail_mem=100.12 GB):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=100.12 GB):  10%|█         | 6/58 [00:02<00:20,  2.57it/s]Capturing num tokens (num_tokens=5120 avail_mem=100.12 GB):  10%|█         | 6/58 [00:02<00:20,  2.57it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=100.12 GB):  12%|█▏        | 7/58 [00:03<00:17,  2.90it/s]Capturing num tokens (num_tokens=4608 avail_mem=100.11 GB):  12%|█▏        | 7/58 [00:03<00:17,  2.90it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=100.11 GB):  14%|█▍        | 8/58 [00:03<00:15,  3.25it/s]Capturing num tokens (num_tokens=4096 avail_mem=100.03 GB):  14%|█▍        | 8/58 [00:03<00:15,  3.25it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=100.03 GB):  16%|█▌        | 9/58 [00:03<00:13,  3.64it/s]Capturing num tokens (num_tokens=3840 avail_mem=100.10 GB):  16%|█▌        | 9/58 [00:03<00:13,  3.64it/s]Capturing num tokens (num_tokens=3840 avail_mem=100.10 GB):  17%|█▋        | 10/58 [00:03<00:11,  4.02it/s]Capturing num tokens (num_tokens=3584 avail_mem=100.08 GB):  17%|█▋        | 10/58 [00:03<00:11,  4.02it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=100.08 GB):  19%|█▉        | 11/58 [00:03<00:10,  4.41it/s]Capturing num tokens (num_tokens=3328 avail_mem=100.03 GB):  19%|█▉        | 11/58 [00:03<00:10,  4.41it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=100.03 GB):  21%|██        | 12/58 [00:04<00:11,  4.12it/s]Capturing num tokens (num_tokens=3072 avail_mem=102.68 GB):  21%|██        | 12/58 [00:04<00:11,  4.12it/s]Capturing num tokens (num_tokens=3072 avail_mem=102.68 GB):  22%|██▏       | 13/58 [00:04<00:09,  4.69it/s]Capturing num tokens (num_tokens=2816 avail_mem=102.67 GB):  22%|██▏       | 13/58 [00:04<00:09,  4.69it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=102.67 GB):  24%|██▍       | 14/58 [00:04<00:08,  5.32it/s]Capturing num tokens (num_tokens=2560 avail_mem=102.67 GB):  24%|██▍       | 14/58 [00:04<00:08,  5.32it/s]Capturing num tokens (num_tokens=2560 avail_mem=102.67 GB):  26%|██▌       | 15/58 [00:04<00:07,  5.99it/s]Capturing num tokens (num_tokens=2304 avail_mem=102.66 GB):  26%|██▌       | 15/58 [00:04<00:07,  5.99it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=102.66 GB):  28%|██▊       | 16/58 [00:04<00:06,  6.64it/s]Capturing num tokens (num_tokens=2048 avail_mem=102.66 GB):  28%|██▊       | 16/58 [00:04<00:06,  6.64it/s]Capturing num tokens (num_tokens=1792 avail_mem=102.65 GB):  28%|██▊       | 16/58 [00:04<00:06,  6.64it/s]Capturing num tokens (num_tokens=1792 avail_mem=102.65 GB):  31%|███       | 18/58 [00:04<00:04,  8.26it/s]Capturing num tokens (num_tokens=1536 avail_mem=102.65 GB):  31%|███       | 18/58 [00:04<00:04,  8.26it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=102.65 GB):  31%|███       | 18/58 [00:04<00:04,  8.26it/s]Capturing num tokens (num_tokens=1280 avail_mem=102.65 GB):  34%|███▍      | 20/58 [00:04<00:03,  9.87it/s]Capturing num tokens (num_tokens=1024 avail_mem=102.64 GB):  34%|███▍      | 20/58 [00:04<00:03,  9.87it/s]Capturing num tokens (num_tokens=960 avail_mem=102.63 GB):  34%|███▍      | 20/58 [00:04<00:03,  9.87it/s] Capturing num tokens (num_tokens=960 avail_mem=102.63 GB):  38%|███▊      | 22/58 [00:05<00:03, 11.66it/s]Capturing num tokens (num_tokens=896 avail_mem=102.62 GB):  38%|███▊      | 22/58 [00:05<00:03, 11.66it/s]

    Capturing num tokens (num_tokens=832 avail_mem=102.62 GB):  38%|███▊      | 22/58 [00:05<00:03, 11.66it/s]Capturing num tokens (num_tokens=832 avail_mem=102.62 GB):  41%|████▏     | 24/58 [00:05<00:02, 13.41it/s]Capturing num tokens (num_tokens=768 avail_mem=102.61 GB):  41%|████▏     | 24/58 [00:05<00:02, 13.41it/s]Capturing num tokens (num_tokens=704 avail_mem=102.60 GB):  41%|████▏     | 24/58 [00:05<00:02, 13.41it/s]Capturing num tokens (num_tokens=640 avail_mem=102.59 GB):  41%|████▏     | 24/58 [00:05<00:02, 13.41it/s]Capturing num tokens (num_tokens=640 avail_mem=102.59 GB):  47%|████▋     | 27/58 [00:05<00:01, 16.11it/s]Capturing num tokens (num_tokens=576 avail_mem=102.58 GB):  47%|████▋     | 27/58 [00:05<00:01, 16.11it/s]

    Capturing num tokens (num_tokens=512 avail_mem=102.58 GB):  47%|████▋     | 27/58 [00:05<00:01, 16.11it/s]Capturing num tokens (num_tokens=480 avail_mem=102.57 GB):  47%|████▋     | 27/58 [00:05<00:01, 16.11it/s]Capturing num tokens (num_tokens=480 avail_mem=102.57 GB):  52%|█████▏    | 30/58 [00:05<00:01, 19.40it/s]Capturing num tokens (num_tokens=448 avail_mem=102.57 GB):  52%|█████▏    | 30/58 [00:05<00:01, 19.40it/s]Capturing num tokens (num_tokens=416 avail_mem=102.56 GB):  52%|█████▏    | 30/58 [00:05<00:01, 19.40it/s]Capturing num tokens (num_tokens=384 avail_mem=102.56 GB):  52%|█████▏    | 30/58 [00:05<00:01, 19.40it/s]Capturing num tokens (num_tokens=352 avail_mem=102.56 GB):  52%|█████▏    | 30/58 [00:05<00:01, 19.40it/s]Capturing num tokens (num_tokens=352 avail_mem=102.56 GB):  59%|█████▊    | 34/58 [00:05<00:01, 23.01it/s]Capturing num tokens (num_tokens=320 avail_mem=102.55 GB):  59%|█████▊    | 34/58 [00:05<00:01, 23.01it/s]

    Capturing num tokens (num_tokens=288 avail_mem=102.55 GB):  59%|█████▊    | 34/58 [00:05<00:01, 23.01it/s]Capturing num tokens (num_tokens=256 avail_mem=102.54 GB):  59%|█████▊    | 34/58 [00:05<00:01, 23.01it/s]Capturing num tokens (num_tokens=240 avail_mem=102.54 GB):  59%|█████▊    | 34/58 [00:05<00:01, 23.01it/s]Capturing num tokens (num_tokens=240 avail_mem=102.54 GB):  66%|██████▌   | 38/58 [00:05<00:00, 26.29it/s]Capturing num tokens (num_tokens=224 avail_mem=102.53 GB):  66%|██████▌   | 38/58 [00:05<00:00, 26.29it/s]Capturing num tokens (num_tokens=208 avail_mem=102.53 GB):  66%|██████▌   | 38/58 [00:05<00:00, 26.29it/s]Capturing num tokens (num_tokens=192 avail_mem=102.53 GB):  66%|██████▌   | 38/58 [00:05<00:00, 26.29it/s]Capturing num tokens (num_tokens=176 avail_mem=102.52 GB):  66%|██████▌   | 38/58 [00:05<00:00, 26.29it/s]

    Capturing num tokens (num_tokens=176 avail_mem=102.52 GB):  72%|███████▏  | 42/58 [00:05<00:00, 28.49it/s]Capturing num tokens (num_tokens=160 avail_mem=102.52 GB):  72%|███████▏  | 42/58 [00:05<00:00, 28.49it/s]Capturing num tokens (num_tokens=144 avail_mem=102.51 GB):  72%|███████▏  | 42/58 [00:05<00:00, 28.49it/s]Capturing num tokens (num_tokens=128 avail_mem=102.51 GB):  72%|███████▏  | 42/58 [00:05<00:00, 28.49it/s]Capturing num tokens (num_tokens=112 avail_mem=102.52 GB):  72%|███████▏  | 42/58 [00:05<00:00, 28.49it/s]Capturing num tokens (num_tokens=112 avail_mem=102.52 GB):  79%|███████▉  | 46/58 [00:05<00:00, 29.42it/s]Capturing num tokens (num_tokens=96 avail_mem=102.52 GB):  79%|███████▉  | 46/58 [00:05<00:00, 29.42it/s] Capturing num tokens (num_tokens=80 avail_mem=102.51 GB):  79%|███████▉  | 46/58 [00:05<00:00, 29.42it/s]

    Capturing num tokens (num_tokens=64 avail_mem=102.51 GB):  79%|███████▉  | 46/58 [00:06<00:00, 29.42it/s]Capturing num tokens (num_tokens=64 avail_mem=102.51 GB):  84%|████████▍ | 49/58 [00:06<00:00, 22.93it/s]Capturing num tokens (num_tokens=48 avail_mem=102.50 GB):  84%|████████▍ | 49/58 [00:06<00:00, 22.93it/s]Capturing num tokens (num_tokens=32 avail_mem=102.50 GB):  84%|████████▍ | 49/58 [00:06<00:00, 22.93it/s]Capturing num tokens (num_tokens=28 avail_mem=102.49 GB):  84%|████████▍ | 49/58 [00:06<00:00, 22.93it/s]Capturing num tokens (num_tokens=28 avail_mem=102.49 GB):  90%|████████▉ | 52/58 [00:06<00:00, 23.73it/s]Capturing num tokens (num_tokens=24 avail_mem=102.49 GB):  90%|████████▉ | 52/58 [00:06<00:00, 23.73it/s]Capturing num tokens (num_tokens=20 avail_mem=102.48 GB):  90%|████████▉ | 52/58 [00:06<00:00, 23.73it/s]

    Capturing num tokens (num_tokens=16 avail_mem=102.48 GB):  90%|████████▉ | 52/58 [00:06<00:00, 23.73it/s]Capturing num tokens (num_tokens=12 avail_mem=102.48 GB):  90%|████████▉ | 52/58 [00:06<00:00, 23.73it/s]Capturing num tokens (num_tokens=12 avail_mem=102.48 GB):  97%|█████████▋| 56/58 [00:06<00:00, 26.55it/s]Capturing num tokens (num_tokens=8 avail_mem=102.47 GB):  97%|█████████▋| 56/58 [00:06<00:00, 26.55it/s] Capturing num tokens (num_tokens=4 avail_mem=102.47 GB):  97%|█████████▋| 56/58 [00:06<00:00, 26.55it/s]Capturing num tokens (num_tokens=4 avail_mem=102.47 GB): 100%|██████████| 58/58 [00:06<00:00,  9.13it/s]


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


<strong style='color: #00008B;'>{'text': '{"name": "Paris", "population": 2147000}', 'output_ids': [5018, 609, 794, 330, 60704, 498, 330, 45541, 794, 220, 11584, 7007, 15, 92, 128009], 'meta_info': {'id': 'baa530700f4044b6bc9d6bd94a927b33', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 50, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 15, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.17326663294807076, 'response_sent_to_client_ts': 1774944029.2178888}}</strong>



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


<strong style='color: #00008B;'>{'text': '{"name": "Paris", "population": 2147000}', 'output_ids': [5018, 609, 794, 330, 60704, 498, 330, 45541, 794, 220, 11584, 7007, 15, 92, 128009], 'meta_info': {'id': '3b75b49194644a519b24ef5aa28df152', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 50, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 15, 'cached_tokens': 49, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.17878921004012227, 'response_sent_to_client_ts': 1774944029.4084153}}</strong>


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


<strong style='color: #00008B;'>[{'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': '9d37bfbf3d9b4732876177c379919e63', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.09184642508625984, 'response_sent_to_client_ts': 1774944029.5377085}}, {'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': '38b60332457e4fbc81a7f49470211a4f', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.09176596580073237, 'response_sent_to_client_ts': 1774944029.537719}}, {'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': '88aa8a013aba44e280b2c244fd6063f5', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.09170269221067429, 'response_sent_to_client_ts': 1774944029.5377235}}]</strong>


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


<strong style='color: #00008B;'>{'text': 'France', 'output_ids': [50100, 128009], 'meta_info': {'id': 'd2db4619037a4f8e94d42f6026d5e9a8', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2, 'cached_tokens': 31, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.03762822970747948, 'response_sent_to_client_ts': 1774944029.584682}}</strong>


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


<strong style='color: #00008B;'>{'text': 'Paris is the capital of France.', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 13, 128009], 'meta_info': {'id': 'e134fe9aaf59420da55fb2f6b45e9873', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 8, 'cached_tokens': 40, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.12507785577327013, 'response_sent_to_client_ts': 1774944030.9819353}}</strong>



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


<strong style='color: #00008B;'>{'text': 'France.', 'output_ids': [50100, 13, 128009], 'meta_info': {'id': 'f330425f0c9e42bea815fb800a139e38', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 3, 'cached_tokens': 40, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.050561164040118456, 'response_sent_to_client_ts': 1774944031.041216}}</strong>



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


    2026-03-31 08:00:44.498 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 08:00:44] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 08:00:44.498 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 08:00:44] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 08:00:44.498 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 08:00:44] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 08:00:44.498 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 08:00:44] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 08:00:44.498 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 08:00:44] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]Loading safetensors checkpoint shards:  25% Completed | 1/4 [00:00<00:00,  6.89it/s]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:00<00:00,  2.18it/s]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:01<00:00,  1.61it/s]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.39it/s]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.60it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:10,  3.34s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:10,  3.34s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:46,  1.90s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:46,  1.90s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:07,  1.23s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:07,  1.23s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:05<00:48,  1.11it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:05<00:48,  1.11it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:37,  1.41it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:37,  1.41it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:30,  1.73it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:30,  1.73it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:06<00:24,  2.07it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:06<00:24,  2.07it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:20,  2.41it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:20,  2.41it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:17,  2.79it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:17,  2.79it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:14,  3.20it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:14,  3.20it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:13,  3.55it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:13,  3.55it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:07<00:11,  3.98it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:07<00:11,  3.98it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:10,  4.42it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:10,  4.42it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:09,  4.87it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:09,  4.87it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:07,  5.39it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:07,  5.39it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:07<00:06,  6.01it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:07<00:06,  6.01it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:07<00:06,  6.57it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:07<00:06,  6.57it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:07<00:06,  6.57it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:08<00:04,  8.00it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:08<00:04,  8.00it/s]

    Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:08<00:04,  8.00it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:08<00:03,  9.71it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:08<00:03,  9.71it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:08<00:03,  9.71it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:08<00:03, 11.60it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:08<00:03, 11.60it/s]

    Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:08<00:03, 11.60it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:08<00:03, 11.60it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:08<00:02, 14.35it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:08<00:02, 14.35it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:08<00:02, 14.35it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:08<00:02, 14.35it/s]

    Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:08<00:01, 16.51it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:08<00:01, 16.51it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:08<00:01, 16.51it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:08<00:01, 16.51it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:08<00:01, 18.66it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:08<00:01, 18.66it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:08<00:01, 18.66it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:08<00:01, 18.66it/s]

    Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:08<00:01, 21.00it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:08<00:01, 21.00it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:08<00:01, 21.00it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:08<00:01, 21.00it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:08<00:01, 21.00it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:08<00:00, 24.10it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:08<00:00, 24.10it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:08<00:00, 24.10it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:08<00:00, 24.10it/s]

    Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:09<00:00, 24.10it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:09<00:00, 26.82it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:09<00:00, 26.82it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:09<00:00, 26.82it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:09<00:00, 26.82it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:09<00:00, 26.82it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:09<00:00, 29.18it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:09<00:00, 29.18it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:09<00:00, 29.18it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:09<00:00, 29.18it/s]

    Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:09<00:00, 29.18it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:09<00:00, 31.44it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:09<00:00, 31.44it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:09<00:00, 31.44it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:09<00:00, 31.44it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:09<00:00, 31.44it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:09<00:00, 31.44it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:09<00:00, 34.64it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:09<00:00, 34.64it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:09<00:00, 34.64it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00,  6.15it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=83.27 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=83.27 GB):   2%|▏         | 1/58 [00:00<00:39,  1.44it/s]Capturing num tokens (num_tokens=7680 avail_mem=83.23 GB):   2%|▏         | 1/58 [00:00<00:39,  1.44it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=83.23 GB):   3%|▎         | 2/58 [00:01<00:37,  1.49it/s]Capturing num tokens (num_tokens=7168 avail_mem=83.24 GB):   3%|▎         | 2/58 [00:01<00:37,  1.49it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=83.24 GB):   5%|▌         | 3/58 [00:01<00:33,  1.65it/s]Capturing num tokens (num_tokens=6656 avail_mem=83.24 GB):   5%|▌         | 3/58 [00:01<00:33,  1.65it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=83.24 GB):   7%|▋         | 4/58 [00:02<00:30,  1.76it/s]Capturing num tokens (num_tokens=6144 avail_mem=83.24 GB):   7%|▋         | 4/58 [00:02<00:30,  1.76it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=83.24 GB):   9%|▊         | 5/58 [00:02<00:28,  1.89it/s]Capturing num tokens (num_tokens=5632 avail_mem=83.23 GB):   9%|▊         | 5/58 [00:02<00:28,  1.89it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=83.23 GB):  10%|█         | 6/58 [00:03<00:25,  2.03it/s]Capturing num tokens (num_tokens=5120 avail_mem=101.95 GB):  10%|█         | 6/58 [00:03<00:25,  2.03it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=101.95 GB):  12%|█▏        | 7/58 [00:03<00:23,  2.20it/s]Capturing num tokens (num_tokens=4608 avail_mem=101.95 GB):  12%|█▏        | 7/58 [00:03<00:23,  2.20it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=101.95 GB):  14%|█▍        | 8/58 [00:03<00:20,  2.41it/s]Capturing num tokens (num_tokens=4096 avail_mem=101.96 GB):  14%|█▍        | 8/58 [00:03<00:20,  2.41it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=101.96 GB):  16%|█▌        | 9/58 [00:04<00:18,  2.61it/s]Capturing num tokens (num_tokens=3840 avail_mem=101.96 GB):  16%|█▌        | 9/58 [00:04<00:18,  2.61it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=101.96 GB):  17%|█▋        | 10/58 [00:04<00:17,  2.82it/s]Capturing num tokens (num_tokens=3584 avail_mem=101.96 GB):  17%|█▋        | 10/58 [00:04<00:17,  2.82it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=101.96 GB):  19%|█▉        | 11/58 [00:04<00:15,  3.04it/s]Capturing num tokens (num_tokens=3328 avail_mem=101.96 GB):  19%|█▉        | 11/58 [00:04<00:15,  3.04it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=101.96 GB):  21%|██        | 12/58 [00:05<00:14,  3.28it/s]Capturing num tokens (num_tokens=3072 avail_mem=101.96 GB):  21%|██        | 12/58 [00:05<00:14,  3.28it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=101.96 GB):  22%|██▏       | 13/58 [00:05<00:12,  3.56it/s]Capturing num tokens (num_tokens=2816 avail_mem=101.96 GB):  22%|██▏       | 13/58 [00:05<00:12,  3.56it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=101.96 GB):  24%|██▍       | 14/58 [00:05<00:11,  3.88it/s]Capturing num tokens (num_tokens=2560 avail_mem=101.96 GB):  24%|██▍       | 14/58 [00:05<00:11,  3.88it/s]Capturing num tokens (num_tokens=2560 avail_mem=101.96 GB):  26%|██▌       | 15/58 [00:05<00:10,  4.23it/s]Capturing num tokens (num_tokens=2304 avail_mem=101.96 GB):  26%|██▌       | 15/58 [00:05<00:10,  4.23it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=101.96 GB):  28%|██▊       | 16/58 [00:05<00:09,  4.56it/s]Capturing num tokens (num_tokens=2048 avail_mem=101.96 GB):  28%|██▊       | 16/58 [00:05<00:09,  4.56it/s]Capturing num tokens (num_tokens=2048 avail_mem=101.96 GB):  29%|██▉       | 17/58 [00:06<00:07,  5.13it/s]Capturing num tokens (num_tokens=1792 avail_mem=101.96 GB):  29%|██▉       | 17/58 [00:06<00:07,  5.13it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=101.96 GB):  31%|███       | 18/58 [00:06<00:07,  5.70it/s]Capturing num tokens (num_tokens=1536 avail_mem=101.95 GB):  31%|███       | 18/58 [00:06<00:07,  5.70it/s]Capturing num tokens (num_tokens=1536 avail_mem=101.95 GB):  33%|███▎      | 19/58 [00:06<00:06,  6.32it/s]Capturing num tokens (num_tokens=1280 avail_mem=101.96 GB):  33%|███▎      | 19/58 [00:06<00:06,  6.32it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=101.96 GB):  34%|███▍      | 20/58 [00:06<00:05,  7.05it/s]Capturing num tokens (num_tokens=1024 avail_mem=101.96 GB):  34%|███▍      | 20/58 [00:06<00:05,  7.05it/s]Capturing num tokens (num_tokens=960 avail_mem=101.95 GB):  34%|███▍      | 20/58 [00:06<00:05,  7.05it/s] Capturing num tokens (num_tokens=960 avail_mem=101.95 GB):  38%|███▊      | 22/58 [00:06<00:04,  8.47it/s]Capturing num tokens (num_tokens=896 avail_mem=101.95 GB):  38%|███▊      | 22/58 [00:06<00:04,  8.47it/s]

    Capturing num tokens (num_tokens=832 avail_mem=101.95 GB):  38%|███▊      | 22/58 [00:06<00:04,  8.47it/s]Capturing num tokens (num_tokens=832 avail_mem=101.95 GB):  41%|████▏     | 24/58 [00:06<00:03,  9.42it/s]Capturing num tokens (num_tokens=768 avail_mem=101.94 GB):  41%|████▏     | 24/58 [00:06<00:03,  9.42it/s]Capturing num tokens (num_tokens=704 avail_mem=101.94 GB):  41%|████▏     | 24/58 [00:06<00:03,  9.42it/s]

    Capturing num tokens (num_tokens=704 avail_mem=101.94 GB):  45%|████▍     | 26/58 [00:06<00:03, 10.34it/s]Capturing num tokens (num_tokens=640 avail_mem=101.93 GB):  45%|████▍     | 26/58 [00:06<00:03, 10.34it/s]Capturing num tokens (num_tokens=576 avail_mem=101.93 GB):  45%|████▍     | 26/58 [00:06<00:03, 10.34it/s]Capturing num tokens (num_tokens=576 avail_mem=101.93 GB):  48%|████▊     | 28/58 [00:07<00:02, 11.36it/s]Capturing num tokens (num_tokens=512 avail_mem=101.92 GB):  48%|████▊     | 28/58 [00:07<00:02, 11.36it/s]

    Capturing num tokens (num_tokens=480 avail_mem=101.92 GB):  48%|████▊     | 28/58 [00:07<00:02, 11.36it/s]Capturing num tokens (num_tokens=480 avail_mem=101.92 GB):  52%|█████▏    | 30/58 [00:07<00:02, 12.40it/s]Capturing num tokens (num_tokens=448 avail_mem=101.91 GB):  52%|█████▏    | 30/58 [00:07<00:02, 12.40it/s]Capturing num tokens (num_tokens=416 avail_mem=101.91 GB):  52%|█████▏    | 30/58 [00:07<00:02, 12.40it/s]

    Capturing num tokens (num_tokens=416 avail_mem=101.91 GB):  55%|█████▌    | 32/58 [00:07<00:01, 13.06it/s]Capturing num tokens (num_tokens=384 avail_mem=101.91 GB):  55%|█████▌    | 32/58 [00:07<00:01, 13.06it/s]Capturing num tokens (num_tokens=352 avail_mem=101.90 GB):  55%|█████▌    | 32/58 [00:07<00:01, 13.06it/s]Capturing num tokens (num_tokens=352 avail_mem=101.90 GB):  59%|█████▊    | 34/58 [00:07<00:01, 14.02it/s]Capturing num tokens (num_tokens=320 avail_mem=101.90 GB):  59%|█████▊    | 34/58 [00:07<00:01, 14.02it/s]Capturing num tokens (num_tokens=288 avail_mem=101.89 GB):  59%|█████▊    | 34/58 [00:07<00:01, 14.02it/s]

    Capturing num tokens (num_tokens=288 avail_mem=101.89 GB):  62%|██████▏   | 36/58 [00:07<00:01, 15.05it/s]Capturing num tokens (num_tokens=256 avail_mem=101.89 GB):  62%|██████▏   | 36/58 [00:07<00:01, 15.05it/s]Capturing num tokens (num_tokens=240 avail_mem=101.89 GB):  62%|██████▏   | 36/58 [00:07<00:01, 15.05it/s]Capturing num tokens (num_tokens=240 avail_mem=101.89 GB):  66%|██████▌   | 38/58 [00:07<00:01, 16.17it/s]Capturing num tokens (num_tokens=224 avail_mem=101.88 GB):  66%|██████▌   | 38/58 [00:07<00:01, 16.17it/s]Capturing num tokens (num_tokens=208 avail_mem=101.88 GB):  66%|██████▌   | 38/58 [00:07<00:01, 16.17it/s]

    Capturing num tokens (num_tokens=192 avail_mem=101.87 GB):  66%|██████▌   | 38/58 [00:07<00:01, 16.17it/s]Capturing num tokens (num_tokens=192 avail_mem=101.87 GB):  71%|███████   | 41/58 [00:07<00:00, 17.60it/s]Capturing num tokens (num_tokens=176 avail_mem=101.87 GB):  71%|███████   | 41/58 [00:07<00:00, 17.60it/s]Capturing num tokens (num_tokens=160 avail_mem=101.86 GB):  71%|███████   | 41/58 [00:07<00:00, 17.60it/s]Capturing num tokens (num_tokens=144 avail_mem=101.86 GB):  71%|███████   | 41/58 [00:07<00:00, 17.60it/s]Capturing num tokens (num_tokens=144 avail_mem=101.86 GB):  76%|███████▌  | 44/58 [00:07<00:00, 18.76it/s]Capturing num tokens (num_tokens=128 avail_mem=101.85 GB):  76%|███████▌  | 44/58 [00:07<00:00, 18.76it/s]

    Capturing num tokens (num_tokens=112 avail_mem=101.86 GB):  76%|███████▌  | 44/58 [00:07<00:00, 18.76it/s]Capturing num tokens (num_tokens=96 avail_mem=101.86 GB):  76%|███████▌  | 44/58 [00:08<00:00, 18.76it/s] Capturing num tokens (num_tokens=96 avail_mem=101.86 GB):  81%|████████  | 47/58 [00:08<00:00, 20.28it/s]Capturing num tokens (num_tokens=80 avail_mem=101.86 GB):  81%|████████  | 47/58 [00:08<00:00, 20.28it/s]Capturing num tokens (num_tokens=64 avail_mem=101.85 GB):  81%|████████  | 47/58 [00:08<00:00, 20.28it/s]Capturing num tokens (num_tokens=48 avail_mem=101.85 GB):  81%|████████  | 47/58 [00:08<00:00, 20.28it/s]

    Capturing num tokens (num_tokens=48 avail_mem=101.85 GB):  86%|████████▌ | 50/58 [00:08<00:00, 21.36it/s]Capturing num tokens (num_tokens=32 avail_mem=101.85 GB):  86%|████████▌ | 50/58 [00:08<00:00, 21.36it/s]Capturing num tokens (num_tokens=28 avail_mem=101.84 GB):  86%|████████▌ | 50/58 [00:08<00:00, 21.36it/s]Capturing num tokens (num_tokens=24 avail_mem=101.83 GB):  86%|████████▌ | 50/58 [00:08<00:00, 21.36it/s]Capturing num tokens (num_tokens=24 avail_mem=101.83 GB):  91%|█████████▏| 53/58 [00:08<00:00, 21.70it/s]Capturing num tokens (num_tokens=20 avail_mem=101.83 GB):  91%|█████████▏| 53/58 [00:08<00:00, 21.70it/s]Capturing num tokens (num_tokens=16 avail_mem=101.83 GB):  91%|█████████▏| 53/58 [00:08<00:00, 21.70it/s]

    Capturing num tokens (num_tokens=12 avail_mem=101.82 GB):  91%|█████████▏| 53/58 [00:08<00:00, 21.70it/s]Capturing num tokens (num_tokens=12 avail_mem=101.82 GB):  97%|█████████▋| 56/58 [00:08<00:00, 22.12it/s]Capturing num tokens (num_tokens=8 avail_mem=101.82 GB):  97%|█████████▋| 56/58 [00:08<00:00, 22.12it/s] Capturing num tokens (num_tokens=4 avail_mem=101.81 GB):  97%|█████████▋| 56/58 [00:08<00:00, 22.12it/s]Capturing num tokens (num_tokens=4 avail_mem=101.81 GB): 100%|██████████| 58/58 [00:08<00:00,  6.80it/s]


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



<strong style='color: #00008B;'>Prompt: <|begin_of_text|><|start_header_id|>system<|end_header_id|><br><br>Cutting Knowledge Date: December 2023<br>Today Date: 26 Jul 2024<br><br><|eot_id|><|start_header_id|>user<|end_header_id|><br><br>Paris is the capital of<|eot_id|><|start_header_id|>assistant<|end_header_id|><br><br><br>Generated text: Paris is the capital of France.</strong>



```python
llm.shutdown()
```
