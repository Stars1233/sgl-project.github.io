# Reasoning Parser

SGLang supports parsing reasoning content out from "normal" content for reasoning models such as [DeepSeek R1](https://huggingface.co/deepseek-ai/DeepSeek-R1).

## Supported Models & Parsers

| Model  |  Reasoning tags      | Parser | Notes |
|---------|-----------------------------|------------------|-------|
| [DeepSeek‑R1 series](https://huggingface.co/collections/deepseek-ai/deepseek-r1-678e1e131c0169c0bc89728d) | `<think>` … `</think>` | `deepseek-r1` | Supports all variants (R1, R1-0528, R1-Distill) |
| [DeepSeek‑V3 series](https://huggingface.co/deepseek-ai/DeepSeek-V3.1) | `<think>` … `</think>` | `deepseek-v3` | Including [DeepSeek‑V3.2](https://huggingface.co/deepseek-ai/DeepSeek-V3.2-Exp). Supports `thinking` parameter |
| [Standard Qwen3 models](https://huggingface.co/collections/Qwen/qwen3-67dd247413f0e2e4f653967f) | `<think>` … `</think>` | `qwen3` | Supports `enable_thinking` parameter |
| [Qwen3-Thinking models](https://huggingface.co/Qwen/Qwen3-235B-A22B-Thinking-2507) | `<think>` … `</think>` | `qwen3` or `qwen3-thinking` | Always generates thinking content |
| [Kimi K2 Thinking](https://huggingface.co/moonshotai/Kimi-K2-Thinking) | `◁think▷` … `◁/think▷` | `kimi_k2` | Uses special thinking delimiters. Also requires `--tool-call-parser kimi_k2` for tool use. |
| [GPT OSS](https://huggingface.co/openai/gpt-oss-120b) | `<\|channel\|>analysis<\|message\|>` … `<\|end\|>` | `gpt-oss` | N/A |
### Model-Specific Behaviors

**DeepSeek-R1 Family:**
- DeepSeek-R1: No `<think>` start tag, jumps directly to thinking content
- DeepSeek-R1-0528: Generates both `<think>` start and `</think>` end tags
- Both are handled by the same `deepseek-r1` parser

**DeepSeek-V3 Family:**
- DeepSeek-V3.1/V3.2: Hybrid model supporting both thinking and non-thinking modes, use the `deepseek-v3` parser and `thinking` parameter (NOTE: not `enable_thinking`)

**Qwen3 Family:**
- Standard Qwen3 (e.g., Qwen3-2507): Use `qwen3` parser, supports `enable_thinking` in chat templates
- Qwen3-Thinking (e.g., Qwen3-235B-A22B-Thinking-2507): Use `qwen3` or `qwen3-thinking` parser, always thinks

**Kimi K2:**
- Kimi K2 Thinking: Uses special `◁think▷` and `◁/think▷` tags. For agentic tool use, also specify `--tool-call-parser kimi_k2`.

**GPT OSS:**
- GPT OSS: Uses special `<|channel|>analysis<|message|>` and `<|end|>` tags

## Usage

### Launching the Server

Specify the `--reasoning-parser` option.


```python
import requests
from openai import OpenAI
from sglang.test.doc_patch import launch_server_cmd
from sglang.utils import wait_for_server, print_highlight, terminate_process

server_process, port = launch_server_cmd(
    "python3 -m sglang.launch_server --model-path deepseek-ai/DeepSeek-R1-Distill-Qwen-7B --host 0.0.0.0 --reasoning-parser deepseek-r1 --log-level warning"
)

wait_for_server(f"http://localhost:{port}", process=server_process)
```

    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


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
      File "/actions-runner/_work/sglang/sglang/python/sglang/srt/utils/common.py", line 2520, in retry
        return fn()
      File "/actions-runner/_work/sglang/sglang/python/sglang/srt/configs/model_config.py", line 727, in <lambda>
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
    [2026-03-27 20:45:00] retry() failed once (0th try, maximum 2 retries). Will delay 0.86s and retry. Error: The read operation timed out


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-27 20:45:09.427 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 20:45:09] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 20:45:09.427 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 20:45:09] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 20:45:09.427 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 20:45:09] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 20:45:09.427 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 20:45:09] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 20:45:09.428 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 20:45:09] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.09s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.42s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.37s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:54,  3.06s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:54,  3.06s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:24,  1.50s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:24,  1.50s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.10it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.10it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.62it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.62it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:23,  2.21it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:23,  2.21it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.85it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.85it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.57it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.57it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.37it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.37it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.25it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.25it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.25it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.93it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.93it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.93it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.44it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.44it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.44it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:04,  9.99it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:04,  9.99it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04,  9.99it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 11.90it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 11.90it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.90it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 11.90it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 15.18it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 15.18it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 15.18it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 15.18it/s]

    Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 15.18it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 20.41it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 20.41it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 20.41it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 20.41it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 20.41it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 20.41it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 27.04it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 27.04it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 27.04it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 27.04it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 27.04it/s]

    Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:05<00:01, 27.04it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:05<00:01, 27.04it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:05<00:00, 33.98it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:05<00:00, 33.98it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:05<00:00, 33.98it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:05<00:00, 33.98it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:05<00:00, 33.98it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:05<00:00, 33.98it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:05<00:00, 33.98it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 39.86it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 39.86it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:05<00:00, 39.86it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:05<00:00, 39.86it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:05<00:00, 39.86it/s]

    Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:05<00:00, 39.86it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:05<00:00, 39.86it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:05<00:00, 43.31it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:05<00:00, 43.31it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:05<00:00, 43.31it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:05<00:00, 43.31it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:05<00:00, 43.31it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:05<00:00, 43.31it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:05<00:00, 43.31it/s]Compiling num tokens (num_tokens=20):  81%|████████  | 47/58 [00:05<00:00, 43.31it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:05<00:00, 50.02it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:05<00:00, 50.02it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:05<00:00, 50.02it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:05<00:00, 50.02it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:05<00:00, 50.02it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:05<00:00,  9.77it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=42.61 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=42.61 GB):   2%|▏         | 1/58 [00:00<00:19,  2.88it/s]Capturing num tokens (num_tokens=7680 avail_mem=42.58 GB):   2%|▏         | 1/58 [00:00<00:19,  2.88it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=42.58 GB):   3%|▎         | 2/58 [00:00<00:19,  2.80it/s]Capturing num tokens (num_tokens=7168 avail_mem=42.58 GB):   3%|▎         | 2/58 [00:00<00:19,  2.80it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=42.58 GB):   5%|▌         | 3/58 [00:00<00:16,  3.29it/s]Capturing num tokens (num_tokens=6656 avail_mem=42.58 GB):   5%|▌         | 3/58 [00:00<00:16,  3.29it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=42.58 GB):   7%|▋         | 4/58 [00:01<00:14,  3.72it/s]Capturing num tokens (num_tokens=6144 avail_mem=42.58 GB):   7%|▋         | 4/58 [00:01<00:14,  3.72it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=42.58 GB):   9%|▊         | 5/58 [00:01<00:12,  4.09it/s]Capturing num tokens (num_tokens=5632 avail_mem=42.59 GB):   9%|▊         | 5/58 [00:01<00:12,  4.09it/s]Capturing num tokens (num_tokens=5632 avail_mem=42.59 GB):  10%|█         | 6/58 [00:01<00:11,  4.55it/s]Capturing num tokens (num_tokens=5120 avail_mem=42.59 GB):  10%|█         | 6/58 [00:01<00:11,  4.55it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=42.59 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.96it/s]Capturing num tokens (num_tokens=4608 avail_mem=42.59 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.96it/s]Capturing num tokens (num_tokens=4608 avail_mem=42.59 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.06it/s]Capturing num tokens (num_tokens=4096 avail_mem=42.60 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.06it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=42.60 GB):  16%|█▌        | 9/58 [00:02<00:08,  5.66it/s]Capturing num tokens (num_tokens=3840 avail_mem=42.60 GB):  16%|█▌        | 9/58 [00:02<00:08,  5.66it/s]Capturing num tokens (num_tokens=3840 avail_mem=42.60 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.23it/s]Capturing num tokens (num_tokens=3584 avail_mem=42.60 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.23it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=42.60 GB):  19%|█▉        | 11/58 [00:02<00:06,  6.83it/s]Capturing num tokens (num_tokens=3328 avail_mem=42.60 GB):  19%|█▉        | 11/58 [00:02<00:06,  6.83it/s]Capturing num tokens (num_tokens=3328 avail_mem=42.60 GB):  21%|██        | 12/58 [00:02<00:06,  7.49it/s]Capturing num tokens (num_tokens=3072 avail_mem=42.60 GB):  21%|██        | 12/58 [00:02<00:06,  7.49it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=42.60 GB):  21%|██        | 12/58 [00:02<00:06,  7.49it/s]Capturing num tokens (num_tokens=2816 avail_mem=42.60 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.75it/s]Capturing num tokens (num_tokens=2560 avail_mem=42.60 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.75it/s]Capturing num tokens (num_tokens=2304 avail_mem=42.60 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.75it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=42.60 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.16it/s]Capturing num tokens (num_tokens=2048 avail_mem=42.60 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.16it/s]Capturing num tokens (num_tokens=1792 avail_mem=42.60 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.16it/s]Capturing num tokens (num_tokens=1792 avail_mem=42.60 GB):  31%|███       | 18/58 [00:02<00:03, 11.78it/s]Capturing num tokens (num_tokens=1536 avail_mem=42.59 GB):  31%|███       | 18/58 [00:02<00:03, 11.78it/s]Capturing num tokens (num_tokens=1280 avail_mem=42.60 GB):  31%|███       | 18/58 [00:02<00:03, 11.78it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=42.60 GB):  31%|███       | 18/58 [00:02<00:03, 11.78it/s]Capturing num tokens (num_tokens=1024 avail_mem=42.60 GB):  36%|███▌      | 21/58 [00:02<00:02, 14.87it/s]Capturing num tokens (num_tokens=960 avail_mem=42.59 GB):  36%|███▌      | 21/58 [00:02<00:02, 14.87it/s] Capturing num tokens (num_tokens=896 avail_mem=42.59 GB):  36%|███▌      | 21/58 [00:02<00:02, 14.87it/s]Capturing num tokens (num_tokens=832 avail_mem=42.59 GB):  36%|███▌      | 21/58 [00:03<00:02, 14.87it/s]Capturing num tokens (num_tokens=832 avail_mem=42.59 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.97it/s]Capturing num tokens (num_tokens=768 avail_mem=42.58 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.97it/s]Capturing num tokens (num_tokens=704 avail_mem=42.58 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.97it/s]

    Capturing num tokens (num_tokens=640 avail_mem=42.58 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.97it/s]Capturing num tokens (num_tokens=576 avail_mem=42.57 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.97it/s]Capturing num tokens (num_tokens=576 avail_mem=42.57 GB):  48%|████▊     | 28/58 [00:03<00:01, 21.71it/s]Capturing num tokens (num_tokens=512 avail_mem=42.57 GB):  48%|████▊     | 28/58 [00:03<00:01, 21.71it/s]Capturing num tokens (num_tokens=480 avail_mem=42.56 GB):  48%|████▊     | 28/58 [00:03<00:01, 21.71it/s]Capturing num tokens (num_tokens=448 avail_mem=42.56 GB):  48%|████▊     | 28/58 [00:03<00:01, 21.71it/s]Capturing num tokens (num_tokens=416 avail_mem=42.56 GB):  48%|████▊     | 28/58 [00:03<00:01, 21.71it/s]Capturing num tokens (num_tokens=416 avail_mem=42.56 GB):  55%|█████▌    | 32/58 [00:03<00:01, 25.21it/s]Capturing num tokens (num_tokens=384 avail_mem=42.55 GB):  55%|█████▌    | 32/58 [00:03<00:01, 25.21it/s]

    Capturing num tokens (num_tokens=352 avail_mem=42.55 GB):  55%|█████▌    | 32/58 [00:03<00:01, 25.21it/s]Capturing num tokens (num_tokens=320 avail_mem=42.54 GB):  55%|█████▌    | 32/58 [00:03<00:01, 25.21it/s]Capturing num tokens (num_tokens=288 avail_mem=42.54 GB):  55%|█████▌    | 32/58 [00:03<00:01, 25.21it/s]Capturing num tokens (num_tokens=288 avail_mem=42.54 GB):  62%|██████▏   | 36/58 [00:03<00:00, 28.58it/s]Capturing num tokens (num_tokens=256 avail_mem=42.54 GB):  62%|██████▏   | 36/58 [00:03<00:00, 28.58it/s]Capturing num tokens (num_tokens=240 avail_mem=42.53 GB):  62%|██████▏   | 36/58 [00:03<00:00, 28.58it/s]Capturing num tokens (num_tokens=224 avail_mem=42.53 GB):  62%|██████▏   | 36/58 [00:03<00:00, 28.58it/s]Capturing num tokens (num_tokens=208 avail_mem=42.52 GB):  62%|██████▏   | 36/58 [00:03<00:00, 28.58it/s]Capturing num tokens (num_tokens=192 avail_mem=42.52 GB):  62%|██████▏   | 36/58 [00:03<00:00, 28.58it/s]

    Capturing num tokens (num_tokens=192 avail_mem=42.52 GB):  71%|███████   | 41/58 [00:03<00:00, 32.28it/s]Capturing num tokens (num_tokens=176 avail_mem=42.52 GB):  71%|███████   | 41/58 [00:03<00:00, 32.28it/s]Capturing num tokens (num_tokens=160 avail_mem=42.52 GB):  71%|███████   | 41/58 [00:03<00:00, 32.28it/s]Capturing num tokens (num_tokens=144 avail_mem=42.51 GB):  71%|███████   | 41/58 [00:03<00:00, 32.28it/s]Capturing num tokens (num_tokens=128 avail_mem=42.52 GB):  71%|███████   | 41/58 [00:03<00:00, 32.28it/s]

    Capturing num tokens (num_tokens=128 avail_mem=42.52 GB):  78%|███████▊  | 45/58 [00:03<00:00, 26.57it/s]Capturing num tokens (num_tokens=112 avail_mem=42.52 GB):  78%|███████▊  | 45/58 [00:03<00:00, 26.57it/s]Capturing num tokens (num_tokens=96 avail_mem=42.51 GB):  78%|███████▊  | 45/58 [00:03<00:00, 26.57it/s] Capturing num tokens (num_tokens=80 avail_mem=42.51 GB):  78%|███████▊  | 45/58 [00:03<00:00, 26.57it/s]Capturing num tokens (num_tokens=80 avail_mem=42.51 GB):  83%|████████▎ | 48/58 [00:03<00:00, 26.40it/s]Capturing num tokens (num_tokens=64 avail_mem=42.50 GB):  83%|████████▎ | 48/58 [00:03<00:00, 26.40it/s]Capturing num tokens (num_tokens=48 avail_mem=42.50 GB):  83%|████████▎ | 48/58 [00:03<00:00, 26.40it/s]Capturing num tokens (num_tokens=32 avail_mem=42.50 GB):  83%|████████▎ | 48/58 [00:03<00:00, 26.40it/s]Capturing num tokens (num_tokens=28 avail_mem=42.50 GB):  83%|████████▎ | 48/58 [00:03<00:00, 26.40it/s]

    Capturing num tokens (num_tokens=24 avail_mem=42.49 GB):  83%|████████▎ | 48/58 [00:03<00:00, 26.40it/s]Capturing num tokens (num_tokens=24 avail_mem=42.49 GB):  91%|█████████▏| 53/58 [00:03<00:00, 30.73it/s]Capturing num tokens (num_tokens=20 avail_mem=42.49 GB):  91%|█████████▏| 53/58 [00:03<00:00, 30.73it/s]Capturing num tokens (num_tokens=16 avail_mem=42.49 GB):  91%|█████████▏| 53/58 [00:04<00:00, 30.73it/s]Capturing num tokens (num_tokens=12 avail_mem=42.48 GB):  91%|█████████▏| 53/58 [00:04<00:00, 30.73it/s]Capturing num tokens (num_tokens=8 avail_mem=42.48 GB):  91%|█████████▏| 53/58 [00:04<00:00, 30.73it/s] Capturing num tokens (num_tokens=8 avail_mem=42.48 GB):  98%|█████████▊| 57/58 [00:04<00:00, 32.66it/s]Capturing num tokens (num_tokens=4 avail_mem=42.48 GB):  98%|█████████▊| 57/58 [00:04<00:00, 32.66it/s]Capturing num tokens (num_tokens=4 avail_mem=42.48 GB): 100%|██████████| 58/58 [00:04<00:00, 14.08it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>


Note that `--reasoning-parser` defines the parser used to interpret responses.

### OpenAI Compatible API

Using the OpenAI compatible API, the contract follows the [DeepSeek API design](https://api-docs.deepseek.com/guides/reasoning_model) established with the release of DeepSeek-R1:

- `reasoning_content`: The content of the CoT.
- `content`: The content of the final answer.


```python
# Initialize OpenAI-like client
client = OpenAI(api_key="None", base_url=f"http://0.0.0.0:{port}/v1")
model_name = client.models.list().data[0].id

messages = [
    {
        "role": "user",
        "content": "What is 1+3?",
    }
]
```

#### Non-Streaming Request


```python
response_non_stream = client.chat.completions.create(
    model=model_name,
    messages=messages,
    temperature=0.6,
    top_p=0.95,
    stream=False,  # Non-streaming
    extra_body={"separate_reasoning": True},
)
print_highlight("==== Reasoning ====")
print_highlight(response_non_stream.choices[0].message.reasoning_content)

print_highlight("==== Text ====")
print_highlight(response_non_stream.choices[0].message.content)
```


<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I'll add the two numbers together.<br><br>Finally, the result of adding 1 and 3 is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>**Answer:** \boxed{4}</strong>


#### Streaming Request


```python
response_stream = client.chat.completions.create(
    model=model_name,
    messages=messages,
    temperature=0.6,
    top_p=0.95,
    stream=True,  # Non-streaming
    extra_body={"separate_reasoning": True},
)

reasoning_content = ""
content = ""
for chunk in response_stream:
    if chunk.choices[0].delta.content:
        content += chunk.choices[0].delta.content
    if chunk.choices[0].delta.reasoning_content:
        reasoning_content += chunk.choices[0].delta.reasoning_content

print_highlight("==== Reasoning ====")
print_highlight(reasoning_content)

print_highlight("==== Text ====")
print_highlight(content)
```


<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>I recognize that the user is asking for the sum of 1 and 3.<br><br>I know that addition is a basic arithmetic operation that combines two numbers to produce a total.<br><br>I will add 1 and 3 together to find the result.<br><br>The sum of 1 and 3 is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>To find the sum of 1 and 3, follow these simple steps:<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Final Answer:**<br>\[<br>\boxed{4}<br>\]</strong>


Optionally, you can buffer the reasoning content to the last reasoning chunk (or the first chunk after the reasoning content).


```python
response_stream = client.chat.completions.create(
    model=model_name,
    messages=messages,
    temperature=0.6,
    top_p=0.95,
    stream=True,  # Non-streaming
    extra_body={"separate_reasoning": True, "stream_reasoning": False},
)

reasoning_content = ""
content = ""
for chunk in response_stream:
    if chunk.choices[0].delta.content:
        content += chunk.choices[0].delta.content
    if chunk.choices[0].delta.reasoning_content:
        reasoning_content += chunk.choices[0].delta.reasoning_content

print_highlight("==== Reasoning ====")
print_highlight(reasoning_content)

print_highlight("==== Text ====")
print_highlight(content)
```


<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of the numbers 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Finally, I conclude that the result of adding 1 and 3 is 4.</strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>We need to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>**Answer:** \(\boxed{4}\)</strong>


The reasoning separation is enable by default when specify . 
**To disable it, set the `separate_reasoning` option to `False` in request.**


```python
response_non_stream = client.chat.completions.create(
    model=model_name,
    messages=messages,
    temperature=0.6,
    top_p=0.95,
    stream=False,  # Non-streaming
    extra_body={"separate_reasoning": False},
)

print_highlight("==== Original Output ====")
print_highlight(response_non_stream.choices[0].message.content)
```


<strong style='color: #00008B;'>==== Original Output ====</strong>



<strong style='color: #00008B;'>I see the question is asking for the sum of 1 and 3.<br><br>To solve this, I'll add the two numbers together.<br><br>1 plus 3 equals 4.<br><br>Therefore, the answer is 4.<br></think><br><br>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the answer is \(\boxed{4}\).</strong>


### SGLang Native API 


```python
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("deepseek-ai/DeepSeek-R1-Distill-Qwen-7B")
input = tokenizer.apply_chat_template(
    messages, tokenize=False, add_generation_prompt=True, return_dict=False
)

gen_url = f"http://localhost:{port}/generate"
gen_data = {
    "text": input,
    "sampling_params": {
        "skip_special_tokens": False,
        "max_new_tokens": 1024,
        "temperature": 0.6,
        "top_p": 0.95,
    },
}
gen_response = requests.post(gen_url, json=gen_data).json()["text"]

print_highlight("==== Original Output ====")
print_highlight(gen_response)

parse_url = f"http://localhost:{port}/separate_reasoning"
separate_reasoning_data = {
    "text": gen_response,
    "reasoning_parser": "deepseek-r1",
}
separate_reasoning_response_json = requests.post(
    parse_url, json=separate_reasoning_data
).json()
print_highlight("==== Reasoning ====")
print_highlight(separate_reasoning_response_json["reasoning_text"])
print_highlight("==== Text ====")
print_highlight(separate_reasoning_response_json["text"])
```


<strong style='color: #00008B;'>==== Original Output ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of the numbers 1 and 3.<br><br>To find the total, I start by identifying the two numbers to be added: 1 and 3.<br><br>Next, I add these numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></think><br><br>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>**Answer:** \(\boxed{4}\)</strong>


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of the numbers 1 and 3.<br><br>To find the total, I start by identifying the two numbers to be added: 1 and 3.<br><br>Next, I add these numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>**Answer:** \(\boxed{4}\)</strong>



```python
terminate_process(server_process)
```

### Offline Engine API


```python
import sglang as sgl
from sglang.srt.parser.reasoning_parser import ReasoningParser
from sglang.utils import print_highlight

llm = sgl.Engine(model_path="deepseek-ai/DeepSeek-R1-Distill-Qwen-7B")
tokenizer = AutoTokenizer.from_pretrained("deepseek-ai/DeepSeek-R1-Distill-Qwen-7B")
input = tokenizer.apply_chat_template(
    messages, tokenize=False, add_generation_prompt=True, return_dict=False
)
sampling_params = {
    "max_new_tokens": 1024,
    "skip_special_tokens": False,
    "temperature": 0.6,
    "top_p": 0.95,
}
result = llm.generate(prompt=input, sampling_params=sampling_params)

generated_text = result["text"]  # Assume there is only one prompt

print_highlight("==== Original Output ====")
print_highlight(generated_text)

parser = ReasoningParser("deepseek-r1")
reasoning_text, text = parser.parse_non_stream(generated_text)
print_highlight("==== Reasoning ====")
print_highlight(reasoning_text)
print_highlight("==== Text ====")
print_highlight(text)
```

    [2026-03-27 20:52:54] Pyspy dump for PID 969538:
    Process [33m[1m969538[0m: /usr/bin/python -m ipykernel_launcher -f /tmp/tmplu6mzs6i.json --HistoryManager.hist_file=:memory:
    Python v[1m3.10.12[0m ([2m/usr/bin/python3.10[0m)
    
    Thread [33m[1m969538[0m (idle): "MainThread"
        [32mread[0m ([36mlibc.so.6[0m)
        [32m_recv[0m ([36mmultiprocessing/connection.py[0m:[2m379[0m)
        [32m_recv_bytes[0m ([36mmultiprocessing/connection.py[0m:[2m414[0m)
        [32mrecv[0m ([36mmultiprocessing/connection.py[0m:[2m250[0m)
        [32m_wait_for_scheduler_ready[0m ([36mengine.py[0m:[2m1198[0m)
        [32mwait_for_ready[0m ([36mengine.py[0m:[2m584[0m)
        [32m_launch_subprocesses[0m ([36mengine.py[0m:[2m684[0m)
        [32m__init__[0m ([36mengine.py[0m:[2m188[0m)
        [32m__call__[0m ([36msglang/utils.py[0m:[2m347[0m)
        [32m<module>[0m ([36m420630080.py[0m:[2m5[0m)
        [32mrun_code[0m ([36mIPython/core/interactiveshell.py[0m:[2m3579[0m)
        [32mrun_ast_nodes[0m ([36mIPython/core/interactiveshell.py[0m:[2m3519[0m)
        [32mrun_cell_async[0m ([36mIPython/core/interactiveshell.py[0m:[2m3305[0m)
        [32m_pseudo_sync_runner[0m ([36mIPython/core/async_helpers.py[0m:[2m128[0m)
        [32m_run_cell[0m ([36mIPython/core/interactiveshell.py[0m:[2m3132[0m)
        [32mrun_cell[0m ([36mIPython/core/interactiveshell.py[0m:[2m3077[0m)
        [32mrun_cell[0m ([36mipykernel/zmqshell.py[0m:[2m663[0m)
        [32mdo_execute[0m ([36mipykernel/ipkernel.py[0m:[2m471[0m)
        [32mexecute_request[0m ([36mipykernel/kernelbase.py[0m:[2m834[0m)
        [32mexecute_request[0m ([36mipykernel/ipkernel.py[0m:[2m372[0m)
        [32mdispatch_shell[0m ([36mipykernel/kernelbase.py[0m:[2m478[0m)
        [32mshell_main[0m ([36mipykernel/kernelbase.py[0m:[2m621[0m)
        [32mpreserve_context[0m ([36mipykernel/utils.py[0m:[2m71[0m)
        [32m__step[0m ([36masyncio/tasks.py[0m:[2m232[0m)
        [32m_run[0m ([36masyncio/events.py[0m:[2m80[0m)
        [32m_run_once[0m ([36mnest_asyncio.py[0m:[2m133[0m)
        [32mrun_forever[0m ([36masyncio/base_events.py[0m:[2m603[0m)
        [32mstart[0m ([36mtornado/platform/asyncio.py[0m:[2m211[0m)
        [32mstart[0m ([36mipykernel/kernelapp.py[0m:[2m758[0m)
        [32mlaunch_instance[0m ([36mtraitlets/config/application.py[0m:[2m1075[0m)
        [32m<module>[0m ([36mipykernel_launcher.py[0m:[2m18[0m)
        [32m_run_code[0m ([36mrunpy.py[0m:[2m86[0m)
        [32m_run_module_as_main[0m ([36mrunpy.py[0m:[2m196[0m)
        [32m0x7d917fbdfd90[0m ([36mlibc.so.6[0m)
    Thread [33m[1m969556[0m (idle): "IOPub"
        [32mepoll_wait[0m ([36mlibc.so.6[0m)
        [32mselect[0m ([36mselectors.py[0m:[2m469[0m)
        [32m_run_once[0m ([36mnest_asyncio.py[0m:[2m115[0m)
        [32mrun_forever[0m ([36masyncio/base_events.py[0m:[2m603[0m)
        [32mstart[0m ([36mtornado/platform/asyncio.py[0m:[2m211[0m)
        [32m_thread_main[0m ([36mipykernel/iostream.py[0m:[2m164[0m)
        [32mrun[0m ([36mthreading.py[0m:[2m953[0m)
        [32m_bootstrap_inner[0m ([36mthreading.py[0m:[2m1016[0m)
        [32m_bootstrap[0m ([36mthreading.py[0m:[2m973[0m)
        [32m0x7d917fc4aac3[0m ([36mlibc.so.6[0m)
        [32m0x7d917fcdc850[0m ([36mlibc.so.6[0m)
    Thread [33m[1m969557[0m (idle): "Heartbeat"
        [32mpoll[0m ([36mlibc.so.6[0m)
        [32mzmq_poll[0m ([36mlibzmq-7b073b3d.so.5.2.5[0m)
        [32m0x7d917f5a87bb[0m ([36mlibzmq-7b073b3d.so.5.2.5[0m)
        [32m0x7d917f5a813e[0m ([36mlibzmq-7b073b3d.so.5.2.5[0m)
        [32mzmq_proxy[0m ([36mlibzmq-7b073b3d.so.5.2.5[0m)
        [32m0x7d917f6540e0[0m ([36mzmq/backend/cython/_zmq.cpython-310-x86_64-linux-gnu.so[0m)
        [32mdevice[0m ([36mzmq/sugar/__init__.py[0m:[2m21[0m)
        [32mrun[0m ([36mipykernel/heartbeat.py[0m:[2m105[0m)
        [32m_bootstrap_inner[0m ([36mthreading.py[0m:[2m1016[0m)
        [32m_bootstrap[0m ([36mthreading.py[0m:[2m973[0m)
        [32m0x7d917fc4aac3[0m ([36mlibc.so.6[0m)
        [32m0x7d917fcdc850[0m ([36mlibc.so.6[0m)
    Thread [33m[1m969568[0m (idle): "Thread-2 (_watch_pipe_fd)"
        [32mread[0m ([36mlibc.so.6[0m)
        [32m_watch_pipe_fd[0m ([36mipykernel/iostream.py[0m:[2m466[0m)
        [32mrun[0m ([36mthreading.py[0m:[2m953[0m)
        [32m_bootstrap_inner[0m ([36mthreading.py[0m:[2m1016[0m)
        [32m_bootstrap[0m ([36mthreading.py[0m:[2m973[0m)
        [32m0x7d917fc4aac3[0m ([36mlibc.so.6[0m)
        [32m0x7d917fcdc850[0m ([36mlibc.so.6[0m)
    Thread [33m[1m969569[0m (idle): "Thread-3 (_watch_pipe_fd)"
        [32mread[0m ([36mlibc.so.6[0m)
        [32m_watch_pipe_fd[0m ([36mipykernel/iostream.py[0m:[2m462[0m)
        [32mrun[0m ([36mthreading.py[0m:[2m953[0m)
        [32m_bootstrap_inner[0m ([36mthreading.py[0m:[2m1016[0m)
        [32m_bootstrap[0m ([36mthreading.py[0m:[2m973[0m)
        [32m0x7d917fc4aac3[0m ([36mlibc.so.6[0m)
        [32m0x7d917fcdc850[0m ([36mlibc.so.6[0m)
    Thread [33m[1m969573[0m (idle): "Control"
        [32mepoll_wait[0m ([36mlibc.so.6[0m)
        [32mselect[0m ([36mselectors.py[0m:[2m469[0m)
        [32m_run_once[0m ([36masyncio/base_events.py[0m:[2m1871[0m)
        [32mrun_forever[0m ([36masyncio/base_events.py[0m:[2m603[0m)
        [32mstart[0m ([36mtornado/platform/asyncio.py[0m:[2m211[0m)
        [32mrun[0m ([36mipykernel/thread.py[0m:[2m24[0m)
        [32m_bootstrap_inner[0m ([36mthreading.py[0m:[2m1016[0m)
        [32m_bootstrap[0m ([36mthreading.py[0m:[2m973[0m)
        [32m0x7d917fc4aac3[0m ([36mlibc.so.6[0m)
        [32m0x7d917fcdc850[0m ([36mlibc.so.6[0m)
    Thread [33m[1m969576[0m (idle): "Shell channel"
        [32mepoll_wait[0m ([36mlibc.so.6[0m)
        [32mselect[0m ([36mselectors.py[0m:[2m469[0m)
        [32m_run_once[0m ([36mnest_asyncio.py[0m:[2m115[0m)
        [32mrun_forever[0m ([36masyncio/base_events.py[0m:[2m603[0m)
        [32mstart[0m ([36mtornado/platform/asyncio.py[0m:[2m211[0m)
        [32mrun[0m ([36mipykernel/thread.py[0m:[2m24[0m)
        [32mrun[0m ([36mipykernel/shellchannel.py[0m:[2m52[0m)
        [32m_bootstrap_inner[0m ([36mthreading.py[0m:[2m1016[0m)
        [32m_bootstrap[0m ([36mthreading.py[0m:[2m973[0m)
        [32m0x7d917fc4aac3[0m ([36mlibc.so.6[0m)
        [32m0x7d917fcdc850[0m ([36mlibc.so.6[0m)
    Thread [33m[1m969583[0m (idle): "Thread-1"
        [32mselect[0m ([36mlibc.so.6[0m)
        [32mrun[0m ([36mipykernel/parentpoller.py[0m:[2m62[0m)
        [32m_bootstrap_inner[0m ([36mthreading.py[0m:[2m1016[0m)
        [32m_bootstrap[0m ([36mthreading.py[0m:[2m973[0m)
        [32m0x7d917fc4aac3[0m ([36mlibc.so.6[0m)
        [32m0x7d917fcdc850[0m ([36mlibc.so.6[0m)
    Thread [33m[1m972585[0m (idle): "Thread-5 (_watchdog_thread)"
        [32mpoll[0m ([36mlibc.so.6[0m)
        [32mselect[0m ([36mselectors.py[0m:[2m416[0m)
        [32m_communicate[0m ([36msubprocess.py[0m:[2m2021[0m)
        [32mcommunicate[0m ([36msubprocess.py[0m:[2m1154[0m)
        [32mrun[0m ([36msubprocess.py[0m:[2m505[0m)
        [32mpyspy_dump_schedulers[0m ([36mutils/common.py[0m:[2m2226[0m)
        [32m_watchdog_once[0m ([36mutils/watchdog.py[0m:[2m150[0m)
        [32m_watchdog_thread[0m ([36mutils/watchdog.py[0m:[2m125[0m)
        [32mrun[0m ([36mthreading.py[0m:[2m953[0m)
        [32m_bootstrap_inner[0m ([36mthreading.py[0m:[2m1016[0m)
        [32m_bootstrap[0m ([36mthreading.py[0m:[2m973[0m)
        [32m0x7d917fc4aac3[0m ([36mlibc.so.6[0m)
        [32m0x7d917fcdc850[0m ([36mlibc.so.6[0m)
    


    [2026-03-27 20:52:54] TokenizerManager watchdog timeout (self.watchdog_timeout=300, self.soft=True)


    


    


    [2026-03-27 20:52:58] Pyspy dump for PID 972452:
    Process [33m[1m972452[0m: sglang::scheduler
    Python v[1m3.10.12[0m ([2m/usr/bin/python3.10[0m)
    
    Thread [33m[1m972452[0m (idle): "MainThread"
        [32mread[0m ([36mlibc.so.6[0m)
        [32m0x73fc6409dddd[0m ([36mlibcrypto.so.3[0m)
        [32m0x73fc6408e25b[0m ([36mlibcrypto.so.3[0m)
        [32m0x73fc64091255[0m ([36mlibcrypto.so.3[0m)
        [32mBIO_read[0m ([36mlibcrypto.so.3[0m)
        [32m0x73fdb3ba83cc[0m ([36mlibssl.so.3[0m)
        [32m0x73fdb3ba9f99[0m ([36mlibssl.so.3[0m)
        [32m0x73fdb3b843fc[0m ([36mlibssl.so.3[0m)
        [32mSSL_read_ex[0m ([36mlibssl.so.3[0m)
        [32m0x73fdd1e8fd09[0m ([36m_ssl.cpython-310-x86_64-linux-gnu.so[0m)
        [32mread[0m ([36mssl.py[0m:[2m1161[0m)
        [32mrecv[0m ([36mssl.py[0m:[2m1288[0m)
        [32mread[0m ([36mhttpcore/backends/sync.py[0m:[2m28[0m)
        [32m_receive_event[0m ([36mhttpcore/_sync/http11.py[0m:[2m191[0m)
        [32m_receive_response_headers[0m ([36mhttpcore/_sync/http11.py[0m:[2m155[0m)
        [32mhandle_request[0m ([36mhttpcore/_sync/http11.py[0m:[2m91[0m)
        [32mhandle_request[0m ([36mhttpcore/_sync/connection.py[0m:[2m90[0m)
        [32mhandle_request[0m ([36mhttpcore/_sync/connection_pool.py[0m:[2m238[0m)
        [32mhandle_request[0m ([36mhttpx/_transports/default.py[0m:[2m218[0m)
        [32m_send_single_request[0m ([36mhttpx/_client.py[0m:[2m1009[0m)
        [32m_send_handling_redirects[0m ([36mhttpx/_client.py[0m:[2m973[0m)
        [32m_send_handling_auth[0m ([36mhttpx/_client.py[0m:[2m936[0m)
        [32msend[0m ([36mhttpx/_client.py[0m:[2m908[0m)
        [32mrequest[0m ([36mhttpx/_client.py[0m:[2m821[0m)
        [32mget[0m ([36mhttpx/_client.py[0m:[2m1045[0m)
        [32mpaginate[0m ([36mhuggingface_hub/utils/_pagination.py[0m:[2m36[0m)
        [32mlist_repo_tree[0m ([36mhuggingface_hub/hf_api.py[0m:[2m3609[0m)
        [32m<listcomp>[0m ([36mtransformers/utils/hub.py[0m:[2m131[0m)
        [32mlist_repo_templates[0m ([36mtransformers/utils/hub.py[0m:[2m131[0m)
        [32mfrom_pretrained[0m ([36mtransformers/tokenization_utils_base.py[0m:[2m1650[0m)
        [32mfrom_pretrained[0m ([36mtransformers/models/auto/tokenization_auto.py[0m:[2m689[0m)
        [32mget_tokenizer[0m ([36mutils/hf_transformers_utils.py[0m:[2m810[0m)
        [32minit_tokenizer[0m ([36mscheduler.py[0m:[2m519[0m)
        [32m__init__[0m ([36mscheduler.py[0m:[2m373[0m)
        [32mrun_scheduler_process[0m ([36mscheduler.py[0m:[2m3566[0m)
        [32mrun[0m ([36mmultiprocessing/process.py[0m:[2m108[0m)
        [32m_bootstrap[0m ([36mmultiprocessing/process.py[0m:[2m314[0m)
        [32m_main[0m ([36mmultiprocessing/spawn.py[0m:[2m129[0m)
        [32mspawn_main[0m ([36mmultiprocessing/spawn.py[0m:[2m116[0m)
        [32m<module>[0m ([36m<string>[0m:[2m1[0m)
        [32m0x73fde7a3dd90[0m ([36mlibc.so.6[0m)
    Thread [33m[1m972754[0m (idle): "Thread-1 (_watchdog_thread)"
        [32mpoll[0m ([36mlibc.so.6[0m)
        [32mselect[0m ([36mselectors.py[0m:[2m416[0m)
        [32m_communicate[0m ([36msubprocess.py[0m:[2m2021[0m)
        [32mcommunicate[0m ([36msubprocess.py[0m:[2m1154[0m)
        [32mrun[0m ([36msubprocess.py[0m:[2m505[0m)
        [32mpyspy_dump_schedulers[0m ([36mutils/common.py[0m:[2m2226[0m)
        [32m_watchdog_once[0m ([36mutils/watchdog.py[0m:[2m150[0m)
        [32m_watchdog_thread[0m ([36mutils/watchdog.py[0m:[2m125[0m)
        [32mrun[0m ([36mthreading.py[0m:[2m953[0m)
        [32m_bootstrap_inner[0m ([36mthreading.py[0m:[2m1016[0m)
        [32m_bootstrap[0m ([36mthreading.py[0m:[2m973[0m)
        [32m0x73fde7aa8ac3[0m ([36mlibc.so.6[0m)
        [32m0x73fde7b3a850[0m ([36mlibc.so.6[0m)
    
    [2026-03-27 20:52:58] Scheduler watchdog timeout (self.watchdog_timeout=300, self.soft=True)
    


    


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-27 20:54:20.580 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 20:54:20] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 20:54:20.580 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 20:54:20] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 20:54:20.580 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 20:54:20] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 20:54:20.580 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 20:54:20] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 20:54:20.580 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 20:54:20] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.08s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.41s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.36s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:40,  2.82s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:40,  2.82s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:18,  1.40s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:18,  1.40s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:46,  1.18it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:46,  1.18it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:31,  1.71it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:31,  1.71it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:22,  2.32it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:22,  2.32it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:03<00:17,  2.97it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:03<00:17,  2.97it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:13,  3.71it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:13,  3.71it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.50it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.50it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.38it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.38it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.38it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  7.05it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  7.05it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  7.05it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.51it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.51it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.51it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:04<00:04, 10.06it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:04<00:04, 10.06it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:04<00:04, 10.06it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:04<00:03, 11.98it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:04<00:03, 11.98it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:04<00:03, 11.98it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:04<00:03, 11.98it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:04<00:02, 15.19it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:04<00:02, 15.19it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 15.19it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 15.19it/s]

    Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 15.19it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 20.45it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 20.45it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 20.45it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 20.45it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 20.45it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 20.45it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 27.03it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 27.03it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 27.03it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 27.03it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 27.03it/s]

    Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:05<00:01, 27.03it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:05<00:01, 27.03it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:05<00:00, 33.92it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:05<00:00, 33.92it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:05<00:00, 33.92it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:05<00:00, 33.92it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:05<00:00, 33.92it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:05<00:00, 33.92it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:05<00:00, 33.92it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 39.70it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 39.70it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:05<00:00, 39.70it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:05<00:00, 39.70it/s]

    Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:05<00:00, 39.70it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:05<00:00, 39.70it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:05<00:00, 39.70it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:05<00:00, 44.01it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:05<00:00, 44.01it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:05<00:00, 44.01it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:05<00:00, 44.01it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:05<00:00, 44.01it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:05<00:00, 44.01it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:05<00:00, 44.01it/s]Compiling num tokens (num_tokens=20):  81%|████████  | 47/58 [00:05<00:00, 44.01it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:05<00:00, 50.38it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:05<00:00, 50.38it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:05<00:00, 50.38it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:05<00:00, 50.38it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:05<00:00, 50.38it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:05<00:00, 10.20it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=61.62 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=61.62 GB):   2%|▏         | 1/58 [00:00<00:16,  3.43it/s]Capturing num tokens (num_tokens=7680 avail_mem=61.59 GB):   2%|▏         | 1/58 [00:00<00:16,  3.43it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=61.59 GB):   3%|▎         | 2/58 [00:00<00:15,  3.60it/s]Capturing num tokens (num_tokens=7168 avail_mem=61.59 GB):   3%|▎         | 2/58 [00:00<00:15,  3.60it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=61.59 GB):   5%|▌         | 3/58 [00:00<00:14,  3.84it/s]Capturing num tokens (num_tokens=6656 avail_mem=61.59 GB):   5%|▌         | 3/58 [00:00<00:14,  3.84it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=61.59 GB):   7%|▋         | 4/58 [00:01<00:13,  4.14it/s]Capturing num tokens (num_tokens=6144 avail_mem=61.60 GB):   7%|▋         | 4/58 [00:01<00:13,  4.14it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=61.60 GB):   9%|▊         | 5/58 [00:01<00:12,  4.40it/s]Capturing num tokens (num_tokens=5632 avail_mem=61.60 GB):   9%|▊         | 5/58 [00:01<00:12,  4.40it/s]Capturing num tokens (num_tokens=5632 avail_mem=61.60 GB):  10%|█         | 6/58 [00:01<00:10,  4.79it/s]Capturing num tokens (num_tokens=5120 avail_mem=61.60 GB):  10%|█         | 6/58 [00:01<00:10,  4.79it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=61.60 GB):  12%|█▏        | 7/58 [00:01<00:09,  5.16it/s]Capturing num tokens (num_tokens=4608 avail_mem=61.61 GB):  12%|█▏        | 7/58 [00:01<00:09,  5.16it/s]Capturing num tokens (num_tokens=4608 avail_mem=61.61 GB):  14%|█▍        | 8/58 [00:01<00:08,  5.68it/s]Capturing num tokens (num_tokens=4096 avail_mem=61.61 GB):  14%|█▍        | 8/58 [00:01<00:08,  5.68it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=61.61 GB):  16%|█▌        | 9/58 [00:01<00:07,  6.19it/s]Capturing num tokens (num_tokens=3840 avail_mem=61.62 GB):  16%|█▌        | 9/58 [00:01<00:07,  6.19it/s]Capturing num tokens (num_tokens=3840 avail_mem=61.62 GB):  17%|█▋        | 10/58 [00:01<00:07,  6.69it/s]Capturing num tokens (num_tokens=3584 avail_mem=61.61 GB):  17%|█▋        | 10/58 [00:01<00:07,  6.69it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=61.61 GB):  19%|█▉        | 11/58 [00:02<00:06,  7.26it/s]Capturing num tokens (num_tokens=3328 avail_mem=61.61 GB):  19%|█▉        | 11/58 [00:02<00:06,  7.26it/s]Capturing num tokens (num_tokens=3328 avail_mem=61.61 GB):  21%|██        | 12/58 [00:02<00:05,  7.89it/s]Capturing num tokens (num_tokens=3072 avail_mem=61.61 GB):  21%|██        | 12/58 [00:02<00:05,  7.89it/s]Capturing num tokens (num_tokens=2816 avail_mem=61.61 GB):  21%|██        | 12/58 [00:02<00:05,  7.89it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=61.61 GB):  24%|██▍       | 14/58 [00:02<00:04,  9.08it/s]Capturing num tokens (num_tokens=2560 avail_mem=61.61 GB):  24%|██▍       | 14/58 [00:02<00:04,  9.08it/s]Capturing num tokens (num_tokens=2304 avail_mem=61.61 GB):  24%|██▍       | 14/58 [00:02<00:04,  9.08it/s]Capturing num tokens (num_tokens=2304 avail_mem=61.61 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.48it/s]Capturing num tokens (num_tokens=2048 avail_mem=61.61 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.48it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=61.61 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.48it/s]Capturing num tokens (num_tokens=1792 avail_mem=61.61 GB):  31%|███       | 18/58 [00:02<00:03, 11.97it/s]Capturing num tokens (num_tokens=1536 avail_mem=61.61 GB):  31%|███       | 18/58 [00:02<00:03, 11.97it/s]Capturing num tokens (num_tokens=1280 avail_mem=61.61 GB):  31%|███       | 18/58 [00:02<00:03, 11.97it/s]Capturing num tokens (num_tokens=1280 avail_mem=61.61 GB):  34%|███▍      | 20/58 [00:02<00:02, 13.55it/s]Capturing num tokens (num_tokens=1024 avail_mem=61.61 GB):  34%|███▍      | 20/58 [00:02<00:02, 13.55it/s]

    Capturing num tokens (num_tokens=960 avail_mem=61.61 GB):  34%|███▍      | 20/58 [00:02<00:02, 13.55it/s] Capturing num tokens (num_tokens=960 avail_mem=61.61 GB):  38%|███▊      | 22/58 [00:02<00:02, 15.11it/s]Capturing num tokens (num_tokens=896 avail_mem=61.60 GB):  38%|███▊      | 22/58 [00:02<00:02, 15.11it/s]Capturing num tokens (num_tokens=832 avail_mem=61.60 GB):  38%|███▊      | 22/58 [00:02<00:02, 15.11it/s]Capturing num tokens (num_tokens=768 avail_mem=61.59 GB):  38%|███▊      | 22/58 [00:02<00:02, 15.11it/s]Capturing num tokens (num_tokens=768 avail_mem=61.59 GB):  43%|████▎     | 25/58 [00:02<00:01, 17.48it/s]Capturing num tokens (num_tokens=704 avail_mem=61.59 GB):  43%|████▎     | 25/58 [00:02<00:01, 17.48it/s]

    Capturing num tokens (num_tokens=640 avail_mem=61.59 GB):  43%|████▎     | 25/58 [00:03<00:01, 17.48it/s]Capturing num tokens (num_tokens=576 avail_mem=61.59 GB):  43%|████▎     | 25/58 [00:03<00:01, 17.48it/s]Capturing num tokens (num_tokens=576 avail_mem=61.59 GB):  48%|████▊     | 28/58 [00:03<00:01, 18.53it/s]Capturing num tokens (num_tokens=512 avail_mem=61.58 GB):  48%|████▊     | 28/58 [00:03<00:01, 18.53it/s]Capturing num tokens (num_tokens=480 avail_mem=61.58 GB):  48%|████▊     | 28/58 [00:03<00:01, 18.53it/s]Capturing num tokens (num_tokens=448 avail_mem=61.58 GB):  48%|████▊     | 28/58 [00:03<00:01, 18.53it/s]

    Capturing num tokens (num_tokens=448 avail_mem=61.58 GB):  53%|█████▎    | 31/58 [00:03<00:01, 20.48it/s]Capturing num tokens (num_tokens=416 avail_mem=61.57 GB):  53%|█████▎    | 31/58 [00:03<00:01, 20.48it/s]Capturing num tokens (num_tokens=384 avail_mem=61.57 GB):  53%|█████▎    | 31/58 [00:03<00:01, 20.48it/s]Capturing num tokens (num_tokens=352 avail_mem=61.56 GB):  53%|█████▎    | 31/58 [00:03<00:01, 20.48it/s]Capturing num tokens (num_tokens=320 avail_mem=61.56 GB):  53%|█████▎    | 31/58 [00:03<00:01, 20.48it/s]Capturing num tokens (num_tokens=320 avail_mem=61.56 GB):  60%|██████    | 35/58 [00:03<00:01, 22.45it/s]Capturing num tokens (num_tokens=288 avail_mem=61.56 GB):  60%|██████    | 35/58 [00:03<00:01, 22.45it/s]Capturing num tokens (num_tokens=256 avail_mem=61.55 GB):  60%|██████    | 35/58 [00:03<00:01, 22.45it/s]

    Capturing num tokens (num_tokens=240 avail_mem=61.55 GB):  60%|██████    | 35/58 [00:03<00:01, 22.45it/s]Capturing num tokens (num_tokens=240 avail_mem=61.55 GB):  66%|██████▌   | 38/58 [00:03<00:00, 23.98it/s]Capturing num tokens (num_tokens=224 avail_mem=61.54 GB):  66%|██████▌   | 38/58 [00:03<00:00, 23.98it/s]Capturing num tokens (num_tokens=208 avail_mem=61.54 GB):  66%|██████▌   | 38/58 [00:03<00:00, 23.98it/s]Capturing num tokens (num_tokens=192 avail_mem=61.54 GB):  66%|██████▌   | 38/58 [00:03<00:00, 23.98it/s]Capturing num tokens (num_tokens=176 avail_mem=61.53 GB):  66%|██████▌   | 38/58 [00:03<00:00, 23.98it/s]Capturing num tokens (num_tokens=176 avail_mem=61.53 GB):  72%|███████▏  | 42/58 [00:03<00:00, 26.50it/s]Capturing num tokens (num_tokens=160 avail_mem=61.53 GB):  72%|███████▏  | 42/58 [00:03<00:00, 26.50it/s]Capturing num tokens (num_tokens=144 avail_mem=61.52 GB):  72%|███████▏  | 42/58 [00:03<00:00, 26.50it/s]

    Capturing num tokens (num_tokens=128 avail_mem=61.53 GB):  72%|███████▏  | 42/58 [00:03<00:00, 26.50it/s]Capturing num tokens (num_tokens=112 avail_mem=61.53 GB):  72%|███████▏  | 42/58 [00:03<00:00, 26.50it/s]Capturing num tokens (num_tokens=112 avail_mem=61.53 GB):  79%|███████▉  | 46/58 [00:03<00:00, 29.58it/s]Capturing num tokens (num_tokens=96 avail_mem=61.53 GB):  79%|███████▉  | 46/58 [00:03<00:00, 29.58it/s] Capturing num tokens (num_tokens=80 avail_mem=61.52 GB):  79%|███████▉  | 46/58 [00:03<00:00, 29.58it/s]Capturing num tokens (num_tokens=64 avail_mem=61.52 GB):  79%|███████▉  | 46/58 [00:03<00:00, 29.58it/s]Capturing num tokens (num_tokens=48 avail_mem=61.52 GB):  79%|███████▉  | 46/58 [00:03<00:00, 29.58it/s]Capturing num tokens (num_tokens=32 avail_mem=61.51 GB):  79%|███████▉  | 46/58 [00:03<00:00, 29.58it/s]Capturing num tokens (num_tokens=32 avail_mem=61.51 GB):  88%|████████▊ | 51/58 [00:03<00:00, 33.17it/s]Capturing num tokens (num_tokens=28 avail_mem=61.51 GB):  88%|████████▊ | 51/58 [00:03<00:00, 33.17it/s]Capturing num tokens (num_tokens=24 avail_mem=61.51 GB):  88%|████████▊ | 51/58 [00:03<00:00, 33.17it/s]

    Capturing num tokens (num_tokens=20 avail_mem=61.50 GB):  88%|████████▊ | 51/58 [00:03<00:00, 33.17it/s]Capturing num tokens (num_tokens=16 avail_mem=61.50 GB):  88%|████████▊ | 51/58 [00:03<00:00, 33.17it/s]Capturing num tokens (num_tokens=12 avail_mem=61.50 GB):  88%|████████▊ | 51/58 [00:03<00:00, 33.17it/s]Capturing num tokens (num_tokens=12 avail_mem=61.50 GB):  97%|█████████▋| 56/58 [00:03<00:00, 35.72it/s]Capturing num tokens (num_tokens=8 avail_mem=61.49 GB):  97%|█████████▋| 56/58 [00:03<00:00, 35.72it/s] Capturing num tokens (num_tokens=4 avail_mem=61.49 GB):  97%|█████████▋| 56/58 [00:03<00:00, 35.72it/s]Capturing num tokens (num_tokens=4 avail_mem=61.49 GB): 100%|██████████| 58/58 [00:03<00:00, 14.55it/s]



<strong style='color: #00008B;'>==== Original Output ====</strong>



<strong style='color: #00008B;'>To solve the problem of adding 1 and 3, I start by identifying the two numbers involved, which are 1 and 3.<br><br>Next, I perform the addition operation by combining these two numbers.<br><br>Finally, I calculate the sum, which is 4.<br></think><br><br>**Solution:**<br><br>To find the sum of \(1\) and \(3\), follow these simple steps:<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Perform the addition:**<br>   \[<br>   1 + 3<br>   \]<br><br>3. **Calculate the result:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Final Answer:**<br>\[<br>\boxed{4}<br>\]</strong>



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>To solve the problem of adding 1 and 3, I start by identifying the two numbers involved, which are 1 and 3.<br><br>Next, I perform the addition operation by combining these two numbers.<br><br>Finally, I calculate the sum, which is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>**Solution:**<br><br>To find the sum of \(1\) and \(3\), follow these simple steps:<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Perform the addition:**<br>   \[<br>   1 + 3<br>   \]<br><br>3. **Calculate the result:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Final Answer:**<br>\[<br>\boxed{4}<br>\]</strong>



```python
llm.shutdown()
```

## Supporting New Reasoning Model Schemas

For future reasoning models, you can implement the reasoning parser as a subclass of `BaseReasoningFormatDetector` in `python/sglang/srt/reasoning_parser.py` and specify the reasoning parser for new reasoning model schemas accordingly.
