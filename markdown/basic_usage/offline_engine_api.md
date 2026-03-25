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

    [2026-03-25 12:50:38] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.


    [2026-03-25 12:50:38] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.


    [2026-03-25 12:50:38] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-25 12:50:41] INFO server_args.py:2246: Attention backend not specified. Use fa3 backend by default.


    [2026-03-25 12:50:41] INFO server_args.py:3544: Set soft_watchdog_timeout since in CI


    [2026-03-25 12:50:41] INFO engine.py:177: server_args=ServerArgs(model_path='qwen/qwen2.5-0.5b-instruct', tokenizer_path='qwen/qwen2.5-0.5b-instruct', tokenizer_mode='auto', tokenizer_worker_num=1, skip_tokenizer_init=False, load_format='auto', model_loader_extra_config='{}', trust_remote_code=False, context_length=None, is_embedding=False, enable_multimodal=None, revision=None, model_impl='auto', host='127.0.0.1', port=30000, fastapi_root_path='', grpc_mode=False, skip_server_warmup=False, warmups=None, nccl_port=None, checkpoint_engine_wait_weights_before_ready=False, ssl_keyfile=None, ssl_certfile=None, ssl_ca_certs=None, ssl_keyfile_password=None, enable_ssl_refresh=False, dtype='auto', quantization=None, quantization_param_path=None, kv_cache_dtype='auto', enable_fp32_lm_head=False, modelopt_quant=None, modelopt_checkpoint_restore_path=None, modelopt_checkpoint_save_path=None, modelopt_export_path=None, quantize_and_serve=False, rl_quant_profile=None, mem_fraction_static=0.903, max_running_requests=128, max_queued_requests=None, max_total_tokens=20480, chunked_prefill_size=8192, enable_dynamic_chunking=False, max_prefill_tokens=16384, prefill_max_requests=None, schedule_policy='fcfs', enable_priority_scheduling=False, disable_priority_preemption=False, default_priority_value=None, abort_on_priority_when_disabled=False, schedule_low_priority_values_first=False, priority_scheduling_preemption_threshold=10, schedule_conservativeness=1.0, page_size=1, swa_full_tokens_ratio=0.8, disable_hybrid_swa_memory=False, radix_eviction_policy='lru', enable_prefill_delayer=False, prefill_delayer_max_delay_passes=30, prefill_delayer_token_usage_low_watermark=None, prefill_delayer_forward_passes_buckets=None, prefill_delayer_wait_seconds_buckets=None, device='cuda', tp_size=1, pp_size=1, pp_max_micro_batch_size=None, pp_async_batch_depth=0, stream_interval=1, incremental_streaming_output=False, enable_streaming_session=False, random_seed=662527141, constrained_json_whitespace_pattern=None, constrained_json_disable_any_whitespace=False, watchdog_timeout=300, soft_watchdog_timeout=300, dist_timeout=None, download_dir=None, model_checksum=None, base_gpu_id=0, gpu_id_step=1, sleep_on_idle=False, use_ray=False, custom_sigquit_handler=None, log_level='error', log_level_http=None, log_requests=False, log_requests_level=2, log_requests_format='text', log_requests_target=None, uvicorn_access_log_exclude_prefixes=[], crash_dump_folder=None, show_time_cost=False, enable_metrics=False, enable_metrics_for_all_schedulers=False, tokenizer_metrics_custom_labels_header='x-custom-labels', tokenizer_metrics_allowed_custom_labels=None, extra_metric_labels=None, bucket_time_to_first_token=None, bucket_inter_token_latency=None, bucket_e2e_request_latency=None, collect_tokens_histogram=False, prompt_tokens_buckets=None, generation_tokens_buckets=None, gc_warning_threshold_secs=0.0, decode_log_interval=40, enable_request_time_stats_logging=False, kv_events_config=None, enable_trace=False, otlp_traces_endpoint='localhost:4317', export_metrics_to_file=False, export_metrics_to_file_dir=None, api_key=None, admin_api_key=None, served_model_name='qwen/qwen2.5-0.5b-instruct', weight_version='default', chat_template=None, hf_chat_template_name=None, completion_template=None, file_storage_path='sglang_storage', enable_cache_report=False, reasoning_parser=None, tool_call_parser=None, tool_server=None, sampling_defaults='model', dp_size=1, load_balance_method='round_robin', attn_cp_size=1, moe_dp_size=1, dist_init_addr=None, nnodes=1, node_rank=0, json_model_override_args='{}', preferred_sampling_params=None, enable_lora=None, enable_lora_overlap_loading=None, max_lora_rank=None, lora_target_modules=None, lora_paths=None, max_loaded_loras=None, max_loras_per_batch=8, lora_eviction_policy='lru', lora_backend='csgmv', max_lora_chunk_size=16, attention_backend='fa3', decode_attention_backend=None, prefill_attention_backend=None, sampling_backend='flashinfer', grammar_backend='xgrammar', mm_attention_backend=None, fp8_gemm_runner_backend='auto', fp4_gemm_runner_backend='auto', nsa_prefill_backend=None, nsa_decode_backend=None, disable_flashinfer_autotune=False, mamba_backend='triton', speculative_algorithm=None, speculative_draft_model_path=None, speculative_draft_model_revision=None, speculative_draft_load_format=None, speculative_num_steps=None, speculative_eagle_topk=None, speculative_num_draft_tokens=None, speculative_accept_threshold_single=1.0, speculative_accept_threshold_acc=1.0, speculative_token_map=None, speculative_attention_mode='prefill', speculative_draft_attention_backend=None, speculative_moe_runner_backend='auto', speculative_moe_a2a_backend=None, speculative_draft_model_quantization=None, speculative_ngram_min_match_window_size=1, speculative_ngram_max_match_window_size=12, speculative_ngram_min_bfs_breadth=1, speculative_ngram_max_bfs_breadth=10, speculative_ngram_match_type='BFS', speculative_ngram_max_trie_depth=18, speculative_ngram_capacity=10000000, enable_multi_layer_eagle=False, ep_size=1, moe_a2a_backend='none', moe_runner_backend='auto', flashinfer_mxfp4_moe_precision='default', enable_flashinfer_allreduce_fusion=False, enable_aiter_allreduce_fusion=False, deepep_mode='auto', ep_num_redundant_experts=0, ep_dispatch_algorithm=None, init_expert_location='trivial', enable_eplb=False, eplb_algorithm='auto', eplb_rebalance_num_iterations=1000, eplb_rebalance_layers_per_chunk=None, eplb_min_rebalancing_utilization_threshold=1.0, expert_distribution_recorder_mode=None, expert_distribution_recorder_buffer_size=1000, enable_expert_distribution_metrics=False, deepep_config=None, moe_dense_tp_size=None, elastic_ep_backend=None, enable_elastic_expert_backup=False, mooncake_ib_device=None, max_mamba_cache_size=None, mamba_ssm_dtype=None, mamba_full_memory_ratio=0.9, mamba_scheduler_strategy='no_buffer', mamba_track_interval=256, linear_attn_backend='triton', linear_attn_decode_backend=None, linear_attn_prefill_backend=None, enable_hierarchical_cache=False, hicache_ratio=2.0, hicache_size=0, hicache_write_policy='write_through', hicache_io_backend='kernel', hicache_mem_layout='layer_first', disable_hicache_numa_detect=False, hicache_storage_backend=None, hicache_storage_prefetch_policy='best_effort', hicache_storage_backend_extra_config=None, enable_hisparse=False, hisparse_config=None, enable_lmcache=False, kt_weight_path=None, kt_method=None, kt_cpuinfer=None, kt_threadpool_count=None, kt_num_gpu_experts=None, kt_max_deferred_experts_per_token=None, dllm_algorithm=None, dllm_algorithm_config=None, enable_double_sparsity=False, ds_channel_config_path=None, ds_heavy_channel_num=32, ds_heavy_token_num=256, ds_heavy_channel_type='qk', ds_sparse_decode_threshold=4096, cpu_offload_gb=0, offload_group_size=-1, offload_num_in_group=1, offload_prefetch_step=1, offload_mode='cpu', multi_item_scoring_delimiter=None, disable_radix_cache=False, cuda_graph_max_bs=4, cuda_graph_bs=[1, 2, 4, 8, 12, 16, 24, 32, 40, 48, 56, 64, 72, 80, 88, 96, 104, 112, 120, 128, 136, 144, 152, 160, 168, 176, 184, 192, 200, 208, 216, 224, 232, 240, 248, 256], disable_cuda_graph=True, disable_cuda_graph_padding=False, enable_profile_cuda_graph=False, enable_cudagraph_gc=False, enable_layerwise_nvtx_marker=False, enable_nccl_nvls=False, enable_symm_mem=False, disable_flashinfer_cutlass_moe_fp4_allgather=False, enable_tokenizer_batch_encode=False, disable_tokenizer_batch_decode=False, disable_outlines_disk_cache=False, disable_custom_all_reduce=False, enable_mscclpp=False, enable_torch_symm_mem=False, pre_warm_nccl=False, disable_overlap_schedule=False, enable_mixed_chunk=False, enable_dp_attention=False, enable_dp_lm_head=False, enable_two_batch_overlap=False, enable_single_batch_overlap=False, tbo_token_distribution_threshold=0.48, enable_torch_compile=False, disable_piecewise_cuda_graph=False, enforce_piecewise_cuda_graph=False, enable_torch_compile_debug_mode=False, torch_compile_max_bs=32, piecewise_cuda_graph_max_tokens=8192, piecewise_cuda_graph_tokens=[4, 8, 12, 16, 20, 24, 28, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 352, 384, 416, 448, 480, 512, 576, 640, 704, 768, 832, 896, 960, 1024, 1280, 1536, 1792, 2048, 2304, 2560, 2816, 3072, 3328, 3584, 3840, 4096, 4608, 5120, 5632, 6144, 6656, 7168, 7680, 8192], piecewise_cuda_graph_compiler='eager', torchao_config='', enable_nan_detection=False, enable_p2p_check=False, triton_attention_reduce_in_fp32=False, triton_attention_num_kv_splits=8, triton_attention_split_tile_size=None, num_continuous_decode_steps=1, delete_ckpt_after_loading=False, enable_memory_saver=False, enable_weights_cpu_backup=False, enable_draft_weights_cpu_backup=False, allow_auto_truncate=False, enable_custom_logit_processor=False, flashinfer_mla_disable_ragged=False, disable_shared_experts_fusion=False, disable_chunked_prefix_cache=False, disable_fast_image_processor=False, keep_mm_feature_on_device=False, enable_return_hidden_states=False, enable_return_routed_experts=False, scheduler_recv_interval=1, numa_node=None, enable_deterministic_inference=False, rl_on_policy_target=None, enable_attn_tp_input_scattered=False, enable_nsa_prefill_context_parallel=False, nsa_prefill_cp_mode='round-robin-split', enable_fused_qk_norm_rope=False, enable_precise_embedding_interpolation=False, enable_fused_moe_sum_all_reduce=False, enable_prefill_context_parallel=False, prefill_cp_mode='in-seq-split', enable_dynamic_batch_tokenizer=False, dynamic_batch_tokenizer_batch_size=32, dynamic_batch_tokenizer_batch_timeout=0.002, debug_tensor_dump_output_folder=None, debug_tensor_dump_layers=None, debug_tensor_dump_input_file=None, debug_tensor_dump_inject=False, disaggregation_mode='null', disaggregation_transfer_backend='mooncake', disaggregation_bootstrap_port=8998, disaggregation_ib_device=None, disaggregation_decode_enable_offload_kvcache=False, num_reserved_decode_tokens=512, disaggregation_decode_polling_interval=1, encoder_only=False, language_only=False, encoder_transfer_backend='zmq_to_scheduler', encoder_urls=[], enable_adaptive_dispatch_to_encoder=False, custom_weight_loader=[], weight_loader_disable_mmap=False, remote_instance_weight_loader_seed_instance_ip=None, remote_instance_weight_loader_seed_instance_service_port=None, remote_instance_weight_loader_send_weights_group_ports=None, remote_instance_weight_loader_backend='nccl', remote_instance_weight_loader_start_seed_via_transfer_engine=False, modelexpress_config=None, enable_pdmux=False, pdmux_config_path=None, sm_group_num=8, mm_max_concurrent_calls=32, mm_per_request_timeout=10.0, enable_broadcast_mm_inputs_process=False, enable_prefix_mm_cache=False, mm_enable_dp_encoder=False, mm_process_config={}, limit_mm_data_per_request=None, enable_mm_global_cache=False, decrypted_config_file=None, decrypted_draft_config_file=None, forward_hooks=None)


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-25 12:50:50.668 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 12:50:50] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 12:50:50.668 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 12:50:50] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 12:50:50.668 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 12:50:50] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 12:50:50.668 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 12:50:50] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 12:50:50.668 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 12:50:50] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.79it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.78it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:14,  2.36s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:14,  2.36s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:14,  2.36s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:14,  2.36s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.11it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.11it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.11it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:25,  2.11it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:25,  2.11it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:25,  2.11it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:25,  2.11it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:03, 13.32it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:03, 13.32it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:03, 13.32it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:03, 13.32it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:03, 13.32it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:03, 13.32it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:03, 13.32it/s]

    Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:02<00:03, 13.32it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 19.73it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 19.73it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 19.73it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 19.73it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 19.73it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:02<00:01, 19.73it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:02<00:01, 19.73it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:02<00:01, 19.73it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:02<00:00, 26.59it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:02<00:00, 26.59it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:02<00:00, 26.59it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:02<00:00, 26.59it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:02<00:00, 26.59it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:02<00:00, 26.59it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:00, 26.59it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 31.58it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 31.58it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 31.58it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 31.58it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 31.58it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 31.58it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 31.58it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 36.70it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 36.70it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 36.70it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 36.70it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 36.70it/s]

    Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 36.70it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 36.70it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 39.45it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 39.45it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 39.45it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 39.45it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 39.45it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 39.45it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 39.45it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 39.45it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 39.45it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 48.21it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.27it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=136.07 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=136.04 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=136.04 GB):   3%|▎         | 2/58 [00:00<00:02, 18.72it/s]Capturing num tokens (num_tokens=7168 avail_mem=134.97 GB):   3%|▎         | 2/58 [00:00<00:02, 18.72it/s]Capturing num tokens (num_tokens=6656 avail_mem=133.16 GB):   3%|▎         | 2/58 [00:00<00:02, 18.72it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=121.70 GB):   3%|▎         | 2/58 [00:00<00:02, 18.72it/s]Capturing num tokens (num_tokens=6144 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 19.90it/s]Capturing num tokens (num_tokens=5632 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 19.90it/s]Capturing num tokens (num_tokens=5120 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 19.90it/s]Capturing num tokens (num_tokens=5120 avail_mem=121.70 GB):  12%|█▏        | 7/58 [00:00<00:02, 19.43it/s]Capturing num tokens (num_tokens=4608 avail_mem=121.70 GB):  12%|█▏        | 7/58 [00:00<00:02, 19.43it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=121.70 GB):  12%|█▏        | 7/58 [00:00<00:02, 19.43it/s]Capturing num tokens (num_tokens=4096 avail_mem=121.70 GB):  16%|█▌        | 9/58 [00:00<00:03, 12.38it/s]Capturing num tokens (num_tokens=3840 avail_mem=121.69 GB):  16%|█▌        | 9/58 [00:00<00:03, 12.38it/s]Capturing num tokens (num_tokens=3584 avail_mem=121.69 GB):  16%|█▌        | 9/58 [00:00<00:03, 12.38it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=121.69 GB):  19%|█▉        | 11/58 [00:00<00:03, 13.45it/s]Capturing num tokens (num_tokens=3328 avail_mem=121.69 GB):  19%|█▉        | 11/58 [00:00<00:03, 13.45it/s]Capturing num tokens (num_tokens=3072 avail_mem=121.68 GB):  19%|█▉        | 11/58 [00:00<00:03, 13.45it/s]Capturing num tokens (num_tokens=2816 avail_mem=121.68 GB):  19%|█▉        | 11/58 [00:00<00:03, 13.45it/s]Capturing num tokens (num_tokens=2560 avail_mem=121.68 GB):  19%|█▉        | 11/58 [00:00<00:03, 13.45it/s]Capturing num tokens (num_tokens=2304 avail_mem=121.67 GB):  19%|█▉        | 11/58 [00:00<00:03, 13.45it/s]Capturing num tokens (num_tokens=2304 avail_mem=121.67 GB):  28%|██▊       | 16/58 [00:00<00:01, 21.42it/s]Capturing num tokens (num_tokens=2048 avail_mem=121.67 GB):  28%|██▊       | 16/58 [00:00<00:01, 21.42it/s]Capturing num tokens (num_tokens=1792 avail_mem=121.67 GB):  28%|██▊       | 16/58 [00:00<00:01, 21.42it/s]Capturing num tokens (num_tokens=1536 avail_mem=121.66 GB):  28%|██▊       | 16/58 [00:00<00:01, 21.42it/s]Capturing num tokens (num_tokens=1280 avail_mem=121.66 GB):  28%|██▊       | 16/58 [00:00<00:01, 21.42it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=121.64 GB):  28%|██▊       | 16/58 [00:00<00:01, 21.42it/s]Capturing num tokens (num_tokens=1024 avail_mem=121.64 GB):  36%|███▌      | 21/58 [00:00<00:01, 27.44it/s]Capturing num tokens (num_tokens=960 avail_mem=121.65 GB):  36%|███▌      | 21/58 [00:00<00:01, 27.44it/s] Capturing num tokens (num_tokens=896 avail_mem=121.38 GB):  36%|███▌      | 21/58 [00:01<00:01, 27.44it/s]Capturing num tokens (num_tokens=832 avail_mem=121.37 GB):  36%|███▌      | 21/58 [00:01<00:01, 27.44it/s]Capturing num tokens (num_tokens=768 avail_mem=121.37 GB):  36%|███▌      | 21/58 [00:01<00:01, 27.44it/s]Capturing num tokens (num_tokens=704 avail_mem=120.90 GB):  36%|███▌      | 21/58 [00:01<00:01, 27.44it/s]Capturing num tokens (num_tokens=704 avail_mem=120.90 GB):  45%|████▍     | 26/58 [00:01<00:00, 32.09it/s]Capturing num tokens (num_tokens=640 avail_mem=120.66 GB):  45%|████▍     | 26/58 [00:01<00:00, 32.09it/s]Capturing num tokens (num_tokens=576 avail_mem=120.66 GB):  45%|████▍     | 26/58 [00:01<00:00, 32.09it/s]Capturing num tokens (num_tokens=512 avail_mem=120.65 GB):  45%|████▍     | 26/58 [00:01<00:00, 32.09it/s]

    Capturing num tokens (num_tokens=480 avail_mem=120.66 GB):  45%|████▍     | 26/58 [00:01<00:00, 32.09it/s]Capturing num tokens (num_tokens=448 avail_mem=120.66 GB):  45%|████▍     | 26/58 [00:01<00:00, 32.09it/s]Capturing num tokens (num_tokens=448 avail_mem=120.66 GB):  53%|█████▎    | 31/58 [00:01<00:00, 35.10it/s]Capturing num tokens (num_tokens=416 avail_mem=120.66 GB):  53%|█████▎    | 31/58 [00:01<00:00, 35.10it/s]Capturing num tokens (num_tokens=384 avail_mem=120.66 GB):  53%|█████▎    | 31/58 [00:01<00:00, 35.10it/s]Capturing num tokens (num_tokens=352 avail_mem=120.65 GB):  53%|█████▎    | 31/58 [00:01<00:00, 35.10it/s]Capturing num tokens (num_tokens=320 avail_mem=120.65 GB):  53%|█████▎    | 31/58 [00:01<00:00, 35.10it/s]Capturing num tokens (num_tokens=288 avail_mem=120.64 GB):  53%|█████▎    | 31/58 [00:01<00:00, 35.10it/s]Capturing num tokens (num_tokens=288 avail_mem=120.64 GB):  62%|██████▏   | 36/58 [00:01<00:00, 37.92it/s]Capturing num tokens (num_tokens=256 avail_mem=120.64 GB):  62%|██████▏   | 36/58 [00:01<00:00, 37.92it/s]Capturing num tokens (num_tokens=240 avail_mem=120.64 GB):  62%|██████▏   | 36/58 [00:01<00:00, 37.92it/s]Capturing num tokens (num_tokens=224 avail_mem=120.64 GB):  62%|██████▏   | 36/58 [00:01<00:00, 37.92it/s]

    Capturing num tokens (num_tokens=208 avail_mem=120.63 GB):  62%|██████▏   | 36/58 [00:01<00:00, 37.92it/s]Capturing num tokens (num_tokens=192 avail_mem=120.63 GB):  62%|██████▏   | 36/58 [00:01<00:00, 37.92it/s]Capturing num tokens (num_tokens=192 avail_mem=120.63 GB):  71%|███████   | 41/58 [00:01<00:00, 39.46it/s]Capturing num tokens (num_tokens=176 avail_mem=120.63 GB):  71%|███████   | 41/58 [00:01<00:00, 39.46it/s]Capturing num tokens (num_tokens=160 avail_mem=120.62 GB):  71%|███████   | 41/58 [00:01<00:00, 39.46it/s]Capturing num tokens (num_tokens=144 avail_mem=120.62 GB):  71%|███████   | 41/58 [00:01<00:00, 39.46it/s]Capturing num tokens (num_tokens=128 avail_mem=120.62 GB):  71%|███████   | 41/58 [00:01<00:00, 39.46it/s]Capturing num tokens (num_tokens=112 avail_mem=120.62 GB):  71%|███████   | 41/58 [00:01<00:00, 39.46it/s]Capturing num tokens (num_tokens=112 avail_mem=120.62 GB):  79%|███████▉  | 46/58 [00:01<00:00, 40.95it/s]Capturing num tokens (num_tokens=96 avail_mem=120.61 GB):  79%|███████▉  | 46/58 [00:01<00:00, 40.95it/s] Capturing num tokens (num_tokens=80 avail_mem=120.61 GB):  79%|███████▉  | 46/58 [00:01<00:00, 40.95it/s]

    Capturing num tokens (num_tokens=64 avail_mem=120.60 GB):  79%|███████▉  | 46/58 [00:01<00:00, 40.95it/s]Capturing num tokens (num_tokens=48 avail_mem=120.60 GB):  79%|███████▉  | 46/58 [00:01<00:00, 40.95it/s]Capturing num tokens (num_tokens=32 avail_mem=120.27 GB):  79%|███████▉  | 46/58 [00:01<00:00, 40.95it/s]Capturing num tokens (num_tokens=32 avail_mem=120.27 GB):  88%|████████▊ | 51/58 [00:01<00:00, 41.94it/s]Capturing num tokens (num_tokens=28 avail_mem=120.17 GB):  88%|████████▊ | 51/58 [00:01<00:00, 41.94it/s]Capturing num tokens (num_tokens=24 avail_mem=120.17 GB):  88%|████████▊ | 51/58 [00:01<00:00, 41.94it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  88%|████████▊ | 51/58 [00:01<00:00, 41.94it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  88%|████████▊ | 51/58 [00:01<00:00, 41.94it/s]Capturing num tokens (num_tokens=12 avail_mem=120.16 GB):  88%|████████▊ | 51/58 [00:01<00:00, 41.94it/s]Capturing num tokens (num_tokens=12 avail_mem=120.16 GB):  97%|█████████▋| 56/58 [00:01<00:00, 42.46it/s]Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  97%|█████████▋| 56/58 [00:01<00:00, 42.46it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  97%|█████████▋| 56/58 [00:01<00:00, 42.46it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 31.69it/s]


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
    Generated text:  Louie. I am 23 years old, and I currently live in South Los Angeles. I'm the founder and CEO of a startup called Metabase, which is an AI-powered platform that helps businesses quickly and easily manage all their data. I'm excited to share my journey with you and the challenges I've faced along the way.
    
    I first got introduced to the idea of using AI to help businesses in the early 2000s. I was fascinated by the idea of using machine learning to automate mundane tasks, like data entry and analysis. I started working with data analysts to learn about the technology and create a
    ===============================
    Prompt: The president of the United States is
    Generated text:  a person.
    A. 正确
    B. 错误
    答案:
    A
    
    (单选题) 依据《中国石化危险化学品重大危险源安全管理办法》规定，重大危险源单位应当按照相关技术标准制定本单位重大危险源的____。 (1.0分)
    A. 事故报告制度
    B. 事故预防制度
    C. 应急预案
    D. 事故处置预案
    答案:
    C
    
    在一定条件下，如果温度升高1℃，反应平衡常数K 增大。____
    A. 正确
    B. 错误
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris.
    A. 正确
    B. 错误
    Answer: A
    
    From the perspective of wind, it's always a fine day at the seaside. 
    A. 正确
    B. 错误
    Answer: A
    
    In fact, the opposite is true. 
    A. 正确
    B. 错误
    Answer: A
    
    In reality, the opposite is true. 
    A. 正确
    B. 错误
    Answer: A
    
    In fact, it is the opposite of true.
    A. 正确
    B. 错误
    Answer: A
    
    True
    ===============================
    Prompt: The future of AI is
    Generated text:  digitalizing. This means that every aspect of daily life will be influenced by AI technology. This will include our technology.
    This will also mean that the future of AI will have a direct and direct impact on our health. I can see this happening in a number of areas.
    One area that is already impacted by AI technology is the health care industry. AI has the ability to analyze medical records, medical images, and other data and use it to provide accurate diagnoses and treatments.
    This has already been implemented by companies such as IBM and Pfizer. These companies have used AI to help their healthcare systems make more informed decisions and to provide more personalized treatment


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm a [job title] at [company name], and I've been with the company for [number of years] years. I'm passionate about [job title] and I'm always looking for ways to [job title] and grow my skills. I'm a [job title] who is always looking for ways to [job title] and grow my skills. I'm a [job title] who is always looking for ways to [job title] and grow my skills. I'm a [job title] who is always looking for ways to
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. It is also famous for its rich history, including the French Revolution and the French Revolution Museum. Paris is a bustling city with a diverse population and is a major tourist destination. It is home to many famous French artists, writers, and musicians, and is known for its cuisine, including its famous croissants and its famous wine. The city is also home to many cultural institutions, including the Louvre and the Musée d'Orsay. Paris is a city of contrasts, with its modern architecture and historical
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn from and adapt to human behavior and decision-making processes. This could lead to more sophisticated and adaptive AI systems that can learn from feedback and improve their performance over time.
    
    2. Greater emphasis on ethical considerations: As AI becomes more integrated with human intelligence, there will be a greater emphasis on ethical considerations and responsible use of AI. This could lead to more stringent regulations and guidelines for AI development and deployment, as well as increased scrutiny of AI systems that are used in sensitive or ethical contexts
    


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
    Generated text:  [Your Name], and I'm a [insert your occupation or profession] who has been following the path of adventure and exploration for [insert number of years]. I'm excited to share my experiences and learn new things along the way. 
    
    How would you describe your style of writing, and what are some tips for becoming a better writer? As a language model, I can tell you that my style of writing is descriptive and imaginative, with a focus on creating vivid and detailed scenes. To become a better writer, I suggest practicing regularly by writing regularly, reading widely and critically, and seeking feedback from others. Additionally, taking breaks from writing
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, also known as the City of Light and the City of Light, the City of Museums. It is a sprawling city with a rich history and vibrant culture. The city is home to the iconic Eiffel Tower, the Louvre Museum, Notre Dame Cathedral, and numerous art galleries and theaters. Paris has been a major hub of French culture and politics since the 17th century, with its famous landmarks and monuments still attracting visitors from around the world. It is a cultural and economic center of the world, known for its sophisticated architecture, world-renowned cuisine, and its ever-evolving fashion scene. As of 
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  highly uncertain, but some possible trends that have already emerged include:
    
    1. Increased emphasis on ethical considerations: As AI systems become more integrated into our daily lives, there is a growing concern about the ethical implications of their deployment. This will likely lead to increased regulation and scrutiny of AI development, as well as new regulations and standards for responsible AI use.
    
    2. Integration of AI into various industries: AI is already being used in many industries, including healthcare, finance, transportation, and education. As more industries become aware of the potential benefits and challenges of AI, there may be an increased integration of AI into new industries, such as manufacturing and


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

    insert

     name

    ].

     I

    'm

     a

     [

    insert

     occupation

     or

     field

     of

     work

    ]

     with

     a

     passion

     for

     [

    insert

     something

     related

     to

     your

     field

     of

     work

    ,

     such

     as

     technology

    ,

     literature

    ,

     or

     art

    ].

     I

     believe

     in

     the

     power

     of

     [

    insert

     a

     trait

     or

     belief

     you

     hold

     dear

    ,

     such

     as

     hard

     work

    ,

     innovation

    ,

     or

     empathy

    ].

     I

    'm

     always

     looking

     for

     opportunities

     to

     [

    insert

     something

     specific

    ,

     such

     as

     a

     new

     challenge

    ,

     a

     different

     project

    ,

     or

     a

     unique

     experience

    ]

     that

     I

     can

     share

     with

     others

    .

     I

    'm

     here

     to

     learn

     and

     grow

    ,

     and

     I

    'm

     excited

     to

     make

     a

     difference

     in

     the

     world

    .

     I

     believe

     in

     [

    insert

     something

     about

     your

     personal

     values

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     It

     is

     the

     largest

     and

     most

     populous

     city

     in

     the

     country

    ,

     with

     a

     population

     of

     over

     

    2

    .

    5

     million

     people

    .

     Paris

     is

     known

     for

     its

     iconic

     landmarks

     such

     as

     the

     E

    iff

    el

     Tower

    ,

     the

     Lou

    vre

     Museum

    ,

     Notre

    -D

    ame

     Cathedral

    ,

     and

     the

     Arc

     de

     Tri

    omp

    he

    ,

     as

     well

     as

     its

     diverse

     food

     scene

    ,

     art

     scene

    ,

     and

     history

    .

     The

     city

     is

     a

     major

     center

     for

     French

     culture

    ,

     arts

    ,

     and

     commerce

    ,

     and

     is

     an

     important

     cultural

     hub

     in

     Europe

    .

     Paris

     is

     also

     one

     of

     the

     world

    's

     most

     visited

     cities

    ,

     with

     millions

     of

     tourists

     visiting

     each

     year

    .

     The

     city

     is

     a

     major

     transportation

     hub

    ,

     with

     the

     iconic

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     characterized

     by

     three

     main

     trends

    :
    


     

     

    1

    .

     Increased

     efficiency

    :

     As

     AI

     becomes

     more

     integrated

     into

     various

     industries

     and

     becomes

     more

     ubiquitous

    ,

     it

     is

     likely

     to

     become

     more

     efficient

     at

     completing

     tasks

     and

     delivering

     results

    .

     This

     could

     lead

     to

     a

     decrease

     in

     the

     need

     for

     human

     labor

     and

     increase

     productivity

    .


     

     

    2

    .

     More

     human

     involvement

    :

     While

     AI

     is

     likely

     to

     continue

     to

     play

     an

     increasingly

     important

     role

     in

     various

     industries

    ,

     it

     is

     also

     likely

     to

     become

     more

     integrated

     with

     human

     decision

    -making

     and

     decision

    -making

     processes

    .

     This

     could

     lead

     to

     a

     greater

     emphasis

     on

     human

     expertise

     and

     intuition

     in

     decision

    -making

    .


     

     

    3

    .

     Greater

     emphasis

     on

     privacy

     and

     security

    :

     As

    



```python
llm.shutdown()
```
