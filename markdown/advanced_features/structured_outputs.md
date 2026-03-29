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


    [2026-03-29 15:05:53] numactl command not found, skipping NUMA node configuration for GPU. Install numactl (e.g., apt-get install numactl) to enable automatic NUMA binding.


    [2026-03-29 15:06:00] numactl command not found, skipping NUMA node configuration for GPU. Install numactl (e.g., apt-get install numactl) to enable automatic NUMA binding.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-03-29 15:06:04.724 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 15:06:04] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 15:06:04.724 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 15:06:04] Persistent cache disabled, using in-memory JIT cache


    2026-03-29 15:06:04.724 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 15:06:04] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 15:06:04.995 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 15:06:04] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 15:06:04.995 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 15:06:04] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  25% Completed | 1/4 [00:00<00:00,  3.86it/s]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:01<00:01,  1.32it/s]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:02<00:01,  1.15s/it]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:04<00:00,  1.33s/it]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:04<00:00,  1.15s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:15,  3.43s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:15,  3.43s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:40,  1.80s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:40,  1.80s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:04,  1.17s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:04,  1.17s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:47,  1.14it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:47,  1.14it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:37,  1.40it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:37,  1.40it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:30,  1.70it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:30,  1.70it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:06<00:25,  2.00it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:06<00:25,  2.00it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:21,  2.31it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:21,  2.31it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:18,  2.70it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:18,  2.70it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:15,  3.14it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:15,  3.14it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:13,  3.48it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:13,  3.48it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:07<00:11,  4.01it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:07<00:11,  4.01it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:10,  4.32it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:10,  4.32it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:08,  5.04it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:08,  5.04it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:08,  5.36it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:08,  5.36it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:07<00:06,  6.23it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:07<00:06,  6.23it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:07<00:05,  6.86it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:07<00:05,  6.86it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:07<00:05,  7.43it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:07<00:05,  7.43it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:07<00:05,  7.43it/s]

    Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:08<00:03,  9.64it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:08<00:03,  9.64it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:08<00:03,  9.64it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:08<00:03, 11.70it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:08<00:03, 11.70it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:08<00:03, 11.70it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:08<00:03, 11.70it/s]

    Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:08<00:02, 14.79it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:08<00:02, 14.79it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:08<00:02, 14.79it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:08<00:02, 14.79it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:08<00:01, 17.94it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:08<00:01, 17.94it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:08<00:01, 17.94it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:08<00:01, 17.94it/s]

    Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:08<00:01, 20.42it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:08<00:01, 20.42it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:08<00:01, 20.42it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:08<00:01, 20.42it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:08<00:01, 20.42it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:08<00:00, 23.25it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:08<00:00, 23.25it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:08<00:00, 23.25it/s]

    Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:08<00:00, 23.25it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:08<00:00, 23.25it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:08<00:00, 26.05it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:08<00:00, 26.05it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:08<00:00, 26.05it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:08<00:00, 26.05it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:08<00:00, 26.05it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:08<00:00, 28.41it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:08<00:00, 28.41it/s]

    Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:08<00:00, 28.41it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:08<00:00, 28.41it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:08<00:00, 28.41it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:09<00:00, 30.54it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:09<00:00, 30.54it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:09<00:00, 30.54it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:09<00:00, 30.54it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:09<00:00, 30.54it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:09<00:00, 32.19it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:09<00:00, 32.19it/s]

    Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:09<00:00, 32.19it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:09<00:00, 32.19it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:09<00:00, 32.19it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:09<00:00, 32.19it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:09<00:00, 32.19it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:09<00:00, 37.65it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:09<00:00, 37.65it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00,  6.25it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=89.15 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=89.15 GB):   2%|▏         | 1/58 [00:00<00:37,  1.52it/s]Capturing num tokens (num_tokens=7680 avail_mem=89.10 GB):   2%|▏         | 1/58 [00:00<00:37,  1.52it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=89.10 GB):   3%|▎         | 2/58 [00:01<00:33,  1.65it/s]Capturing num tokens (num_tokens=7168 avail_mem=89.08 GB):   3%|▎         | 2/58 [00:01<00:33,  1.65it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=89.08 GB):   5%|▌         | 3/58 [00:01<00:30,  1.82it/s]Capturing num tokens (num_tokens=6656 avail_mem=98.16 GB):   5%|▌         | 3/58 [00:01<00:30,  1.82it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=98.16 GB):   7%|▋         | 4/58 [00:02<00:25,  2.16it/s]Capturing num tokens (num_tokens=6144 avail_mem=98.14 GB):   7%|▋         | 4/58 [00:02<00:25,  2.16it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=98.14 GB):   9%|▊         | 5/58 [00:02<00:21,  2.52it/s]Capturing num tokens (num_tokens=5632 avail_mem=98.14 GB):   9%|▊         | 5/58 [00:02<00:21,  2.52it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=98.14 GB):  10%|█         | 6/58 [00:02<00:18,  2.86it/s]Capturing num tokens (num_tokens=5120 avail_mem=98.13 GB):  10%|█         | 6/58 [00:02<00:18,  2.86it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=98.13 GB):  12%|█▏        | 7/58 [00:02<00:15,  3.21it/s]Capturing num tokens (num_tokens=4608 avail_mem=98.12 GB):  12%|█▏        | 7/58 [00:02<00:15,  3.21it/s]Capturing num tokens (num_tokens=4608 avail_mem=98.12 GB):  14%|█▍        | 8/58 [00:02<00:13,  3.69it/s]Capturing num tokens (num_tokens=4096 avail_mem=98.12 GB):  14%|█▍        | 8/58 [00:02<00:13,  3.69it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=98.12 GB):  16%|█▌        | 9/58 [00:03<00:11,  4.29it/s]Capturing num tokens (num_tokens=3840 avail_mem=98.12 GB):  16%|█▌        | 9/58 [00:03<00:11,  4.29it/s]Capturing num tokens (num_tokens=3840 avail_mem=98.12 GB):  17%|█▋        | 10/58 [00:03<00:09,  4.87it/s]Capturing num tokens (num_tokens=3584 avail_mem=98.13 GB):  17%|█▋        | 10/58 [00:03<00:09,  4.87it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=98.13 GB):  19%|█▉        | 11/58 [00:03<00:08,  5.49it/s]Capturing num tokens (num_tokens=3328 avail_mem=98.12 GB):  19%|█▉        | 11/58 [00:03<00:08,  5.49it/s]Capturing num tokens (num_tokens=3328 avail_mem=98.12 GB):  21%|██        | 12/58 [00:03<00:07,  6.11it/s]Capturing num tokens (num_tokens=3072 avail_mem=98.12 GB):  21%|██        | 12/58 [00:03<00:07,  6.11it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=98.12 GB):  22%|██▏       | 13/58 [00:03<00:06,  6.75it/s]Capturing num tokens (num_tokens=2816 avail_mem=98.12 GB):  22%|██▏       | 13/58 [00:03<00:06,  6.75it/s]Capturing num tokens (num_tokens=2816 avail_mem=98.12 GB):  24%|██▍       | 14/58 [00:03<00:05,  7.49it/s]Capturing num tokens (num_tokens=2560 avail_mem=98.12 GB):  24%|██▍       | 14/58 [00:03<00:05,  7.49it/s]Capturing num tokens (num_tokens=2304 avail_mem=98.12 GB):  24%|██▍       | 14/58 [00:03<00:05,  7.49it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=98.12 GB):  28%|██▊       | 16/58 [00:03<00:04,  8.86it/s]Capturing num tokens (num_tokens=2048 avail_mem=98.12 GB):  28%|██▊       | 16/58 [00:03<00:04,  8.86it/s]Capturing num tokens (num_tokens=2048 avail_mem=98.12 GB):  29%|██▉       | 17/58 [00:04<00:04,  8.32it/s]Capturing num tokens (num_tokens=1792 avail_mem=102.61 GB):  29%|██▉       | 17/58 [00:04<00:04,  8.32it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=102.61 GB):  29%|██▉       | 17/58 [00:04<00:04,  8.32it/s]Capturing num tokens (num_tokens=1536 avail_mem=102.61 GB):  33%|███▎      | 19/58 [00:04<00:03, 10.53it/s]Capturing num tokens (num_tokens=1280 avail_mem=102.61 GB):  33%|███▎      | 19/58 [00:04<00:03, 10.53it/s]Capturing num tokens (num_tokens=1024 avail_mem=102.61 GB):  33%|███▎      | 19/58 [00:04<00:03, 10.53it/s]Capturing num tokens (num_tokens=960 avail_mem=102.61 GB):  33%|███▎      | 19/58 [00:04<00:03, 10.53it/s] Capturing num tokens (num_tokens=960 avail_mem=102.61 GB):  38%|███▊      | 22/58 [00:04<00:02, 14.04it/s]Capturing num tokens (num_tokens=896 avail_mem=102.60 GB):  38%|███▊      | 22/58 [00:04<00:02, 14.04it/s]

    Capturing num tokens (num_tokens=832 avail_mem=102.60 GB):  38%|███▊      | 22/58 [00:04<00:02, 14.04it/s]Capturing num tokens (num_tokens=768 avail_mem=102.60 GB):  38%|███▊      | 22/58 [00:04<00:02, 14.04it/s]Capturing num tokens (num_tokens=768 avail_mem=102.60 GB):  43%|████▎     | 25/58 [00:04<00:01, 17.01it/s]Capturing num tokens (num_tokens=704 avail_mem=102.59 GB):  43%|████▎     | 25/58 [00:04<00:01, 17.01it/s]Capturing num tokens (num_tokens=640 avail_mem=102.59 GB):  43%|████▎     | 25/58 [00:04<00:01, 17.01it/s]Capturing num tokens (num_tokens=576 avail_mem=102.58 GB):  43%|████▎     | 25/58 [00:04<00:01, 17.01it/s]Capturing num tokens (num_tokens=576 avail_mem=102.58 GB):  48%|████▊     | 28/58 [00:04<00:01, 19.80it/s]Capturing num tokens (num_tokens=512 avail_mem=102.58 GB):  48%|████▊     | 28/58 [00:04<00:01, 19.80it/s]

    Capturing num tokens (num_tokens=480 avail_mem=102.57 GB):  48%|████▊     | 28/58 [00:04<00:01, 19.80it/s]Capturing num tokens (num_tokens=448 avail_mem=102.57 GB):  48%|████▊     | 28/58 [00:04<00:01, 19.80it/s]Capturing num tokens (num_tokens=448 avail_mem=102.57 GB):  53%|█████▎    | 31/58 [00:04<00:01, 22.31it/s]Capturing num tokens (num_tokens=416 avail_mem=102.56 GB):  53%|█████▎    | 31/58 [00:04<00:01, 22.31it/s]Capturing num tokens (num_tokens=384 avail_mem=102.56 GB):  53%|█████▎    | 31/58 [00:04<00:01, 22.31it/s]Capturing num tokens (num_tokens=352 avail_mem=102.56 GB):  53%|█████▎    | 31/58 [00:04<00:01, 22.31it/s]Capturing num tokens (num_tokens=320 avail_mem=102.55 GB):  53%|█████▎    | 31/58 [00:04<00:01, 22.31it/s]Capturing num tokens (num_tokens=320 avail_mem=102.55 GB):  60%|██████    | 35/58 [00:04<00:00, 25.48it/s]Capturing num tokens (num_tokens=288 avail_mem=102.55 GB):  60%|██████    | 35/58 [00:04<00:00, 25.48it/s]

    Capturing num tokens (num_tokens=256 avail_mem=102.54 GB):  60%|██████    | 35/58 [00:04<00:00, 25.48it/s]Capturing num tokens (num_tokens=240 avail_mem=102.54 GB):  60%|██████    | 35/58 [00:04<00:00, 25.48it/s]Capturing num tokens (num_tokens=224 avail_mem=102.53 GB):  60%|██████    | 35/58 [00:04<00:00, 25.48it/s]Capturing num tokens (num_tokens=224 avail_mem=102.53 GB):  67%|██████▋   | 39/58 [00:04<00:00, 28.07it/s]Capturing num tokens (num_tokens=208 avail_mem=102.53 GB):  67%|██████▋   | 39/58 [00:04<00:00, 28.07it/s]Capturing num tokens (num_tokens=192 avail_mem=102.53 GB):  67%|██████▋   | 39/58 [00:04<00:00, 28.07it/s]Capturing num tokens (num_tokens=176 avail_mem=102.52 GB):  67%|██████▋   | 39/58 [00:04<00:00, 28.07it/s]Capturing num tokens (num_tokens=160 avail_mem=102.52 GB):  67%|██████▋   | 39/58 [00:04<00:00, 28.07it/s]

    Capturing num tokens (num_tokens=160 avail_mem=102.52 GB):  74%|███████▍  | 43/58 [00:05<00:00, 29.91it/s]Capturing num tokens (num_tokens=144 avail_mem=102.51 GB):  74%|███████▍  | 43/58 [00:05<00:00, 29.91it/s]Capturing num tokens (num_tokens=128 avail_mem=102.51 GB):  74%|███████▍  | 43/58 [00:05<00:00, 29.91it/s]Capturing num tokens (num_tokens=112 avail_mem=102.52 GB):  74%|███████▍  | 43/58 [00:05<00:00, 29.91it/s]Capturing num tokens (num_tokens=96 avail_mem=102.52 GB):  74%|███████▍  | 43/58 [00:05<00:00, 29.91it/s] Capturing num tokens (num_tokens=96 avail_mem=102.52 GB):  81%|████████  | 47/58 [00:05<00:00, 31.27it/s]Capturing num tokens (num_tokens=80 avail_mem=102.51 GB):  81%|████████  | 47/58 [00:05<00:00, 31.27it/s]Capturing num tokens (num_tokens=64 avail_mem=102.51 GB):  81%|████████  | 47/58 [00:05<00:00, 31.27it/s]Capturing num tokens (num_tokens=48 avail_mem=102.50 GB):  81%|████████  | 47/58 [00:05<00:00, 31.27it/s]

    Capturing num tokens (num_tokens=32 avail_mem=102.50 GB):  81%|████████  | 47/58 [00:05<00:00, 31.27it/s]Capturing num tokens (num_tokens=32 avail_mem=102.50 GB):  88%|████████▊ | 51/58 [00:05<00:00, 32.14it/s]Capturing num tokens (num_tokens=28 avail_mem=102.49 GB):  88%|████████▊ | 51/58 [00:05<00:00, 32.14it/s]Capturing num tokens (num_tokens=24 avail_mem=102.49 GB):  88%|████████▊ | 51/58 [00:05<00:00, 32.14it/s]Capturing num tokens (num_tokens=20 avail_mem=102.48 GB):  88%|████████▊ | 51/58 [00:05<00:00, 32.14it/s]Capturing num tokens (num_tokens=16 avail_mem=102.48 GB):  88%|████████▊ | 51/58 [00:05<00:00, 32.14it/s]Capturing num tokens (num_tokens=16 avail_mem=102.48 GB):  95%|█████████▍| 55/58 [00:05<00:00, 32.76it/s]Capturing num tokens (num_tokens=12 avail_mem=102.48 GB):  95%|█████████▍| 55/58 [00:05<00:00, 32.76it/s]Capturing num tokens (num_tokens=8 avail_mem=102.47 GB):  95%|█████████▍| 55/58 [00:05<00:00, 32.76it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=102.47 GB):  95%|█████████▍| 55/58 [00:05<00:00, 32.76it/s]Capturing num tokens (num_tokens=4 avail_mem=102.47 GB): 100%|██████████| 58/58 [00:05<00:00, 10.65it/s]


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


