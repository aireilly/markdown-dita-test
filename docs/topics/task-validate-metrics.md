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

Use the following metrics to evaluate the performance of the LLM model being served with vLLM:

* **Time to first token (TTFT)**: The time from when a request is sent to when the first token of the response is received.
* **Time per output token (TPOT)**: The average time it takes to generate each token after the first one.
* **Latency**: The total time required to generate the full response.
* **Throughput**: The total number of output tokens the model can produce at the same time across all users and requests.

Complete the procedure below to run a benchmark test that shows how vLLM, and other inference servers, perform according to these metrics.

## Prerequisites

* vLLM container image
* GitHub account
* Python 3.9 or higher

## Steps

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

## Verification

The results show how vLLM performs according to key server metrics:

```text
============ Serving Benchmark Result ============
Successful requests:                    100
Benchmark duration (s):                 4.61
Total input tokens:                     102300
Total generated tokens:                 40493
Request throughput (req/s):             21.67
Output token throughput (tok/s):        8775.85
Total Token throughput (tok/s):         30946.83
---------------Time to First Token----------------
Mean TTFT (ms):                         193.61
Median TTFT (ms):                       193.82
P99 TTFT (ms):                          303.90
-----Time per Output Token (excl. 1st token)------
Mean TPOT (ms):                         9.06
Median TPOT (ms):                       8.57
P99 TPOT (ms):                          13.57
---------------Inter-token Latency----------------
Mean ITL (ms):                          8.54
Median ITL (ms):                        8.49
P99 ITL (ms):                           13.14
==================================================
```

Try changing the parameters of this benchmark and running it again. Notice how `vllm` as a backend compares to other options. Throughput should be consistently higher, while latency should be lower.

* Other options for `--backend` are: `tgi`, `lmdeploy`, `deepspeed-mii`, `openai`, and `openai-chat`
* Other options for `--dataset-name` are: `sharegpt`, `burstgpt`, `sonnet`, `random`, `hf`

## Additional resources

* [vLLM documentation](https://docs.vllm.ai/en/latest/)
* [LLM Inference Performance Engineering: Best Practices](https://www.databricks.com/blog/llm-inference-performance-engineering-best-practices), by Mosaic AI Research, which explains metrics such as throughput and latency
