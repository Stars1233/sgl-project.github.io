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

    [2026-03-25 12:53:01] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.


    [2026-03-25 12:53:01] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.


    [2026-03-25 12:53:01] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-25 12:53:06] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-25 12:53:06] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-25 12:53:06] INFO utils.py:164: NumExpr defaulting to 16 threads.


    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    [2026-03-25 12:53:08] INFO server_args.py:2246: Attention backend not specified. Use fa3 backend by default.
    [2026-03-25 12:53:08] INFO server_args.py:3544: Set soft_watchdog_timeout since in CI


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [2026-03-25 12:53:14] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-25 12:53:14] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-25 12:53:14] INFO utils.py:164: NumExpr defaulting to 16 threads.
    [2026-03-25 12:53:14] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-25 12:53:14] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-25 12:53:14] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-03-25 12:53:20.428 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 12:53:20] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 12:53:20.428 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 12:53:20] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 12:53:20.428 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 12:53:20] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 12:53:20.428 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 12:53:20] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 12:53:20.428 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 12:53:20] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  25% Completed | 1/4 [00:00<00:01,  1.58it/s]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:01<00:01,  1.22it/s]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:02<00:00,  1.50it/s]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.34it/s]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.36it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:14,  3.41s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:14,  3.41s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:53,  2.02s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:53,  2.02s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:11,  1.29s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:11,  1.29s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:05<00:49,  1.09it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:05<00:49,  1.09it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:36,  1.44it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:36,  1.44it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:28,  1.83it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:28,  1.83it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:22,  2.29it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:22,  2.29it/s]Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:17,  2.85it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:17,  2.85it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:13,  3.51it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:13,  3.51it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:12,  3.83it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:12,  3.83it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:12,  3.91it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:12,  3.91it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:06<00:10,  4.32it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:06<00:10,  4.32it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:09,  4.69it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:09,  4.69it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:09,  4.88it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:09,  4.88it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:08,  5.26it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:08,  5.26it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:07<00:07,  5.78it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:07<00:07,  5.78it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:07<00:06,  6.15it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:07<00:06,  6.15it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:07<00:05,  6.75it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:07<00:05,  6.75it/s]

    Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:07<00:05,  7.39it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:07<00:05,  7.39it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:08<00:05,  7.39it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:08<00:03,  9.63it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:08<00:03,  9.63it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:08<00:03,  9.63it/s]

    Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:08<00:03, 11.15it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:08<00:03, 11.15it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:08<00:03, 11.15it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:08<00:02, 12.69it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:08<00:02, 12.69it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:08<00:02, 12.69it/s]

    Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:08<00:02, 12.69it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:08<00:01, 15.56it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:08<00:01, 15.56it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:08<00:01, 15.56it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:08<00:01, 15.56it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:08<00:01, 17.92it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:08<00:01, 17.92it/s]

    Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:08<00:01, 17.92it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:08<00:01, 18.04it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:08<00:01, 18.04it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:08<00:01, 18.04it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:08<00:01, 18.04it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:08<00:01, 18.04it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:08<00:00, 22.03it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:08<00:00, 22.03it/s]

    Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:08<00:00, 22.03it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:08<00:00, 22.03it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:08<00:00, 22.82it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:08<00:00, 22.82it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:08<00:00, 22.82it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:09<00:00, 22.82it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:09<00:00, 24.24it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:09<00:00, 24.24it/s]

    Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:09<00:00, 24.24it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:09<00:00, 24.24it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:09<00:00, 24.24it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:09<00:00, 26.57it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:09<00:00, 26.57it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:09<00:00, 26.57it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:09<00:00, 26.57it/s]

    Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:09<00:00, 27.17it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:09<00:00, 27.17it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:09<00:00, 27.17it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:09<00:00, 27.17it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:09<00:00, 27.17it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:09<00:00, 29.75it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:09<00:00, 29.75it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:09<00:00, 29.75it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:09<00:00, 29.75it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:09<00:00, 29.75it/s]

    Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00, 32.40it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00,  6.11it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=99.75 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=99.75 GB):   2%|▏         | 1/58 [00:00<00:36,  1.55it/s]Capturing num tokens (num_tokens=7680 avail_mem=99.82 GB):   2%|▏         | 1/58 [00:00<00:36,  1.55it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=99.82 GB):   3%|▎         | 2/58 [00:01<00:33,  1.68it/s]Capturing num tokens (num_tokens=7168 avail_mem=99.92 GB):   3%|▎         | 2/58 [00:01<00:33,  1.68it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=99.92 GB):   5%|▌         | 3/58 [00:01<00:27,  1.98it/s]Capturing num tokens (num_tokens=6656 avail_mem=100.01 GB):   5%|▌         | 3/58 [00:01<00:27,  1.98it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=100.01 GB):   7%|▋         | 4/58 [00:01<00:24,  2.20it/s]Capturing num tokens (num_tokens=6144 avail_mem=100.03 GB):   7%|▋         | 4/58 [00:01<00:24,  2.20it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=100.03 GB):   9%|▊         | 5/58 [00:02<00:21,  2.46it/s]Capturing num tokens (num_tokens=5632 avail_mem=100.14 GB):   9%|▊         | 5/58 [00:02<00:21,  2.46it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=100.14 GB):  10%|█         | 6/58 [00:02<00:18,  2.74it/s]Capturing num tokens (num_tokens=5120 avail_mem=100.13 GB):  10%|█         | 6/58 [00:02<00:18,  2.74it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=100.13 GB):  12%|█▏        | 7/58 [00:02<00:16,  3.04it/s]Capturing num tokens (num_tokens=4608 avail_mem=100.12 GB):  12%|█▏        | 7/58 [00:02<00:16,  3.04it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=100.12 GB):  14%|█▍        | 8/58 [00:03<00:14,  3.37it/s]Capturing num tokens (num_tokens=4096 avail_mem=100.05 GB):  14%|█▍        | 8/58 [00:03<00:14,  3.37it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=100.05 GB):  16%|█▌        | 9/58 [00:03<00:13,  3.74it/s]Capturing num tokens (num_tokens=3840 avail_mem=100.07 GB):  16%|█▌        | 9/58 [00:03<00:13,  3.74it/s]Capturing num tokens (num_tokens=3840 avail_mem=100.07 GB):  17%|█▋        | 10/58 [00:03<00:11,  4.15it/s]Capturing num tokens (num_tokens=3584 avail_mem=100.07 GB):  17%|█▋        | 10/58 [00:03<00:11,  4.15it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=100.07 GB):  19%|█▉        | 11/58 [00:03<00:10,  4.52it/s]Capturing num tokens (num_tokens=3328 avail_mem=100.06 GB):  19%|█▉        | 11/58 [00:03<00:10,  4.52it/s]Capturing num tokens (num_tokens=3328 avail_mem=100.06 GB):  21%|██        | 12/58 [00:03<00:09,  4.97it/s]Capturing num tokens (num_tokens=3072 avail_mem=100.06 GB):  21%|██        | 12/58 [00:03<00:09,  4.97it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=100.06 GB):  22%|██▏       | 13/58 [00:03<00:08,  5.45it/s]Capturing num tokens (num_tokens=2816 avail_mem=100.05 GB):  22%|██▏       | 13/58 [00:03<00:08,  5.45it/s]Capturing num tokens (num_tokens=2816 avail_mem=100.05 GB):  24%|██▍       | 14/58 [00:04<00:07,  5.94it/s]Capturing num tokens (num_tokens=2560 avail_mem=100.04 GB):  24%|██▍       | 14/58 [00:04<00:07,  5.94it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=100.04 GB):  26%|██▌       | 15/58 [00:04<00:06,  6.54it/s]Capturing num tokens (num_tokens=2304 avail_mem=100.04 GB):  26%|██▌       | 15/58 [00:04<00:06,  6.54it/s]Capturing num tokens (num_tokens=2304 avail_mem=100.04 GB):  28%|██▊       | 16/58 [00:04<00:05,  7.11it/s]Capturing num tokens (num_tokens=2048 avail_mem=100.03 GB):  28%|██▊       | 16/58 [00:04<00:05,  7.11it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=100.02 GB):  28%|██▊       | 16/58 [00:04<00:05,  7.11it/s]Capturing num tokens (num_tokens=1792 avail_mem=100.02 GB):  31%|███       | 18/58 [00:04<00:04,  8.62it/s]Capturing num tokens (num_tokens=1536 avail_mem=100.02 GB):  31%|███       | 18/58 [00:04<00:04,  8.62it/s]Capturing num tokens (num_tokens=1280 avail_mem=100.02 GB):  31%|███       | 18/58 [00:04<00:04,  8.62it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=100.02 GB):  34%|███▍      | 20/58 [00:04<00:03, 10.22it/s]Capturing num tokens (num_tokens=1024 avail_mem=100.01 GB):  34%|███▍      | 20/58 [00:04<00:03, 10.22it/s]Capturing num tokens (num_tokens=960 avail_mem=100.00 GB):  34%|███▍      | 20/58 [00:04<00:03, 10.22it/s] Capturing num tokens (num_tokens=896 avail_mem=100.00 GB):  34%|███▍      | 20/58 [00:04<00:03, 10.22it/s]Capturing num tokens (num_tokens=896 avail_mem=100.00 GB):  40%|███▉      | 23/58 [00:04<00:02, 13.71it/s]Capturing num tokens (num_tokens=832 avail_mem=99.99 GB):  40%|███▉      | 23/58 [00:04<00:02, 13.71it/s] Capturing num tokens (num_tokens=768 avail_mem=99.99 GB):  40%|███▉      | 23/58 [00:04<00:02, 13.71it/s]

    Capturing num tokens (num_tokens=704 avail_mem=99.98 GB):  40%|███▉      | 23/58 [00:04<00:02, 13.71it/s]Capturing num tokens (num_tokens=704 avail_mem=99.98 GB):  45%|████▍     | 26/58 [00:04<00:01, 16.79it/s]Capturing num tokens (num_tokens=640 avail_mem=99.98 GB):  45%|████▍     | 26/58 [00:04<00:01, 16.79it/s]Capturing num tokens (num_tokens=576 avail_mem=99.98 GB):  45%|████▍     | 26/58 [00:04<00:01, 16.79it/s]Capturing num tokens (num_tokens=512 avail_mem=99.97 GB):  45%|████▍     | 26/58 [00:04<00:01, 16.79it/s]Capturing num tokens (num_tokens=512 avail_mem=99.97 GB):  50%|█████     | 29/58 [00:04<00:01, 19.84it/s]Capturing num tokens (num_tokens=480 avail_mem=99.97 GB):  50%|█████     | 29/58 [00:04<00:01, 19.84it/s]Capturing num tokens (num_tokens=448 avail_mem=99.96 GB):  50%|█████     | 29/58 [00:04<00:01, 19.84it/s]

    Capturing num tokens (num_tokens=416 avail_mem=99.96 GB):  50%|█████     | 29/58 [00:05<00:01, 19.84it/s]Capturing num tokens (num_tokens=384 avail_mem=99.95 GB):  50%|█████     | 29/58 [00:05<00:01, 19.84it/s]Capturing num tokens (num_tokens=384 avail_mem=99.95 GB):  57%|█████▋    | 33/58 [00:05<00:01, 23.12it/s]Capturing num tokens (num_tokens=352 avail_mem=99.95 GB):  57%|█████▋    | 33/58 [00:05<00:01, 23.12it/s]Capturing num tokens (num_tokens=320 avail_mem=99.95 GB):  57%|█████▋    | 33/58 [00:05<00:01, 23.12it/s]Capturing num tokens (num_tokens=288 avail_mem=99.94 GB):  57%|█████▋    | 33/58 [00:05<00:01, 23.12it/s]Capturing num tokens (num_tokens=256 avail_mem=99.94 GB):  57%|█████▋    | 33/58 [00:05<00:01, 23.12it/s]Capturing num tokens (num_tokens=256 avail_mem=99.94 GB):  64%|██████▍   | 37/58 [00:05<00:00, 26.24it/s]Capturing num tokens (num_tokens=240 avail_mem=99.94 GB):  64%|██████▍   | 37/58 [00:05<00:00, 26.24it/s]

    Capturing num tokens (num_tokens=224 avail_mem=99.93 GB):  64%|██████▍   | 37/58 [00:05<00:00, 26.24it/s]Capturing num tokens (num_tokens=208 avail_mem=99.93 GB):  64%|██████▍   | 37/58 [00:05<00:00, 26.24it/s]Capturing num tokens (num_tokens=192 avail_mem=99.92 GB):  64%|██████▍   | 37/58 [00:05<00:00, 26.24it/s]Capturing num tokens (num_tokens=192 avail_mem=99.92 GB):  71%|███████   | 41/58 [00:05<00:00, 28.39it/s]Capturing num tokens (num_tokens=176 avail_mem=99.91 GB):  71%|███████   | 41/58 [00:05<00:00, 28.39it/s]Capturing num tokens (num_tokens=160 avail_mem=99.91 GB):  71%|███████   | 41/58 [00:05<00:00, 28.39it/s]Capturing num tokens (num_tokens=144 avail_mem=99.91 GB):  71%|███████   | 41/58 [00:05<00:00, 28.39it/s]Capturing num tokens (num_tokens=128 avail_mem=99.90 GB):  71%|███████   | 41/58 [00:05<00:00, 28.39it/s]

    Capturing num tokens (num_tokens=128 avail_mem=99.90 GB):  78%|███████▊  | 45/58 [00:05<00:00, 30.13it/s]Capturing num tokens (num_tokens=112 avail_mem=99.91 GB):  78%|███████▊  | 45/58 [00:05<00:00, 30.13it/s]Capturing num tokens (num_tokens=96 avail_mem=99.91 GB):  78%|███████▊  | 45/58 [00:05<00:00, 30.13it/s] Capturing num tokens (num_tokens=80 avail_mem=99.91 GB):  78%|███████▊  | 45/58 [00:05<00:00, 30.13it/s]Capturing num tokens (num_tokens=64 avail_mem=99.90 GB):  78%|███████▊  | 45/58 [00:05<00:00, 30.13it/s]Capturing num tokens (num_tokens=64 avail_mem=99.90 GB):  84%|████████▍ | 49/58 [00:05<00:00, 31.27it/s]Capturing num tokens (num_tokens=48 avail_mem=99.90 GB):  84%|████████▍ | 49/58 [00:05<00:00, 31.27it/s]Capturing num tokens (num_tokens=32 avail_mem=99.89 GB):  84%|████████▍ | 49/58 [00:05<00:00, 31.27it/s]Capturing num tokens (num_tokens=28 avail_mem=99.89 GB):  84%|████████▍ | 49/58 [00:05<00:00, 31.27it/s]

    Capturing num tokens (num_tokens=24 avail_mem=99.88 GB):  84%|████████▍ | 49/58 [00:05<00:00, 31.27it/s]Capturing num tokens (num_tokens=24 avail_mem=99.88 GB):  91%|█████████▏| 53/58 [00:05<00:00, 32.07it/s]Capturing num tokens (num_tokens=20 avail_mem=99.88 GB):  91%|█████████▏| 53/58 [00:05<00:00, 32.07it/s]Capturing num tokens (num_tokens=16 avail_mem=99.87 GB):  91%|█████████▏| 53/58 [00:05<00:00, 32.07it/s]Capturing num tokens (num_tokens=12 avail_mem=99.87 GB):  91%|█████████▏| 53/58 [00:05<00:00, 32.07it/s]Capturing num tokens (num_tokens=8 avail_mem=99.87 GB):  91%|█████████▏| 53/58 [00:05<00:00, 32.07it/s] Capturing num tokens (num_tokens=8 avail_mem=99.87 GB):  98%|█████████▊| 57/58 [00:05<00:00, 32.84it/s]Capturing num tokens (num_tokens=4 avail_mem=99.86 GB):  98%|█████████▊| 57/58 [00:05<00:00, 32.84it/s]Capturing num tokens (num_tokens=4 avail_mem=99.86 GB): 100%|██████████| 58/58 [00:05<00:00,  9.96it/s]


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


