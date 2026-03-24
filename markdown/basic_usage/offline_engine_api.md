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

    [2026-03-24 23:44:13] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.


    [2026-03-24 23:44:13] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.


    [2026-03-24 23:44:13] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-24 23:44:16] INFO server_args.py:2240: Attention backend not specified. Use fa3 backend by default.


    [2026-03-24 23:44:16] INFO server_args.py:3523: Set soft_watchdog_timeout since in CI


    [2026-03-24 23:44:16] INFO engine.py:177: server_args=ServerArgs(model_path='qwen/qwen2.5-0.5b-instruct', tokenizer_path='qwen/qwen2.5-0.5b-instruct', tokenizer_mode='auto', tokenizer_worker_num=1, skip_tokenizer_init=False, load_format='auto', model_loader_extra_config='{}', trust_remote_code=False, context_length=None, is_embedding=False, enable_multimodal=None, revision=None, model_impl='auto', host='127.0.0.1', port=30000, fastapi_root_path='', grpc_mode=False, skip_server_warmup=False, warmups=None, nccl_port=None, checkpoint_engine_wait_weights_before_ready=False, ssl_keyfile=None, ssl_certfile=None, ssl_ca_certs=None, ssl_keyfile_password=None, enable_ssl_refresh=False, dtype='auto', quantization=None, quantization_param_path=None, kv_cache_dtype='auto', enable_fp32_lm_head=False, modelopt_quant=None, modelopt_checkpoint_restore_path=None, modelopt_checkpoint_save_path=None, modelopt_export_path=None, quantize_and_serve=False, rl_quant_profile=None, mem_fraction_static=0.903, max_running_requests=128, max_queued_requests=None, max_total_tokens=20480, chunked_prefill_size=8192, enable_dynamic_chunking=False, max_prefill_tokens=16384, prefill_max_requests=None, schedule_policy='fcfs', enable_priority_scheduling=False, disable_priority_preemption=False, default_priority_value=None, abort_on_priority_when_disabled=False, schedule_low_priority_values_first=False, priority_scheduling_preemption_threshold=10, schedule_conservativeness=1.0, page_size=1, swa_full_tokens_ratio=0.8, disable_hybrid_swa_memory=False, radix_eviction_policy='lru', enable_prefill_delayer=False, prefill_delayer_max_delay_passes=30, prefill_delayer_token_usage_low_watermark=None, prefill_delayer_forward_passes_buckets=None, prefill_delayer_wait_seconds_buckets=None, device='cuda', tp_size=1, pp_size=1, pp_max_micro_batch_size=None, pp_async_batch_depth=0, stream_interval=1, incremental_streaming_output=False, enable_streaming_session=False, random_seed=826717700, constrained_json_whitespace_pattern=None, constrained_json_disable_any_whitespace=False, watchdog_timeout=300, soft_watchdog_timeout=300, dist_timeout=None, download_dir=None, model_checksum=None, base_gpu_id=0, gpu_id_step=1, sleep_on_idle=False, use_ray=False, custom_sigquit_handler=None, log_level='error', log_level_http=None, log_requests=False, log_requests_level=2, log_requests_format='text', log_requests_target=None, uvicorn_access_log_exclude_prefixes=[], crash_dump_folder=None, show_time_cost=False, enable_metrics=False, enable_metrics_for_all_schedulers=False, tokenizer_metrics_custom_labels_header='x-custom-labels', tokenizer_metrics_allowed_custom_labels=None, extra_metric_labels=None, bucket_time_to_first_token=None, bucket_inter_token_latency=None, bucket_e2e_request_latency=None, collect_tokens_histogram=False, prompt_tokens_buckets=None, generation_tokens_buckets=None, gc_warning_threshold_secs=0.0, decode_log_interval=40, enable_request_time_stats_logging=False, kv_events_config=None, enable_trace=False, otlp_traces_endpoint='localhost:4317', export_metrics_to_file=False, export_metrics_to_file_dir=None, api_key=None, admin_api_key=None, served_model_name='qwen/qwen2.5-0.5b-instruct', weight_version='default', chat_template=None, hf_chat_template_name=None, completion_template=None, file_storage_path='sglang_storage', enable_cache_report=False, reasoning_parser=None, tool_call_parser=None, tool_server=None, sampling_defaults='model', dp_size=1, load_balance_method='round_robin', attn_cp_size=1, moe_dp_size=1, dist_init_addr=None, nnodes=1, node_rank=0, json_model_override_args='{}', preferred_sampling_params=None, enable_lora=None, enable_lora_overlap_loading=None, max_lora_rank=None, lora_target_modules=None, lora_paths=None, max_loaded_loras=None, max_loras_per_batch=8, lora_eviction_policy='lru', lora_backend='csgmv', max_lora_chunk_size=16, attention_backend='fa3', decode_attention_backend=None, prefill_attention_backend=None, sampling_backend='flashinfer', grammar_backend='xgrammar', mm_attention_backend=None, fp8_gemm_runner_backend='auto', fp4_gemm_runner_backend='auto', nsa_prefill_backend=None, nsa_decode_backend=None, disable_flashinfer_autotune=False, mamba_backend='triton', speculative_algorithm=None, speculative_draft_model_path=None, speculative_draft_model_revision=None, speculative_draft_load_format=None, speculative_num_steps=None, speculative_eagle_topk=None, speculative_num_draft_tokens=None, speculative_accept_threshold_single=1.0, speculative_accept_threshold_acc=1.0, speculative_token_map=None, speculative_attention_mode='prefill', speculative_draft_attention_backend=None, speculative_moe_runner_backend='auto', speculative_moe_a2a_backend=None, speculative_draft_model_quantization=None, speculative_ngram_min_match_window_size=1, speculative_ngram_max_match_window_size=12, speculative_ngram_min_bfs_breadth=1, speculative_ngram_max_bfs_breadth=10, speculative_ngram_match_type='BFS', speculative_ngram_max_trie_depth=18, speculative_ngram_capacity=10000000, enable_multi_layer_eagle=False, ep_size=1, moe_a2a_backend='none', moe_runner_backend='auto', flashinfer_mxfp4_moe_precision='default', enable_flashinfer_allreduce_fusion=False, enable_aiter_allreduce_fusion=False, deepep_mode='auto', ep_num_redundant_experts=0, ep_dispatch_algorithm=None, init_expert_location='trivial', enable_eplb=False, eplb_algorithm='auto', eplb_rebalance_num_iterations=1000, eplb_rebalance_layers_per_chunk=None, eplb_min_rebalancing_utilization_threshold=1.0, expert_distribution_recorder_mode=None, expert_distribution_recorder_buffer_size=1000, enable_expert_distribution_metrics=False, deepep_config=None, moe_dense_tp_size=None, elastic_ep_backend=None, enable_elastic_expert_backup=False, mooncake_ib_device=None, max_mamba_cache_size=None, mamba_ssm_dtype=None, mamba_full_memory_ratio=0.9, mamba_scheduler_strategy='no_buffer', mamba_track_interval=256, linear_attn_backend='triton', linear_attn_decode_backend=None, linear_attn_prefill_backend=None, enable_hierarchical_cache=False, hicache_ratio=2.0, hicache_size=0, hicache_write_policy='write_through', hicache_io_backend='kernel', hicache_mem_layout='layer_first', disable_hicache_numa_detect=False, hicache_storage_backend=None, hicache_storage_prefetch_policy='best_effort', hicache_storage_backend_extra_config=None, enable_hisparse=False, hisparse_config=None, enable_lmcache=False, kt_weight_path=None, kt_method=None, kt_cpuinfer=None, kt_threadpool_count=None, kt_num_gpu_experts=None, kt_max_deferred_experts_per_token=None, dllm_algorithm=None, dllm_algorithm_config=None, enable_double_sparsity=False, ds_channel_config_path=None, ds_heavy_channel_num=32, ds_heavy_token_num=256, ds_heavy_channel_type='qk', ds_sparse_decode_threshold=4096, cpu_offload_gb=0, offload_group_size=-1, offload_num_in_group=1, offload_prefetch_step=1, offload_mode='cpu', multi_item_scoring_delimiter=None, disable_radix_cache=False, cuda_graph_max_bs=4, cuda_graph_bs=[1, 2, 4, 8, 12, 16, 24, 32, 40, 48, 56, 64, 72, 80, 88, 96, 104, 112, 120, 128, 136, 144, 152, 160, 168, 176, 184, 192, 200, 208, 216, 224, 232, 240, 248, 256], disable_cuda_graph=True, disable_cuda_graph_padding=False, enable_profile_cuda_graph=False, enable_cudagraph_gc=False, enable_layerwise_nvtx_marker=False, enable_nccl_nvls=False, enable_symm_mem=False, disable_flashinfer_cutlass_moe_fp4_allgather=False, enable_tokenizer_batch_encode=False, disable_tokenizer_batch_decode=False, disable_outlines_disk_cache=False, disable_custom_all_reduce=False, enable_mscclpp=False, enable_torch_symm_mem=False, pre_warm_nccl=False, disable_overlap_schedule=False, enable_mixed_chunk=False, enable_dp_attention=False, enable_dp_lm_head=False, enable_two_batch_overlap=False, enable_single_batch_overlap=False, tbo_token_distribution_threshold=0.48, enable_torch_compile=False, disable_piecewise_cuda_graph=False, enforce_piecewise_cuda_graph=False, enable_torch_compile_debug_mode=False, torch_compile_max_bs=32, piecewise_cuda_graph_max_tokens=8192, piecewise_cuda_graph_tokens=[4, 8, 12, 16, 20, 24, 28, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 352, 384, 416, 448, 480, 512, 576, 640, 704, 768, 832, 896, 960, 1024, 1280, 1536, 1792, 2048, 2304, 2560, 2816, 3072, 3328, 3584, 3840, 4096, 4608, 5120, 5632, 6144, 6656, 7168, 7680, 8192], piecewise_cuda_graph_compiler='eager', torchao_config='', enable_nan_detection=False, enable_p2p_check=False, triton_attention_reduce_in_fp32=False, triton_attention_num_kv_splits=8, triton_attention_split_tile_size=None, num_continuous_decode_steps=1, delete_ckpt_after_loading=False, enable_memory_saver=False, enable_weights_cpu_backup=False, enable_draft_weights_cpu_backup=False, allow_auto_truncate=False, enable_custom_logit_processor=False, flashinfer_mla_disable_ragged=False, disable_shared_experts_fusion=False, disable_chunked_prefix_cache=False, disable_fast_image_processor=False, keep_mm_feature_on_device=False, enable_return_hidden_states=False, enable_return_routed_experts=False, scheduler_recv_interval=1, numa_node=None, enable_deterministic_inference=False, rl_on_policy_target=None, enable_attn_tp_input_scattered=False, enable_nsa_prefill_context_parallel=False, nsa_prefill_cp_mode='round-robin-split', enable_fused_qk_norm_rope=False, enable_precise_embedding_interpolation=False, enable_fused_moe_sum_all_reduce=False, enable_prefill_context_parallel=False, prefill_cp_mode='in-seq-split', enable_dynamic_batch_tokenizer=False, dynamic_batch_tokenizer_batch_size=32, dynamic_batch_tokenizer_batch_timeout=0.002, debug_tensor_dump_output_folder=None, debug_tensor_dump_layers=None, debug_tensor_dump_input_file=None, debug_tensor_dump_inject=False, disaggregation_mode='null', disaggregation_transfer_backend='mooncake', disaggregation_bootstrap_port=8998, disaggregation_ib_device=None, disaggregation_decode_enable_offload_kvcache=False, num_reserved_decode_tokens=512, disaggregation_decode_polling_interval=1, encoder_only=False, language_only=False, encoder_transfer_backend='zmq_to_scheduler', encoder_urls=[], enable_adaptive_dispatch_to_encoder=False, custom_weight_loader=[], weight_loader_disable_mmap=False, remote_instance_weight_loader_seed_instance_ip=None, remote_instance_weight_loader_seed_instance_service_port=None, remote_instance_weight_loader_send_weights_group_ports=None, remote_instance_weight_loader_backend='nccl', remote_instance_weight_loader_start_seed_via_transfer_engine=False, modelexpress_config=None, enable_pdmux=False, pdmux_config_path=None, sm_group_num=8, mm_max_concurrent_calls=32, mm_per_request_timeout=10.0, enable_broadcast_mm_inputs_process=False, enable_prefix_mm_cache=False, mm_enable_dp_encoder=False, mm_process_config={}, limit_mm_data_per_request=None, enable_mm_global_cache=False, decrypted_config_file=None, decrypted_draft_config_file=None, forward_hooks=None)


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-24 23:44:25.997 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 23:44:25] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 23:44:25.997 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 23:44:25] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 23:44:25.997 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 23:44:25] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 23:44:25.997 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 23:44:25] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 23:44:25.997 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 23:44:25] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.56it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.54it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:56,  3.09s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:56,  3.09s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:03<02:56,  3.09s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:03<02:56,  3.09s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:03<02:56,  3.09s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:25,  2.06it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:25,  2.06it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:03<00:25,  2.06it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:03<00:25,  2.06it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:03<00:25,  2.06it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:03<00:25,  2.06it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:03<00:25,  2.06it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:03<00:08,  5.45it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:03<00:08,  5.45it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:03<00:08,  5.45it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:03<00:08,  5.45it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:03<00:08,  5.45it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:03<00:08,  5.45it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:03<00:08,  5.45it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:03<00:08,  5.45it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:03, 10.35it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:03, 10.35it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 10.35it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 10.35it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 10.35it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 10.35it/s]

    Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 10.35it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 10.35it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:02, 15.99it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:02, 15.99it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:02, 15.99it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:02, 15.99it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:02, 15.99it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:02, 15.99it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:02, 15.99it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:03<00:01, 21.31it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:03<00:01, 21.31it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:03<00:01, 21.31it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:03<00:01, 21.31it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:03<00:01, 21.31it/s]Compiling num tokens (num_tokens=288):  53%|█████▎    | 31/58 [00:03<00:01, 21.31it/s]

    Compiling num tokens (num_tokens=256):  53%|█████▎    | 31/58 [00:03<00:01, 21.31it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:03<00:00, 26.77it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:03<00:00, 26.77it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:03<00:00, 26.77it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:03<00:00, 26.77it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:03<00:00, 26.77it/s]Compiling num tokens (num_tokens=176):  64%|██████▍   | 37/58 [00:03<00:00, 26.77it/s]Compiling num tokens (num_tokens=160):  64%|██████▍   | 37/58 [00:03<00:00, 26.77it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:03<00:00, 31.95it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:03<00:00, 31.95it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:03<00:00, 31.95it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:03<00:00, 31.95it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:03<00:00, 31.95it/s] 

    Compiling num tokens (num_tokens=80):  74%|███████▍  | 43/58 [00:03<00:00, 31.95it/s]Compiling num tokens (num_tokens=64):  74%|███████▍  | 43/58 [00:03<00:00, 31.95it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:03<00:00, 35.55it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:03<00:00, 35.55it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:04<00:00, 35.55it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:04<00:00, 35.55it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:04<00:00, 35.55it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:04<00:00, 35.55it/s]Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:04<00:00, 35.55it/s]Compiling num tokens (num_tokens=12):  84%|████████▍ | 49/58 [00:04<00:00, 35.55it/s]Compiling num tokens (num_tokens=8):  84%|████████▍ | 49/58 [00:04<00:00, 35.55it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:04<00:00, 43.96it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:04<00:00, 43.96it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 14.12it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.33 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.30 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 17.48it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 17.48it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 17.48it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 17.48it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 20.72it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 20.72it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 20.72it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 20.72it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.13it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.13it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.13it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.28 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.13it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.13it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  21%|██        | 12/58 [00:00<00:01, 29.04it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:01, 29.04it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:01, 29.04it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:01, 29.04it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:01, 29.04it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.26 GB):  21%|██        | 12/58 [00:00<00:01, 29.04it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.26 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.94it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.94it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.94it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.25 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.94it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=120.23 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.94it/s]Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.94it/s] Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  38%|███▊      | 22/58 [00:00<00:00, 36.74it/s]Capturing num tokens (num_tokens=896 avail_mem=120.24 GB):  38%|███▊      | 22/58 [00:00<00:00, 36.74it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  38%|███▊      | 22/58 [00:00<00:00, 36.74it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  38%|███▊      | 22/58 [00:00<00:00, 36.74it/s]Capturing num tokens (num_tokens=704 avail_mem=120.23 GB):  38%|███▊      | 22/58 [00:00<00:00, 36.74it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  38%|███▊      | 22/58 [00:00<00:00, 36.74it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.97it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.97it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.97it/s]

    Capturing num tokens (num_tokens=480 avail_mem=120.23 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.97it/s]

    Capturing num tokens (num_tokens=448 avail_mem=120.23 GB):  47%|████▋     | 27/58 [00:01<00:00, 38.97it/s]Capturing num tokens (num_tokens=448 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:01<00:01, 18.83it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:01<00:01, 18.83it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:01<00:01, 18.83it/s]Capturing num tokens (num_tokens=352 avail_mem=120.22 GB):  53%|█████▎    | 31/58 [00:01<00:01, 18.83it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  53%|█████▎    | 31/58 [00:01<00:01, 18.83it/s]Capturing num tokens (num_tokens=288 avail_mem=120.25 GB):  53%|█████▎    | 31/58 [00:01<00:01, 18.83it/s]Capturing num tokens (num_tokens=288 avail_mem=120.25 GB):  62%|██████▏   | 36/58 [00:01<00:00, 23.34it/s]Capturing num tokens (num_tokens=256 avail_mem=120.25 GB):  62%|██████▏   | 36/58 [00:01<00:00, 23.34it/s]Capturing num tokens (num_tokens=240 avail_mem=119.82 GB):  62%|██████▏   | 36/58 [00:01<00:00, 23.34it/s]

    Capturing num tokens (num_tokens=224 avail_mem=118.93 GB):  62%|██████▏   | 36/58 [00:01<00:00, 23.34it/s]Capturing num tokens (num_tokens=208 avail_mem=118.92 GB):  62%|██████▏   | 36/58 [00:01<00:00, 23.34it/s]Capturing num tokens (num_tokens=192 avail_mem=118.92 GB):  62%|██████▏   | 36/58 [00:01<00:00, 23.34it/s]Capturing num tokens (num_tokens=192 avail_mem=118.92 GB):  71%|███████   | 41/58 [00:01<00:00, 27.44it/s]Capturing num tokens (num_tokens=176 avail_mem=118.92 GB):  71%|███████   | 41/58 [00:01<00:00, 27.44it/s]Capturing num tokens (num_tokens=160 avail_mem=118.92 GB):  71%|███████   | 41/58 [00:01<00:00, 27.44it/s]Capturing num tokens (num_tokens=144 avail_mem=118.91 GB):  71%|███████   | 41/58 [00:01<00:00, 27.44it/s]Capturing num tokens (num_tokens=128 avail_mem=118.91 GB):  71%|███████   | 41/58 [00:01<00:00, 27.44it/s]Capturing num tokens (num_tokens=112 avail_mem=118.91 GB):  71%|███████   | 41/58 [00:01<00:00, 27.44it/s]Capturing num tokens (num_tokens=112 avail_mem=118.91 GB):  79%|███████▉  | 46/58 [00:01<00:00, 30.61it/s]Capturing num tokens (num_tokens=96 avail_mem=118.90 GB):  79%|███████▉  | 46/58 [00:01<00:00, 30.61it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=118.90 GB):  79%|███████▉  | 46/58 [00:01<00:00, 30.61it/s]Capturing num tokens (num_tokens=64 avail_mem=118.89 GB):  79%|███████▉  | 46/58 [00:01<00:00, 30.61it/s]Capturing num tokens (num_tokens=48 avail_mem=118.89 GB):  79%|███████▉  | 46/58 [00:01<00:00, 30.61it/s]Capturing num tokens (num_tokens=48 avail_mem=118.89 GB):  86%|████████▌ | 50/58 [00:01<00:00, 29.59it/s]Capturing num tokens (num_tokens=32 avail_mem=118.88 GB):  86%|████████▌ | 50/58 [00:01<00:00, 29.59it/s]Capturing num tokens (num_tokens=28 avail_mem=118.88 GB):  86%|████████▌ | 50/58 [00:01<00:00, 29.59it/s]Capturing num tokens (num_tokens=24 avail_mem=118.88 GB):  86%|████████▌ | 50/58 [00:01<00:00, 29.59it/s]

    Capturing num tokens (num_tokens=20 avail_mem=118.87 GB):  86%|████████▌ | 50/58 [00:01<00:00, 29.59it/s]Capturing num tokens (num_tokens=20 avail_mem=118.87 GB):  93%|█████████▎| 54/58 [00:01<00:00, 31.64it/s]Capturing num tokens (num_tokens=16 avail_mem=118.87 GB):  93%|█████████▎| 54/58 [00:01<00:00, 31.64it/s]Capturing num tokens (num_tokens=12 avail_mem=118.87 GB):  93%|█████████▎| 54/58 [00:01<00:00, 31.64it/s]Capturing num tokens (num_tokens=8 avail_mem=118.87 GB):  93%|█████████▎| 54/58 [00:01<00:00, 31.64it/s] Capturing num tokens (num_tokens=4 avail_mem=118.86 GB):  93%|█████████▎| 54/58 [00:01<00:00, 31.64it/s]Capturing num tokens (num_tokens=4 avail_mem=118.86 GB): 100%|██████████| 58/58 [00:01<00:00, 29.22it/s]


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
    Generated text:  Mira Patel and I'm a Data Scientist and an Associate Professor at the University of Michigan. I work with a team of data scientists, researchers, and clinicians to help improve health, safety, and wellness in the real world. I specialize in the use of big data and AI to improve outcomes in underserved populations.
    My research interests include the use of machine learning and AI to improve public health. In particular, I am interested in the use of AI to enhance the quality of care in underserved communities.
    I was an undergraduate student in the Master of Science in Data Science program at the University of Michigan. I studied machine learning, natural
    ===============================
    Prompt: The president of the United States is
    Generated text:  visiting some countries. The cost of staying in each country is the same. He spends $1000 on flights and $3000 on accommodation. The cost of his trip is $5000. How many countries is he visiting? Let's denote the number of countries as \( n \). The total cost of the trip consists of the cost of the flights and the accommodation, which is given as $5000. The total cost of the stay in each country is the same, so if the cost per country is \( c \), then the total cost for \( n \) countries would be \(
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. Paris is a very large city, so it has 3, 200, 000 residents. If you have a group of people going to a big concert, you know it's going to be a big event, so you calculate how many people would be in the concert. You know that the group will have 1, 000, 000 people, and you calculate that there will be 20% more people than the group size attending. But instead of counting the people who attended, you have to figure out how many people would be left behind. If the group of
    ===============================
    Prompt: The future of AI is
    Generated text:  going to be different from the past. In the past, the AI was used for machine learning to process data and provide better results. But the world now uses AI to make big decisions such as healthcare, security, and transportation. AI is now used to create a positive impact in the world, but it also has some negative effects on people. AI has been used to create a lot of jobs, but it has also created new jobs. Some people find that AI has increased the need for a skilled workforce. But others find that the jobs are not as well-paid or as secure as they used to be. AI has also led to discrimination


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a short, positive description of your personality or skills]. I enjoy [insert a short, positive description of your hobbies or interests]. I'm always looking for ways to [insert a short, positive description of your goal or purpose]. I'm always eager to learn and grow, and I'm always looking for new opportunities to contribute to [insert a short, positive description of your company or industry]. I'm a [insert a short,
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city that is home to the iconic Eiffel Tower and is known for its rich history, art, and culture. It is also the largest city in France and has a population of over 2.5 million people. Paris is a popular tourist destination and is home to many famous landmarks such as the Louvre Museum, Notre-Dame Cathedral, and the Arc de Triomphe. It is also a major center for business and finance, with many of the world’s largest companies headquartered there. The city is known for its fashion industry, with many famous fashion designers and boutiques located there. Overall, Paris is a
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that could be expected in the future:
    
    1. Increased automation: One of the most significant trends in AI is the increasing automation of tasks that were previously done by humans. This could lead to the development of more efficient and cost-effective AI systems that can perform a wide range of tasks with greater accuracy and speed.
    
    2. Improved privacy and security: As AI systems become more sophisticated, there will be an increased risk of privacy and security breaches. This could lead to the development of new privacy
    


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
    Generated text:  [insert character's name], and I'm a [insert character's profession or background] who has [insert short personal summary or life story]. In my spare time, I enjoy [insert hobbies or interests]. I'm always looking for opportunities to learn and grow, so I'm always eager to learn new things and improve myself. I'm always looking for new challenges and opportunities to make a difference, so I'm always willing to step up and take on new tasks and responsibilities. I'm always striving to be the best version of myself and to do what I'm passionate about. I'm confident in myself and I believe that I can accomplish
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. 
    
    Paris is the largest city in France, located in the center of the country, near the Mediterranean Sea, and is the largest and most populous city in Europe. It is the cultural, political, economic, and financial center of France. Paris is a major international city and tourist destination. It is also a major cultural and educational center for France. The city is known for its rich history, beautiful architecture, and diverse cultural and artistic activities. Paris is known for its cafes, theaters, museums, and nightclubs, making it one of the world’s most dynamic and diverse cities. Paris is a city that combines modernity,
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  incredibly exciting, as it continues to evolve and make significant advancements. Here are some potential future trends that are likely to shape the AI landscape:
    
    1. Increased sophistication: As AI continues to advance, we may see more sophisticated AI models that are able to understand and interpret complex human languages and emotions on a par with human intelligence. This could lead to more intelligent and nuanced AI that can make informed decisions based on context and knowledge.
    
    2. AI-powered healthcare: AI is already transforming the healthcare industry, with predictive analytics and personalized treatment options becoming more common. In the future, we may see even more AI-powered healthcare solutions that can help diagnose diseases


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

    Your

     Name

    ],

     and

     I

    'm

     here

     to

     share

     my

     experiences

     and

     insights

     with

     you

    .

     I

    'm

     a

     [

    age

    ],

     [

    gender

    ],

     [

    occupation

    ],

     with

     [

    education

     level

    ],

     [

    employment

     status

    ]

     and

     [

    any

     other

     relevant

     attributes

    ].

     I

    've

     always

     been

     passionate

     about

     [

    specific

     field

     or

     area

     of

     interest

    ],

     and

     I

    'm

     excited

     to

     share

     my

     knowledge

     and

     experiences

     with

     anyone

     who

     is

     interested

     in

     learning

     and

     growing

    .
    


    Thank

     you

     for

     taking

     the

     time

     to

     meet

     me

    .

     I

    'm

     looking

     forward

     to

     talking

     to

     you

    .

     Let

    's

     connect

    .

     [

    Your

     Contact

     Information

    ]

     [

    Your

     Email

     Address

    ]

     [

    Your

     Phone

     Number

    ]

     [

    Your

     Website

    /

    LinkedIn

     Profile

    ]

     [

    Your

     Other

     Relevant

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     
    


    A

    .

     True

    


    B

    .

     False

    
    


    B

    .

     False

    
    


    The

     capital

     city

     of

     France

     is

     not

     Paris

    .

     The

     capital

     is

     not

     Paris

    .

     The

     correct

     answer

     is

     false

    .

     The

     capital

     city

     of

     France

     is

     Paris

    ,

     which

     is

     the

     largest

     city

     in

     France

     and

     the

     capital

     of

     France

    .

     
    


    The

     other

     options

     are

     not

     correct

     because

     they

     have

     changed

     the

     capital

     city

    .

     The

     correct

     answer

     is

     false

    .

     The

     correct

     answer

     is

     Paris

    .

     
    


    Given

     the

     context

    ,

     it

    's

     important

     to

     note

     that

     "

    Paris

    "

     can

     refer

     to

     different

     things

     depending

     on

     the

     context

    ,

     but

     in

     this

     sentence

    ,

     it

     specifically

     refers

     to

     the

     capital

     city

     of

     France

    .

     The

     correct

     answer

     is

     B

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     diverse

     and

     rapidly

     evolving

    ,

     with

     numerous

     potential

     directions

     and

     directions

     of

     exploration

    .

     Here

     are

     some

     possible

     future

     trends

     in

     artificial

     intelligence

    :
    


    1

    .

     Increased

     personal

    ization

    :

     As

     AI

     becomes

     more

     capable

    ,

     it

     will

     be

     able

     to

     provide

     more

     tailored

     and

     personalized

     experiences

     to

     users

    .

     This

     includes

     things

     like

     recommending

     products

    ,

     suggesting

     content

    ,

     and

     even

     autom

    ating

     certain

     tasks

    .
    


    2

    .

     Autonomous

     vehicles

    :

     AI

     will

     continue

     to

     improve

    ,

     making

     autonomous

     vehicles

     increasingly

     feasible

    .

     These

     vehicles

     could

     be

     able

     to

     navigate

     traffic

    ,

     interact

     with

     pedestrians

    ,

     and

     even

     perform

     tasks

     like

     picking

     up

     garbage

    .
    


    3

    .

     Smart

     cities

    :

     AI

     will

     be

     used

     to

     create

     smart

     cities

     that

     can

     be

     designed

     to

     optimize

     energy

     use

    ,

    



```python
llm.shutdown()
```
