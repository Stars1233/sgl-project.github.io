# Query VLM with Offline Engine

This tutorial demonstrates how to use SGLang's **offline Engine API** to query VLMs. We will demonstrate usage with Qwen2.5-VL and Llama 4. This section demonstrates three different calling approaches:

1. **Basic Call**: Directly pass images and text.
2. **Processor Output**: Use HuggingFace processor for data preprocessing.
3. **Precomputed Embeddings**: Pre-calculate image features to improve inference efficiency.

## Understanding the Three Input Formats

SGLang supports three ways to pass visual data, each optimized for different scenarios:

### 1. **Raw Images** - Simplest approach
- Pass PIL Images, file paths, URLs, or base64 strings directly
- SGLang handles all preprocessing automatically
- Best for: Quick prototyping, simple applications

### 2. **Processor Output** - For custom preprocessing
- Pre-process images with HuggingFace processor
- Pass the complete processor output dict with `format: "processor_output"`
- Best for: Custom image transformations, integration with existing pipelines
- Requirement: Must use `input_ids` instead of text prompt

### 3. **Precomputed Embeddings** - For maximum performance
- Pre-calculate visual embeddings using the vision encoder
- Pass embeddings with `format: "precomputed_embedding"`
- Best for: Repeated queries on same images, caching, high-throughput serving
- Performance gain: Avoids redundant vision encoder computation (30-50% speedup)

**Key Rule**: Within a single request, use only one format for all images. Don't mix formats.

The examples below demonstrate all three approaches with both Qwen2.5-VL and Llama 4 models.

## Querying Qwen2.5-VL Model


```python
import nest_asyncio

nest_asyncio.apply()

import sglang.test.doc_patch  # noqa: F401

model_path = "Qwen/Qwen2.5-VL-3B-Instruct"
chat_template = "qwen2-vl"
example_image_url = "https://raw.githubusercontent.com/sgl-project/sglang/main/examples/assets/example_image.png"
```


```python
from io import BytesIO
import requests
from PIL import Image

from sglang.srt.parser.conversation import chat_templates

image = Image.open(BytesIO(requests.get(example_image_url).content))

conv = chat_templates[chat_template].copy()
conv.append_message(conv.roles[0], f"What's shown here: {conv.image_token}?")
conv.append_message(conv.roles[1], "")
conv.image_data = [image]

print("Generated prompt text:")
print(conv.get_prompt())
print(f"\nImage size: {image.size}")
image
```

    Generated prompt text:
    <|im_start|>system
    You are a helpful assistant.<|im_end|>
    <|im_start|>user
    What's shown here: <|vision_start|><|image_pad|><|vision_end|>?<|im_end|>
    <|im_start|>assistant
    
    
    Image size: (570, 380)





    
![png](vlm_query_files/vlm_query_4_1.png)
    



### Basic Offline Engine API Call


```python
from sglang import Engine

llm = Engine(model_path=model_path, chat_template=chat_template, log_level="warning")
```

    2026-04-03 04:41:30.941 DEBUG Persistent cache disabled, using in-memory JIT cache


    [2026-04-03 04:41:30] Persistent cache disabled, using in-memory JIT cache


    2026-04-03 04:41:30.942 DEBUG Persistent cache disabled, using in-memory JIT cache


    [2026-04-03 04:41:30] Persistent cache disabled, using in-memory JIT cache


    2026-04-03 04:41:30.943 DEBUG Persistent cache disabled, using in-memory JIT cache


    [2026-04-03 04:41:30] Persistent cache disabled, using in-memory JIT cache


    2026-04-03 04:41:30.943 DEBUG Persistent cache disabled, using in-memory JIT cache


    [2026-04-03 04:41:30] Persistent cache disabled, using in-memory JIT cache


    2026-04-03 04:41:30.944 DEBUG Persistent cache disabled, using in-memory JIT cache


    [2026-04-03 04:41:30] Persistent cache disabled, using in-memory JIT cache


    2026-04-03 04:41:41.428 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 04:41:41] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 04:41:41.428 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 04:41:41] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 04:41:41.428 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 04:41:41] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 04:41:41.428 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 04:41:41] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 04:41:41.428 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 04:41:41] Persistent cache disabled, using in-memory JIT cache


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:00<00:00,  1.78it/s]

    Multi-thread loading shards: 100% Completed | 2/2 [00:01<00:00,  1.31it/s]Multi-thread loading shards: 100% Completed | 2/2 [00:01<00:00,  1.36it/s]



```python
out = llm.generate(prompt=conv.get_prompt(), image_data=[image])
print("Model response:")
print(out["text"])
```

    2026-04-03 04:41:51,311 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 04:41:51] Unexpected error during package walk: cutlass.cute.experimental


    Model response:
    Here's what's shown in the image:
    
    1. Two yellow taxis are parked parallel to each other on a city street.
    2. A man is stripping a cover or luggage off the back of a grayardy cart.
    3. The man is wearing a yellow shirt.
    4. There are storefronts in the background, with signs and flags hanging outside.
    5. The scene appears to be in a busy urban area with tall buildings and a few trees.
    6. Streetlights and other city elements are visible.
    
    The image depicts an urban scene where a man is loading or unloading luggage from a cart, likely in a taxi.


### Call with Processor Output

Using a HuggingFace processor to preprocess text and images, and passing the `processor_output` directly into `Engine.generate`.


