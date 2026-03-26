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

    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    [2026-03-26 04:03:26] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.


    [2026-03-26 04:03:26] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.


    [2026-03-26 04:03:26] INFO utils.py:164: NumExpr defaulting to 16 threads.


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    [2026-03-26 04:03:29] INFO server_args.py:2245: Attention backend not specified. Use fa3 backend by default.


    [2026-03-26 04:03:29] INFO server_args.py:3543: Set soft_watchdog_timeout since in CI


    [2026-03-26 04:03:29] INFO engine.py:177: server_args=ServerArgs(model_path='qwen/qwen2.5-0.5b-instruct', tokenizer_path='qwen/qwen2.5-0.5b-instruct', tokenizer_mode='auto', tokenizer_worker_num=1, skip_tokenizer_init=False, load_format='auto', model_loader_extra_config='{}', trust_remote_code=False, context_length=None, is_embedding=False, enable_multimodal=None, revision=None, model_impl='auto', host='127.0.0.1', port=30000, fastapi_root_path='', grpc_mode=False, skip_server_warmup=False, warmups=None, nccl_port=None, checkpoint_engine_wait_weights_before_ready=False, ssl_keyfile=None, ssl_certfile=None, ssl_ca_certs=None, ssl_keyfile_password=None, enable_ssl_refresh=False, dtype='auto', quantization=None, quantization_param_path=None, kv_cache_dtype='auto', enable_fp32_lm_head=False, modelopt_quant=None, modelopt_checkpoint_restore_path=None, modelopt_checkpoint_save_path=None, modelopt_export_path=None, quantize_and_serve=False, rl_quant_profile=None, mem_fraction_static=0.903, max_running_requests=128, max_queued_requests=None, max_total_tokens=20480, chunked_prefill_size=8192, enable_dynamic_chunking=False, max_prefill_tokens=16384, prefill_max_requests=None, schedule_policy='fcfs', enable_priority_scheduling=False, disable_priority_preemption=False, default_priority_value=None, abort_on_priority_when_disabled=False, schedule_low_priority_values_first=False, priority_scheduling_preemption_threshold=10, schedule_conservativeness=1.0, page_size=1, swa_full_tokens_ratio=0.8, disable_hybrid_swa_memory=False, radix_eviction_policy='lru', enable_prefill_delayer=False, prefill_delayer_max_delay_passes=30, prefill_delayer_token_usage_low_watermark=None, prefill_delayer_forward_passes_buckets=None, prefill_delayer_wait_seconds_buckets=None, device='cuda', tp_size=1, pp_size=1, pp_max_micro_batch_size=None, pp_async_batch_depth=0, stream_interval=1, incremental_streaming_output=False, enable_streaming_session=False, random_seed=728929166, constrained_json_whitespace_pattern=None, constrained_json_disable_any_whitespace=False, watchdog_timeout=300, soft_watchdog_timeout=300, dist_timeout=None, download_dir=None, model_checksum=None, base_gpu_id=0, gpu_id_step=1, sleep_on_idle=False, use_ray=False, custom_sigquit_handler=None, log_level='error', log_level_http=None, log_requests=False, log_requests_level=2, log_requests_format='text', log_requests_target=None, uvicorn_access_log_exclude_prefixes=[], crash_dump_folder=None, show_time_cost=False, enable_metrics=False, enable_metrics_for_all_schedulers=False, tokenizer_metrics_custom_labels_header='x-custom-labels', tokenizer_metrics_allowed_custom_labels=None, extra_metric_labels=None, bucket_time_to_first_token=None, bucket_inter_token_latency=None, bucket_e2e_request_latency=None, collect_tokens_histogram=False, prompt_tokens_buckets=None, generation_tokens_buckets=None, gc_warning_threshold_secs=0.0, decode_log_interval=40, enable_request_time_stats_logging=False, kv_events_config=None, enable_trace=False, otlp_traces_endpoint='localhost:4317', export_metrics_to_file=False, export_metrics_to_file_dir=None, api_key=None, admin_api_key=None, served_model_name='qwen/qwen2.5-0.5b-instruct', weight_version='default', chat_template=None, hf_chat_template_name=None, completion_template=None, file_storage_path='sglang_storage', enable_cache_report=False, reasoning_parser=None, tool_call_parser=None, tool_server=None, sampling_defaults='model', dp_size=1, load_balance_method='round_robin', attn_cp_size=1, moe_dp_size=1, dist_init_addr=None, nnodes=1, node_rank=0, json_model_override_args='{}', preferred_sampling_params=None, enable_lora=None, enable_lora_overlap_loading=None, max_lora_rank=None, lora_target_modules=None, lora_paths=None, max_loaded_loras=None, max_loras_per_batch=8, lora_eviction_policy='lru', lora_backend='csgmv', max_lora_chunk_size=16, attention_backend='fa3', decode_attention_backend=None, prefill_attention_backend=None, sampling_backend='flashinfer', grammar_backend='xgrammar', mm_attention_backend=None, fp8_gemm_runner_backend='auto', fp4_gemm_runner_backend='auto', nsa_prefill_backend=None, nsa_decode_backend=None, disable_flashinfer_autotune=False, mamba_backend='triton', speculative_algorithm=None, speculative_draft_model_path=None, speculative_draft_model_revision=None, speculative_draft_load_format=None, speculative_num_steps=None, speculative_eagle_topk=None, speculative_num_draft_tokens=None, speculative_accept_threshold_single=1.0, speculative_accept_threshold_acc=1.0, speculative_token_map=None, speculative_attention_mode='prefill', speculative_draft_attention_backend=None, speculative_moe_runner_backend='auto', speculative_moe_a2a_backend=None, speculative_draft_model_quantization=None, speculative_ngram_min_match_window_size=1, speculative_ngram_max_match_window_size=12, speculative_ngram_min_bfs_breadth=1, speculative_ngram_max_bfs_breadth=10, speculative_ngram_match_type='BFS', speculative_ngram_max_trie_depth=18, speculative_ngram_capacity=10000000, enable_multi_layer_eagle=False, ep_size=1, moe_a2a_backend='none', moe_runner_backend='auto', flashinfer_mxfp4_moe_precision='default', enable_flashinfer_allreduce_fusion=False, enable_aiter_allreduce_fusion=False, deepep_mode='auto', ep_num_redundant_experts=0, ep_dispatch_algorithm=None, init_expert_location='trivial', enable_eplb=False, eplb_algorithm='auto', eplb_rebalance_num_iterations=1000, eplb_rebalance_layers_per_chunk=None, eplb_min_rebalancing_utilization_threshold=1.0, expert_distribution_recorder_mode=None, expert_distribution_recorder_buffer_size=1000, enable_expert_distribution_metrics=False, deepep_config=None, moe_dense_tp_size=None, elastic_ep_backend=None, enable_elastic_expert_backup=False, mooncake_ib_device=None, max_mamba_cache_size=None, mamba_ssm_dtype=None, mamba_full_memory_ratio=0.9, mamba_scheduler_strategy='no_buffer', mamba_track_interval=256, linear_attn_backend='triton', linear_attn_decode_backend=None, linear_attn_prefill_backend=None, enable_hierarchical_cache=False, hicache_ratio=2.0, hicache_size=0, hicache_write_policy='write_through', hicache_io_backend='kernel', hicache_mem_layout='layer_first', disable_hicache_numa_detect=False, hicache_storage_backend=None, hicache_storage_prefetch_policy='best_effort', hicache_storage_backend_extra_config=None, enable_hisparse=False, hisparse_config=None, enable_lmcache=False, kt_weight_path=None, kt_method=None, kt_cpuinfer=None, kt_threadpool_count=None, kt_num_gpu_experts=None, kt_max_deferred_experts_per_token=None, dllm_algorithm=None, dllm_algorithm_config=None, enable_double_sparsity=False, ds_channel_config_path=None, ds_heavy_channel_num=32, ds_heavy_token_num=256, ds_heavy_channel_type='qk', ds_sparse_decode_threshold=4096, cpu_offload_gb=0, offload_group_size=-1, offload_num_in_group=1, offload_prefetch_step=1, offload_mode='cpu', multi_item_scoring_delimiter=None, disable_radix_cache=False, cuda_graph_max_bs=4, cuda_graph_bs=[1, 2, 4, 8, 12, 16, 24, 32, 40, 48, 56, 64, 72, 80, 88, 96, 104, 112, 120, 128, 136, 144, 152, 160, 168, 176, 184, 192, 200, 208, 216, 224, 232, 240, 248, 256], disable_cuda_graph=True, disable_cuda_graph_padding=False, enable_profile_cuda_graph=False, enable_cudagraph_gc=False, enable_layerwise_nvtx_marker=False, enable_nccl_nvls=False, enable_symm_mem=False, disable_flashinfer_cutlass_moe_fp4_allgather=False, enable_tokenizer_batch_encode=False, disable_tokenizer_batch_decode=False, disable_outlines_disk_cache=False, disable_custom_all_reduce=False, enable_mscclpp=False, enable_torch_symm_mem=False, pre_warm_nccl=False, disable_overlap_schedule=False, enable_mixed_chunk=False, enable_dp_attention=False, enable_dp_lm_head=False, enable_two_batch_overlap=False, enable_single_batch_overlap=False, tbo_token_distribution_threshold=0.48, enable_torch_compile=False, disable_piecewise_cuda_graph=False, enforce_piecewise_cuda_graph=False, enable_torch_compile_debug_mode=False, torch_compile_max_bs=32, piecewise_cuda_graph_max_tokens=8192, piecewise_cuda_graph_tokens=[4, 8, 12, 16, 20, 24, 28, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 352, 384, 416, 448, 480, 512, 576, 640, 704, 768, 832, 896, 960, 1024, 1280, 1536, 1792, 2048, 2304, 2560, 2816, 3072, 3328, 3584, 3840, 4096, 4608, 5120, 5632, 6144, 6656, 7168, 7680, 8192], piecewise_cuda_graph_compiler='eager', torchao_config='', enable_nan_detection=False, enable_p2p_check=False, triton_attention_reduce_in_fp32=False, triton_attention_num_kv_splits=8, triton_attention_split_tile_size=None, num_continuous_decode_steps=1, delete_ckpt_after_loading=False, enable_memory_saver=False, enable_weights_cpu_backup=False, enable_draft_weights_cpu_backup=False, allow_auto_truncate=False, enable_custom_logit_processor=False, flashinfer_mla_disable_ragged=False, disable_shared_experts_fusion=False, disable_chunked_prefix_cache=False, disable_fast_image_processor=False, keep_mm_feature_on_device=False, enable_return_hidden_states=False, enable_return_routed_experts=False, scheduler_recv_interval=1, numa_node=None, enable_deterministic_inference=False, rl_on_policy_target=None, enable_attn_tp_input_scattered=False, enable_nsa_prefill_context_parallel=False, nsa_prefill_cp_mode='round-robin-split', enable_fused_qk_norm_rope=False, enable_precise_embedding_interpolation=False, enable_fused_moe_sum_all_reduce=False, enable_prefill_context_parallel=False, prefill_cp_mode='in-seq-split', enable_dynamic_batch_tokenizer=False, dynamic_batch_tokenizer_batch_size=32, dynamic_batch_tokenizer_batch_timeout=0.002, debug_tensor_dump_output_folder=None, debug_tensor_dump_layers=None, debug_tensor_dump_input_file=None, debug_tensor_dump_inject=False, disaggregation_mode='null', disaggregation_transfer_backend='mooncake', disaggregation_bootstrap_port=8998, disaggregation_ib_device=None, disaggregation_decode_enable_offload_kvcache=False, num_reserved_decode_tokens=512, disaggregation_decode_polling_interval=1, encoder_only=False, language_only=False, encoder_transfer_backend='zmq_to_scheduler', encoder_urls=[], enable_adaptive_dispatch_to_encoder=False, custom_weight_loader=[], weight_loader_disable_mmap=False, remote_instance_weight_loader_seed_instance_ip=None, remote_instance_weight_loader_seed_instance_service_port=None, remote_instance_weight_loader_send_weights_group_ports=None, remote_instance_weight_loader_backend='nccl', remote_instance_weight_loader_start_seed_via_transfer_engine=False, modelexpress_config=None, enable_pdmux=False, pdmux_config_path=None, sm_group_num=8, mm_max_concurrent_calls=32, mm_per_request_timeout=10.0, enable_broadcast_mm_inputs_process=False, enable_prefix_mm_cache=False, mm_enable_dp_encoder=False, mm_process_config={}, limit_mm_data_per_request=None, enable_mm_global_cache=False, decrypted_config_file=None, decrypted_draft_config_file=None, forward_hooks=None)


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-26 04:03:38.574 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 04:03:38] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 04:03:38.574 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 04:03:38] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 04:03:38.574 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 04:03:38] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 04:03:38.574 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 04:03:38] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 04:03:38.574 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 04:03:38] Persistent cache disabled, using in-memory JIT cache


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.62it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.61it/s]


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:19,  2.69it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:19,  2.69it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:19,  2.69it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:19,  2.69it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:19,  2.69it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:19,  2.69it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:19,  2.69it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:06,  6.99it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:06,  6.99it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:06,  6.99it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:06,  6.99it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:06,  6.99it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:06,  6.99it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:06,  6.99it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:06,  6.99it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:02<00:06,  6.99it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 13.98it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 13.98it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 13.98it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 13.98it/s] ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 13.98it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 13.98it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 13.98it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 13.98it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:02<00:01, 20.47it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:02<00:01, 20.47it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:02<00:01, 20.47it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:02<00:01, 20.47it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:02<00:01, 20.47it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:02<00:01, 20.47it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:02<00:01, 20.47it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:02<00:01, 20.47it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:02<00:00, 27.33it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:02<00:00, 27.33it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:02<00:00, 27.33it/s]

    Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:02<00:00, 27.33it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:02<00:00, 27.33it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:02<00:00, 27.33it/s]

    Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:02<00:00, 27.33it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:02<00:00, 32.31it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:02<00:00, 32.31it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 32.31it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 32.31it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 32.31it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 32.31it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 32.31it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 37.04it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 37.04it/s]

    Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 37.04it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 37.04it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 37.04it/s]

    Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 37.04it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 37.04it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 40.85it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 40.85it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 40.85it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 40.85it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 40.85it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 40.85it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 40.85it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 40.85it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.57it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=121.74 GB):   0%|          | 0/58 [00:00<?, ?it/s]ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    Capturing num tokens (num_tokens=7680 avail_mem=121.71 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=121.71 GB):   3%|▎         | 2/58 [00:00<00:02, 18.73it/s]Capturing num tokens (num_tokens=7168 avail_mem=121.70 GB):   3%|▎         | 2/58 [00:00<00:02, 18.73it/s]Capturing num tokens (num_tokens=6656 avail_mem=121.70 GB):   3%|▎         | 2/58 [00:00<00:02, 18.73it/s]Capturing num tokens (num_tokens=6144 avail_mem=121.70 GB):   3%|▎         | 2/58 [00:00<00:02, 18.73it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 21.68it/s]Capturing num tokens (num_tokens=5632 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 21.68it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 21.68it/s]Capturing num tokens (num_tokens=4608 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 21.68it/s]Capturing num tokens (num_tokens=4096 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 21.68it/s]Capturing num tokens (num_tokens=4096 avail_mem=121.70 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.38it/s]Capturing num tokens (num_tokens=3840 avail_mem=121.69 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.38it/s]Capturing num tokens (num_tokens=3584 avail_mem=121.41 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.38it/s]Capturing num tokens (num_tokens=3328 avail_mem=121.41 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.38it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=121.26 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.38it/s]Capturing num tokens (num_tokens=3072 avail_mem=121.26 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.47it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.70 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.47it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=120.70 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.47it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.70 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.47it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.69 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.47it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.69 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.47it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.69 GB):  31%|███       | 18/58 [00:00<00:01, 35.56it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.69 GB):  31%|███       | 18/58 [00:00<00:01, 35.56it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.68 GB):  31%|███       | 18/58 [00:00<00:01, 35.56it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.66 GB):  31%|███       | 18/58 [00:00<00:01, 35.56it/s]

    Capturing num tokens (num_tokens=960 avail_mem=120.68 GB):  31%|███       | 18/58 [00:00<00:01, 35.56it/s] Capturing num tokens (num_tokens=896 avail_mem=120.67 GB):  31%|███       | 18/58 [00:00<00:01, 35.56it/s]Capturing num tokens (num_tokens=896 avail_mem=120.67 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.58it/s]Capturing num tokens (num_tokens=832 avail_mem=120.67 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.58it/s]

    Capturing num tokens (num_tokens=768 avail_mem=120.67 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.58it/s]Capturing num tokens (num_tokens=704 avail_mem=120.66 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.58it/s]Capturing num tokens (num_tokens=640 avail_mem=120.66 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.58it/s]Capturing num tokens (num_tokens=576 avail_mem=120.66 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.58it/s]Capturing num tokens (num_tokens=576 avail_mem=120.66 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.62it/s]Capturing num tokens (num_tokens=512 avail_mem=120.65 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.62it/s]Capturing num tokens (num_tokens=480 avail_mem=120.66 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.62it/s]Capturing num tokens (num_tokens=448 avail_mem=120.66 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.62it/s]

    Capturing num tokens (num_tokens=416 avail_mem=120.66 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.62it/s]Capturing num tokens (num_tokens=384 avail_mem=120.66 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.62it/s]Capturing num tokens (num_tokens=384 avail_mem=120.66 GB):  57%|█████▋    | 33/58 [00:00<00:00, 42.30it/s]Capturing num tokens (num_tokens=352 avail_mem=120.65 GB):  57%|█████▋    | 33/58 [00:00<00:00, 42.30it/s]

    Capturing num tokens (num_tokens=320 avail_mem=120.63 GB):  57%|█████▋    | 33/58 [00:00<00:00, 42.30it/s]Capturing num tokens (num_tokens=288 avail_mem=120.22 GB):  57%|█████▋    | 33/58 [00:00<00:00, 42.30it/s]Capturing num tokens (num_tokens=256 avail_mem=120.22 GB):  57%|█████▋    | 33/58 [00:00<00:00, 42.30it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  57%|█████▋    | 33/58 [00:01<00:00, 42.30it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  66%|██████▌   | 38/58 [00:01<00:00, 42.88it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  66%|██████▌   | 38/58 [00:01<00:00, 42.88it/s]ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    Capturing num tokens (num_tokens=208 avail_mem=120.21 GB):  66%|██████▌   | 38/58 [00:01<00:00, 42.88it/s]

    Capturing num tokens (num_tokens=192 avail_mem=120.21 GB):  66%|██████▌   | 38/58 [00:01<00:00, 42.88it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  66%|██████▌   | 38/58 [00:01<00:00, 42.88it/s]Capturing num tokens (num_tokens=160 avail_mem=120.20 GB):  66%|██████▌   | 38/58 [00:01<00:00, 42.88it/s]

    Capturing num tokens (num_tokens=160 avail_mem=120.20 GB):  74%|███████▍  | 43/58 [00:01<00:00, 43.38it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  74%|███████▍  | 43/58 [00:01<00:00, 43.38it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  74%|███████▍  | 43/58 [00:01<00:00, 43.38it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  74%|███████▍  | 43/58 [00:01<00:00, 43.38it/s]Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  74%|███████▍  | 43/58 [00:01<00:00, 43.38it/s] Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  74%|███████▍  | 43/58 [00:01<00:00, 43.38it/s]

    Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  83%|████████▎ | 48/58 [00:01<00:00, 43.54it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  83%|████████▎ | 48/58 [00:01<00:00, 43.54it/s]

    Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  83%|████████▎ | 48/58 [00:01<00:00, 43.54it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  83%|████████▎ | 48/58 [00:01<00:00, 43.54it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  83%|████████▎ | 48/58 [00:01<00:00, 43.54it/s]

    Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  83%|████████▎ | 48/58 [00:01<00:00, 43.54it/s]Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  91%|█████████▏| 53/58 [00:01<00:00, 44.14it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  91%|█████████▏| 53/58 [00:01<00:00, 44.14it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  91%|█████████▏| 53/58 [00:01<00:00, 44.14it/s]Capturing num tokens (num_tokens=12 avail_mem=120.15 GB):  91%|█████████▏| 53/58 [00:01<00:00, 44.14it/s]Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  91%|█████████▏| 53/58 [00:01<00:00, 44.14it/s] Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  91%|█████████▏| 53/58 [00:01<00:00, 44.14it/s]

    Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 44.94it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 39.54it/s]


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


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

    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    ===============================
    Prompt: Hello, my name is
    Generated text:  Misra. I want to talk about my research, research outputs, and my motivation and goals. I'm doing my PhD on the role of the endocannabinoid system in the development of Alzheimer's disease. I'm funded by the UK Medical Research Council. In this research, I want to see if endocannabinoids help to prevent Alzheimer's disease. I'm working with an international team and we also have a grant from the Alzheimer's Society to do a randomised controlled trial. I'm also doing research on the endocannabinoid system and the anti-inflammatory function of cannabidiol, a non-psychoactive
    ===============================
    Prompt: The president of the United States is
    Generated text:  trying to decide whether to hold another presidential election. He has data from the last two presidential elections, which he considers to be "good" and "bad." In the "good" election, he got 51% of the votes. In the "bad" election, he got 48% of the votes. He also knows that the margin of error for the "good" election is 4%, and for the "bad" election is 2%. The president wants to know the standard error of the difference in his votes between the two elections. What is the standard error of the difference in votes between the "good
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. The city has many tall buildings and is very famous for its Opera House and Louvre Museum. However, they recently decided to put the name "Paris" on the ground instead of the name of the Opera House and the Louvre Museum. So, they decided to make the capital of the city "Paris Plus". 
    
    Let's use the following notations:
    - Capital of the city: \( \mathcal{C} \)
    - Opera House: \( \mathcal{O} \)
    - Louvre Museum: \( \mathcal{M} \)
    
    Given that:
    1. \( \mathcal{C}
    ===============================
    Prompt: The future of AI is
    Generated text:  a question that we all want to know, and with good reason. The technology has already changed the way we communicate and even our daily lives.
    But as much as we want to see the future of AI, we need to be careful. In fact, there are already some very serious problems with AI. The company that controls the technology is not at all interested in the future of AI.
    Here are five things you need to be aware of when it comes to AI.
    1. Autonomous Weapons: We already have autonomous weapons. They are designed to make decisions on their own, without being humanely programmed. They are used for a variety of


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


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    Generated text:  Paris, which is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a major center for art, culture, and politics, and is home to many famous museums and historical sites. Paris is a bustling city with a rich cultural heritage and is a popular tourist destination. Its location on the Mediterranean Sea makes it a popular tourist destination, attracting millions of visitors each year. Paris is also known for its cuisine, with its famous dishes such as croissants, boudin, and escargot. The city is also home to many other notable landmarks and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends:
    
    1. Increased focus on ethical AI: As more people become aware of the potential risks of AI, there will be a greater emphasis on developing ethical AI that is designed to minimize harm and maximize benefits. This will likely involve developing AI that is transparent, accountable, and accountable to humans.
    
    2. Greater use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes and reduce costs. As AI becomes more advanced, we may see even more widespread use of AI
    


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


    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    
    Prompt: Write a short, neutral self-introduction for a fictional character. Hello, my name is
    Generated text:  [insert name here], and I'm a computer scientist with a master's degree in computer science. I specialize in designing algorithms and developing software solutions that solve complex problems. I'm passionate about making technology accessible to everyone, regardless of their background or experience. I'm always looking for ways to improve my skills and stay up-to-date with the latest technologies. I'm a constant learner, always trying to stay informed about new developments in the field. Thank you for considering me as a potential match. 
    
    Feel free to add any additional information or details about your background that you think would be helpful to the AI.
    
    Hello, my name is [
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, a historical and cultural center known for its vibrant arts and culture scene, ancient monuments, and high standards of living. Its rich history, romantic atmosphere, and diverse neighborhoods make it a popular tourist destination. Paris is home to many famous landmarks, museums, theaters, and art galleries, as well as renowned culinary and gastronomic offerings. The city also plays a significant role in France’s economy, with its central business district, financial district, and service industry being the largest in the country. Paris is also an important center for international affairs and diplomacy, hosting the European Parliament and other high-profile international gatherings. Finally, Paris is renowned for
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  incredibly exciting and diverse, with many different trends emerging as technology continues to evolve. Here are some of the key trends that are likely to shape AI in the coming years:
    
    1. Increased Autonomy: As AI becomes more sophisticated, it will become more autonomous, with the ability to learn and adapt without human intervention. This will lead to more efficient and effective use of AI-powered technologies.
    
    2. Personalized AI: AI will become more personalized, with algorithms that learn from data to provide more accurate and relevant recommendations and predictions.
    
    3. Autonomous robots: Robots will become more autonomous, able to perform tasks without human intervention, such as cleaning,


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

    ],

     and

     I

    'm

     a

    /an

     [

    Occup

    ation

     or

     Profession

    ].

     I

    've

     always

     been

     fascinated

     by

     [

    Your

     Passion

    /F

    antasy

    /

    Interest

    ]

     and

     have

     always

     dreamed

     of

     traveling

     to

     [

    Your

     Destination

    /

    World

    ].

     I

    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


     love

     [

    How

     you

     see

     yourself

     evolving

     as

     an

     individual

    ]

     and

     would

     love

     to

     explore

     new

     cultures

     and

     try

     new

     experiences

    .

     What

     kind

     of

     experiences

     do

     you

     have

     in

     mind

    ?


    Welcome

     to

     [

    Your

     Name

    ],

     a

     storyt

    eller

     and

     adventurer

    .

     I

     am

     a

     passionate

     traveler

     and

     a

     lifelong

     learner

    ,

     always

     seeking

     to

     expand

     my

     hor

    izons

     and

     explore

     new

     cultures

    .

     I

     love

     the

     thrill

     of

     the

     unknown

     and

     the

     joy

     of

     discovery

    ,

     and

     I

    'm

     always

     on

     the

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     also

     known

     as

     the

     City

     of

     Light

    ,

     a

     bustling

     met

    ropolis

     with

     a

     rich

     history

     and

     culture

    .
    


    That

    's

     a

     great

     summary

    !

     Could

     you

     provide

     me

     with

     some

     interesting

     facts

     about

     Paris

    ?

     I

    'm

     really

     interested

     in

     learning

     more

     about

    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


     its

     landmarks

     and

     attractions

    .

     Sure

    !

     Paris

     is

     home

     to

     a

     vast

     array

     of

     landmarks

     and

     attractions

     that

     showcase

     the

     city

    's

     rich

     history

     and

     culture

    .

     Here

     are

     a

     few

     interesting

     facts

     about

     Paris

    :
    


    1

    .

     The

     Lou

    vre

     is

     the

     world

    's

     largest

     museum

    ,

     with

     over

     

    1

    ,

    7

    0

    0

     displayed

     works

     of

     art

     and

     

    1

    ,

    8

    0

    0

     other

     objects

    .
    


    2

    .

     The

     Ch

    amps

    -E

    lys

    ées

     is

     the

     famous

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     shaped

     by

     a

     range

     of

     factors

    ,

     including

     advancements

     in

     computing

     power

    ,

     data

     processing

     and

     analysis

    ,

     and

     the

     development

     of

     new

     technologies

     and

     algorithms

    .

     Here

     are

     some

     potential

     trends

     that

     could

     be

     expected

     in

     the

     field

     of

     AI

     in

     the

     next

     decade

    ERROR:tornado.general:Uncaught exception in ZMQStream callback
    Traceback (most recent call last):
      File "/usr/local/lib/python3.10/dist-packages/zmq/eventloop/zmqstream.py", line 565, in _log_error
        f.result()
      File "/usr/lib/python3.10/asyncio/futures.py", line 201, in result
        raise self._exception.with_traceback(self._exception_tb)
      File "/usr/lib/python3.10/asyncio/tasks.py", line 232, in __step
        result = coro.send(None)
      File "/usr/local/lib/python3.10/dist-packages/ipykernel/kernelbase.py", line 584, in shell_channel_thread_main
        _, msg2 = self.session.feed_identities(msg, copy=False)
      File "/usr/local/lib/python3.10/dist-packages/jupyter_client/session.py", line 998, in feed_identities
        raise ValueError(msg)
    ValueError: DELIM not in msg_list


    :
    


    1

    .

     Increased

     focus

     on

     ethical

     AI

    :

     As

     more

     and

     more

     data

     is

     collected

     and

     analyzed

    ,

     there

     is

     a

     growing

     concern

     about

     the

     potential

     misuse

     of

     AI

     technologies

    .

     There

     may

     be

     a

     push

     towards

     developing

     more

     ethical

     AI

     that

     is

     transparent

    ,

     accountable

     and

     responsible

     for

     its

     decisions

     and

     outputs

    .
    


    2

    .

     Integration

     of

     AI

     with

     other

     technologies

    :

     AI

     is

     already

     being

     integrated

     into

     a

     wide

     range

     of

     technologies

    ,

     including

     transportation

    ,

     healthcare

    



```python
llm.shutdown()
```