<strong style='color: #00008B;'><function=get_current_date>{"timezone": "America/New_York"}</function><br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "fahrenheit"}</function><br><br>Sources:<br>- get_current_date function<br>- get_current_weather function</strong>



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


<strong style='color: #00008B;'>{'text': '{"name": "Paris", "population": 2147000}', 'output_ids': [5018, 609, 794, 330, 60704, 498, 330, 45541, 794, 220, 11584, 7007, 15, 92, 128009], 'meta_info': {'id': '4ae325f7c0c442688ed868d99a6db521', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 50, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 15, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.17548165982589126, 'response_sent_to_client_ts': 1774443235.712879}}</strong>



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


<strong style='color: #00008B;'>{'text': '{"name": "Paris", "population": 2147000}', 'output_ids': [5018, 609, 794, 330, 60704, 498, 330, 45541, 794, 220, 11584, 7007, 15, 92, 128009], 'meta_info': {'id': '2af5da7d54124c31b502aaf85f4a0c41', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 50, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 15, 'cached_tokens': 49, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.17553279595449567, 'response_sent_to_client_ts': 1774443235.903082}}</strong>


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


<strong style='color: #00008B;'>[{'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': '40b8da47acb44f119f79956db484a1dd', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.15659676305949688, 'response_sent_to_client_ts': 1774443236.0961673}}, {'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': '43161b4f6fdb4b33a7d438a52696ef53', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.15649720607325435, 'response_sent_to_client_ts': 1774443236.0961804}}, {'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': 'a37376213b8d4780a9842e918e2e6d54', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.15645552286878228, 'response_sent_to_client_ts': 1774443236.0961864}}]</strong>


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


