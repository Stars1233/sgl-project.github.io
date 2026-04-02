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
    2026-04-02 17:53:08.282 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 17:53:08] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 17:53:08.282 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 17:53:08] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 17:53:08.282 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 17:53:08] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 17:53:08.282 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 17:53:08] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 17:53:08.282 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 17:53:08] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/4 [00:00<?, ?it/s]Multi-thread loading shards:  25% Completed | 1/4 [00:00<00:00,  5.21it/s]

    Multi-thread loading shards:  50% Completed | 2/4 [00:01<00:01,  1.72it/s]

    Multi-thread loading shards:  75% Completed | 3/4 [00:02<00:00,  1.19it/s]

    Multi-thread loading shards: 100% Completed | 4/4 [00:03<00:00,  1.07s/it]Multi-thread loading shards: 100% Completed | 4/4 [00:03<00:00,  1.11it/s]


    2026-04-02 17:53:17,279 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 17:53:17] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:20,  3.52s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:20,  3.52s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<02:00,  2.14s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<02:00,  2.14s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:05<01:15,  1.37s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:05<01:15,  1.37s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:05<00:53,  1.01it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:05<00:53,  1.01it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:40,  1.32it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:40,  1.32it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:06<00:30,  1.68it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:06<00:30,  1.68it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:06<00:24,  2.10it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:06<00:24,  2.10it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:19,  2.57it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:19,  2.57it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:15,  3.14it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:15,  3.14it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:12,  3.78it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:12,  3.78it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:07<00:10,  4.41it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:07<00:10,  4.41it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:07<00:09,  5.10it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:07<00:09,  5.10it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:07,  5.81it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:07,  5.81it/s]

    Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:07<00:07,  5.81it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:05,  7.75it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:05,  7.75it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:07<00:05,  7.75it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:07<00:04,  9.92it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:07<00:04,  9.92it/s]

    Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:07<00:04,  9.92it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:07<00:04,  9.92it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:07<00:02, 13.19it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:07<00:02, 13.19it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:07<00:02, 13.19it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:07<00:02, 14.52it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:07<00:02, 14.52it/s]

    Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:07<00:02, 14.52it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:07<00:02, 14.72it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:07<00:02, 14.72it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:08<00:02, 14.72it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:08<00:02, 15.49it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:08<00:02, 15.49it/s]

    Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:08<00:02, 15.49it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:08<00:02, 15.49it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:08<00:01, 17.73it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:08<00:01, 17.73it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:08<00:01, 17.73it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:08<00:01, 17.73it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:08<00:01, 17.73it/s]

    Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:08<00:01, 20.85it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:08<00:01, 20.85it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:08<00:01, 20.85it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:08<00:01, 20.85it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:08<00:01, 21.85it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:08<00:01, 21.85it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:08<00:01, 21.85it/s]

    Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:08<00:01, 21.85it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:08<00:00, 23.00it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:08<00:00, 23.00it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:08<00:00, 23.00it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:08<00:00, 23.00it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:08<00:00, 24.57it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:08<00:00, 24.57it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:08<00:00, 24.57it/s]

    Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:08<00:00, 24.57it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:08<00:00, 24.89it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:08<00:00, 24.89it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:08<00:00, 24.89it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:08<00:00, 24.89it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:08<00:00, 25.60it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:08<00:00, 25.60it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:08<00:00, 25.60it/s]

    Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:08<00:00, 25.60it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:09<00:00, 26.50it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:09<00:00, 26.50it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:09<00:00, 26.50it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:09<00:00, 26.50it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:09<00:00, 26.50it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:09<00:00, 28.81it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:09<00:00, 28.81it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:09<00:00, 28.81it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:09<00:00, 28.81it/s]

    Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00,  6.29it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=88.93 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=88.93 GB):   2%|▏         | 1/58 [00:00<00:38,  1.50it/s]Capturing num tokens (num_tokens=7680 avail_mem=88.90 GB):   2%|▏         | 1/58 [00:00<00:38,  1.50it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=88.90 GB):   3%|▎         | 2/58 [00:01<00:34,  1.60it/s]Capturing num tokens (num_tokens=7168 avail_mem=88.91 GB):   3%|▎         | 2/58 [00:01<00:34,  1.60it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=88.91 GB):   5%|▌         | 3/58 [00:01<00:30,  1.78it/s]Capturing num tokens (num_tokens=6656 avail_mem=88.91 GB):   5%|▌         | 3/58 [00:01<00:30,  1.78it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=88.91 GB):   7%|▋         | 4/58 [00:02<00:27,  1.93it/s]Capturing num tokens (num_tokens=6144 avail_mem=88.91 GB):   7%|▋         | 4/58 [00:02<00:27,  1.93it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=88.91 GB):   9%|▊         | 5/58 [00:02<00:24,  2.15it/s]Capturing num tokens (num_tokens=5632 avail_mem=88.92 GB):   9%|▊         | 5/58 [00:02<00:24,  2.15it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=88.92 GB):  10%|█         | 6/58 [00:02<00:21,  2.40it/s]Capturing num tokens (num_tokens=5120 avail_mem=88.92 GB):  10%|█         | 6/58 [00:02<00:21,  2.40it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=88.92 GB):  12%|█▏        | 7/58 [00:03<00:20,  2.54it/s]Capturing num tokens (num_tokens=4608 avail_mem=88.92 GB):  12%|█▏        | 7/58 [00:03<00:20,  2.54it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=88.92 GB):  14%|█▍        | 8/58 [00:03<00:18,  2.69it/s]Capturing num tokens (num_tokens=4096 avail_mem=88.92 GB):  14%|█▍        | 8/58 [00:03<00:18,  2.69it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=88.92 GB):  16%|█▌        | 9/58 [00:03<00:17,  2.87it/s]Capturing num tokens (num_tokens=3840 avail_mem=88.91 GB):  16%|█▌        | 9/58 [00:03<00:17,  2.87it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=88.91 GB):  17%|█▋        | 10/58 [00:04<00:15,  3.06it/s]Capturing num tokens (num_tokens=3584 avail_mem=88.91 GB):  17%|█▋        | 10/58 [00:04<00:15,  3.06it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=88.91 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.30it/s]Capturing num tokens (num_tokens=3328 avail_mem=88.89 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.30it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=88.89 GB):  21%|██        | 12/58 [00:04<00:12,  3.56it/s]Capturing num tokens (num_tokens=3072 avail_mem=88.89 GB):  21%|██        | 12/58 [00:04<00:12,  3.56it/s]Capturing num tokens (num_tokens=3072 avail_mem=88.89 GB):  22%|██▏       | 13/58 [00:04<00:11,  4.08it/s]Capturing num tokens (num_tokens=2816 avail_mem=88.86 GB):  22%|██▏       | 13/58 [00:04<00:11,  4.08it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=88.86 GB):  24%|██▍       | 14/58 [00:04<00:09,  4.70it/s]Capturing num tokens (num_tokens=2560 avail_mem=88.87 GB):  24%|██▍       | 14/58 [00:04<00:09,  4.70it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=88.87 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.71it/s]Capturing num tokens (num_tokens=2304 avail_mem=98.19 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.71it/s]Capturing num tokens (num_tokens=2304 avail_mem=98.19 GB):  28%|██▊       | 16/58 [00:05<00:07,  5.46it/s]Capturing num tokens (num_tokens=2048 avail_mem=98.19 GB):  28%|██▊       | 16/58 [00:05<00:07,  5.46it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=98.18 GB):  28%|██▊       | 16/58 [00:05<00:07,  5.46it/s]Capturing num tokens (num_tokens=1792 avail_mem=98.18 GB):  31%|███       | 18/58 [00:05<00:05,  7.12it/s]Capturing num tokens (num_tokens=1536 avail_mem=98.17 GB):  31%|███       | 18/58 [00:05<00:05,  7.12it/s]Capturing num tokens (num_tokens=1280 avail_mem=98.17 GB):  31%|███       | 18/58 [00:05<00:05,  7.12it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=98.17 GB):  34%|███▍      | 20/58 [00:05<00:04,  8.84it/s]Capturing num tokens (num_tokens=1024 avail_mem=98.18 GB):  34%|███▍      | 20/58 [00:05<00:04,  8.84it/s]Capturing num tokens (num_tokens=960 avail_mem=98.17 GB):  34%|███▍      | 20/58 [00:05<00:04,  8.84it/s] Capturing num tokens (num_tokens=960 avail_mem=98.17 GB):  38%|███▊      | 22/58 [00:05<00:03, 10.66it/s]Capturing num tokens (num_tokens=896 avail_mem=98.14 GB):  38%|███▊      | 22/58 [00:05<00:03, 10.66it/s]Capturing num tokens (num_tokens=832 avail_mem=98.15 GB):  38%|███▊      | 22/58 [00:05<00:03, 10.66it/s]

    Capturing num tokens (num_tokens=832 avail_mem=98.15 GB):  41%|████▏     | 24/58 [00:05<00:02, 12.49it/s]Capturing num tokens (num_tokens=768 avail_mem=98.14 GB):  41%|████▏     | 24/58 [00:05<00:02, 12.49it/s]Capturing num tokens (num_tokens=704 avail_mem=98.13 GB):  41%|████▏     | 24/58 [00:05<00:02, 12.49it/s]Capturing num tokens (num_tokens=640 avail_mem=98.13 GB):  41%|████▏     | 24/58 [00:05<00:02, 12.49it/s]Capturing num tokens (num_tokens=640 avail_mem=98.13 GB):  47%|████▋     | 27/58 [00:05<00:02, 15.00it/s]Capturing num tokens (num_tokens=576 avail_mem=98.12 GB):  47%|████▋     | 27/58 [00:05<00:02, 15.00it/s]Capturing num tokens (num_tokens=512 avail_mem=98.09 GB):  47%|████▋     | 27/58 [00:05<00:02, 15.00it/s]

    Capturing num tokens (num_tokens=480 avail_mem=98.08 GB):  47%|████▋     | 27/58 [00:06<00:02, 15.00it/s]Capturing num tokens (num_tokens=480 avail_mem=98.08 GB):  52%|█████▏    | 30/58 [00:06<00:01, 17.13it/s]Capturing num tokens (num_tokens=448 avail_mem=98.09 GB):  52%|█████▏    | 30/58 [00:06<00:01, 17.13it/s]Capturing num tokens (num_tokens=416 avail_mem=98.08 GB):  52%|█████▏    | 30/58 [00:06<00:01, 17.13it/s]Capturing num tokens (num_tokens=384 avail_mem=98.08 GB):  52%|█████▏    | 30/58 [00:06<00:01, 17.13it/s]Capturing num tokens (num_tokens=384 avail_mem=98.08 GB):  57%|█████▋    | 33/58 [00:06<00:01, 19.11it/s]Capturing num tokens (num_tokens=352 avail_mem=98.07 GB):  57%|█████▋    | 33/58 [00:06<00:01, 19.11it/s]Capturing num tokens (num_tokens=320 avail_mem=98.06 GB):  57%|█████▋    | 33/58 [00:06<00:01, 19.11it/s]

    Capturing num tokens (num_tokens=288 avail_mem=98.06 GB):  57%|█████▋    | 33/58 [00:06<00:01, 19.11it/s]Capturing num tokens (num_tokens=256 avail_mem=98.05 GB):  57%|█████▋    | 33/58 [00:06<00:01, 19.11it/s]Capturing num tokens (num_tokens=256 avail_mem=98.05 GB):  64%|██████▍   | 37/58 [00:06<00:00, 23.13it/s]Capturing num tokens (num_tokens=240 avail_mem=98.05 GB):  64%|██████▍   | 37/58 [00:06<00:00, 23.13it/s]Capturing num tokens (num_tokens=224 avail_mem=98.05 GB):  64%|██████▍   | 37/58 [00:06<00:00, 23.13it/s]Capturing num tokens (num_tokens=208 avail_mem=98.04 GB):  64%|██████▍   | 37/58 [00:06<00:00, 23.13it/s]Capturing num tokens (num_tokens=192 avail_mem=98.04 GB):  64%|██████▍   | 37/58 [00:06<00:00, 23.13it/s]Capturing num tokens (num_tokens=192 avail_mem=98.04 GB):  71%|███████   | 41/58 [00:06<00:00, 26.08it/s]Capturing num tokens (num_tokens=176 avail_mem=98.03 GB):  71%|███████   | 41/58 [00:06<00:00, 26.08it/s]

    Capturing num tokens (num_tokens=160 avail_mem=98.03 GB):  71%|███████   | 41/58 [00:06<00:00, 26.08it/s]Capturing num tokens (num_tokens=144 avail_mem=98.02 GB):  71%|███████   | 41/58 [00:06<00:00, 26.08it/s]Capturing num tokens (num_tokens=128 avail_mem=98.02 GB):  71%|███████   | 41/58 [00:06<00:00, 26.08it/s]Capturing num tokens (num_tokens=128 avail_mem=98.02 GB):  78%|███████▊  | 45/58 [00:06<00:00, 28.34it/s]Capturing num tokens (num_tokens=112 avail_mem=98.03 GB):  78%|███████▊  | 45/58 [00:06<00:00, 28.34it/s]Capturing num tokens (num_tokens=96 avail_mem=98.03 GB):  78%|███████▊  | 45/58 [00:06<00:00, 28.34it/s] Capturing num tokens (num_tokens=80 avail_mem=98.02 GB):  78%|███████▊  | 45/58 [00:06<00:00, 28.34it/s]Capturing num tokens (num_tokens=64 avail_mem=98.02 GB):  78%|███████▊  | 45/58 [00:06<00:00, 28.34it/s]

    Capturing num tokens (num_tokens=64 avail_mem=98.02 GB):  84%|████████▍ | 49/58 [00:06<00:00, 29.96it/s]Capturing num tokens (num_tokens=48 avail_mem=98.01 GB):  84%|████████▍ | 49/58 [00:06<00:00, 29.96it/s]Capturing num tokens (num_tokens=32 avail_mem=98.01 GB):  84%|████████▍ | 49/58 [00:06<00:00, 29.96it/s]Capturing num tokens (num_tokens=28 avail_mem=98.00 GB):  84%|████████▍ | 49/58 [00:06<00:00, 29.96it/s]Capturing num tokens (num_tokens=24 avail_mem=98.00 GB):  84%|████████▍ | 49/58 [00:06<00:00, 29.96it/s]Capturing num tokens (num_tokens=24 avail_mem=98.00 GB):  91%|█████████▏| 53/58 [00:06<00:00, 30.98it/s]Capturing num tokens (num_tokens=20 avail_mem=97.99 GB):  91%|█████████▏| 53/58 [00:06<00:00, 30.98it/s]Capturing num tokens (num_tokens=16 avail_mem=97.99 GB):  91%|█████████▏| 53/58 [00:06<00:00, 30.98it/s]Capturing num tokens (num_tokens=12 avail_mem=97.99 GB):  91%|█████████▏| 53/58 [00:06<00:00, 30.98it/s]

    Capturing num tokens (num_tokens=8 avail_mem=97.98 GB):  91%|█████████▏| 53/58 [00:06<00:00, 30.98it/s] Capturing num tokens (num_tokens=8 avail_mem=97.98 GB):  98%|█████████▊| 57/58 [00:06<00:00, 31.88it/s]Capturing num tokens (num_tokens=4 avail_mem=97.98 GB):  98%|█████████▊| 57/58 [00:06<00:00, 31.88it/s]Capturing num tokens (num_tokens=4 avail_mem=97.98 GB): 100%|██████████| 58/58 [00:06<00:00,  8.36it/s]


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


