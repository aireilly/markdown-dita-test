---
$schema: urn:oasis:names:tc:dita:xsd:concept.xsd
id: about-tool-calling
author: vLLM Documentation Team
category: AI Inference
keyword:
  - tool-calling
  - vllm
  - inference
workflow: review
---

# About tool calling

Tool calling extends model functionality beyond text generation, allowing it to perform actions such as querying databases, calling APIs, executing calculations, or retrieving real-time information.

Red Hat AI Inference Server provides comprehensive support for tool calling through the upstream [vLLM project](https://github.com/vllm-project/vllm).
With tool calling, the language model can understand when it needs to call an external tool and select the appropriate tool based on the context.
The model generates properly formatted tool call requests and integrates the tool response back into the conversation flow.

## More details

Tool calling provides several advantages for building intelligent applications with Red Hat AI Inference Server:

* Provides access to real-time information beyond the model training data
* Enables the model to perform precise calculations and data processing
* Integrates with existing APIs and services
* Enhances reliability through structured function calls
* Separates reasoning and execution