```python
from transformers import AutoProcessor

processor = AutoProcessor.from_pretrained(model_path, use_fast=True)
processor_output = processor(
    images=[image], text=conv.get_prompt(), return_tensors="pt"
)

out = llm.generate(
    input_ids=processor_output["input_ids"][0].detach().cpu().tolist(),
    image_data=[dict(processor_output, format="processor_output")],
)
print("Response using processor output:")
print(out["text"])
```

    Response using processor output:
    This image shows a scene with several taxis and a参勤sns.


### Call with Precomputed Embeddings

You can pre-calculate image features to avoid repeated visual encoding processes.


```python
from transformers import AutoProcessor
from transformers import Qwen2_5_VLForConditionalGeneration

processor = AutoProcessor.from_pretrained(model_path, use_fast=True)
model = Qwen2_5_VLForConditionalGeneration.from_pretrained(model_path).eval()
vision = model.model.visual.cuda()
```


    Downloading (incomplete total...): 0.00B [00:00, ?B/s]



    Fetching 2 files:   0%|          | 0/2 [00:00<?, ?it/s]



    Loading weights:   0%|          | 0/824 [00:00<?, ?it/s]



```python
processor_output = processor(
    images=[image], text=conv.get_prompt(), return_tensors="pt"
)

input_ids = processor_output["input_ids"][0].detach().cpu().tolist()

precomputed_embeddings = vision(
    processor_output["pixel_values"].cuda(), processor_output["image_grid_thw"].cuda()
)
precomputed_embeddings = precomputed_embeddings.pooler_output

multi_modal_item = dict(
    processor_output,
    format="precomputed_embedding",
    feature=precomputed_embeddings,
)

out = llm.generate(input_ids=input_ids, image_data=[multi_modal_item])
print("Response using precomputed embeddings:")
print(out["text"])

llm.shutdown()
```

    Response using precomputed embeddings:
    The image shows a street scene featuring a yellow taxi in New York City. The taxi has been adorned with colorful fabric, creating a makeshift banner-like display. This is likely part of a celebratory event or artistic performance, as the taxi was not originally designed to be decorated. The scene is lively and festive, with other elements like buildings, flags, and street signs visible in the background.


## Querying Llama 4 Vision Model

```python
model_path = "meta-llama/Llama-4-Scout-17B-16E-Instruct"
chat_template = "llama-4"

from io import BytesIO
import requests
from PIL import Image

from sglang.srt.parser.conversation import chat_templates

# Download the same example image
image = Image.open(BytesIO(requests.get(example_image_url).content))

conv = chat_templates[chat_template].copy()
conv.append_message(conv.roles[0], f"What's shown here: {conv.image_token}?")
conv.append_message(conv.roles[1], "")
conv.image_data = [image]

print("Llama 4 generated prompt text:")
print(conv.get_prompt())
print(f"Image size: {image.size}")

image
```

### Llama 4 Basic Call

Llama 4 requires more computational resources, so it's configured with multi-GPU parallelism (tp_size=4) and larger context length.

```python
llm = Engine(
    model_path=model_path,
    enable_multimodal=True,
    attention_backend="fa3",
    tp_size=4,
    context_length=65536,
)

out = llm.generate(prompt=conv.get_prompt(), image_data=[image])
print("Llama 4 response:")
print(out["text"])
```

### Call with Processor Output

Using HuggingFace processor to preprocess data can reduce computational overhead during inference.

```python
from transformers import AutoProcessor

processor = AutoProcessor.from_pretrained(model_path, use_fast=True)
processor_output = processor(
    images=[image], text=conv.get_prompt(), return_tensors="pt"
)

out = llm.generate(
    input_ids=processor_output["input_ids"][0].detach().cpu().tolist(),
    image_data=[dict(processor_output, format="processor_output")],
)
print("Response using processor output:")
print(out)
```

### Call with Precomputed Embeddings

```python
from transformers import AutoProcessor
from transformers import Llama4ForConditionalGeneration

processor = AutoProcessor.from_pretrained(model_path, use_fast=True)
model = Llama4ForConditionalGeneration.from_pretrained(
    model_path, torch_dtype="auto"
).eval()

vision = model.vision_model.cuda()
multi_modal_projector = model.multi_modal_projector.cuda()

print(f'Image pixel values shape: {processor_output["pixel_values"].shape}')
input_ids = processor_output["input_ids"][0].detach().cpu().tolist()

# Process image through vision encoder
image_outputs = vision(
    processor_output["pixel_values"].to("cuda"), 
    aspect_ratio_ids=processor_output["aspect_ratio_ids"].to("cuda"),
    aspect_ratio_mask=processor_output["aspect_ratio_mask"].to("cuda"),
    output_hidden_states=False
)
image_features = image_outputs.last_hidden_state

# Flatten image features and pass through multimodal projector
vision_flat = image_features.view(-1, image_features.size(-1))
precomputed_embeddings = multi_modal_projector(vision_flat)

# Build precomputed embedding data item
mm_item = dict(
    processor_output, 
    format="precomputed_embedding", 
    feature=precomputed_embeddings
)

# Use precomputed embeddings for efficient inference
out = llm.generate(input_ids=input_ids, image_data=[mm_item])
print("Llama 4 precomputed embedding response:")
print(out["text"])
```
