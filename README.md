# LLMFineTuningEx
Fine tune an LLM with scalable infra, and continually updating docs

## What this is?

I want to work seriously on LLM finetuning in the near future. 
To do that there is a lot I need to understand, and I'd like to document what I learn as I go.

There are a lot of examples in Jupyter notebooks that do not follow design patterns and are therefore a PITA to maintain or to further finetune later.

In this repo, the plan is to show simple finetuning pipelines in a jupyter notebook but then to show the process of taking that and deploying it on cloud infrastructure in a reasonable way.

## The plan

1. Find a dataset that would be good to do finetuning on.
    - Show the sizes of the dataset and show what the format the input and output should be to retain chat style output from the associated LLM.
    - Make sure this is a dataset that can be easily added to, searched and modified
    - Dataset list here: https://github.com/Zjh-819/LLMDataHub
        - Arxiv CS questions: https://huggingface.co/datasets/ArtifactAI/arxiv-beir-cs-ml-generated-queries
            - This is particularly good, because it would be easy to further query arxiv and do further finetuning as more information came in.
        - Phi-1 from textbooks are all you need also looks good: https://huggingface.co/datasets/teleprint-me/phi-1
        - SQL Query Gen: I really like the idea of building a model that given a schema and question instantly generates a query
2. Pick an LLM that is balances my own poverty with getting a model that actually does something useful. This will likely be a llama 3 8B model, but we might pop it up all the way up to 70B or down to a phi 3B model
    - Using [TorchTune](https://github.com/pytorch/torchtune) 
4.  Spin up infrastructure and do robust system design on this dataset so that we are set up to feed it to an LLM.
5.  Set up the LLM with weights and biases and do finetuning with LoRA
6.  Scale the model so we can hit it with as many requests as needed
    - Serve with [VLLM](https://github.com/vllm-project/vllm)
        - [VLLM Docs](https://docs.vllm.ai/en/latest/) VLLM looks so much more friendly and it isn't even close
    - or [TensorRT](https://github.com/NVIDIA/TensorRT-LLM) It seems like the more hardcore and valuable option is TensorRT per this [blog](https://towardsdatascience.com/deploying-llms-into-production-using-tensorrt-llm-ed36e620dac4).
    - It is interesting to me that both TensorRT and VLLM save/push/pull your model to and from huggingface. It seems this is very locked in for VLLM, and if you want to own and not share your weights at all, you can manage them directly with TensorRT but it is more annoying.
    - [TensorRT Deploy blog](https://developer.nvidia.com/blog/tune-and-deploy-lora-llms-with-nvidia-tensorrt-llm/)
    - If I do go with TensorRT, it appears it will be significantly more complicated, and it may require deploying with something like Triton for a model inference server? I think VLLM may have this baked in?

Some things I want to learn more about.
1. Can we do online learning continually in any meaningful way as we change our dataset and/or get more data?
    - I think we can do this with multiple LoRA models and hot swap them out as we retrain
3. Can we just generate synthetic data to do this? Make a sql query dataset from GPT-4o for instance
4. Do we need to or want to quantize the models? What are the real trade offs here?
   - TensorRT appears to let us do this at compile time which may be really nice
