---
$schema: urn:oasis:names:tc:dita:xsd:concept.xsd
id: validating-metrics
author: vLLM Documentation Team
category: AI Inference
keyword:
  - metrics
  - benchmarking
  - vllm
  - performance
  - validation
workflow: review
---

# Validating Red Hat AI Inference Server metrics

Benchmarking your Red Hat AI Inference Server deployment helps you understand how the server performs under realistic workloads and identify opportunities to optimize throughput, latency, and resource utilization.

## Why validate metrics

Running benchmark tests against your deployment provides measurable data points that you can use to:

* Confirm that your hardware and configuration deliver the expected performance
* Compare results across different model sizes, quantization levels, and server arguments
* Identify bottlenecks in GPU memory, network throughput, or batch scheduling
* Establish a performance baseline before and after configuration changes

## Key metrics

The vLLM benchmarking tools report several metrics that are critical for evaluating inference server performance:

* **Time to First Token (TTFT)**: The latency from sending a request to receiving the first output token. Lower values indicate a more responsive server.
* **Inter-Token Latency (ITL)**: The average time between consecutive output tokens. This metric reflects the streaming experience for end users.
* **Throughput (tokens/s)**: The total number of tokens generated per second across all concurrent requests. Higher throughput means the server can handle more work.
* **Request latency**: The end-to-end time to complete a single inference request, including queuing, prefill, and decode phases.
