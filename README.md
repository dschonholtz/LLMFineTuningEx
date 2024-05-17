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
2. Pick an LLM that is balances my own poverty with getting a model that actually does something useful. This will likely be a llama 3 8B model, but we might pop it up all the way up to 70B or down to a phi 3B model
    - Using [TorchTune](https://github.com/pytorch/torchtune) 
4.  Spin up infrastructure and do robust system design on this dataset so that we are set up to feed it to an LLM.
5.  Set up the LLM with weights and biases and do finetuning with LoRA

Some things I want to learn more about.
1. Can we do online learning continually in any meaningful way as we change our dataset and/or get more data?
2. Can we just generate synthetic data to do this? Make a sql query dataset from GPT-4o for instance
3. Do we need to or want to quantize the models? What are the real trade offs here?