<strong style='color: #00008B;'>{'text': 'France', 'output_ids': [50100, 128009], 'meta_info': {'id': '0a1c8b79ff81429dbed57503365b4fb9', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2, 'cached_tokens': 31, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.03628148790448904, 'response_sent_to_client_ts': 1774443236.141566}}</strong>


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


<strong style='color: #00008B;'>{'text': 'France.', 'output_ids': [50100, 13, 128009], 'meta_info': {'id': '967d63df8b524e45808057f092ea40bf', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 3, 'cached_tokens': 40, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.10011961497366428, 'response_sent_to_client_ts': 1774443237.5914512}}</strong>



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


<strong style='color: #00008B;'>{'text': 'France.', 'output_ids': [50100, 13, 128009], 'meta_info': {'id': 'fafa1aa98923483daa47380caa0f107f', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 3, 'cached_tokens': 40, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.062135139014571905, 'response_sent_to_client_ts': 1774443237.6629703}}</strong>



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

    [2026-03-25 12:54:00] INFO server_args.py:2246: Attention backend not specified. Use fa3 backend by default.


    [2026-03-25 12:54:00] INFO server_args.py:3544: Set soft_watchdog_timeout since in CI


    [2026-03-25 12:54:00] INFO engine.py:177: server_args=ServerArgs(model_path='meta-llama/Meta-Llama-3.1-8B-Instruct', tokenizer_path='meta-llama/Meta-Llama-3.1-8B-Instruct', tokenizer_mode='auto', tokenizer_worker_num=1, skip_tokenizer_init=False, load_format='auto', model_loader_extra_config='{}', trust_remote_code=False, context_length=None, is_embedding=False, enable_multimodal=None, revision=None, model_impl='auto', host='127.0.0.1', port=30000, fastapi_root_path='', grpc_mode=False, skip_server_warmup=False, warmups=None, nccl_port=None, checkpoint_engine_wait_weights_before_ready=False, ssl_keyfile=None, ssl_certfile=None, ssl_ca_certs=None, ssl_keyfile_password=None, enable_ssl_refresh=False, dtype='auto', quantization=None, quantization_param_path=None, kv_cache_dtype='auto', enable_fp32_lm_head=False, modelopt_quant=None, modelopt_checkpoint_restore_path=None, modelopt_checkpoint_save_path=None, modelopt_export_path=None, quantize_and_serve=False, rl_quant_profile=None, mem_fraction_static=0.903, max_running_requests=128, max_queued_requests=None, max_total_tokens=20480, chunked_prefill_size=8192, enable_dynamic_chunking=False, max_prefill_tokens=16384, prefill_max_requests=None, schedule_policy='fcfs', enable_priority_scheduling=False, disable_priority_preemption=False, default_priority_value=None, abort_on_priority_when_disabled=False, schedule_low_priority_values_first=False, priority_scheduling_preemption_threshold=10, schedule_conservativeness=1.0, page_size=1, swa_full_tokens_ratio=0.8, disable_hybrid_swa_memory=False, radix_eviction_policy='lru', enable_prefill_delayer=False, prefill_delayer_max_delay_passes=30, prefill_delayer_token_usage_low_watermark=None, prefill_delayer_forward_passes_buckets=None, prefill_delayer_wait_seconds_buckets=None, device='cuda', tp_size=1, pp_size=1, pp_max_micro_batch_size=None, pp_async_batch_depth=0, stream_interval=1, incremental_streaming_output=False, enable_streaming_session=False, random_seed=80407263, constrained_json_whitespace_pattern=None, constrained_json_disable_any_whitespace=False, watchdog_timeout=300, soft_watchdog_timeout=300, dist_timeout=None, download_dir=None, model_checksum=None, base_gpu_id=0, gpu_id_step=1, sleep_on_idle=False, use_ray=False, custom_sigquit_handler=None, log_level='error', log_level_http=None, log_requests=False, log_requests_level=2, log_requests_format='text', log_requests_target=None, uvicorn_access_log_exclude_prefixes=[], crash_dump_folder=None, show_time_cost=False, enable_metrics=False, enable_metrics_for_all_schedulers=False, tokenizer_metrics_custom_labels_header='x-custom-labels', tokenizer_metrics_allowed_custom_labels=None, extra_metric_labels=None, bucket_time_to_first_token=None, bucket_inter_token_latency=None, bucket_e2e_request_latency=None, collect_tokens_histogram=False, prompt_tokens_buckets=None, generation_tokens_buckets=None, gc_warning_threshold_secs=0.0, decode_log_interval=40, enable_request_time_stats_logging=False, kv_events_config=None, enable_trace=False, otlp_traces_endpoint='localhost:4317', export_metrics_to_file=False, export_metrics_to_file_dir=None, api_key=None, admin_api_key=None, served_model_name='meta-llama/Meta-Llama-3.1-8B-Instruct', weight_version='default', chat_template=None, hf_chat_template_name=None, completion_template=None, file_storage_path='sglang_storage', enable_cache_report=False, reasoning_parser=None, tool_call_parser=None, tool_server=None, sampling_defaults='model', dp_size=1, load_balance_method='round_robin', attn_cp_size=1, moe_dp_size=1, dist_init_addr=None, nnodes=1, node_rank=0, json_model_override_args='{}', preferred_sampling_params=None, enable_lora=None, enable_lora_overlap_loading=None, max_lora_rank=None, lora_target_modules=None, lora_paths=None, max_loaded_loras=None, max_loras_per_batch=8, lora_eviction_policy='lru', lora_backend='csgmv', max_lora_chunk_size=16, attention_backend='fa3', decode_attention_backend=None, prefill_attention_backend=None, sampling_backend='flashinfer', grammar_backend='xgrammar', mm_attention_backend=None, fp8_gemm_runner_backend='auto', fp4_gemm_runner_backend='auto', nsa_prefill_backend=None, nsa_decode_backend=None, disable_flashinfer_autotune=False, mamba_backend='triton', speculative_algorithm=None, speculative_draft_model_path=None, speculative_draft_model_revision=None, speculative_draft_load_format=None, speculative_num_steps=None, speculative_eagle_topk=None, speculative_num_draft_tokens=None, speculative_accept_threshold_single=1.0, speculative_accept_threshold_acc=1.0, speculative_token_map=None, speculative_attention_mode='prefill', speculative_draft_attention_backend=None, speculative_moe_runner_backend='auto', speculative_moe_a2a_backend=None, speculative_draft_model_quantization=None, speculative_ngram_min_match_window_size=1, speculative_ngram_max_match_window_size=12, speculative_ngram_min_bfs_breadth=1, speculative_ngram_max_bfs_breadth=10, speculative_ngram_match_type='BFS', speculative_ngram_max_trie_depth=18, speculative_ngram_capacity=10000000, enable_multi_layer_eagle=False, ep_size=1, moe_a2a_backend='none', moe_runner_backend='auto', flashinfer_mxfp4_moe_precision='default', enable_flashinfer_allreduce_fusion=False, enable_aiter_allreduce_fusion=False, deepep_mode='auto', ep_num_redundant_experts=0, ep_dispatch_algorithm=None, init_expert_location='trivial', enable_eplb=False, eplb_algorithm='auto', eplb_rebalance_num_iterations=1000, eplb_rebalance_layers_per_chunk=None, eplb_min_rebalancing_utilization_threshold=1.0, expert_distribution_recorder_mode=None, expert_distribution_recorder_buffer_size=1000, enable_expert_distribution_metrics=False, deepep_config=None, moe_dense_tp_size=None, elastic_ep_backend=None, enable_elastic_expert_backup=False, mooncake_ib_device=None, max_mamba_cache_size=None, mamba_ssm_dtype=None, mamba_full_memory_ratio=0.9, mamba_scheduler_strategy='no_buffer', mamba_track_interval=256, linear_attn_backend='triton', linear_attn_decode_backend=None, linear_attn_prefill_backend=None, enable_hierarchical_cache=False, hicache_ratio=2.0, hicache_size=0, hicache_write_policy='write_through', hicache_io_backend='kernel', hicache_mem_layout='layer_first', disable_hicache_numa_detect=False, hicache_storage_backend=None, hicache_storage_prefetch_policy='best_effort', hicache_storage_backend_extra_config=None, enable_hisparse=False, hisparse_config=None, enable_lmcache=False, kt_weight_path=None, kt_method=None, kt_cpuinfer=None, kt_threadpool_count=None, kt_num_gpu_experts=None, kt_max_deferred_experts_per_token=None, dllm_algorithm=None, dllm_algorithm_config=None, enable_double_sparsity=False, ds_channel_config_path=None, ds_heavy_channel_num=32, ds_heavy_token_num=256, ds_heavy_channel_type='qk', ds_sparse_decode_threshold=4096, cpu_offload_gb=0, offload_group_size=-1, offload_num_in_group=1, offload_prefetch_step=1, offload_mode='cpu', multi_item_scoring_delimiter=None, disable_radix_cache=False, cuda_graph_max_bs=4, cuda_graph_bs=[1, 2, 4, 8, 12, 16, 24, 32, 40, 48, 56, 64, 72, 80, 88, 96, 104, 112, 120, 128, 136, 144, 152, 160, 168, 176, 184, 192, 200, 208, 216, 224, 232, 240, 248, 256], disable_cuda_graph=True, disable_cuda_graph_padding=False, enable_profile_cuda_graph=False, enable_cudagraph_gc=False, enable_layerwise_nvtx_marker=False, enable_nccl_nvls=False, enable_symm_mem=False, disable_flashinfer_cutlass_moe_fp4_allgather=False, enable_tokenizer_batch_encode=False, disable_tokenizer_batch_decode=False, disable_outlines_disk_cache=False, disable_custom_all_reduce=False, enable_mscclpp=False, enable_torch_symm_mem=False, pre_warm_nccl=False, disable_overlap_schedule=False, enable_mixed_chunk=False, enable_dp_attention=False, enable_dp_lm_head=False, enable_two_batch_overlap=False, enable_single_batch_overlap=False, tbo_token_distribution_threshold=0.48, enable_torch_compile=False, disable_piecewise_cuda_graph=False, enforce_piecewise_cuda_graph=False, enable_torch_compile_debug_mode=False, torch_compile_max_bs=32, piecewise_cuda_graph_max_tokens=8192, piecewise_cuda_graph_tokens=[4, 8, 12, 16, 20, 24, 28, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 352, 384, 416, 448, 480, 512, 576, 640, 704, 768, 832, 896, 960, 1024, 1280, 1536, 1792, 2048, 2304, 2560, 2816, 3072, 3328, 3584, 3840, 4096, 4608, 5120, 5632, 6144, 6656, 7168, 7680, 8192], piecewise_cuda_graph_compiler='eager', torchao_config='', enable_nan_detection=False, enable_p2p_check=False, triton_attention_reduce_in_fp32=False, triton_attention_num_kv_splits=8, triton_attention_split_tile_size=None, num_continuous_decode_steps=1, delete_ckpt_after_loading=False, enable_memory_saver=False, enable_weights_cpu_backup=False, enable_draft_weights_cpu_backup=False, allow_auto_truncate=False, enable_custom_logit_processor=False, flashinfer_mla_disable_ragged=False, disable_shared_experts_fusion=False, disable_chunked_prefix_cache=False, disable_fast_image_processor=False, keep_mm_feature_on_device=False, enable_return_hidden_states=False, enable_return_routed_experts=False, scheduler_recv_interval=1, numa_node=None, enable_deterministic_inference=False, rl_on_policy_target=None, enable_attn_tp_input_scattered=False, enable_nsa_prefill_context_parallel=False, nsa_prefill_cp_mode='round-robin-split', enable_fused_qk_norm_rope=False, enable_precise_embedding_interpolation=False, enable_fused_moe_sum_all_reduce=False, enable_prefill_context_parallel=False, prefill_cp_mode='in-seq-split', enable_dynamic_batch_tokenizer=False, dynamic_batch_tokenizer_batch_size=32, dynamic_batch_tokenizer_batch_timeout=0.002, debug_tensor_dump_output_folder=None, debug_tensor_dump_layers=None, debug_tensor_dump_input_file=None, debug_tensor_dump_inject=False, disaggregation_mode='null', disaggregation_transfer_backend='mooncake', disaggregation_bootstrap_port=8998, disaggregation_ib_device=None, disaggregation_decode_enable_offload_kvcache=False, num_reserved_decode_tokens=512, disaggregation_decode_polling_interval=1, encoder_only=False, language_only=False, encoder_transfer_backend='zmq_to_scheduler', encoder_urls=[], enable_adaptive_dispatch_to_encoder=False, custom_weight_loader=[], weight_loader_disable_mmap=False, remote_instance_weight_loader_seed_instance_ip=None, remote_instance_weight_loader_seed_instance_service_port=None, remote_instance_weight_loader_send_weights_group_ports=None, remote_instance_weight_loader_backend='nccl', remote_instance_weight_loader_start_seed_via_transfer_engine=False, modelexpress_config=None, enable_pdmux=False, pdmux_config_path=None, sm_group_num=8, mm_max_concurrent_calls=32, mm_per_request_timeout=10.0, enable_broadcast_mm_inputs_process=False, enable_prefix_mm_cache=False, mm_enable_dp_encoder=False, mm_process_config={}, limit_mm_data_per_request=None, enable_mm_global_cache=False, decrypted_config_file=None, decrypted_draft_config_file=None, forward_hooks=None)


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-25 12:54:11.372 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 12:54:11] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 12:54:11.372 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 12:54:11] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 12:54:11.372 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 12:54:11] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 12:54:11.372 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 12:54:11] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 12:54:11.372 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 12:54:11] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  25% Completed | 1/4 [00:00<00:02,  1.46it/s]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:01<00:01,  1.17it/s]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:02<00:00,  1.47it/s]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:03<00:00,  1.29it/s]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:03<00:00,  1.31it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:04<04:02,  4.26s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:04<04:02,  4.26s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:05<02:22,  2.54s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:05<02:22,  2.54s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:06<01:31,  1.66s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:06<01:31,  1.66s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:06<01:06,  1.24s/it]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:06<01:06,  1.24s/it]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:07<00:52,  1.01it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:07<00:52,  1.01it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:07<00:41,  1.24it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:07<00:41,  1.24it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:08<00:34,  1.48it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:08<00:34,  1.48it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:08<00:28,  1.73it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:08<00:28,  1.73it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:08<00:24,  1.98it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:08<00:24,  1.98it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:09<00:21,  2.27it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:09<00:21,  2.27it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:09<00:17,  2.63it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:09<00:17,  2.63it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:09<00:15,  2.90it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:09<00:15,  2.90it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:09<00:13,  3.32it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:09<00:13,  3.32it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:10<00:12,  3.56it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:10<00:12,  3.56it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:10<00:10,  4.03it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:10<00:10,  4.03it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:10<00:09,  4.44it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:10<00:09,  4.44it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:10<00:08,  5.02it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:10<00:08,  5.02it/s]

    Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:10<00:07,  5.43it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:10<00:07,  5.43it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:10<00:06,  6.29it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:10<00:06,  6.29it/s]

    Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:10<00:05,  7.04it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:10<00:05,  7.04it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:11<00:05,  7.04it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:11<00:03,  9.12it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:11<00:03,  9.12it/s]

    Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:11<00:03,  9.12it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:11<00:03, 10.35it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:11<00:03, 10.35it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:11<00:03, 10.35it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:11<00:02, 12.19it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:11<00:02, 12.19it/s]

    Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:11<00:02, 12.19it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:11<00:02, 13.00it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:11<00:02, 13.00it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:11<00:02, 13.00it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:11<00:02, 13.00it/s]

    Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:11<00:01, 15.96it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:11<00:01, 15.96it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:11<00:01, 15.96it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:11<00:01, 16.29it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:11<00:01, 16.29it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:11<00:01, 16.29it/s]

    Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:11<00:01, 16.29it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:11<00:01, 18.23it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:11<00:01, 18.23it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:11<00:01, 18.23it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:11<00:01, 18.23it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:12<00:00, 20.03it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:12<00:00, 20.03it/s]

    Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:12<00:00, 20.03it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:12<00:00, 20.03it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:12<00:00, 22.11it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:12<00:00, 22.11it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:12<00:00, 22.11it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:12<00:00, 22.11it/s]

    Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:12<00:00, 20.57it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:12<00:00, 20.57it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:12<00:00, 20.57it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:12<00:00, 20.57it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:12<00:00, 22.28it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:12<00:00, 22.28it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:12<00:00, 22.28it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:12<00:00, 22.28it/s]

    Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:12<00:00, 23.70it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:12<00:00, 23.70it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:12<00:00, 23.70it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:12<00:00, 23.70it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:12<00:00, 23.70it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:12<00:00, 25.81it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:12<00:00, 25.81it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:12<00:00, 25.81it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:12<00:00, 25.81it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:12<00:00,  4.55it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=83.36 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=83.36 GB):   2%|▏         | 1/58 [00:00<00:47,  1.19it/s]Capturing num tokens (num_tokens=7680 avail_mem=83.32 GB):   2%|▏         | 1/58 [00:00<00:47,  1.19it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=83.32 GB):   3%|▎         | 2/58 [00:01<00:43,  1.29it/s]Capturing num tokens (num_tokens=7168 avail_mem=83.35 GB):   3%|▎         | 2/58 [00:01<00:43,  1.29it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=83.35 GB):   5%|▌         | 3/58 [00:02<00:38,  1.41it/s]Capturing num tokens (num_tokens=6656 avail_mem=83.33 GB):   5%|▌         | 3/58 [00:02<00:38,  1.41it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=83.33 GB):   7%|▋         | 4/58 [00:02<00:35,  1.53it/s]Capturing num tokens (num_tokens=6144 avail_mem=83.34 GB):   7%|▋         | 4/58 [00:02<00:35,  1.53it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=83.34 GB):   9%|▊         | 5/58 [00:03<00:31,  1.69it/s]Capturing num tokens (num_tokens=5632 avail_mem=83.31 GB):   9%|▊         | 5/58 [00:03<00:31,  1.69it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=83.31 GB):  10%|█         | 6/58 [00:03<00:29,  1.76it/s]Capturing num tokens (num_tokens=5120 avail_mem=83.33 GB):  10%|█         | 6/58 [00:03<00:29,  1.76it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=83.33 GB):  12%|█▏        | 7/58 [00:04<00:26,  1.93it/s]Capturing num tokens (num_tokens=4608 avail_mem=83.32 GB):  12%|█▏        | 7/58 [00:04<00:26,  1.93it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=83.32 GB):  14%|█▍        | 8/58 [00:04<00:22,  2.19it/s]Capturing num tokens (num_tokens=4096 avail_mem=83.31 GB):  14%|█▍        | 8/58 [00:04<00:22,  2.19it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=83.31 GB):  16%|█▌        | 9/58 [00:04<00:19,  2.46it/s]Capturing num tokens (num_tokens=3840 avail_mem=83.30 GB):  16%|█▌        | 9/58 [00:04<00:19,  2.46it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=83.30 GB):  17%|█▋        | 10/58 [00:05<00:18,  2.65it/s]Capturing num tokens (num_tokens=3584 avail_mem=83.30 GB):  17%|█▋        | 10/58 [00:05<00:18,  2.65it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=83.30 GB):  19%|█▉        | 11/58 [00:05<00:16,  2.93it/s]Capturing num tokens (num_tokens=3328 avail_mem=83.30 GB):  19%|█▉        | 11/58 [00:05<00:16,  2.93it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=83.30 GB):  21%|██        | 12/58 [00:05<00:14,  3.20it/s]Capturing num tokens (num_tokens=3072 avail_mem=83.30 GB):  21%|██        | 12/58 [00:05<00:14,  3.20it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=83.30 GB):  22%|██▏       | 13/58 [00:05<00:12,  3.49it/s]Capturing num tokens (num_tokens=2816 avail_mem=83.30 GB):  22%|██▏       | 13/58 [00:05<00:12,  3.49it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=83.30 GB):  24%|██▍       | 14/58 [00:06<00:11,  3.83it/s]Capturing num tokens (num_tokens=2560 avail_mem=83.30 GB):  24%|██▍       | 14/58 [00:06<00:11,  3.83it/s]Capturing num tokens (num_tokens=2560 avail_mem=83.30 GB):  26%|██▌       | 15/58 [00:06<00:10,  4.21it/s]Capturing num tokens (num_tokens=2304 avail_mem=83.30 GB):  26%|██▌       | 15/58 [00:06<00:10,  4.21it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=83.30 GB):  28%|██▊       | 16/58 [00:06<00:09,  4.59it/s]Capturing num tokens (num_tokens=2048 avail_mem=83.29 GB):  28%|██▊       | 16/58 [00:06<00:09,  4.59it/s]Capturing num tokens (num_tokens=2048 avail_mem=83.29 GB):  29%|██▉       | 17/58 [00:06<00:07,  5.17it/s]Capturing num tokens (num_tokens=1792 avail_mem=83.29 GB):  29%|██▉       | 17/58 [00:06<00:07,  5.17it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=83.29 GB):  31%|███       | 18/58 [00:06<00:06,  5.72it/s]Capturing num tokens (num_tokens=1536 avail_mem=83.29 GB):  31%|███       | 18/58 [00:06<00:06,  5.72it/s]Capturing num tokens (num_tokens=1536 avail_mem=83.29 GB):  33%|███▎      | 19/58 [00:06<00:06,  6.38it/s]Capturing num tokens (num_tokens=1280 avail_mem=83.29 GB):  33%|███▎      | 19/58 [00:06<00:06,  6.38it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=83.29 GB):  34%|███▍      | 20/58 [00:06<00:05,  7.14it/s]Capturing num tokens (num_tokens=1024 avail_mem=83.29 GB):  34%|███▍      | 20/58 [00:06<00:05,  7.14it/s]Capturing num tokens (num_tokens=960 avail_mem=83.29 GB):  34%|███▍      | 20/58 [00:06<00:05,  7.14it/s] Capturing num tokens (num_tokens=960 avail_mem=83.29 GB):  38%|███▊      | 22/58 [00:07<00:04,  8.59it/s]Capturing num tokens (num_tokens=896 avail_mem=83.28 GB):  38%|███▊      | 22/58 [00:07<00:04,  8.59it/s]

    Capturing num tokens (num_tokens=832 avail_mem=83.28 GB):  38%|███▊      | 22/58 [00:07<00:04,  8.59it/s]Capturing num tokens (num_tokens=832 avail_mem=83.28 GB):  41%|████▏     | 24/58 [00:07<00:03,  9.68it/s]Capturing num tokens (num_tokens=768 avail_mem=83.28 GB):  41%|████▏     | 24/58 [00:07<00:03,  9.68it/s]Capturing num tokens (num_tokens=704 avail_mem=83.27 GB):  41%|████▏     | 24/58 [00:07<00:03,  9.68it/s]

    Capturing num tokens (num_tokens=704 avail_mem=83.27 GB):  45%|████▍     | 26/58 [00:07<00:03, 10.64it/s]Capturing num tokens (num_tokens=640 avail_mem=83.27 GB):  45%|████▍     | 26/58 [00:07<00:03, 10.64it/s]Capturing num tokens (num_tokens=576 avail_mem=83.27 GB):  45%|████▍     | 26/58 [00:07<00:03, 10.64it/s]Capturing num tokens (num_tokens=576 avail_mem=83.27 GB):  48%|████▊     | 28/58 [00:07<00:02, 11.75it/s]Capturing num tokens (num_tokens=512 avail_mem=83.26 GB):  48%|████▊     | 28/58 [00:07<00:02, 11.75it/s]Capturing num tokens (num_tokens=480 avail_mem=83.26 GB):  48%|████▊     | 28/58 [00:07<00:02, 11.75it/s]

    Capturing num tokens (num_tokens=480 avail_mem=83.26 GB):  52%|█████▏    | 30/58 [00:07<00:02, 12.84it/s]Capturing num tokens (num_tokens=448 avail_mem=83.25 GB):  52%|█████▏    | 30/58 [00:07<00:02, 12.84it/s]Capturing num tokens (num_tokens=416 avail_mem=83.25 GB):  52%|█████▏    | 30/58 [00:07<00:02, 12.84it/s]Capturing num tokens (num_tokens=416 avail_mem=83.25 GB):  55%|█████▌    | 32/58 [00:07<00:01, 13.49it/s]Capturing num tokens (num_tokens=384 avail_mem=83.24 GB):  55%|█████▌    | 32/58 [00:07<00:01, 13.49it/s]Capturing num tokens (num_tokens=352 avail_mem=83.24 GB):  55%|█████▌    | 32/58 [00:07<00:01, 13.49it/s]

    Capturing num tokens (num_tokens=352 avail_mem=83.24 GB):  59%|█████▊    | 34/58 [00:07<00:01, 14.61it/s]Capturing num tokens (num_tokens=320 avail_mem=83.24 GB):  59%|█████▊    | 34/58 [00:07<00:01, 14.61it/s]Capturing num tokens (num_tokens=288 avail_mem=83.23 GB):  59%|█████▊    | 34/58 [00:07<00:01, 14.61it/s]Capturing num tokens (num_tokens=288 avail_mem=83.23 GB):  62%|██████▏   | 36/58 [00:08<00:01, 15.74it/s]Capturing num tokens (num_tokens=256 avail_mem=83.23 GB):  62%|██████▏   | 36/58 [00:08<00:01, 15.74it/s]Capturing num tokens (num_tokens=240 avail_mem=83.22 GB):  62%|██████▏   | 36/58 [00:08<00:01, 15.74it/s]

    Capturing num tokens (num_tokens=240 avail_mem=83.22 GB):  66%|██████▌   | 38/58 [00:08<00:01, 16.04it/s]Capturing num tokens (num_tokens=224 avail_mem=83.22 GB):  66%|██████▌   | 38/58 [00:08<00:01, 16.04it/s]Capturing num tokens (num_tokens=208 avail_mem=83.21 GB):  66%|██████▌   | 38/58 [00:08<00:01, 16.04it/s]Capturing num tokens (num_tokens=192 avail_mem=83.21 GB):  66%|██████▌   | 38/58 [00:08<00:01, 16.04it/s]Capturing num tokens (num_tokens=192 avail_mem=83.21 GB):  71%|███████   | 41/58 [00:08<00:00, 17.61it/s]Capturing num tokens (num_tokens=176 avail_mem=83.20 GB):  71%|███████   | 41/58 [00:08<00:00, 17.61it/s]Capturing num tokens (num_tokens=160 avail_mem=83.20 GB):  71%|███████   | 41/58 [00:08<00:00, 17.61it/s]

    Capturing num tokens (num_tokens=144 avail_mem=83.19 GB):  71%|███████   | 41/58 [00:08<00:00, 17.61it/s]Capturing num tokens (num_tokens=144 avail_mem=83.19 GB):  76%|███████▌  | 44/58 [00:08<00:00, 18.71it/s]Capturing num tokens (num_tokens=128 avail_mem=83.19 GB):  76%|███████▌  | 44/58 [00:08<00:00, 18.71it/s]Capturing num tokens (num_tokens=112 avail_mem=83.20 GB):  76%|███████▌  | 44/58 [00:08<00:00, 18.71it/s]Capturing num tokens (num_tokens=96 avail_mem=83.20 GB):  76%|███████▌  | 44/58 [00:08<00:00, 18.71it/s] Capturing num tokens (num_tokens=96 avail_mem=83.20 GB):  81%|████████  | 47/58 [00:08<00:00, 19.86it/s]Capturing num tokens (num_tokens=80 avail_mem=83.19 GB):  81%|████████  | 47/58 [00:08<00:00, 19.86it/s]

    Capturing num tokens (num_tokens=64 avail_mem=83.19 GB):  81%|████████  | 47/58 [00:08<00:00, 19.86it/s]Capturing num tokens (num_tokens=48 avail_mem=83.19 GB):  81%|████████  | 47/58 [00:08<00:00, 19.86it/s]Capturing num tokens (num_tokens=48 avail_mem=83.19 GB):  86%|████████▌ | 50/58 [00:08<00:00, 20.74it/s]Capturing num tokens (num_tokens=32 avail_mem=83.18 GB):  86%|████████▌ | 50/58 [00:08<00:00, 20.74it/s]Capturing num tokens (num_tokens=28 avail_mem=83.18 GB):  86%|████████▌ | 50/58 [00:08<00:00, 20.74it/s]Capturing num tokens (num_tokens=24 avail_mem=83.17 GB):  86%|████████▌ | 50/58 [00:08<00:00, 20.74it/s]

    Capturing num tokens (num_tokens=24 avail_mem=83.17 GB):  91%|█████████▏| 53/58 [00:08<00:00, 21.15it/s]Capturing num tokens (num_tokens=20 avail_mem=83.17 GB):  91%|█████████▏| 53/58 [00:08<00:00, 21.15it/s]Capturing num tokens (num_tokens=16 avail_mem=83.16 GB):  91%|█████████▏| 53/58 [00:08<00:00, 21.15it/s]Capturing num tokens (num_tokens=12 avail_mem=83.16 GB):  91%|█████████▏| 53/58 [00:08<00:00, 21.15it/s]Capturing num tokens (num_tokens=12 avail_mem=83.16 GB):  97%|█████████▋| 56/58 [00:08<00:00, 21.48it/s]Capturing num tokens (num_tokens=8 avail_mem=83.16 GB):  97%|█████████▋| 56/58 [00:08<00:00, 21.48it/s] Capturing num tokens (num_tokens=4 avail_mem=83.15 GB):  97%|█████████▋| 56/58 [00:08<00:00, 21.48it/s]

    Capturing num tokens (num_tokens=4 avail_mem=83.15 GB): 100%|██████████| 58/58 [00:09<00:00,  6.42it/s]


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



<strong style='color: #00008B;'>Prompt: <|begin_of_text|><|start_header_id|>system<|end_header_id|><br><br>Cutting Knowledge Date: December 2023<br>Today Date: 26 Jul 2024<br><br><|eot_id|><|start_header_id|>user<|end_header_id|><br><br>Paris is the capital of<|eot_id|><|start_header_id|>assistant<|end_header_id|><br><br><br>Generated text: Paris is the capital of France.</strong>



```python
llm.shutdown()
```