<strong style='color: #00008B;'><function=get_current_date>{"timezone": "America/New_York"}</function><br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "fahrenheit"}</function><br><br>Sources: <br>- get_current_date function<br>- get_current_weather function</strong>


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


<strong style='color: #00008B;'>{'text': '{"name": "Paris", "population": 2147000}', 'output_ids': [5018, 609, 794, 330, 60704, 498, 330, 45541, 794, 220, 11584, 7007, 15, 92, 128009], 'meta_info': {'id': 'b6acc1cab33040c6a925d4c7ef3e2139', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 50, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 15, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.16172778699547052, 'response_sent_to_client_ts': 1774796801.3977942}}</strong>



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


<strong style='color: #00008B;'>{'text': '{"name": "Paris", "population": 2147000}', 'output_ids': [5018, 609, 794, 330, 60704, 498, 330, 45541, 794, 220, 11584, 7007, 15, 92, 128009], 'meta_info': {'id': 'beaf2430f87747598a4417a8368a4ece', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 50, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 15, 'cached_tokens': 49, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.15529335523024201, 'response_sent_to_client_ts': 1774796801.5666716}}</strong>


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


<strong style='color: #00008B;'>[{'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': 'decafb43a28f4dfbac880e459e12e5ea', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.093373519834131, 'response_sent_to_client_ts': 1774796801.6843479}}, {'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': '677191f20d0c475f893dc6c03408c15c', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.09310507168993354, 'response_sent_to_client_ts': 1774796801.684363}}, {'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': 'acdd71044783420dba994b88285fe7e4', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.09304182790219784, 'response_sent_to_client_ts': 1774796801.6843684}}]</strong>


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


