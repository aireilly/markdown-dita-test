---
$schema: urn:oasis:names:tc:dita:xsd:reference.xsd
id: key-server-arguments
author: vLLM Documentation Team
category: AI Inference
keyword:
  - server-arguments
  - configuration
  - vllm
  - reference
workflow: review
---

# Key vLLM server arguments

There are 4 key arguments that you use to configure vLLM to run on your hardware:

| Argument | Description | Default |
| -------- | ----------- | ------- |
| `--tensor-parallel-size` | Distributes your model across your host GPUs. | `1` |
| `--gpu-memory-utilization` | Adjusts accelerator memory utilization for model weights, activations, and KV cache. Measured as a fraction from 0.0 to 1.0. For example, you can set this value to 0.8 to limit GPU memory consumption by vLLM to 80%. Use the largest value that is stable for your deployment to maximize throughput. | `0.9` |
| `--max-model-len` | Limits the maximum context length of the model, measured in tokens. Set this to prevent problems with memory if the model's default context length is too long. | Model default |
| `--max-num-batched-tokens` | Limits the maximum batch size of tokens to process per step, measured in tokens. Increasing this improves throughput but can affect output token latency. | Model default |

For example, to run the Red Hat AI Inference Server container and serve a model with vLLM, run the following, changing server arguments as required:

```bash
$ podman run --rm -it \
--device nvidia.com/gpu=all \
--security-opt=label=disable \
--shm-size=4GB -p 8000:8000 \
--userns=keep-id:uid=1001 \
--env "HUGGING_FACE_HUB_TOKEN=$HF_TOKEN" \
--env "HF_HUB_OFFLINE=0" \
-v ./rhaiis-cache:/opt/app-root/src/.cache \
registry.redhat.io/rhaiis/vllm-cuda-rhel9:1.0 \
--model RedHatAI/Llama-3.2-1B-Instruct-FP8 \
--tensor-parallel-size 2 \
--gpu-memory-utilization 0.8 \
--max-model-len 16384 \
--max-num-batched-tokens 2048
```

**Additional resources**

* [Getting started with Red Hat AI Inference Server](https://docs.redhat.com/en/documentation/red_hat_ai_inference_server/latest/html-single/getting_started/index)