<strong style='color: #00008B;'>{'text': '{"name": "Paris", "population": 2147000}', 'output_ids': [5018, 609, 794, 330, 60704, 498, 330, 45541, 794, 220, 11584, 7007, 15, 92, 128009], 'meta_info': {'id': 'e893e14c64214dcaa6ba944800a34bce', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 50, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 15, 'cached_tokens': 1, 'cached_tokens_details': {'device': 1, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.17167761595919728, 'response_sent_to_client_ts': 1775152428.1929612}}</strong>



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


<strong style='color: #00008B;'>{'text': '{"name": "Paris", "population": 2147000}', 'output_ids': [5018, 609, 794, 330, 60704, 498, 330, 45541, 794, 220, 11584, 7007, 15, 92, 128009], 'meta_info': {'id': 'baab8fbd7b7641509afc3d8038963445', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 50, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 15, 'cached_tokens': 49, 'cached_tokens_details': {'device': 49, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.16138056991621852, 'response_sent_to_client_ts': 1775152428.3636053}}</strong>


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


<strong style='color: #00008B;'>[{'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': 'a283206a3d7849c6906607fa86de806e', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': {'device': 45, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.08965020999312401, 'response_sent_to_client_ts': 1775152428.4760134}}, {'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': '0cb3e9e2900645c188ec0f4abe4393a9', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': {'device': 45, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.08957823272794485, 'response_sent_to_client_ts': 1775152428.4760249}}, {'text': 'Paris is the capital of France', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 128009], 'meta_info': {'id': '9fc3ee9f879f47108e8c53e6e698f5a0', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 46, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 7, 'cached_tokens': 45, 'cached_tokens_details': {'device': 45, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.08953503891825676, 'response_sent_to_client_ts': 1775152428.4760296}}]</strong>


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


<strong style='color: #00008B;'>{'text': 'France', 'output_ids': [50100, 128009], 'meta_info': {'id': 'ad9445ce73f8405e8830db7df76d67c2', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2, 'cached_tokens': 31, 'cached_tokens_details': {'device': 31, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.033580235205590725, 'response_sent_to_client_ts': 1775152428.5176194}}</strong>


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


<strong style='color: #00008B;'>{'text': 'France.', 'output_ids': [50100, 13, 128009], 'meta_info': {'id': '3097b099f57d451db700694cb58a9f9d', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 3, 'cached_tokens': 40, 'cached_tokens_details': {'device': 40, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.12171924021095037, 'response_sent_to_client_ts': 1775152430.0430293}}</strong>



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


<strong style='color: #00008B;'>{'text': 'Paris is the capital of France.', 'output_ids': [60704, 374, 279, 6864, 315, 9822, 13, 128009], 'meta_info': {'id': '66f2364c818f4bde81f8b2033d6949fa', 'finish_reason': {'type': 'stop', 'matched': 128009}, 'prompt_tokens': 41, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 8, 'cached_tokens': 40, 'cached_tokens_details': {'device': 40, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.10291994269937277, 'response_sent_to_client_ts': 1775152430.1548889}}</strong>



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


    2026-04-02 17:54:07.878 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 17:54:07] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 17:54:07.878 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 17:54:07] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 17:54:07.878 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 17:54:07] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 17:54:07.879 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 17:54:07] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 17:54:07.879 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 17:54:07] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/4 [00:00<?, ?it/s]Multi-thread loading shards:  25% Completed | 1/4 [00:00<00:00,  7.25it/s]

    Multi-thread loading shards:  50% Completed | 2/4 [00:00<00:00,  2.18it/s]

    Multi-thread loading shards:  75% Completed | 3/4 [00:01<00:00,  1.48it/s]

    Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.29it/s]Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.49it/s]


    2026-04-02 17:54:24,887 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 17:54:24] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:19,  3.49s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:19,  3.49s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<02:09,  2.31s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<02:09,  2.31s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:05<01:27,  1.58s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:05<01:27,  1.58s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:06<01:05,  1.22s/it]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:06<01:05,  1.22s/it]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:06<00:53,  1.00s/it]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:06<00:53,  1.00s/it]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:07<00:43,  1.19it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:07<00:43,  1.19it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:07<00:36,  1.38it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:07<00:36,  1.38it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:08<00:31,  1.59it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:08<00:31,  1.59it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:08<00:26,  1.81it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:08<00:26,  1.81it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:09<00:22,  2.09it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:09<00:22,  2.09it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:09<00:20,  2.32it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:09<00:20,  2.32it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:09<00:17,  2.64it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:09<00:17,  2.64it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:09<00:14,  3.03it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:09<00:14,  3.03it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:10<00:12,  3.41it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:10<00:12,  3.41it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:10<00:10,  3.91it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:10<00:10,  3.91it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:10<00:09,  4.50it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:10<00:09,  4.50it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:10<00:07,  5.22it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:10<00:07,  5.22it/s]

    Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:10<00:06,  6.08it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:10<00:06,  6.08it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:10<00:06,  6.08it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:10<00:04,  8.09it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:10<00:04,  8.09it/s]

    Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:10<00:04,  8.09it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:10<00:03, 10.29it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:10<00:03, 10.29it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:10<00:03, 10.29it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:11<00:03, 10.29it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:11<00:02, 13.68it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:11<00:02, 13.68it/s]

    Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:11<00:02, 13.68it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:11<00:02, 13.68it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:11<00:01, 16.81it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:11<00:01, 16.81it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:11<00:01, 16.81it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:11<00:01, 16.81it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:11<00:01, 19.80it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:11<00:01, 19.80it/s]

    Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:11<00:01, 19.80it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:11<00:01, 19.80it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:11<00:01, 19.80it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:11<00:00, 23.03it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:11<00:00, 23.03it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:11<00:00, 23.03it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:11<00:00, 23.03it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:11<00:00, 23.03it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:11<00:00, 23.03it/s]

    Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:11<00:00, 28.02it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:11<00:00, 28.02it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:11<00:00, 28.02it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:11<00:00, 28.02it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:11<00:00, 28.02it/s]Compiling num tokens (num_tokens=128):  69%|██████▉   | 40/58 [00:11<00:00, 28.02it/s]Compiling num tokens (num_tokens=112):  69%|██████▉   | 40/58 [00:11<00:00, 28.02it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:11<00:00, 34.62it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:11<00:00, 34.62it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:11<00:00, 34.62it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:11<00:00, 34.62it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:11<00:00, 34.62it/s]Compiling num tokens (num_tokens=32):  79%|███████▉  | 46/58 [00:11<00:00, 34.62it/s]

    Compiling num tokens (num_tokens=28):  79%|███████▉  | 46/58 [00:11<00:00, 34.62it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:11<00:00, 41.09it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:11<00:00, 41.09it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:11<00:00, 41.09it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:11<00:00, 41.09it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:11<00:00, 41.09it/s]Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:11<00:00, 41.09it/s] Compiling num tokens (num_tokens=4):  90%|████████▉ | 52/58 [00:11<00:00, 41.09it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:11<00:00,  4.91it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=83.00 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=83.00 GB):   2%|▏         | 1/58 [00:01<01:05,  1.14s/it]Capturing num tokens (num_tokens=7680 avail_mem=83.20 GB):   2%|▏         | 1/58 [00:01<01:05,  1.14s/it]

    Capturing num tokens (num_tokens=7680 avail_mem=83.20 GB):   3%|▎         | 2/58 [00:02<01:00,  1.08s/it]Capturing num tokens (num_tokens=7168 avail_mem=83.49 GB):   3%|▎         | 2/58 [00:02<01:00,  1.08s/it]

    Capturing num tokens (num_tokens=7168 avail_mem=83.49 GB):   5%|▌         | 3/58 [00:03<00:54,  1.01it/s]Capturing num tokens (num_tokens=6656 avail_mem=83.63 GB):   5%|▌         | 3/58 [00:03<00:54,  1.01it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=83.63 GB):   7%|▋         | 4/58 [00:03<00:49,  1.09it/s]Capturing num tokens (num_tokens=6144 avail_mem=84.56 GB):   7%|▋         | 4/58 [00:03<00:49,  1.09it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=84.56 GB):   9%|▊         | 5/58 [00:04<00:43,  1.22it/s]Capturing num tokens (num_tokens=5632 avail_mem=84.58 GB):   9%|▊         | 5/58 [00:04<00:43,  1.22it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=84.58 GB):  10%|█         | 6/58 [00:05<00:38,  1.35it/s]Capturing num tokens (num_tokens=5120 avail_mem=84.19 GB):  10%|█         | 6/58 [00:05<00:38,  1.35it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=84.19 GB):  12%|█▏        | 7/58 [00:05<00:34,  1.48it/s]Capturing num tokens (num_tokens=4608 avail_mem=84.75 GB):  12%|█▏        | 7/58 [00:05<00:34,  1.48it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=84.75 GB):  14%|█▍        | 8/58 [00:06<00:29,  1.69it/s]Capturing num tokens (num_tokens=4096 avail_mem=84.75 GB):  14%|█▍        | 8/58 [00:06<00:29,  1.69it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=84.75 GB):  16%|█▌        | 9/58 [00:06<00:26,  1.84it/s]Capturing num tokens (num_tokens=3840 avail_mem=84.85 GB):  16%|█▌        | 9/58 [00:06<00:26,  1.84it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=84.85 GB):  17%|█▋        | 10/58 [00:06<00:23,  2.09it/s]Capturing num tokens (num_tokens=3584 avail_mem=85.19 GB):  17%|█▋        | 10/58 [00:06<00:23,  2.09it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=85.19 GB):  19%|█▉        | 11/58 [00:07<00:20,  2.28it/s]Capturing num tokens (num_tokens=3328 avail_mem=84.65 GB):  19%|█▉        | 11/58 [00:07<00:20,  2.28it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=84.65 GB):  21%|██        | 12/58 [00:07<00:17,  2.61it/s]Capturing num tokens (num_tokens=3072 avail_mem=85.18 GB):  21%|██        | 12/58 [00:07<00:17,  2.61it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=85.18 GB):  22%|██▏       | 13/58 [00:07<00:15,  2.93it/s]Capturing num tokens (num_tokens=2816 avail_mem=84.97 GB):  22%|██▏       | 13/58 [00:07<00:15,  2.93it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=84.97 GB):  24%|██▍       | 14/58 [00:07<00:13,  3.26it/s]Capturing num tokens (num_tokens=2560 avail_mem=85.05 GB):  24%|██▍       | 14/58 [00:07<00:13,  3.26it/s]Capturing num tokens (num_tokens=2560 avail_mem=85.05 GB):  26%|██▌       | 15/58 [00:08<00:11,  3.73it/s]Capturing num tokens (num_tokens=2304 avail_mem=85.04 GB):  26%|██▌       | 15/58 [00:08<00:11,  3.73it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=85.04 GB):  28%|██▊       | 16/58 [00:08<00:10,  4.20it/s]Capturing num tokens (num_tokens=2048 avail_mem=85.15 GB):  28%|██▊       | 16/58 [00:08<00:10,  4.20it/s]Capturing num tokens (num_tokens=2048 avail_mem=85.15 GB):  29%|██▉       | 17/58 [00:08<00:08,  4.71it/s]Capturing num tokens (num_tokens=1792 avail_mem=85.13 GB):  29%|██▉       | 17/58 [00:08<00:08,  4.71it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=85.13 GB):  31%|███       | 18/58 [00:08<00:07,  5.20it/s]Capturing num tokens (num_tokens=1536 avail_mem=85.03 GB):  31%|███       | 18/58 [00:08<00:07,  5.20it/s]Capturing num tokens (num_tokens=1536 avail_mem=85.03 GB):  33%|███▎      | 19/58 [00:08<00:06,  6.07it/s]Capturing num tokens (num_tokens=1280 avail_mem=85.11 GB):  33%|███▎      | 19/58 [00:08<00:06,  6.07it/s]Capturing num tokens (num_tokens=1024 avail_mem=85.06 GB):  33%|███▎      | 19/58 [00:08<00:06,  6.07it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=85.06 GB):  36%|███▌      | 21/58 [00:08<00:04,  7.51it/s]Capturing num tokens (num_tokens=960 avail_mem=85.04 GB):  36%|███▌      | 21/58 [00:08<00:04,  7.51it/s] Capturing num tokens (num_tokens=896 avail_mem=85.07 GB):  36%|███▌      | 21/58 [00:08<00:04,  7.51it/s]Capturing num tokens (num_tokens=896 avail_mem=85.07 GB):  40%|███▉      | 23/58 [00:09<00:03,  9.03it/s]Capturing num tokens (num_tokens=832 avail_mem=85.06 GB):  40%|███▉      | 23/58 [00:09<00:03,  9.03it/s]

    Capturing num tokens (num_tokens=768 avail_mem=84.99 GB):  40%|███▉      | 23/58 [00:09<00:03,  9.03it/s]Capturing num tokens (num_tokens=768 avail_mem=84.99 GB):  43%|████▎     | 25/58 [00:09<00:03, 10.09it/s]Capturing num tokens (num_tokens=704 avail_mem=85.03 GB):  43%|████▎     | 25/58 [00:09<00:03, 10.09it/s]Capturing num tokens (num_tokens=640 avail_mem=85.02 GB):  43%|████▎     | 25/58 [00:09<00:03, 10.09it/s]

    Capturing num tokens (num_tokens=640 avail_mem=85.02 GB):  47%|████▋     | 27/58 [00:09<00:02, 11.48it/s]Capturing num tokens (num_tokens=576 avail_mem=85.01 GB):  47%|████▋     | 27/58 [00:09<00:02, 11.48it/s]Capturing num tokens (num_tokens=512 avail_mem=84.99 GB):  47%|████▋     | 27/58 [00:09<00:02, 11.48it/s]Capturing num tokens (num_tokens=512 avail_mem=84.99 GB):  50%|█████     | 29/58 [00:09<00:02, 12.52it/s]Capturing num tokens (num_tokens=480 avail_mem=84.98 GB):  50%|█████     | 29/58 [00:09<00:02, 12.52it/s]Capturing num tokens (num_tokens=448 avail_mem=84.97 GB):  50%|█████     | 29/58 [00:09<00:02, 12.52it/s]

    Capturing num tokens (num_tokens=448 avail_mem=84.97 GB):  53%|█████▎    | 31/58 [00:09<00:02, 13.37it/s]Capturing num tokens (num_tokens=416 avail_mem=84.96 GB):  53%|█████▎    | 31/58 [00:09<00:02, 13.37it/s]Capturing num tokens (num_tokens=384 avail_mem=84.95 GB):  53%|█████▎    | 31/58 [00:09<00:02, 13.37it/s]Capturing num tokens (num_tokens=384 avail_mem=84.95 GB):  57%|█████▋    | 33/58 [00:09<00:01, 14.68it/s]Capturing num tokens (num_tokens=352 avail_mem=84.94 GB):  57%|█████▋    | 33/58 [00:09<00:01, 14.68it/s]Capturing num tokens (num_tokens=320 avail_mem=84.95 GB):  57%|█████▋    | 33/58 [00:09<00:01, 14.68it/s]

    Capturing num tokens (num_tokens=320 avail_mem=84.95 GB):  60%|██████    | 35/58 [00:09<00:01, 14.84it/s]Capturing num tokens (num_tokens=288 avail_mem=84.93 GB):  60%|██████    | 35/58 [00:09<00:01, 14.84it/s]Capturing num tokens (num_tokens=256 avail_mem=84.90 GB):  60%|██████    | 35/58 [00:09<00:01, 14.84it/s]Capturing num tokens (num_tokens=256 avail_mem=84.90 GB):  64%|██████▍   | 37/58 [00:09<00:01, 16.02it/s]Capturing num tokens (num_tokens=240 avail_mem=84.89 GB):  64%|██████▍   | 37/58 [00:09<00:01, 16.02it/s]Capturing num tokens (num_tokens=224 avail_mem=84.90 GB):  64%|██████▍   | 37/58 [00:09<00:01, 16.02it/s]

    Capturing num tokens (num_tokens=224 avail_mem=84.90 GB):  67%|██████▋   | 39/58 [00:09<00:01, 16.82it/s]Capturing num tokens (num_tokens=208 avail_mem=84.89 GB):  67%|██████▋   | 39/58 [00:09<00:01, 16.82it/s]Capturing num tokens (num_tokens=192 avail_mem=84.85 GB):  67%|██████▋   | 39/58 [00:10<00:01, 16.82it/s]Capturing num tokens (num_tokens=176 avail_mem=84.86 GB):  67%|██████▋   | 39/58 [00:10<00:01, 16.82it/s]Capturing num tokens (num_tokens=176 avail_mem=84.86 GB):  72%|███████▏  | 42/58 [00:10<00:00, 18.24it/s]Capturing num tokens (num_tokens=160 avail_mem=84.85 GB):  72%|███████▏  | 42/58 [00:10<00:00, 18.24it/s]Capturing num tokens (num_tokens=144 avail_mem=84.84 GB):  72%|███████▏  | 42/58 [00:10<00:00, 18.24it/s]

    Capturing num tokens (num_tokens=128 avail_mem=84.82 GB):  72%|███████▏  | 42/58 [00:10<00:00, 18.24it/s]Capturing num tokens (num_tokens=128 avail_mem=84.82 GB):  78%|███████▊  | 45/58 [00:10<00:00, 19.30it/s]Capturing num tokens (num_tokens=112 avail_mem=84.83 GB):  78%|███████▊  | 45/58 [00:10<00:00, 19.30it/s]Capturing num tokens (num_tokens=96 avail_mem=84.82 GB):  78%|███████▊  | 45/58 [00:10<00:00, 19.30it/s] Capturing num tokens (num_tokens=80 avail_mem=84.82 GB):  78%|███████▊  | 45/58 [00:10<00:00, 19.30it/s]Capturing num tokens (num_tokens=64 avail_mem=84.81 GB):  78%|███████▊  | 45/58 [00:10<00:00, 19.30it/s]Capturing num tokens (num_tokens=64 avail_mem=84.81 GB):  84%|████████▍ | 49/58 [00:10<00:00, 22.99it/s]Capturing num tokens (num_tokens=48 avail_mem=84.81 GB):  84%|████████▍ | 49/58 [00:10<00:00, 22.99it/s]Capturing num tokens (num_tokens=32 avail_mem=84.81 GB):  84%|████████▍ | 49/58 [00:10<00:00, 22.99it/s]

    Capturing num tokens (num_tokens=28 avail_mem=84.80 GB):  84%|████████▍ | 49/58 [00:10<00:00, 22.99it/s]Capturing num tokens (num_tokens=28 avail_mem=84.80 GB):  90%|████████▉ | 52/58 [00:10<00:00, 24.22it/s]Capturing num tokens (num_tokens=24 avail_mem=84.79 GB):  90%|████████▉ | 52/58 [00:10<00:00, 24.22it/s]Capturing num tokens (num_tokens=20 avail_mem=84.79 GB):  90%|████████▉ | 52/58 [00:10<00:00, 24.22it/s]Capturing num tokens (num_tokens=16 avail_mem=84.79 GB):  90%|████████▉ | 52/58 [00:10<00:00, 24.22it/s]Capturing num tokens (num_tokens=12 avail_mem=84.78 GB):  90%|████████▉ | 52/58 [00:10<00:00, 24.22it/s]Capturing num tokens (num_tokens=12 avail_mem=84.78 GB):  97%|█████████▋| 56/58 [00:10<00:00, 26.03it/s]Capturing num tokens (num_tokens=8 avail_mem=84.78 GB):  97%|█████████▋| 56/58 [00:10<00:00, 26.03it/s] Capturing num tokens (num_tokens=4 avail_mem=84.77 GB):  97%|█████████▋| 56/58 [00:10<00:00, 26.03it/s]

    Capturing num tokens (num_tokens=4 avail_mem=84.77 GB): 100%|██████████| 58/58 [00:10<00:00,  5.42it/s]


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
