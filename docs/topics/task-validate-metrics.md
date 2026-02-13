---
$schema: urn:oasis:names:tc:dita:xsd:task.xsd
id: validating-benefits-with-key-metrics
author: vLLM Documentation Team
category: AI Inference
keyword:
  - benchmarking
  - metrics
  - vllm
  - performance
workflow: review
---

# Validating Red Hat AI Inference Server benefits using key metrics

Run a benchmark test that shows how vLLM, and other inference servers, perform according to key processing metrics.

You can add another paragraph here, this is the context. Can you add a prereq?

* One
* Two

1. On your host system, start a vLLM container and serve a model.

      ```bash
      $ podman run --rm -it --device nvidia.com/gpu=all \
      --shm-size=4GB -p 8000:8000 \
      --env "HUGGING_FACE_HUB_TOKEN=$HF_TOKEN" \
      --env "HF_HUB_OFFLINE=0" \
      -v ./rhaiis-cache:/opt/app-root/src/.cache \
      --security-opt=label=disable \
      registry.redhat.io/rhaiis/vllm-cuda-rhel9:1.0 \
      --model RedHatAI/Llama-3.2-1B-Instruct-FP8
      ```

2. In a separate terminal tab, install the benchmark tool dependencies.

      ```bash
      $ pip install vllm pandas datasets
      ```

3. Clone the [vLLM Git repository](https://github.com/vllm-project/vllm):

      ```bash
      $ git clone https://github.com/vllm-project/vllm.git
      ```

4. Run the `./vllm/benchmarks/benchmark_serving.py` script.

      ```bash
      $ python vllm/benchmarks/benchmark_serving.py --backend vllm --model RedHatAI/Llama-3.2-1B-Instruct-FP8 --num-prompts 100 --dataset-name random  --random-input 1024 --random-output 512 --port 8000
      ```
