# Compose Setup

This compose file is a proof of concept for selfhosted LLM/GPT with vLLM and OpenwebUI.
It sets up vLLM and OpenwebUI in docker containers and connects them.

Right now, vLLM is running on a single GPU, but it should be possible to run it on multiple GPUs with some changes to the compose file.

## vLLM models
The models can be changed in the `command` section of the vllm-api:   command:
```yaml
    command:
      - "--model"
      - "unsloth/DeepSeek-R1-Distill-Llama-70B-bnb-4bit"
      - "--quantization"
      - "bitsandbytes"
      - "--load-format"
      - "bitsandbytes"
      - "--max-model-len"
      - "4000" # 4k tokens to fit into KV
      - "--tokenizer-revision"
      - "d4a2af1d9c04255cfab2d7ddc3abe58ad8d42e9d" # unsloth/DeepSeek-R1-Distill-Llama-70B-bnb-4bit
```

This uses the 70B quantized model from [unsloth/DeepSeek-R1-Distill-Llama-70B-bnb-4bit](https://huggingface.co/unsloth/DeepSeek-R1-Distill-Llama-70B-bnb-4bit).
On the current setup, there is only space for 4k context left.

Recently, the deepseek tokenizer was updated to force the model to think and right now, the model would not output the starting `<think>` tag, see 
[this discussion on GitHub](https://github.com/vllm-project/vllm/issues/13375#issuecomment-2662592856).
To fix this, we use the old tokenizer until OpenwebUI and vllm are updated.