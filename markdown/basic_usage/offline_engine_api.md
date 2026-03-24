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

    [2026-03-24 05:16:17] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.


    [2026-03-24 05:16:17] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.


    [2026-03-24 05:16:17] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-24 05:16:20] INFO server_args.py:2240: Attention backend not specified. Use fa3 backend by default.


    [2026-03-24 05:16:21] INFO server_args.py:3523: Set soft_watchdog_timeout since in CI


    [2026-03-24 05:16:21] INFO engine.py:177: server_args=ServerArgs(model_path='qwen/qwen2.5-0.5b-instruct', tokenizer_path='qwen/qwen2.5-0.5b-instruct', tokenizer_mode='auto', tokenizer_worker_num=1, skip_tokenizer_init=False, load_format='auto', model_loader_extra_config='{}', trust_remote_code=False, context_length=None, is_embedding=False, enable_multimodal=None, revision=None, model_impl='auto', host='127.0.0.1', port=30000, fastapi_root_path='', grpc_mode=False, skip_server_warmup=False, warmups=None, nccl_port=None, checkpoint_engine_wait_weights_before_ready=False, ssl_keyfile=None, ssl_certfile=None, ssl_ca_certs=None, ssl_keyfile_password=None, enable_ssl_refresh=False, dtype='auto', quantization=None, quantization_param_path=None, kv_cache_dtype='auto', enable_fp32_lm_head=False, modelopt_quant=None, modelopt_checkpoint_restore_path=None, modelopt_checkpoint_save_path=None, modelopt_export_path=None, quantize_and_serve=False, rl_quant_profile=None, mem_fraction_static=0.903, max_running_requests=128, max_queued_requests=None, max_total_tokens=20480, chunked_prefill_size=8192, enable_dynamic_chunking=False, max_prefill_tokens=16384, prefill_max_requests=None, schedule_policy='fcfs', enable_priority_scheduling=False, disable_priority_preemption=False, default_priority_value=None, abort_on_priority_when_disabled=False, schedule_low_priority_values_first=False, priority_scheduling_preemption_threshold=10, schedule_conservativeness=1.0, page_size=1, swa_full_tokens_ratio=0.8, disable_hybrid_swa_memory=False, radix_eviction_policy='lru', enable_prefill_delayer=False, prefill_delayer_max_delay_passes=30, prefill_delayer_token_usage_low_watermark=None, prefill_delayer_forward_passes_buckets=None, prefill_delayer_wait_seconds_buckets=None, device='cuda', tp_size=1, pp_size=1, pp_max_micro_batch_size=None, pp_async_batch_depth=0, stream_interval=1, incremental_streaming_output=False, enable_streaming_session=False, random_seed=589558584, constrained_json_whitespace_pattern=None, constrained_json_disable_any_whitespace=False, watchdog_timeout=300, soft_watchdog_timeout=300, dist_timeout=None, download_dir=None, model_checksum=None, base_gpu_id=0, gpu_id_step=1, sleep_on_idle=False, use_ray=False, custom_sigquit_handler=None, log_level='error', log_level_http=None, log_requests=False, log_requests_level=2, log_requests_format='text', log_requests_target=None, uvicorn_access_log_exclude_prefixes=[], crash_dump_folder=None, show_time_cost=False, enable_metrics=False, enable_metrics_for_all_schedulers=False, tokenizer_metrics_custom_labels_header='x-custom-labels', tokenizer_metrics_allowed_custom_labels=None, extra_metric_labels=None, bucket_time_to_first_token=None, bucket_inter_token_latency=None, bucket_e2e_request_latency=None, collect_tokens_histogram=False, prompt_tokens_buckets=None, generation_tokens_buckets=None, gc_warning_threshold_secs=0.0, decode_log_interval=40, enable_request_time_stats_logging=False, kv_events_config=None, enable_trace=False, otlp_traces_endpoint='localhost:4317', export_metrics_to_file=False, export_metrics_to_file_dir=None, api_key=None, admin_api_key=None, served_model_name='qwen/qwen2.5-0.5b-instruct', weight_version='default', chat_template=None, hf_chat_template_name=None, completion_template=None, file_storage_path='sglang_storage', enable_cache_report=False, reasoning_parser=None, tool_call_parser=None, tool_server=None, sampling_defaults='model', dp_size=1, load_balance_method='round_robin', attn_cp_size=1, moe_dp_size=1, dist_init_addr=None, nnodes=1, node_rank=0, json_model_override_args='{}', preferred_sampling_params=None, enable_lora=None, enable_lora_overlap_loading=None, max_lora_rank=None, lora_target_modules=None, lora_paths=None, max_loaded_loras=None, max_loras_per_batch=8, lora_eviction_policy='lru', lora_backend='csgmv', max_lora_chunk_size=16, attention_backend='fa3', decode_attention_backend=None, prefill_attention_backend=None, sampling_backend='flashinfer', grammar_backend='xgrammar', mm_attention_backend=None, fp8_gemm_runner_backend='auto', fp4_gemm_runner_backend='auto', nsa_prefill_backend=None, nsa_decode_backend=None, disable_flashinfer_autotune=False, mamba_backend='triton', speculative_algorithm=None, speculative_draft_model_path=None, speculative_draft_model_revision=None, speculative_draft_load_format=None, speculative_num_steps=None, speculative_eagle_topk=None, speculative_num_draft_tokens=None, speculative_accept_threshold_single=1.0, speculative_accept_threshold_acc=1.0, speculative_token_map=None, speculative_attention_mode='prefill', speculative_draft_attention_backend=None, speculative_moe_runner_backend='auto', speculative_moe_a2a_backend=None, speculative_draft_model_quantization=None, speculative_ngram_min_match_window_size=1, speculative_ngram_max_match_window_size=12, speculative_ngram_min_bfs_breadth=1, speculative_ngram_max_bfs_breadth=10, speculative_ngram_match_type='BFS', speculative_ngram_max_trie_depth=18, speculative_ngram_capacity=10000000, enable_multi_layer_eagle=False, ep_size=1, moe_a2a_backend='none', moe_runner_backend='auto', flashinfer_mxfp4_moe_precision='default', enable_flashinfer_allreduce_fusion=False, enable_aiter_allreduce_fusion=False, deepep_mode='auto', ep_num_redundant_experts=0, ep_dispatch_algorithm=None, init_expert_location='trivial', enable_eplb=False, eplb_algorithm='auto', eplb_rebalance_num_iterations=1000, eplb_rebalance_layers_per_chunk=None, eplb_min_rebalancing_utilization_threshold=1.0, expert_distribution_recorder_mode=None, expert_distribution_recorder_buffer_size=1000, enable_expert_distribution_metrics=False, deepep_config=None, moe_dense_tp_size=None, elastic_ep_backend=None, enable_elastic_expert_backup=False, mooncake_ib_device=None, max_mamba_cache_size=None, mamba_ssm_dtype=None, mamba_full_memory_ratio=0.9, mamba_scheduler_strategy='no_buffer', mamba_track_interval=256, linear_attn_backend='triton', linear_attn_decode_backend=None, linear_attn_prefill_backend=None, enable_hierarchical_cache=False, hicache_ratio=2.0, hicache_size=0, hicache_write_policy='write_through', hicache_io_backend='kernel', hicache_mem_layout='layer_first', disable_hicache_numa_detect=False, hicache_storage_backend=None, hicache_storage_prefetch_policy='best_effort', hicache_storage_backend_extra_config=None, enable_hisparse=False, hisparse_config=None, enable_lmcache=False, kt_weight_path=None, kt_method=None, kt_cpuinfer=None, kt_threadpool_count=None, kt_num_gpu_experts=None, kt_max_deferred_experts_per_token=None, dllm_algorithm=None, dllm_algorithm_config=None, enable_double_sparsity=False, ds_channel_config_path=None, ds_heavy_channel_num=32, ds_heavy_token_num=256, ds_heavy_channel_type='qk', ds_sparse_decode_threshold=4096, cpu_offload_gb=0, offload_group_size=-1, offload_num_in_group=1, offload_prefetch_step=1, offload_mode='cpu', multi_item_scoring_delimiter=None, disable_radix_cache=False, cuda_graph_max_bs=4, cuda_graph_bs=[1, 2, 4, 8, 12, 16, 24, 32, 40, 48, 56, 64, 72, 80, 88, 96, 104, 112, 120, 128, 136, 144, 152, 160, 168, 176, 184, 192, 200, 208, 216, 224, 232, 240, 248, 256], disable_cuda_graph=True, disable_cuda_graph_padding=False, enable_profile_cuda_graph=False, enable_cudagraph_gc=False, enable_layerwise_nvtx_marker=False, enable_nccl_nvls=False, enable_symm_mem=False, disable_flashinfer_cutlass_moe_fp4_allgather=False, enable_tokenizer_batch_encode=False, disable_tokenizer_batch_decode=False, disable_outlines_disk_cache=False, disable_custom_all_reduce=False, enable_mscclpp=False, enable_torch_symm_mem=False, pre_warm_nccl=False, disable_overlap_schedule=False, enable_mixed_chunk=False, enable_dp_attention=False, enable_dp_lm_head=False, enable_two_batch_overlap=False, enable_single_batch_overlap=False, tbo_token_distribution_threshold=0.48, enable_torch_compile=False, disable_piecewise_cuda_graph=False, enforce_piecewise_cuda_graph=False, enable_torch_compile_debug_mode=False, torch_compile_max_bs=32, piecewise_cuda_graph_max_tokens=8192, piecewise_cuda_graph_tokens=[4, 8, 12, 16, 20, 24, 28, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 352, 384, 416, 448, 480, 512, 576, 640, 704, 768, 832, 896, 960, 1024, 1280, 1536, 1792, 2048, 2304, 2560, 2816, 3072, 3328, 3584, 3840, 4096, 4608, 5120, 5632, 6144, 6656, 7168, 7680, 8192], piecewise_cuda_graph_compiler='eager', torchao_config='', enable_nan_detection=False, enable_p2p_check=False, triton_attention_reduce_in_fp32=False, triton_attention_num_kv_splits=8, triton_attention_split_tile_size=None, num_continuous_decode_steps=1, delete_ckpt_after_loading=False, enable_memory_saver=False, enable_weights_cpu_backup=False, enable_draft_weights_cpu_backup=False, allow_auto_truncate=False, enable_custom_logit_processor=False, flashinfer_mla_disable_ragged=False, disable_shared_experts_fusion=False, disable_chunked_prefix_cache=False, disable_fast_image_processor=False, keep_mm_feature_on_device=False, enable_return_hidden_states=False, enable_return_routed_experts=False, scheduler_recv_interval=1, numa_node=None, enable_deterministic_inference=False, rl_on_policy_target=None, enable_attn_tp_input_scattered=False, enable_nsa_prefill_context_parallel=False, nsa_prefill_cp_mode='round-robin-split', enable_fused_qk_norm_rope=False, enable_precise_embedding_interpolation=False, enable_fused_moe_sum_all_reduce=False, enable_prefill_context_parallel=False, prefill_cp_mode='in-seq-split', enable_dynamic_batch_tokenizer=False, dynamic_batch_tokenizer_batch_size=32, dynamic_batch_tokenizer_batch_timeout=0.002, debug_tensor_dump_output_folder=None, debug_tensor_dump_layers=None, debug_tensor_dump_input_file=None, debug_tensor_dump_inject=False, disaggregation_mode='null', disaggregation_transfer_backend='mooncake', disaggregation_bootstrap_port=8998, disaggregation_ib_device=None, disaggregation_decode_enable_offload_kvcache=False, num_reserved_decode_tokens=512, disaggregation_decode_polling_interval=1, encoder_only=False, language_only=False, encoder_transfer_backend='zmq_to_scheduler', encoder_urls=[], enable_adaptive_dispatch_to_encoder=False, custom_weight_loader=[], weight_loader_disable_mmap=False, remote_instance_weight_loader_seed_instance_ip=None, remote_instance_weight_loader_seed_instance_service_port=None, remote_instance_weight_loader_send_weights_group_ports=None, remote_instance_weight_loader_backend='nccl', remote_instance_weight_loader_start_seed_via_transfer_engine=False, modelexpress_config=None, enable_pdmux=False, pdmux_config_path=None, sm_group_num=8, mm_max_concurrent_calls=32, mm_per_request_timeout=10.0, enable_broadcast_mm_inputs_process=False, enable_prefix_mm_cache=False, mm_enable_dp_encoder=False, mm_process_config={}, limit_mm_data_per_request=None, enable_mm_global_cache=False, decrypted_config_file=None, decrypted_draft_config_file=None, forward_hooks=None)


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-24 05:16:29.742 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 05:16:29] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 05:16:29.742 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 05:16:29] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 05:16:29.742 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 05:16:29] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 05:16:29.742 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 05:16:29] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 05:16:29.742 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 05:16:29] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.00it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.99it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:19,  2.45s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:19,  2.45s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:19,  2.45s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:19,  2.45s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.17it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.17it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.17it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.17it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.17it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.17it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.17it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.17it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.17it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:03, 12.91it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:03, 12.91it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:03, 12.91it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:03, 12.91it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:03, 12.91it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:03, 12.91it/s]

    Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:03, 12.91it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:02<00:03, 12.91it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 19.28it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 19.28it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 19.28it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 19.28it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 19.28it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:02<00:01, 19.28it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:02<00:01, 19.28it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:02<00:01, 19.28it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:00, 26.08it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:00, 26.08it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:00, 26.08it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:00, 26.08it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:00, 26.08it/s]

    Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:00, 26.08it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:00, 26.08it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 31.14it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 31.14it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 31.14it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 31.14it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 31.14it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 31.14it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 31.14it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 36.43it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 36.43it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 36.43it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 36.43it/s] 

    Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 36.43it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 36.43it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 36.43it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 39.44it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 39.44it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 39.44it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 39.44it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 39.44it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 39.44it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 39.44it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 39.44it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 39.44it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.87it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=117.90 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=117.87 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=117.87 GB):   3%|▎         | 2/58 [00:00<00:03, 18.02it/s]Capturing num tokens (num_tokens=7168 avail_mem=117.87 GB):   3%|▎         | 2/58 [00:00<00:03, 18.02it/s]Capturing num tokens (num_tokens=6656 avail_mem=117.87 GB):   3%|▎         | 2/58 [00:00<00:03, 18.02it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=117.87 GB):   3%|▎         | 2/58 [00:00<00:03, 18.02it/s]Capturing num tokens (num_tokens=6144 avail_mem=117.87 GB):   9%|▊         | 5/58 [00:00<00:02, 21.01it/s]Capturing num tokens (num_tokens=5632 avail_mem=117.86 GB):   9%|▊         | 5/58 [00:00<00:02, 21.01it/s]Capturing num tokens (num_tokens=5120 avail_mem=117.87 GB):   9%|▊         | 5/58 [00:00<00:02, 21.01it/s]Capturing num tokens (num_tokens=4608 avail_mem=117.86 GB):   9%|▊         | 5/58 [00:00<00:02, 21.01it/s]Capturing num tokens (num_tokens=4608 avail_mem=117.86 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.40it/s]Capturing num tokens (num_tokens=4096 avail_mem=117.86 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.40it/s]Capturing num tokens (num_tokens=3840 avail_mem=117.86 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.40it/s]Capturing num tokens (num_tokens=3584 avail_mem=117.85 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.40it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=117.85 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.40it/s]Capturing num tokens (num_tokens=3328 avail_mem=117.85 GB):  21%|██        | 12/58 [00:00<00:01, 29.38it/s]Capturing num tokens (num_tokens=3072 avail_mem=117.85 GB):  21%|██        | 12/58 [00:00<00:01, 29.38it/s]Capturing num tokens (num_tokens=2816 avail_mem=117.85 GB):  21%|██        | 12/58 [00:00<00:01, 29.38it/s]Capturing num tokens (num_tokens=2560 avail_mem=117.84 GB):  21%|██        | 12/58 [00:00<00:01, 29.38it/s]Capturing num tokens (num_tokens=2304 avail_mem=117.84 GB):  21%|██        | 12/58 [00:00<00:01, 29.38it/s]Capturing num tokens (num_tokens=2048 avail_mem=117.83 GB):  21%|██        | 12/58 [00:00<00:01, 29.38it/s]Capturing num tokens (num_tokens=2048 avail_mem=117.83 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.32it/s]Capturing num tokens (num_tokens=1792 avail_mem=117.83 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.32it/s]Capturing num tokens (num_tokens=1536 avail_mem=117.83 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.32it/s]Capturing num tokens (num_tokens=1280 avail_mem=117.82 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.32it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=117.80 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.32it/s]Capturing num tokens (num_tokens=960 avail_mem=117.82 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.32it/s] Capturing num tokens (num_tokens=960 avail_mem=117.82 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.05it/s]Capturing num tokens (num_tokens=896 avail_mem=117.82 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.05it/s]Capturing num tokens (num_tokens=832 avail_mem=117.81 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.05it/s]Capturing num tokens (num_tokens=768 avail_mem=117.81 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.05it/s]Capturing num tokens (num_tokens=704 avail_mem=117.81 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.05it/s]Capturing num tokens (num_tokens=640 avail_mem=117.80 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.05it/s]Capturing num tokens (num_tokens=640 avail_mem=117.80 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.93it/s]Capturing num tokens (num_tokens=576 avail_mem=117.80 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.93it/s]Capturing num tokens (num_tokens=512 avail_mem=117.79 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.93it/s]

    Capturing num tokens (num_tokens=480 avail_mem=117.81 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.93it/s]Capturing num tokens (num_tokens=448 avail_mem=117.80 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.93it/s]Capturing num tokens (num_tokens=416 avail_mem=117.80 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.93it/s]Capturing num tokens (num_tokens=416 avail_mem=117.80 GB):  55%|█████▌    | 32/58 [00:00<00:00, 40.88it/s]Capturing num tokens (num_tokens=384 avail_mem=117.80 GB):  55%|█████▌    | 32/58 [00:00<00:00, 40.88it/s]Capturing num tokens (num_tokens=352 avail_mem=117.79 GB):  55%|█████▌    | 32/58 [00:00<00:00, 40.88it/s]Capturing num tokens (num_tokens=320 avail_mem=117.79 GB):  55%|█████▌    | 32/58 [00:00<00:00, 40.88it/s]Capturing num tokens (num_tokens=288 avail_mem=117.79 GB):  55%|█████▌    | 32/58 [00:00<00:00, 40.88it/s]Capturing num tokens (num_tokens=256 avail_mem=117.78 GB):  55%|█████▌    | 32/58 [00:01<00:00, 40.88it/s]Capturing num tokens (num_tokens=256 avail_mem=117.78 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.07it/s]Capturing num tokens (num_tokens=240 avail_mem=117.78 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.07it/s]Capturing num tokens (num_tokens=224 avail_mem=117.78 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.07it/s]

    Capturing num tokens (num_tokens=208 avail_mem=117.77 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.07it/s]Capturing num tokens (num_tokens=192 avail_mem=117.77 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.07it/s]Capturing num tokens (num_tokens=176 avail_mem=117.77 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.07it/s]Capturing num tokens (num_tokens=176 avail_mem=117.77 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.15it/s]Capturing num tokens (num_tokens=160 avail_mem=117.77 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.15it/s]Capturing num tokens (num_tokens=144 avail_mem=117.76 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.15it/s]Capturing num tokens (num_tokens=128 avail_mem=117.76 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.15it/s]Capturing num tokens (num_tokens=112 avail_mem=117.76 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.15it/s]Capturing num tokens (num_tokens=96 avail_mem=117.75 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.15it/s] Capturing num tokens (num_tokens=96 avail_mem=117.75 GB):  81%|████████  | 47/58 [00:01<00:00, 43.63it/s]Capturing num tokens (num_tokens=80 avail_mem=117.75 GB):  81%|████████  | 47/58 [00:01<00:00, 43.63it/s]Capturing num tokens (num_tokens=64 avail_mem=117.75 GB):  81%|████████  | 47/58 [00:01<00:00, 43.63it/s]

    Capturing num tokens (num_tokens=48 avail_mem=117.74 GB):  81%|████████  | 47/58 [00:01<00:00, 43.63it/s]Capturing num tokens (num_tokens=32 avail_mem=117.74 GB):  81%|████████  | 47/58 [00:01<00:00, 43.63it/s]Capturing num tokens (num_tokens=28 avail_mem=117.74 GB):  81%|████████  | 47/58 [00:01<00:00, 43.63it/s]Capturing num tokens (num_tokens=28 avail_mem=117.74 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.11it/s]Capturing num tokens (num_tokens=24 avail_mem=117.73 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.11it/s]Capturing num tokens (num_tokens=20 avail_mem=117.73 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.11it/s]Capturing num tokens (num_tokens=16 avail_mem=117.77 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.11it/s]Capturing num tokens (num_tokens=12 avail_mem=117.76 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.11it/s]Capturing num tokens (num_tokens=8 avail_mem=117.76 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.11it/s] Capturing num tokens (num_tokens=8 avail_mem=117.76 GB):  98%|█████████▊| 57/58 [00:01<00:00, 44.68it/s]Capturing num tokens (num_tokens=4 avail_mem=116.79 GB):  98%|█████████▊| 57/58 [00:01<00:00, 44.68it/s]

    Capturing num tokens (num_tokens=4 avail_mem=116.79 GB): 100%|██████████| 58/58 [00:01<00:00, 37.84it/s]


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
    Generated text:  Anna. I have been in the computer programming field for 10 years. I have been here for the last 5 years now.
    A: 5 years. I have been working in the field for 10 years, and I was here for 5 years now. 
    B: 10 years and 5 years.
    Which one of the following is correct?
    A. A is correct.
    B. B is correct.
    C. Both A and B are correct.
    D. Neither A nor B is correct.
    E. All of them are incorrect.
    
    To determine the correct answer, we need to compare the ages mentioned
    ===============================
    Prompt: The president of the United States is
    Generated text:  24 years older than the president of Central America. If the president of the United States is currently 74 years old, how old would the president of Central America be when the president of the United States turns 35 years old?
    
    To determine how old the president of Central America would be when the president of the United States turns 35 years old, we need to follow these steps:
    
    1. Identify the current age of the president of the United States.
    2. Determine the age difference between the presidents of the two countries.
    3. Calculate the age of the president of Central America when the president of the United States
    ===============================
    Prompt: The capital of France is
    Generated text: :
    A. Paris
    B. London
    C. Rome
    D. Berlin
    E. Tokyo
    
    A. Paris is the capital of France, and it is a magnificent city known for its architecture, history, and numerous attractions, including the Eiffel Tower, the Louvre Museum, and Notre-Dame Cathedral. Paris is often referred to as the "City of Light" due to its vibrant nightlife and artistic scene. The other options listed are not the capital of France. London, Rome, Berlin, and Tokyo are all capitals of their respective countries. So, the correct answer is not listed among the choices provided. Tokyo is
    ===============================
    Prompt: The future of AI is
    Generated text:  uncertain. It can be predicted that in the coming years, the technical and societal impact of AI will only increase. AI is expected to be a crucial element in the future of the world, and it will not only change the way we live and work, but also drive significant technological advancements.
    AI has the potential to completely transform the way we interact with technology. For instance, it can automate many tasks that are currently done manually, such as data entry, transcription, and document processing. With AI, we can also use algorithms to analyze large datasets and identify patterns that would be difficult or impossible for humans to discern.
    In addition, AI can


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm a [job title] with [number of years] years of experience in [industry]. I'm passionate about [reason for interest] and I'm always looking for ways to [action or goal]. I'm a [reason for interest] and I'm always looking for ways to [action or goal]. I'm a [reason for interest] and I'm always looking for ways to [action or goal]. I'm a [reason for interest] and I'm always looking for ways to [action or goal]. I'm a [reason for interest
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. It is also home to the French Parliament and the French Academy of Sciences. Paris is a bustling city with a rich history and culture, and is a popular tourist destination. It is the largest city in France and the second-largest city in the European Union. The city is known for its fashion industry, art scene, and food culture. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly. It is a city of people, with a diverse population of over 10 million people.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that are expected to shape the future of AI:
    
    1. Increased focus on ethical considerations: As AI becomes more integrated into our daily lives, there will be a growing emphasis on ethical considerations. This will include issues such as bias, transparency, accountability, and the potential for AI to be used for harmful purposes.
    
    2. Greater integration with other technologies: AI is likely to become more integrated with other technologies, such as machine learning, natural language processing, and computer vision. This will allow
    


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
    Generated text:  [Name], I'm a [Type] [Background], [Job Title] at [Company Name], working on [Company Name]'s [Project Name] for [Year]. I'm excited to be here and help my team achieve [Achievement]. 
    
    I'm looking forward to learning more about the [Company Name] and contributing to [Project Name] in the way that I believe is best for my skills and goals. Thank you for taking the time to meet me. Looking forward to talking to you! 📅💼💼💼
    
    (Please note that this is a fictional character and does not represent any company or organization)
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    Paris is known for its historical significance, art, cuisine, and fashion, and it is also the largest city in Europe. It is home to some of the world's most famous landmarks, including the Eiffel Tower, the Louvre Museum, Notre-Dame Cathedral, and the Champs-Élysées. The city's cuisine is a reflection of its rich history and the diverse cultures that have influenced it. Paris is a vibrant and dynamic city that continues to be a global capital of culture and influence. The city's importance in global affairs makes it one of the most important cities in the world. Therefore, Paris is
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  exciting and rapidly evolving. Here are some potential future trends in AI:
    
    1. Increased autonomy: AI will continue to become more autonomous, with the ability to make decisions based on data and patterns learned from past interactions.
    
    2. Personalized AI: AI will become more personalized, as it learns from individual users' data and preferences to provide more accurate and relevant recommendations.
    
    3. Human-like AI: AI will continue to evolve and become more human-like, as it learns from human interactions and uses this feedback to improve its algorithms.
    
    4. Enhanced emotional intelligence: AI will become more empathetic and able to understand human emotions, leading to more effective


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

     a

     [

    Your

     Profession

    ]

    !

     I

    'm

     passionate

     about

     [

    Your

     Field

     of

     Expert

    ise

     or

     Career

     Goal

    ],

     and

     I

     love

     [

    Your

     Mot

    iv

    ational

     Phrase

     or

     Goal

    ].

     I

    'm

     always

     learning

     and

     growing

    ,

     and

     I

     believe

     that

     my

     skills

     and

     knowledge

     are

     valuable

     to

     anyone

     who

     has

     the

     same

     goal

    .

     I

    'm

     excited

     to

     be

     here

     and

     share

     my

     knowledge

     with

     anyone

     who

    's

     interested

    .

     What

    's

     your

     field

     of

     expertise

     or

     career

     goal

    ?

     Let

    's

     chat

    !

     You

     got

     it

    .

     [

    Your

     Name

    ].

     [

    Your

     Name

    ]

     is

     a

     [

    Your

     Profession

    ]

     who

     is

     passionate

     about

     [

    Your

     Field

     of

     Expert

    ise

     or

     Career

     Goal

    ]

     and

     is

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     the

     city

     of

     lights

     and

     of

     the

     E

    iff

    el

     Tower

    .
    


    Summary

    :

     Paris

     is

     the

     capital

     city

     of

     France

    ,

     located

     on

     the

     banks

     of

     the

     Se

    ine

     River

     in

     the

     center

     of

     the

     country

    .

     The

     city

     is

     known

     for

     its

     rich

     history

    ,

     iconic

     landmarks

    ,

     and

     vibrant

     cultural

     scene

    .

     As

     the

     largest

     city

     in

     Europe

     and

     the

     world

    ,

     Paris

     is

     home

     to

     many

     of

     the

     world

    's

     top

     museums

    ,

     theaters

    ,

     and

     cultural

     institutions

    ,

     including

     the

     Lou

    vre

     Museum

    ,

     Notre

     Dame

     Cathedral

    ,

     and

     the

     Mus

    ée

     d

    '

    Or

    say

    .

     The

     city

     is

     also

     home

     to

     many

     of

     the

     world

    's

     most

     famous

     artists

    ,

     including

     Vincent

     van

     G

    ogh

     and

     Pablo

     Picasso

    ,

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     shaped

     by

     several

     trends

    ,

     including

    :
    


    1

    .

     Increased

     use

     of

     AI

     for

     autonomous

     vehicles

    :

     As

     driver

    less

     cars

     become

     more

     common

    ,

     AI

     will

     play

     an

     increasingly

     important

     role

     in

     autonomous

     driving

     systems

    ,

     reducing

     the

     risk

     of

     human

     error

     and

     improving

     safety

    .
    


    2

    .

     Expansion

     of

     AI

     applications

     to

     healthcare

    :

     AI

     is

     already

     being

     used

     in

     healthcare

     to

     improve

     patient

     care

    ,

     diagnose

     diseases

    ,

     and

     predict

     health

     outcomes

    .

     As

     AI

     technology

     continues

     to

     improve

    ,

     we

     may

     see

     further

     expansion

     into

     areas

     such

     as

     genetics

    ,

     personalized

     medicine

    ,

     and

     mental

     health

    .
    


    3

    .

     Integration

     of

     AI

     with

     IoT

    :

     As

     more

     devices

     are

     connected

     to

     the

     internet

    ,

     AI

     will

     become

     an

     increasingly

     important

     tool

    



```python
llm.shutdown()
```
