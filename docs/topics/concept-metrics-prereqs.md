---
$schema: urn:oasis:names:tc:dita:xsd:concept.xsd
id: metrics-prerequisites
author: vLLM Documentation Team
category: AI Inference
keyword:
  - metrics
  - prerequisites
  - benchmarking
  - vllm
workflow: review
---

# Prerequisites for metrics validation

Before you can benchmark and validate key metrics for Red Hat AI Inference Server, you must ensure that your environment meets the following requirements.

## Hardware requirements

You need the following hardware to run metrics validation:

* A host system with at least one NVIDIA GPU
* A minimum of 16 GB of GPU memory
* Sufficient shared memory (at least 4 GB) for model loading

## Software requirements

Ensure the following software is installed and configured on your host system:

* **Podman** or **Docker**: A container runtime to pull and run the vLLM serving container.
* **Python 3.9 or later**: Required to run the benchmarking scripts.
* **pip**: The Python package manager, used to install benchmark dependencies such as `vllm`, `pandas`, and `datasets`.
* **Git**: Used to clone the vLLM repository, which contains the benchmarking scripts.

## Access requirements

You must have the following credentials and access:

* A **Hugging Face** account with an API token (`HF_TOKEN`) that has read access to the model repository.
* Access to the **Red Hat container registry** (`registry.redhat.io`) to pull the vLLM container image. You can authenticate by running `podman login registry.redhat.io`.

## Network considerations

* The benchmark script communicates with the vLLM server over port `8000` by default. Ensure that this port is available and not blocked by a firewall.
* The first run downloads the model weights from Hugging Face. Subsequent runs can use a local cache directory to avoid repeated downloads.
