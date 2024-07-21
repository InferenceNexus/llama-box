# LLaMA Box

[![](https://img.shields.io/github/actions/workflow/status/thxcode/llama-box/ci.yml?label=ci)](https://github.com/thxcode/llama-box/actions)
[![](https://img.shields.io/github/license/thxcode/llama-box?label=license)](https://github.com/thxcode/llama-box#license)
[![](https://img.shields.io/github/downloads/thxcode/llama-box/total)](https://github.com/thxcode/llama-box/releases)

LLaMA box is a clean, pure API(without frontend assets) LLMs inference server rather
than [llama-server](https://github.com/ggerganov/llama.cpp/blob/master/examples/server).

## Download

Download LLaMA Box from [the latest release](https://github.com/thxCode/llama-box/releases/latest) page please, now
LLaMA Box supports the following platforms.

| Backend             | OS/Arch                       | Supported Devices                                                                                                                                                                                                       |
|---------------------|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Apple Metal 3       | `darwin/amd64` `darwin/arm64` | Install macOS Ventura or later, see https://support.apple.com/en-sg/102894.                                                                                                                                             |
| NVIDIA CUDA 11.8-s  | `linux/amd64` `windows/amd64` | Compute capability is `8.0`, `8.6` or `8.9`, see https://developer.nvidia.com/cuda-gpus.                                                                                                                                |
| NVIDIA CUDA 12.5-s  | `linux/amd64` `windows/amd64` | Compute capability is `8.0`, `8.6` or `8.9`, see https://developer.nvidia.com/cuda-gpus.                                                                                                                                |
| NVIDIA CUDA 12.5-l  | `linux/amd64` `windows/amd64` | Compute capability is `6.0`, `6.1`, `7.0`, `7.5` ,`8.0`, `8.6` or `8.9`, see https://developer.nvidia.com/cuda-gpus.                                                                                                    |
| AMD ROCm/HIP 5.5-s  | `windows/amd64`               | LLVM target is `gfx1030`, `gfx1100`, `gfx1101` or `gfx1102`, see https://rocm.docs.amd.com/en/docs-5.5.1/release/windows_support.html.                                                                                  |
| AMD ROCm/HIP 5.7-s  | `linux/amd64` `windows/amd64` | LLVM target is `gfx1030`, `gfx1100`, `gfx1101` or `gfx1102`, see https://rocm.docs.amd.com/en/docs-5.7.1/release/gpu_os_support.html, https://rocm.docs.amd.com/en/docs-5.7.1/release/windows_support.html.             |
| AMD ROCm/HIP 5.7-l  | `windows/amd64`               | LLVM target is `gfx900`, `gfx906`,`gfx908`, `gfx90a`, `gfx940`, `gfx1030`, `gfx1100`, `gfx1101` or `gfx1102`, see https://rocm.docs.amd.com/en/docs-5.7.1/release/windows_support.html.                                 |
| AMD ROCm/HIP 6.1-s  | `linux/amd64`                 | LLVM target is `gfx1030`, `gfx1100`, `gfx1101` or `gfx1102`, see https://rocm.docs.amd.com/en/docs-5.5.1/release/windows_support.html.                                                                                  |
| AMD ROCm/HIP 6.1-l  | `linux/amd64`                 | LLVM target is `gfx900`, `gfx906`,`gfx908`, `gfx90a`, `gfx940`, `gfx1030`, `gfx1100`, `gfx1101` or `gfx1102`, see https://rocm.docs.amd.com/projects/install-on-linux/en/docs-6.1.2/reference/system-requirements.html. |
| Intel oneAPI 2024.1 | `linux/amd64` `windows/amd64` | See Linux/Windows distributions with oneAPI 2024.1 from https://www.intel.com/content/www/us/en/developer/articles/system-requirements/intel-oneapi-base-toolkit-system-requirements.html.                              |
| Intel oneAPI 2024.2 | `linux/amd64` `windows/amd64` | See Linux/Windows distributions with oneAPI 2024.2 from https://www.intel.com/content/www/us/en/developer/articles/system-requirements/intel-oneapi-base-toolkit-system-requirements.html.                              |
| Ascend CANN 8.0     | `linux/amd64` `linux/arm64`   | See https://www.hiascend.com/document/detail/zh/CANNCommunityEdition/80RC3alpha001/quickstart/quickstart/quickstart_18_0002.html                                                                                        |                                                                                       |

## Examples

> **Note**:
> [LM Studio](https://lmstudio.ai/) provides a fantastic UI for downloading the GGUF model from Hugging Face.
> The GGUF model files used in the following examples are downloaded via LM Studio.

- Chat completion via [Nous-Hermes-2-Mistral-7B-DPO](https://huggingface.co/NousResearch/Nous-Hermes-2-Mistral-7B-DPO)
  model.

    ```shell
    $ # Provide 4 sessions(allowing 4 parallel chat users), with a max of 2048 tokens per session.
    $ llama-box -c 8192 -np 4 --host 0.0.0.0 -m ~/.cache/lm-studio/models/NousResearch/Nous-Hermes-2-Mistral-7B-DPO/Nous-Hermes-2-Mistral-7B-DPO.Q5_K_M.gguf
    
    $ curl http://localhost:8080/v1/chat/completions -H "Content-Type: application/json" -d '{"model": "qwen2", "messages": [{"role":"user", "content":"Introduce Beijing in 50 words."}]}'
    ```

- Legacy completion via [GLM-4-9B-Chat](https://huggingface.co/THUDM/glm-4-9b-chat) model.

    ```shell
    $ # Provide 4 session(allowing 4 parallel chat users), with a max of 2048 tokens per session.
    $ llama-box -c 8192 -np 4 --host 0.0.0.0 -m ~/.cache/lm-studio/models/second-state/glm-4-9b-chat-GGUF/glm-4-9b-chat-Q5_K_M.gguf
    
    $ curl http://localhost:8080/v1/completions -H "Content-Type: application/json" -d '{"model": "glm4", "prompt": "<|system|>You are a helpful assistant.<|user|>Tell me a joke.<|assistant|>"}'
    ```

- Vision explanation via [LLaVA-Phi-3-Mini](https://huggingface.co/xtuner/llava-phi-3-mini-hf) model.

    ```shell
    $ # Provide 4 session(allowing 4 parallel chat users), with a max of 2048 tokens per session.
    $ llama-box -c 8192 -np 4 --host 0.0.0.0 -m ~/.cache/lm-studio/models/xtuner/llava-phi-3-mini-gguf/llava-phi-3-mini-f16.gguf --mmproj ~/.cache/lm-studio/models/xtuner/llava-phi-3-mini-gguf/llava-phi-3-mini-mmproj-f16.gguf
    
    $ IMAGE_URL="$(echo "data:image/jpeg;base64,$(curl https://llava.hliu.cc/file\=/nobackup/haotian/tmp/gradio/ca10383cc943e99941ecffdc4d34c51afb2da472/extreme_ironing.jpg --output - | base64)")"; \
      echo "{\"model\": \"llava-phi-3\", \"temperature\": 0.1, \"messages\": [{\"role\":\"user\", \"content\": [{\"type\": \"image_url\", \"image_url\": {\"url\": \"$IMAGE_URL\"}}, {\"type\": \"text\", \"text\": \"What is unusual about this image?\"}]}]}" > /tmp/llava-phi-3.json
    
    $ curl http://localhost:8080/v1/chat/completions -H "Content-Type: application/json" -d @/tmp/llava-phi-3.json
    ```

- Speculative decoding via [Qwen2-7B-Instruct](https://huggingface.co/Qwen/Qwen2-7B-Instruct)
  and [Qwen2-1.5B-Instruct](https://huggingface.co/Qwen/Qwen2-1.5B-Instruct) models.

    ```shell
    $ # Provide 4 session(allowing 4 parallel chat users), with a max of 2048 tokens per session.
    $ llama-box -c 8192 -np 4 --host 0.0.0.0 -m ~/.cache/lm-studio/models/QuantFactory/Qwen2-7B-Instruct-GGUF/Qwen2-7B-Instruct.Q5_K_M.gguf -md ~/.cache/lm-studio/models/QuantFactory/Qwen2-1.5B-Instruct-GGUF/Qwen2-1.5B-Instruct.Q5_K_M.gguf --draft 8
    
    $ curl http://localhost:8080/v1/completions -H "Content-Type: application/json" -d '{"model": "qwen2", "stream": true, "prompt": "Write a short story about a cat and a dog, more than 100 words."}'
    ```

## Usage

```shell
usage: llama-box [options]

general:

  -h,    --help, --usage          print usage and exit
         --version                show version and build info
  -m,    --model FILE             model path (default: models/7B/ggml-model-f16.gguf)
  -a,    --alias NAME             model name alias (default: unknown)
  -s,    --seed N                 RNG seed (default: -1, use random seed for < 0)
  -t,    --threads N              number of threads to use during generation (default: 8)
  -tb,   --threads-batch N        number of threads to use during batch and prompt processing (default: same as --threads)
  -lcs,  --lookup-cache-static FILE
                                  path to static lookup cache to use for lookup decoding (not updated by generation)
  -lcd,  --lookup-cache-dynamic FILE
                                  path to dynamic lookup cache to use for lookup decoding (updated by generation)
  -c,    --ctx-size N             size of the prompt context (default: 0, 0 = loaded from model)
  -n,    --predict N              number of tokens to predict (default: -1, -1 = infinity, -2 = until context filled)
  -b,    --batch-size N           logical maximum batch size (default: 2048)
  -ub,   --ubatch-size N          physical maximum batch size (default: 512)
         --keep N                 number of tokens to keep from the initial prompt (default: 0, -1 = all)
         --chunks N               max number of chunks to process (default: -1, -1 = all)
  -fa,   --flash-attn             enable Flash Attention (default: disabled)
  -e,    --escape                 process escapes sequences (\n, \r, \t, \', \", \\) (default: true)
         --no-escape              do not process escape sequences
         --samplers SAMPLERS      samplers that will be used for generation in the order, separated by ';'
                                  (default: top_k;tfs_z;typical_p;top_p;min_p;temperature)
         --sampling-seq SEQUENCE  simplified sequence for samplers that will be used (default: kfypmt)
         --penalize-nl            penalize newline tokens (default: false)
         --temp N                 temperature (default: 0.8)
         --top-k N                top-k sampling (default: 40, 0 = disabled)
         --top-p N                top-p sampling (default: 0.9, 1.0 = disabled)
         --min-p N                min-p sampling (default: 0.1, 0.0 = disabled)
         --tfs N                  tail free sampling, parameter z (default: 1.0, 1.0 = disabled)
         --typical N              locally typical sampling, parameter p (default: 1.0, 1.0 = disabled)
         --repeat-last-n N        last n tokens to consider for penalize (default: 64, 0 = disabled, -1 = ctx_size)
         --repeat-penalty N       penalize repeat sequence of tokens (default: 1.0, 1.0 = disabled)
         --presence-penalty N     repeat alpha presence penalty (default: 0.0, 0.0 = disabled)
         --frequency-penalty N    repeat alpha frequency penalty (default: 0.0, 0.0 = disabled)
         --dynatemp-range N       dynamic temperature range (default: 0.0, 0.0 = disabled)
         --dynatemp-exp N         dynamic temperature exponent (default: 1.0)
         --mirostat N             use Mirostat sampling.
                                  Top K, Nucleus, Tail Free and Locally Typical samplers are ignored if used.
                                  (default: 0, 0 = disabled, 1 = Mirostat, 2 = Mirostat 2.0)
         --mirostat-lr N          Mirostat learning rate, parameter eta (default: 0.1)
         --mirostat-ent N         Mirostat target entropy, parameter tau (default: 5.0)
  -l     --logit-bias TOKEN_ID(+/-)BIAS
                                  modifies the likelihood of token appearing in the completion,
                                  i.e. `--logit-bias 15043+1` to increase likelihood of token ' Hello',
                                  or `--logit-bias 15043-1` to decrease likelihood of token ' Hello'
         --grammar GRAMMAR        BNF-like grammar to constrain generations (see samples in grammars/ dir) (default: '')
         --grammar-file FILE      file to read grammar from
  -j,    --json-schema SCHEMA     JSON schema to constrain generations (https://json-schema.org/), e.g. `{}` for any JSON object
                                  For schemas w/ external $refs, use --grammar + example/json_schema_to_grammar.py instead
         --rope-scaling {none,linear,yarn}
                                  RoPE frequency scaling method, defaults to linear unless specified by the model
         --rope-scale N           RoPE context scaling factor, expands context by a factor of N
         --rope-freq-base N       RoPE base frequency, used by NTK-aware scaling (default: loaded from model)
         --rope-freq-scale N      RoPE frequency scaling factor, expands context by a factor of 1/N
         --yarn-orig-ctx N        YaRN: original context size of model (default: 0 = model training context size)
         --yarn-ext-factor N      YaRN: extrapolation mix factor (default: -1.0, 0.0 = full interpolation)
         --yarn-attn-factor N     YaRN: scale sqrt(t) or attention magnitude (default: 1.0)
         --yarn-beta-fast N       YaRN: low correction dim or beta (default: 32.0)
         --yarn-beta-slow N       YaRN: high correction dim or alpha (default: 1.0)
  -gan,  --grp-attn-n N           group-attention factor (default: 1)
  -gaw,  --grp-attn-w N           group-attention width (default: 512.0)
  -nkvo, --no-kv-offload          disable KV offload
  -ctk,  --cache-type-k TYPE      KV cache data type for K (default: f16)
  -ctv,  --cache-type-v TYPE      KV cache data type for V (default: f16)
  -dt,   --defrag-thold N         KV cache defragmentation threshold (default: -1.0, < 0 - disabled)
  -np,   --parallel N             number of parallel sequences to decode (default: 1)
  -cb,   --cont-batching          enable continuous batching (a.k.a dynamic batching) (default: enabled)
  -nocb, --no-cont-batching       disable continuous batching
         --mmproj FILE            path to a multimodal projector file for LLaVA
         --mlock                  force system to keep model in RAM rather than swapping or compressing
         --no-mmap                do not memory-map model (slower load but may reduce pageouts if not using mlock)
         --numa TYPE              attempt optimizations that help on some NUMA systems
                                    - distribute: spread execution evenly over all nodes
                                    - isolate: only spawn threads on CPUs on the node that execution started on
                                    - numactl: use the CPU map provided by numactl
                                  if run without this previously, it is recommended to drop the system page cache before using this
                                  see https://github.com/ggerganov/llama.cpp/issues/1437
         --override-kv KEY=TYPE:VALUE
                                  advanced option to override model metadata by key. may be specified multiple times.
                                  types: int, float, bool, str. example: --override-kv tokenizer.ggml.add_bos_token=bool:false
         --lora FILE              apply LoRA adapter (implies --no-mmap)
         --lora-scaled FILE SCALE 
                                  apply LoRA adapter with user defined scaling S (implies --no-mmap)
         --lora-base FILE         optional model to use as a base for the layers modified by the LoRA adapter
         --control-vector FILE    add a control vector
         --control-vector-scaled FILE SCALE
                                  add a control vector with user defined scaling SCALE
         --control-vector-layer-range START END
                                  layer range to apply the control vector(s) to, start and end inclusive
         --spm-infill             use Suffix/Prefix/Middle pattern for infill (instead of Prefix/Suffix/Middle) as some models prefer this. (default: disabled)
  -sp,   --special                special tokens output enabled (default: false)
  -ngl,  --gpu-layers N           number of layers to store in VRAM
  -sm,   --split-mode SPLIT_MODE  how to split the model across multiple GPUs, one of:
                                    - none: use one GPU only
                                    - layer (default): split layers and KV across GPUs
                                    - row: split rows across GPUs
  -ts,   --tensor-split SPLIT     fraction of the model to offload to each GPU, comma-separated list of proportions, e.g. 3,1
  -mg,   --main-gpu N             the GPU to use for the model (with split-mode = none),
                                  or for intermediate results and KV (with split-mode = row) (default: 0)

server:

         --host HOST              ip address to listen (default: 127.0.0.1)
         --port PORT              port to listen (default: 8080)
  -to    --timeout N              server read/write timeout in seconds (default: 600)
         --threads-http N         number of threads used to process HTTP requests (default: -1)
         --system-prompt-file FILE
                                  set a file to load a system prompt (initial prompt of all slots), this is useful for chat applications
         --metrics                enable prometheus compatible metrics endpoint (default: disabled)
         --infill                 enable infill endpoint (default: disabled)
         --embeddings             enable embedding endpoint (default: disabled)
         --no-slots               disables slots monitoring endpoint (default: enabled)
         --slot-save-path PATH    path to save slot kv cache (default: disabled)
         --chat-template JINJA_TEMPLATE
                                  set custom jinja chat template (default: template taken from model's metadata)
                                  only commonly used templates are accepted:
                                  https://github.com/ggerganov/llama.cpp/wiki/Templates-supported-by-llama_chat_apply_template
         --chat-template-file FILE
                                  set a file to load a custom jinja chat template (default: template taken from model's metadata)
  -sps,  --slot-prompt-similarity N
                                  how much the prompt of a request must match the prompt of a slot in order to use that slot (default: 0.50, 0.0 = disabled)
                                  
         --conn-idle N            server connection idle in seconds (default: 60)
         --conn-keepalive N       server connection keep-alive in seconds (default: 15)
  -tps   --tokens-per-second N    maximum number of tokens per second (default: 0, 0 = disabled, -1 = try to detect)
                                  when enabled, limit the request within its X-Request-Tokens-Per-Second HTTP header.

logging:

         --log-format {text,json} 
                                  log output format: json or text (default: json)

speculative:

  -md,   --model-draft FNAME      draft model for speculative decoding (default: unused)
  -td,   --threads-draft N        number of threads to use during generation (default: same as --threads)
  -tbd,  --threads-batch-draft N  number of threads to use during batch and prompt processing (default: same as --threads-draft)
         --draft N                number of tokens to draft for speculative decoding (default: 5)
  -ngld, --gpu-layers-draft N     number of layers to store in VRAM for the draft model

```

## API

- **GET** `/health`: Returns the current state of the LLaMA Box.
    + 503 -> `{"status": "loading model"}` if the model is still being loaded.
    + 500 -> `{"status": "error"}` if the model failed to load.
    + 200 -> `{"status": "ok", "slots_idle": 1, "slots_processing": 2 }` if the model is successfully loaded and the
      server is ready for further requests mentioned below.
    + 200 -> `{"status": "no slot available", "slots_idle": 0, "slots_processing": 32}` if no slots are currently
      available.
    + 503 -> `{"status": "no slot available", "slots_idle": 0, "slots_processing": 32}` if the query
      parameter `fail_on_no_slot` is provided and no slots are currently available.

- **GET** `/metrics`: Returns the Prometheus compatible metrics of the LLaMA Box.
    + This endpoint is only available if the `--metrics` flag is enabled.
    + `llamacpp:prompt_tokens_total`: (Counter) Number of prompt tokens processed.
    + `llamacpp:prompt_seconds_total`: (Counter) Prompt process time.
    + `llamacpp:tokens_predicted_total`: (Counter) Number of generation tokens processed.
    + `llamacpp:tokens_predicted_seconds_total`: (Counter) Predict process time.
    + `llamacpp:tokens_drafted_total`: (Counter) Number of speculative decoding tokens processed.
    + `llamacpp:tokens_drafted_accepted_total`: (Counter) Number of speculative decoding tokens to be accepted.
    + `llamacpp:prompt_tokens_seconds`: (Gauge) Average prompt throughput in tokens/s.
    + `llamacpp:predicted_tokens_seconds`: (Gauge) Average generation throughput in tokens/s.
    + `llamacpp:kv_cache_usage_ratio`: (Gauge) KV-cache usage. 1 means 100 percent usage.
    + `llamacpp:kv_cache_tokens`: (Gauge) KV-cache tokens.
    + `llamacpp:requests_processing`: (Gauge) Number of request processing.
    + `llamacpp:requests_deferred`: (Gauge) Number of request deferred.

- **GET** `/props`: Returns current server settings.

- **POST** `/infill`: Returns the completion of the given prompt.
    + This endpoint is only available if the `--infill` flag is enabled.

- **POST** `/tokenize`: Convert text to tokens.

- **POST** `/detokenize`: Convert tokens to text.

- **GET** `/slots`: Returns the current slots processing state.
    + This endpoint is only available if the `--no-slots` flag is no provided.

- **POST** `/slots/:id_slot?action={save|restore|erase}`: Operate specific slot via ID.
    + This endpoint is only available if the `--no-slots` flag is no provided and `--slot-save-path` is provided.

- **POST** `/completion`: Returns the completion of the given prompt.

- **GET** `/v1/models`: (OpenAI-compatible) Returns the list of available models,
  see https://platform.openai.com/docs/api-reference/models/list.

- **POST** `/v1/completions`: (OpenAI-compatible) Returns the completion of the given prompt,
  see https://platform.openai.com/docs/api-reference/completions/create.

- **POST** `/v1/chat/completions` (OpenAI-compatible) Returns the completion of the given prompt,
  see https://platform.openai.com/docs/api-reference/chat/create.
    + This endpoint is compatible with [OpenAI Chat Vision API](https://platform.openai.com/docs/guides/vision) when
      enabled `--mmproj` flag,
      see https://huggingface.co/xtuner/llava-phi-3-mini-gguf/tree/main. (Note: do not support link `url`, use base64
      encoded image
      instead.)

- **POST** `/v1/embeddings`: (OpenAI-compatible) Returns the embeddings of the given prompt,
  see https://platform.openai.com/docs/api-reference/embeddings/create.
    + This endpoint is only available if the `--embeddings` flag is enabled.

## Tools

It was so hard to find a Chat UI that was directly compatible with OpenAI, that mean, no installation required (I can live
with `docker run`), no tokens (or optional), no [Ollama](https://github.com/ollama/ollama) required, just a simple
RESTful API.

So we are inspired by
the [llama.cpp/chat.sh](https://github.com/ggerganov/llama.cpp/blob/e6f291d15844398f8326940fe5ad7f2e02b5aa56/examples/server/chat.sh)
and adjust it to interact with LLaMA Box.

All you need is a Bash shell and curl.

- **completion.sh**: A simple script to interact with the `/completion` endpoint.
- **chat.sh**: A simple script to interact with the `/v1/chat/completions` endpoint.

Both `completion.sh` and `chat.sh` are used for talking with the LLaMA Box,
but `completion.sh` embeds a fixed pattern to format the given prompt format, 
while `chat.sh` can leverage the chat template from the model's metadata or user defined.

```shell
$ # one-shot completion
$ N_PREDICT=4096 TOP_K=1 ./llama-box/tools/completion.sh "// Quick-sort implementation in C (4 spaces indentation + detailed comments) and sample usage:\n\n#include"

$ # interactive completion
$ N_PREDICT=4096 ./llama-box/tools/completion.sh

$ # one-shot chat
$ MAX_TOKENS=4096 ./llama-box/tools/chat.sh "Tell me a joke."

$ # interactive chat
$ MAX_TOKENS=4096 ./llama-box/tools/chat.sh
```

## License

MIT