<strong style='color: #00008B;'>{'text': 'France', 'output_ids': [50100, 128009], 'meta_info': {'id': '1a4d7b13429e4e9797c1ce8e199dd09f', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2, 'cached_tokens': 31, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.03273751866072416, 'response_sent_to_client_ts': 1774796801.7263603}}</strong>


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


<strong style='color: #00008B;'>{'text': 'France.', 'output_ids': [50100, 13, 128009], 'meta_info': {'id': 'deab144e65ab42ac9285ae9bac4243f5', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 3, 'cached_tokens': 40, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.1498072911053896, 'response_sent_to_client_ts': 1774796803.3145673}}</strong>



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


<strong style='color: #00008B;'>{'text': 'France.', 'output_ids': [50100, 13, 128009], 'meta_info': {'id': 'b23543a2ae124be885143c16af6e00dc', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 3, 'cached_tokens': 40, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.043285490944981575, 'response_sent_to_client_ts': 1774796803.367196}}</strong>



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

    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/httpcore/_exceptions.py", line 10, in map_exceptions
        yield
      File "/usr/local/lib/python3.10/dist-packages/httpcore/backends/sync.py", line 28, in read
        return self._sock.recv(max_bytes)
      File "/usr/lib/python3.10/ssl.py", line 1288, in recv
        return self.read(buflen)
      File "/usr/lib/python3.10/ssl.py", line 1161, in read
        return self._sslobj.read(len)
    TimeoutError: The read operation timed out
    
    During handling of the above exception, another exception occurred:
    
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/httpx/_transports/default.py", line 60, in map_httpcore_exceptions
        yield
      File "/usr/local/lib/python3.10/dist-packages/httpx/_transports/default.py", line 218, in handle_request
        resp = self._pool.handle_request(req)
      File "/usr/local/lib/python3.10/dist-packages/httpcore/_sync/connection_pool.py", line 253, in handle_request
        raise exc
      File "/usr/local/lib/python3.10/dist-packages/httpcore/_sync/connection_pool.py", line 237, in handle_request
        response = connection.handle_request(request)
      File "/usr/local/lib/python3.10/dist-packages/httpcore/_sync/connection.py", line 90, in handle_request
        return self._connection.handle_request(request)
      File "/usr/local/lib/python3.10/dist-packages/httpcore/_sync/http11.py", line 112, in handle_request
        raise exc
      File "/usr/local/lib/python3.10/dist-packages/httpcore/_sync/http11.py", line 91, in handle_request
        ) = self._receive_response_headers(**kwargs)
      File "/usr/local/lib/python3.10/dist-packages/httpcore/_sync/http11.py", line 155, in _receive_response_headers
        event = self._receive_event(timeout=timeout)
      File "/usr/local/lib/python3.10/dist-packages/httpcore/_sync/http11.py", line 191, in _receive_event
        data = self._network_stream.read(
      File "/usr/local/lib/python3.10/dist-packages/httpcore/backends/sync.py", line 26, in read
        with map_exceptions(exc_map):
      File "/usr/lib/python3.10/contextlib.py", line 153, in __exit__
        self.gen.throw(typ, value, traceback)
      File "/usr/local/lib/python3.10/dist-packages/httpcore/_exceptions.py", line 14, in map_exceptions
        raise to_exc(exc)
    httpcore.ReadTimeout: The read operation timed out
    
    The above exception was the direct cause of the following exception:
    
    Traceback (most recent call last):
      File "/actions-runner/_work/sglang/sglang/python/sglang/srt/utils/common.py", line 2525, in retry
        return fn()
      File "/actions-runner/_work/sglang/sglang/python/sglang/srt/configs/model_config.py", line 721, in <lambda>
        lambda: hf_api.file_exists(
      File "/usr/local/lib/python3.10/dist-packages/huggingface_hub/utils/_validators.py", line 89, in _inner_fn
        return fn(*args, **kwargs)
      File "/usr/local/lib/python3.10/dist-packages/huggingface_hub/hf_api.py", line 3436, in file_exists
        get_hf_file_metadata(url, token=token)
      File "/usr/local/lib/python3.10/dist-packages/huggingface_hub/utils/_validators.py", line 89, in _inner_fn
        return fn(*args, **kwargs)
      File "/usr/local/lib/python3.10/dist-packages/huggingface_hub/file_download.py", line 1576, in get_hf_file_metadata
        response = _httpx_follow_relative_redirects_with_backoff(
      File "/usr/local/lib/python3.10/dist-packages/huggingface_hub/utils/_http.py", line 685, in _httpx_follow_relative_redirects_with_backoff
        response = http_backoff(
      File "/usr/local/lib/python3.10/dist-packages/huggingface_hub/utils/_http.py", line 559, in http_backoff
        return next(
      File "/usr/local/lib/python3.10/dist-packages/huggingface_hub/utils/_http.py", line 467, in _http_backoff_base
        response = client.request(method=method, url=url, **kwargs)
      File "/usr/local/lib/python3.10/dist-packages/httpx/_client.py", line 821, in request
        return self.send(request, auth=auth, follow_redirects=follow_redirects)
      File "/usr/local/lib/python3.10/dist-packages/httpx/_client.py", line 908, in send
        response = self._send_handling_auth(
      File "/usr/local/lib/python3.10/dist-packages/httpx/_client.py", line 936, in _send_handling_auth
        response = self._send_handling_redirects(
      File "/usr/local/lib/python3.10/dist-packages/httpx/_client.py", line 973, in _send_handling_redirects
        response = self._send_single_request(request)
      File "/usr/local/lib/python3.10/dist-packages/httpx/_client.py", line 1009, in _send_single_request
        response = transport.handle_request(request)
      File "/usr/local/lib/python3.10/dist-packages/httpx/_transports/default.py", line 217, in handle_request
        with map_httpcore_exceptions():
      File "/usr/lib/python3.10/contextlib.py", line 153, in __exit__
        self.gen.throw(typ, value, traceback)
      File "/usr/local/lib/python3.10/dist-packages/httpx/_transports/default.py", line 77, in map_httpcore_exceptions
        raise mapped_exc(message) from exc
    httpx.ReadTimeout: The read operation timed out
    retry() failed once (0th try, maximum 2 retries). Will delay 0.78s and retry. Error: The read operation timed out


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-29 15:07:12.754 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 15:07:12] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 15:07:12.754 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 15:07:12] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 15:07:12.754 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 15:07:12] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 15:07:12.754 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 15:07:12] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 15:07:12.754 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 15:07:12] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]Loading safetensors checkpoint shards:  25% Completed | 1/4 [00:00<00:00,  7.20it/s]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:00<00:00,  2.22it/s]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:01<00:00,  1.62it/s]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.44it/s]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.64it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:14,  3.40s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:14,  3.40s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:33,  1.67s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:33,  1.67s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<00:55,  1.01s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<00:55,  1.01s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:37,  1.46it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:37,  1.46it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:26,  1.97it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:26,  1.97it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:20,  2.57it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:20,  2.57it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:15,  3.23it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:15,  3.23it/s]Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:12,  3.94it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:12,  3.94it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:10,  4.74it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:10,  4.74it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:08,  5.64it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:08,  5.64it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:07,  6.47it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:07,  6.47it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:05<00:07,  6.47it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:05<00:05,  8.18it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:05<00:05,  8.18it/s]

    Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:05<00:05,  8.18it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:04,  9.74it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:04,  9.74it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04,  9.74it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 11.59it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 11.59it/s]

    Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.59it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 11.59it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 14.85it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 14.85it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 14.85it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 14.85it/s]Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 14.85it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 20.41it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 20.41it/s]

    Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 20.41it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 20.41it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 20.41it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 20.41it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:06<00:01, 27.19it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:06<00:01, 27.19it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:06<00:01, 27.19it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:06<00:01, 27.19it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:06<00:01, 27.19it/s]Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:06<00:01, 27.19it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:06<00:01, 27.19it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:06<00:00, 34.75it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:06<00:00, 34.75it/s]

    Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:06<00:00, 34.75it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:06<00:00, 34.75it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:06<00:00, 34.75it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:06<00:00, 34.75it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:06<00:00, 34.75it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:06<00:00, 41.12it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:06<00:00, 41.12it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:06<00:00, 41.12it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:06<00:00, 41.12it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:06<00:00, 41.12it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:06<00:00, 41.12it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:06<00:00, 41.12it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:06<00:00, 46.01it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:06<00:00, 46.01it/s]

    Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:06<00:00, 46.01it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:06<00:00, 46.01it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:06<00:00, 46.01it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:06<00:00, 46.01it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:06<00:00, 46.01it/s]Compiling num tokens (num_tokens=20):  81%|████████  | 47/58 [00:06<00:00, 46.01it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:06<00:00, 51.01it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:06<00:00, 51.01it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:06<00:00, 51.01it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:06<00:00, 51.01it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:06<00:00, 51.01it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  8.95it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=102.62 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=102.62 GB):   2%|▏         | 1/58 [00:00<00:26,  2.17it/s]Capturing num tokens (num_tokens=7680 avail_mem=102.59 GB):   2%|▏         | 1/58 [00:00<00:26,  2.17it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=102.59 GB):   3%|▎         | 2/58 [00:00<00:21,  2.57it/s]Capturing num tokens (num_tokens=7168 avail_mem=102.59 GB):   3%|▎         | 2/58 [00:00<00:21,  2.57it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=102.59 GB):   5%|▌         | 3/58 [00:01<00:18,  2.95it/s]Capturing num tokens (num_tokens=6656 avail_mem=102.59 GB):   5%|▌         | 3/58 [00:01<00:18,  2.95it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=102.59 GB):   7%|▋         | 4/58 [00:01<00:20,  2.61it/s]Capturing num tokens (num_tokens=6144 avail_mem=102.60 GB):   7%|▋         | 4/58 [00:01<00:20,  2.61it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=102.60 GB):   9%|▊         | 5/58 [00:01<00:17,  3.01it/s]Capturing num tokens (num_tokens=5632 avail_mem=102.60 GB):   9%|▊         | 5/58 [00:01<00:17,  3.01it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=102.60 GB):  10%|█         | 6/58 [00:01<00:15,  3.46it/s]Capturing num tokens (num_tokens=5120 avail_mem=102.61 GB):  10%|█         | 6/58 [00:01<00:15,  3.46it/s]Capturing num tokens (num_tokens=5120 avail_mem=102.61 GB):  12%|█▏        | 7/58 [00:02<00:13,  3.90it/s]Capturing num tokens (num_tokens=4608 avail_mem=102.61 GB):  12%|█▏        | 7/58 [00:02<00:13,  3.90it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=102.61 GB):  14%|█▍        | 8/58 [00:02<00:11,  4.41it/s]Capturing num tokens (num_tokens=4096 avail_mem=102.61 GB):  14%|█▍        | 8/58 [00:02<00:11,  4.41it/s]Capturing num tokens (num_tokens=4096 avail_mem=102.61 GB):  16%|█▌        | 9/58 [00:02<00:09,  4.94it/s]Capturing num tokens (num_tokens=3840 avail_mem=102.61 GB):  16%|█▌        | 9/58 [00:02<00:09,  4.94it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=102.61 GB):  17%|█▋        | 10/58 [00:02<00:08,  5.44it/s]Capturing num tokens (num_tokens=3584 avail_mem=102.62 GB):  17%|█▋        | 10/58 [00:02<00:08,  5.44it/s]Capturing num tokens (num_tokens=3584 avail_mem=102.62 GB):  19%|█▉        | 11/58 [00:02<00:07,  5.99it/s]Capturing num tokens (num_tokens=3328 avail_mem=102.61 GB):  19%|█▉        | 11/58 [00:02<00:07,  5.99it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=102.61 GB):  21%|██        | 12/58 [00:02<00:07,  6.55it/s]Capturing num tokens (num_tokens=3072 avail_mem=102.61 GB):  21%|██        | 12/58 [00:02<00:07,  6.55it/s]Capturing num tokens (num_tokens=3072 avail_mem=102.61 GB):  22%|██▏       | 13/58 [00:02<00:06,  7.14it/s]Capturing num tokens (num_tokens=2816 avail_mem=102.61 GB):  22%|██▏       | 13/58 [00:02<00:06,  7.14it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=102.61 GB):  22%|██▏       | 13/58 [00:03<00:06,  7.14it/s]Capturing num tokens (num_tokens=2560 avail_mem=102.61 GB):  26%|██▌       | 15/58 [00:03<00:05,  8.44it/s]Capturing num tokens (num_tokens=2304 avail_mem=102.61 GB):  26%|██▌       | 15/58 [00:03<00:05,  8.44it/s]Capturing num tokens (num_tokens=2048 avail_mem=102.61 GB):  26%|██▌       | 15/58 [00:03<00:05,  8.44it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=102.61 GB):  29%|██▉       | 17/58 [00:03<00:04,  9.82it/s]Capturing num tokens (num_tokens=1792 avail_mem=102.61 GB):  29%|██▉       | 17/58 [00:03<00:04,  9.82it/s]Capturing num tokens (num_tokens=1536 avail_mem=102.61 GB):  29%|██▉       | 17/58 [00:03<00:04,  9.82it/s]Capturing num tokens (num_tokens=1536 avail_mem=102.61 GB):  33%|███▎      | 19/58 [00:03<00:03, 11.57it/s]Capturing num tokens (num_tokens=1280 avail_mem=102.61 GB):  33%|███▎      | 19/58 [00:03<00:03, 11.57it/s]Capturing num tokens (num_tokens=1024 avail_mem=102.61 GB):  33%|███▎      | 19/58 [00:03<00:03, 11.57it/s]

    Capturing num tokens (num_tokens=960 avail_mem=102.61 GB):  33%|███▎      | 19/58 [00:03<00:03, 11.57it/s] Capturing num tokens (num_tokens=960 avail_mem=102.61 GB):  38%|███▊      | 22/58 [00:03<00:02, 14.65it/s]Capturing num tokens (num_tokens=896 avail_mem=102.60 GB):  38%|███▊      | 22/58 [00:03<00:02, 14.65it/s]Capturing num tokens (num_tokens=832 avail_mem=102.60 GB):  38%|███▊      | 22/58 [00:03<00:02, 14.65it/s]Capturing num tokens (num_tokens=768 avail_mem=102.60 GB):  38%|███▊      | 22/58 [00:03<00:02, 14.65it/s]Capturing num tokens (num_tokens=768 avail_mem=102.60 GB):  43%|████▎     | 25/58 [00:03<00:01, 17.43it/s]Capturing num tokens (num_tokens=704 avail_mem=102.59 GB):  43%|████▎     | 25/58 [00:03<00:01, 17.43it/s]

    Capturing num tokens (num_tokens=640 avail_mem=102.59 GB):  43%|████▎     | 25/58 [00:03<00:01, 17.43it/s]Capturing num tokens (num_tokens=576 avail_mem=102.58 GB):  43%|████▎     | 25/58 [00:03<00:01, 17.43it/s]Capturing num tokens (num_tokens=576 avail_mem=102.58 GB):  48%|████▊     | 28/58 [00:03<00:01, 19.96it/s]Capturing num tokens (num_tokens=512 avail_mem=102.58 GB):  48%|████▊     | 28/58 [00:03<00:01, 19.96it/s]Capturing num tokens (num_tokens=480 avail_mem=102.57 GB):  48%|████▊     | 28/58 [00:03<00:01, 19.96it/s]Capturing num tokens (num_tokens=448 avail_mem=102.57 GB):  48%|████▊     | 28/58 [00:03<00:01, 19.96it/s]Capturing num tokens (num_tokens=448 avail_mem=102.57 GB):  53%|█████▎    | 31/58 [00:03<00:01, 22.45it/s]Capturing num tokens (num_tokens=416 avail_mem=102.56 GB):  53%|█████▎    | 31/58 [00:03<00:01, 22.45it/s]

    Capturing num tokens (num_tokens=384 avail_mem=102.56 GB):  53%|█████▎    | 31/58 [00:03<00:01, 22.45it/s]Capturing num tokens (num_tokens=352 avail_mem=102.56 GB):  53%|█████▎    | 31/58 [00:03<00:01, 22.45it/s]Capturing num tokens (num_tokens=320 avail_mem=102.55 GB):  53%|█████▎    | 31/58 [00:03<00:01, 22.45it/s]Capturing num tokens (num_tokens=320 avail_mem=102.55 GB):  60%|██████    | 35/58 [00:04<00:00, 25.63it/s]Capturing num tokens (num_tokens=288 avail_mem=102.55 GB):  60%|██████    | 35/58 [00:04<00:00, 25.63it/s]Capturing num tokens (num_tokens=256 avail_mem=102.54 GB):  60%|██████    | 35/58 [00:04<00:00, 25.63it/s]Capturing num tokens (num_tokens=240 avail_mem=102.54 GB):  60%|██████    | 35/58 [00:04<00:00, 25.63it/s]Capturing num tokens (num_tokens=224 avail_mem=102.53 GB):  60%|██████    | 35/58 [00:04<00:00, 25.63it/s]

    Capturing num tokens (num_tokens=224 avail_mem=102.53 GB):  67%|██████▋   | 39/58 [00:04<00:00, 28.22it/s]Capturing num tokens (num_tokens=208 avail_mem=102.53 GB):  67%|██████▋   | 39/58 [00:04<00:00, 28.22it/s]Capturing num tokens (num_tokens=192 avail_mem=102.53 GB):  67%|██████▋   | 39/58 [00:04<00:00, 28.22it/s]Capturing num tokens (num_tokens=176 avail_mem=102.52 GB):  67%|██████▋   | 39/58 [00:04<00:00, 28.22it/s]Capturing num tokens (num_tokens=160 avail_mem=102.52 GB):  67%|██████▋   | 39/58 [00:04<00:00, 28.22it/s]Capturing num tokens (num_tokens=160 avail_mem=102.52 GB):  74%|███████▍  | 43/58 [00:04<00:00, 30.14it/s]Capturing num tokens (num_tokens=144 avail_mem=102.51 GB):  74%|███████▍  | 43/58 [00:04<00:00, 30.14it/s]Capturing num tokens (num_tokens=128 avail_mem=102.51 GB):  74%|███████▍  | 43/58 [00:04<00:00, 30.14it/s]Capturing num tokens (num_tokens=112 avail_mem=102.52 GB):  74%|███████▍  | 43/58 [00:04<00:00, 30.14it/s]

    Capturing num tokens (num_tokens=96 avail_mem=102.52 GB):  74%|███████▍  | 43/58 [00:04<00:00, 30.14it/s] Capturing num tokens (num_tokens=96 avail_mem=102.52 GB):  81%|████████  | 47/58 [00:04<00:00, 31.54it/s]Capturing num tokens (num_tokens=80 avail_mem=102.51 GB):  81%|████████  | 47/58 [00:04<00:00, 31.54it/s]Capturing num tokens (num_tokens=64 avail_mem=102.51 GB):  81%|████████  | 47/58 [00:04<00:00, 31.54it/s]Capturing num tokens (num_tokens=48 avail_mem=102.50 GB):  81%|████████  | 47/58 [00:04<00:00, 31.54it/s]Capturing num tokens (num_tokens=32 avail_mem=102.50 GB):  81%|████████  | 47/58 [00:04<00:00, 31.54it/s]Capturing num tokens (num_tokens=32 avail_mem=102.50 GB):  88%|████████▊ | 51/58 [00:04<00:00, 32.11it/s]Capturing num tokens (num_tokens=28 avail_mem=102.49 GB):  88%|████████▊ | 51/58 [00:04<00:00, 32.11it/s]Capturing num tokens (num_tokens=24 avail_mem=102.49 GB):  88%|████████▊ | 51/58 [00:04<00:00, 32.11it/s]

    Capturing num tokens (num_tokens=20 avail_mem=102.48 GB):  88%|████████▊ | 51/58 [00:04<00:00, 32.11it/s]Capturing num tokens (num_tokens=16 avail_mem=102.48 GB):  88%|████████▊ | 51/58 [00:04<00:00, 32.11it/s]Capturing num tokens (num_tokens=16 avail_mem=102.48 GB):  95%|█████████▍| 55/58 [00:04<00:00, 30.94it/s]Capturing num tokens (num_tokens=12 avail_mem=102.48 GB):  95%|█████████▍| 55/58 [00:04<00:00, 30.94it/s]Capturing num tokens (num_tokens=8 avail_mem=102.47 GB):  95%|█████████▍| 55/58 [00:04<00:00, 30.94it/s] Capturing num tokens (num_tokens=4 avail_mem=102.47 GB):  95%|█████████▍| 55/58 [00:04<00:00, 30.94it/s]Capturing num tokens (num_tokens=4 avail_mem=102.47 GB): 100%|██████████| 58/58 [00:04<00:00, 12.29it/s]


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



<strong style='color: #00008B;'>Prompt: Give me the information of the capital of Italy.<br>Generated text: Rome is the capital of Italy</strong>


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
