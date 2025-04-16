# Reinforcement learning from human feedback {.concept}

Reinforcement Learning from Human Feedback (RLHF) is a technique that fine-tunes language models by using human-generated preference data to align model outputs with the behaviors that you want.

You can use vLLM to generate the completions for RLHF. The best way to do this is with libraries like [OpenRLHF](https://github.com/huggingface/trl[TRL], link:https://github.com/OpenRLHF/OpenRLHF) and [verl](https://github.com/volcengine/verl)

**Additional resources**
If you do not want to use an existing library, see the following examples in the vLLM documentation:

* [RLHF Utils](https://docs.vllm.ai/en/latest/getting_started/examples/rlhf_utils.html)
