# Offline Engine API

SGLang provides a direct inference engine without the need for an HTTP server, especially for use cases where additional HTTP server adds unnecessary complexity or overhead. Here are two general use cases:

- Offline Batch Inference
- Custom Server on Top of the Engine

This document focuses on the offline batch inference, demonstrating four different inference modes:

- Non-streaming synchronous generation
- Streaming synchronous generation
- Non-streaming asynchronous generation
- Streaming asynchronous generation

Additionally, you can easily build a custom server on top of the SGLang offline engine. A detailed example working in a python script can be found in [custom_server](https://github.com/sgl-project/sglang/blob/main/examples/runtime/engine/custom_server.py).



## Nest Asyncio
Note that if you want to use **Offline Engine** in ipython or some other nested loop code, you need to add the following code:
```python
import nest_asyncio

nest_asyncio.apply()

```

## Advanced Usage

The engine supports [vlm inference](https://github.com/sgl-project/sglang/blob/main/examples/runtime/engine/offline_batch_inference_vlm.py) as well as [extracting hidden states](https://github.com/sgl-project/sglang/blob/main/examples/runtime/hidden_states). 

Please see [the examples](https://github.com/sgl-project/sglang/tree/main/examples/runtime/engine) for further use cases.

## Offline Batch Inference

SGLang offline engine supports batch inference with efficient scheduling.


```python
# launch the offline engine
import asyncio

import sglang as sgl
import sglang.test.doc_patch  # noqa: F401
from sglang.utils import async_stream_and_merge, stream_and_merge

llm = sgl.Engine(model_path="qwen/qwen2.5-0.5b-instruct")
```

    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-27 21:03:41.876 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 21:03:41] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 21:03:41.876 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 21:03:41] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 21:03:41.876 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 21:03:41] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 21:03:41.876 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 21:03:41] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 21:03:41.876 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 21:03:41] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  4.95it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  4.94it/s]


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


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:03,  2.17s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:03,  2.17s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:03,  2.17s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:03,  2.17s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:23,  2.28it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:23,  2.28it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:23,  2.28it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:23,  2.28it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:23,  2.28it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:23,  2.28it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:23,  2.28it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:06,  6.91it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:06,  6.91it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:06,  6.91it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:06,  6.91it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:06,  6.91it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:06,  6.91it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:06,  6.91it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:06,  6.91it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:06,  6.91it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:02<00:06,  6.91it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 15.29it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 15.29it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 15.29it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 15.29it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 15.29it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 15.29it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 15.29it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 15.29it/s]

    Compiling num tokens (num_tokens=640):  33%|███▎      | 19/58 [00:02<00:02, 15.29it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:02<00:01, 23.21it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:02<00:01, 23.21it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:02<00:01, 23.21it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:02<00:01, 23.21it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:02<00:01, 23.21it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:02<00:01, 23.21it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:02<00:01, 23.21it/s]Compiling num tokens (num_tokens=352):  47%|████▋     | 27/58 [00:02<00:01, 23.21it/s]Compiling num tokens (num_tokens=320):  47%|████▋     | 27/58 [00:02<00:01, 23.21it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:02<00:00, 31.40it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:02<00:00, 31.40it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:02<00:00, 31.40it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:02<00:00, 31.40it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:02<00:00, 31.40it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:02<00:00, 31.40it/s]

    Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:02<00:00, 31.40it/s]Compiling num tokens (num_tokens=176):  60%|██████    | 35/58 [00:02<00:00, 31.40it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:02<00:00, 37.92it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:02<00:00, 37.92it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:02<00:00, 37.92it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:02<00:00, 37.92it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:02<00:00, 37.92it/s]Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:02<00:00, 37.92it/s] Compiling num tokens (num_tokens=80):  72%|███████▏  | 42/58 [00:02<00:00, 37.92it/s]Compiling num tokens (num_tokens=64):  72%|███████▏  | 42/58 [00:02<00:00, 37.92it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:02<00:00, 43.78it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:02<00:00, 43.78it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:02<00:00, 43.78it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:02<00:00, 43.78it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:02<00:00, 43.78it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:02<00:00, 43.78it/s]Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:02<00:00, 43.78it/s]

    Compiling num tokens (num_tokens=12):  84%|████████▍ | 49/58 [00:02<00:00, 43.78it/s]Compiling num tokens (num_tokens=8):  84%|████████▍ | 49/58 [00:03<00:00, 43.78it/s] Compiling num tokens (num_tokens=4):  84%|████████▍ | 49/58 [00:03<00:00, 43.78it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 19.23it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=71.05 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=71.02 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=71.02 GB):   3%|▎         | 2/58 [00:00<00:02, 19.39it/s]Capturing num tokens (num_tokens=7168 avail_mem=71.01 GB):   3%|▎         | 2/58 [00:00<00:02, 19.39it/s]Capturing num tokens (num_tokens=6656 avail_mem=71.01 GB):   3%|▎         | 2/58 [00:00<00:02, 19.39it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=71.01 GB):   7%|▋         | 4/58 [00:00<00:02, 18.27it/s]Capturing num tokens (num_tokens=6144 avail_mem=71.01 GB):   7%|▋         | 4/58 [00:00<00:02, 18.27it/s]Capturing num tokens (num_tokens=5632 avail_mem=71.01 GB):   7%|▋         | 4/58 [00:00<00:02, 18.27it/s]Capturing num tokens (num_tokens=5120 avail_mem=71.01 GB):   7%|▋         | 4/58 [00:00<00:02, 18.27it/s]Capturing num tokens (num_tokens=4608 avail_mem=71.01 GB):   7%|▋         | 4/58 [00:00<00:02, 18.27it/s]Capturing num tokens (num_tokens=4608 avail_mem=71.01 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.55it/s]Capturing num tokens (num_tokens=4096 avail_mem=71.01 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.55it/s]Capturing num tokens (num_tokens=3840 avail_mem=71.00 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.55it/s]Capturing num tokens (num_tokens=3584 avail_mem=71.00 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.55it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=71.00 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.55it/s]Capturing num tokens (num_tokens=3072 avail_mem=70.99 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.55it/s]Capturing num tokens (num_tokens=3072 avail_mem=70.99 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.50it/s]Capturing num tokens (num_tokens=2816 avail_mem=70.99 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.50it/s]Capturing num tokens (num_tokens=2560 avail_mem=70.99 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.50it/s]Capturing num tokens (num_tokens=2304 avail_mem=70.98 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.50it/s]Capturing num tokens (num_tokens=2048 avail_mem=70.98 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.50it/s]Capturing num tokens (num_tokens=1792 avail_mem=70.98 GB):  22%|██▏       | 13/58 [00:00<00:01, 31.50it/s]Capturing num tokens (num_tokens=1792 avail_mem=70.98 GB):  31%|███       | 18/58 [00:00<00:01, 34.43it/s]Capturing num tokens (num_tokens=1536 avail_mem=70.97 GB):  31%|███       | 18/58 [00:00<00:01, 34.43it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=70.97 GB):  31%|███       | 18/58 [00:00<00:01, 34.43it/s]Capturing num tokens (num_tokens=1024 avail_mem=70.95 GB):  31%|███       | 18/58 [00:00<00:01, 34.43it/s]Capturing num tokens (num_tokens=960 avail_mem=70.96 GB):  31%|███       | 18/58 [00:00<00:01, 34.43it/s] Capturing num tokens (num_tokens=960 avail_mem=70.96 GB):  38%|███▊      | 22/58 [00:00<00:01, 32.15it/s]Capturing num tokens (num_tokens=896 avail_mem=70.96 GB):  38%|███▊      | 22/58 [00:00<00:01, 32.15it/s]Capturing num tokens (num_tokens=832 avail_mem=70.96 GB):  38%|███▊      | 22/58 [00:00<00:01, 32.15it/s]Capturing num tokens (num_tokens=768 avail_mem=70.95 GB):  38%|███▊      | 22/58 [00:00<00:01, 32.15it/s]Capturing num tokens (num_tokens=704 avail_mem=70.95 GB):  38%|███▊      | 22/58 [00:00<00:01, 32.15it/s]Capturing num tokens (num_tokens=640 avail_mem=70.95 GB):  38%|███▊      | 22/58 [00:00<00:01, 32.15it/s]Capturing num tokens (num_tokens=576 avail_mem=70.95 GB):  38%|███▊      | 22/58 [00:00<00:01, 32.15it/s]

    Capturing num tokens (num_tokens=576 avail_mem=70.95 GB):  48%|████▊     | 28/58 [00:00<00:00, 38.26it/s]Capturing num tokens (num_tokens=512 avail_mem=70.93 GB):  48%|████▊     | 28/58 [00:00<00:00, 38.26it/s]Capturing num tokens (num_tokens=480 avail_mem=70.95 GB):  48%|████▊     | 28/58 [00:00<00:00, 38.26it/s]Capturing num tokens (num_tokens=448 avail_mem=70.95 GB):  48%|████▊     | 28/58 [00:00<00:00, 38.26it/s]Capturing num tokens (num_tokens=416 avail_mem=70.95 GB):  48%|████▊     | 28/58 [00:00<00:00, 38.26it/s]Capturing num tokens (num_tokens=384 avail_mem=70.94 GB):  48%|████▊     | 28/58 [00:00<00:00, 38.26it/s]Capturing num tokens (num_tokens=352 avail_mem=70.94 GB):  48%|████▊     | 28/58 [00:00<00:00, 38.26it/s]Capturing num tokens (num_tokens=352 avail_mem=70.94 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.62it/s]Capturing num tokens (num_tokens=320 avail_mem=70.93 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.62it/s]Capturing num tokens (num_tokens=288 avail_mem=70.93 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.62it/s]Capturing num tokens (num_tokens=256 avail_mem=70.93 GB):  59%|█████▊    | 34/58 [00:01<00:00, 42.62it/s]Capturing num tokens (num_tokens=240 avail_mem=70.93 GB):  59%|█████▊    | 34/58 [00:01<00:00, 42.62it/s]Capturing num tokens (num_tokens=224 avail_mem=70.92 GB):  59%|█████▊    | 34/58 [00:01<00:00, 42.62it/s]

    Capturing num tokens (num_tokens=208 avail_mem=70.92 GB):  59%|█████▊    | 34/58 [00:01<00:00, 42.62it/s]Capturing num tokens (num_tokens=208 avail_mem=70.92 GB):  69%|██████▉   | 40/58 [00:01<00:00, 45.23it/s]Capturing num tokens (num_tokens=192 avail_mem=70.92 GB):  69%|██████▉   | 40/58 [00:01<00:00, 45.23it/s]Capturing num tokens (num_tokens=176 avail_mem=70.91 GB):  69%|██████▉   | 40/58 [00:01<00:00, 45.23it/s]Capturing num tokens (num_tokens=160 avail_mem=70.91 GB):  69%|██████▉   | 40/58 [00:01<00:00, 45.23it/s]Capturing num tokens (num_tokens=144 avail_mem=70.91 GB):  69%|██████▉   | 40/58 [00:01<00:00, 45.23it/s]Capturing num tokens (num_tokens=128 avail_mem=70.91 GB):  69%|██████▉   | 40/58 [00:01<00:00, 45.23it/s]Capturing num tokens (num_tokens=128 avail_mem=70.91 GB):  78%|███████▊  | 45/58 [00:01<00:00, 46.51it/s]Capturing num tokens (num_tokens=112 avail_mem=70.90 GB):  78%|███████▊  | 45/58 [00:01<00:00, 46.51it/s]Capturing num tokens (num_tokens=96 avail_mem=70.90 GB):  78%|███████▊  | 45/58 [00:01<00:00, 46.51it/s] Capturing num tokens (num_tokens=80 avail_mem=70.90 GB):  78%|███████▊  | 45/58 [00:01<00:00, 46.51it/s]Capturing num tokens (num_tokens=64 avail_mem=70.89 GB):  78%|███████▊  | 45/58 [00:01<00:00, 46.51it/s]Capturing num tokens (num_tokens=48 avail_mem=70.89 GB):  78%|███████▊  | 45/58 [00:01<00:00, 46.51it/s]

    Capturing num tokens (num_tokens=32 avail_mem=70.89 GB):  78%|███████▊  | 45/58 [00:01<00:00, 46.51it/s]Capturing num tokens (num_tokens=32 avail_mem=70.89 GB):  88%|████████▊ | 51/58 [00:01<00:00, 47.55it/s]Capturing num tokens (num_tokens=28 avail_mem=70.88 GB):  88%|████████▊ | 51/58 [00:01<00:00, 47.55it/s]Capturing num tokens (num_tokens=24 avail_mem=70.88 GB):  88%|████████▊ | 51/58 [00:01<00:00, 47.55it/s]Capturing num tokens (num_tokens=20 avail_mem=70.87 GB):  88%|████████▊ | 51/58 [00:01<00:00, 47.55it/s]Capturing num tokens (num_tokens=16 avail_mem=70.87 GB):  88%|████████▊ | 51/58 [00:01<00:00, 47.55it/s]Capturing num tokens (num_tokens=12 avail_mem=70.87 GB):  88%|████████▊ | 51/58 [00:01<00:00, 47.55it/s]Capturing num tokens (num_tokens=8 avail_mem=70.87 GB):  88%|████████▊ | 51/58 [00:01<00:00, 47.55it/s] Capturing num tokens (num_tokens=8 avail_mem=70.87 GB):  98%|█████████▊| 57/58 [00:01<00:00, 48.62it/s]Capturing num tokens (num_tokens=4 avail_mem=70.86 GB):  98%|█████████▊| 57/58 [00:01<00:00, 48.62it/s]Capturing num tokens (num_tokens=4 avail_mem=70.86 GB): 100%|██████████| 58/58 [00:01<00:00, 40.35it/s]


### Non-streaming Synchronous Generation


```python
prompts = [
    "Hello, my name is",
    "The president of the United States is",
    "The capital of France is",
    "The future of AI is",
]

sampling_params = {"temperature": 0.8, "top_p": 0.95}

outputs = llm.generate(prompts, sampling_params)
for prompt, output in zip(prompts, outputs):
    print("===============================")
    print(f"Prompt: {prompt}\nGenerated text: {output['text']}")
```

    ===============================
    Prompt: Hello, my name is
    Generated text:  Tammy and I am 17 years old and currently I am in a difficult situation. My mother has passed away, my father has passed away, and I have been living at home with my two brothers. I haven’t been with any other family for a while. I am very lonely, and I feel very sad.
    
    When I was growing up, I always had a brother and sister. I was always happy and did well in school. I always had no problem with my mother and father and no problems. I was always good with my brothers, but I didn’t feel lonely because they had a brother and sister. 
    
    I
    ===============================
    Prompt: The president of the United States is
    Generated text:  proposing to issue a new bill to expand gun control. The bill is expected to pass with a high percentage of support among the majority of Congressmen and women. However, if passed, the bill will have a significant impact on the lives of an estimated 4.0 million Americans. The bill will require the government to have the power to regulate firearms if it is passed. The bill will also require the government to have the power to punish those who violate the law if it is passed. If this bill is passed, the government will be able to issue a series of permits for people to buy guns, and this will create new jobs in
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, which is the most visited city in the world. The population of the capital is 8.5 million, with approximately 70% of the population under 30 years. How many people are under 30 years old in Paris? To determine how many people under 30 years old are in Paris, we start by calculating the total population of the capital of France. The population is given as 8.5 million. Since the population is 70% of the total population, we can express the total population as:
    
    \[
    \text{Total population} = 8.5 \times 
    ===============================
    Prompt: The future of AI is
    Generated text:  here. It’s going to be everywhere and in many different ways, from the way we interact with the internet to the way we interact with our fellow citizens and our local communities. The technologies that will drive these advancements will be the one that allow us to process and understand the data that is generated by these interactions, and that is what will enable us to identify and respond to the issue at hand – such as climate change, in our local, national, or global communities, and beyond.
    AI is, by definition, a method of computing that allows systems to learn and perform tasks without being explicitly programmed. To do this, a machine must


### Streaming Synchronous Generation


```python
prompts = [
    "Write a short, neutral self-introduction for a fictional character. Hello, my name is",
    "Provide a concise factual statement about France’s capital city. The capital of France is",
    "Explain possible future trends in artificial intelligence. The future of AI is",
]

sampling_params = {
    "temperature": 0.2,
    "top_p": 0.9,
}

print("\n=== Testing synchronous streaming generation with overlap removal ===\n")

for prompt in prompts:
    print(f"Prompt: {prompt}")
    merged_output = stream_and_merge(llm, prompt, sampling_params)
    print("Generated text:", merged_output)
    print()
```

    
    === Testing synchronous streaming generation with overlap removal ===
    
    Prompt: Write a short, neutral self-introduction for a fictional character. Hello, my name is


    Generated text:  [Name] and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as "La Ville Fluviale" (The River City). It is the largest city in France and the second-largest city in the European Union. Paris is a cultural and historical center with many famous landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also known for its cuisine, fashion, and art. Paris is a major transportation hub and a major tourist destination, attracting millions of visitors each year. The city is home to many international organizations and institutions, including the European Parliament and the United Nations. Paris is a city of contrasts, with its modern architecture and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn and adapt to human behavior and preferences. This could lead to more personalized and adaptive AI systems that can better understand and respond to human needs.
    
    2. Enhanced ethical considerations: As AI becomes more integrated with human intelligence, there will be increased scrutiny of its ethical implications. This could lead to more stringent regulations and guidelines for AI development and deployment.
    
    3. Greater reliance on AI for decision-making: AI is likely to become more integrated into decision-making processes, allowing machines to make more informed
    


### Non-streaming Asynchronous Generation


```python
prompts = [
    "Write a short, neutral self-introduction for a fictional character. Hello, my name is",
    "Provide a concise factual statement about France’s capital city. The capital of France is",
    "Explain possible future trends in artificial intelligence. The future of AI is",
]

sampling_params = {"temperature": 0.8, "top_p": 0.95}

print("\n=== Testing asynchronous batch generation ===")


async def main():
    outputs = await llm.async_generate(prompts, sampling_params)

    for prompt, output in zip(prompts, outputs):
        print(f"\nPrompt: {prompt}")
        print(f"Generated text: {output['text']}")


asyncio.run(main())
```

    
    === Testing asynchronous batch generation ===


    
    Prompt: Write a short, neutral self-introduction for a fictional character. Hello, my name is
    Generated text:  [Name] and I am [Position] at [Company]. I have always been passionate about [what you might call my main interest] and have always been a [how long ago did you start at [Company], and what position in the company did you start with?] assistant. I am always eager to learn and adapt to new things, and I am always looking for ways to contribute to the company and its goals. Thank you for having me! Let me know if you'd like to learn more about me. [Name] [Position] at [Company] [When you started at [Company], what role did you play?
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    That statement is accurate and factual. Paris is the capital city of France, located in the heart of the country, serving as the political, economic, and cultural center. The city is known for its rich history, diverse art and culture scene, and iconic landmarks such as the Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, and the Paris Opera. Paris is also a global hub for fashion, food, and entertainment, making it an important cultural and business center.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  highly uncertain, and it is difficult to predict with certainty what trends will emerge. However, some potential trends that may be seen in AI in the years to come include:
    
    1. Increased focus on ethical AI: As AI becomes more sophisticated, there will be a greater emphasis on ensuring that it is used ethically and responsibly. This may involve implementing ethical guidelines and standards to ensure that AI systems are not used to perpetuate biases or discriminate against certain groups of people.
    
    2. AI integration with other technologies: As AI becomes more advanced, it is likely to become more integrated with other technologies, such as medical devices, transportation systems, and communication


### Streaming Asynchronous Generation


```python
prompts = [
    "Write a short, neutral self-introduction for a fictional character. Hello, my name is",
    "Provide a concise factual statement about France’s capital city. The capital of France is",
    "Explain possible future trends in artificial intelligence. The future of AI is",
]

sampling_params = {"temperature": 0.8, "top_p": 0.95}

print("\n=== Testing asynchronous streaming generation (no repeats) ===")


async def main():
    for prompt in prompts:
        print(f"\nPrompt: {prompt}")
        print("Generated text: ", end="", flush=True)

        # Replace direct calls to async_generate with our custom overlap-aware version
        async for cleaned_chunk in async_stream_and_merge(llm, prompt, sampling_params):
            print(cleaned_chunk, end="", flush=True)

        print()  # New line after each prompt


asyncio.run(main())
```

    
    === Testing asynchronous streaming generation (no repeats) ===
    
    Prompt: Write a short, neutral self-introduction for a fictional character. Hello, my name is
    Generated text: 

     [

    Name

    ]

     and

     I

    'm

     a

     [

    occupation

     or

     type

     of

     work

    ]

     who

     is

     passionate

     about

     [

    what

     you

     like

     to

     do

    ].

     I

    'm

     always

     looking

     for

     ways

     to

     make

     the

     world

     a

     better

     place

    ,

     so

     I

     try

     to

     use

     my

     skills

     and

     knowledge

     to

     help

     others

    .

     I

     enjoy

     working

     with

     people

     and

     my

     team

     always

     works

     hard

     to

     achieve

     our

     goals

    .

     I

     have

     a

     great

     sense

     of

     humor

     and

     love

     to

     laugh

    .

     In

     my

     free

     time

    ,

     I

     enjoy

     playing

     basketball

    ,

     hiking

    ,

     and

     exploring

     the

     outdoors

    .

     I

    'm

     always

     learning

     and

     striving

     to

     grow

     as

     a

     person

    .

     I

     believe

     in

     continuous

     improvement

     and

     I

    'm

     always

     looking

     for

     ways

     to

     stay

     up

    -to

    -date

     with

     the

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    That

    's

     correct

    !

     Paris

     is

     the

     capital

     city

     of

     France

     and

     serves

     as

     the

     country

    's

     largest

     city

    .

     It

    's

     known

     for

     its

     magnificent

     architecture

    ,

     vibrant

     culture

    ,

     and

     beautiful

     parks

    ,

     making

     it

     a

     popular

     tourist

     destination

    .

     The

     city

     is

     also

     known

     for

     its

     annual

     Carn

    aval

    ,

     which

     is

     a

     major

     cultural

     event

     and

     an

     important

     part

     of

     French

     culture

    .

     Paris

     is

     a

     city

     with

     a

     rich

     history

     and

     a

     unique

     blend

     of

     French

    ,

     Spanish

    ,

     and

     Italian

     cultures

    .

     It

    's

     a

     popular

     place

     for

     people

     of

     all

     ages

     to

     explore

    ,

     learn

     about

    ,

     and

     enjoy

    .

     That

    's

     why

     Paris

     is

     widely

     regarded

     as

     one

     of

     the

     most

     beautiful

     cities

     in

     the

     world

    .

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     one

     of

     unprecedented

     growth

     and

     complexity

    .

     Some

     possible

     trends

     that

     could

     shape

     the

     AI

     landscape

     in

     the

     coming

     years

     include

    :
    


    1

    .

     AI

     will

     become

     more

     integrated

     into

     various

     industries

    ,

     from

     healthcare

     and

     finance

     to

     manufacturing

     and

     transportation

    .
    


    2

    .

     AI

     will

     become

     more

     sophisticated

     and

     capable

     of

     performing

     tasks

     that

     were

     previously

     thought

     impossible

    .

     This

     includes

     image

     recognition

    ,

     natural

     language

     processing

    ,

     and

     decision

    -making

    .
    


    3

    .

     AI

     will

     become

     more

     ubiquitous

     and

     accessible

    ,

     with

     more

     people

     gaining

     access

     to

     it

     through

     new

     tools

     and

     platforms

    .
    


    4

    .

     AI

     will

     become

     more

     personalized

     and

     adaptive

    ,

     with

     machines

     learning

     from

     user

     behavior

     and

     feedback

     to

     improve

     their

     performance

    .
    


    5

    .

     AI

     will

     become

     more

    



```python
llm.shutdown()
```
