# PromptTools: Tools for Prompts

Welcome to prompttools! This repo offers a set of tools for testing and experimenting with prompts. The core idea is to enable developers to evaluate prompts using familiar interfaces like _code_ and _notebooks_.

## Using prompttools

There are primarily two ways you can use prompttools in your LLM workflow:

1. Run experiments in [notebooks](/examples/notebooks/).
1. Write [unit tests](/examples/prompttests/example.py).

## Notebooks

There are a few different ways to run an experiment in a notebook. 

The simplest way is to define an experimentation harness and an evaluation function:

```python
def eval_fn(prompt: str, results: Dict, metadata: Dict) -> float:
    # Your logic here
    pass

prompt_templates = [
    "Answer the following question: {{input}}", 
    "Respond the following query: {{input}}"
]

user_inputs = [
    {"input": "Who was the first president?"}, 
    {"input": "Who was the first president of India?"}
]

harness = PromptTemplateExperimentationHarness("text-davinci-003", 
                                               prompt_templates, 
                                               user_inputs)


harness.prepare()
harness.run()
harness.evaluate("metric_name", eval_fn)
harness.visualize()
```

You can also manually enter feedback to evaluate prompts, see [HumanFeedback.ipynb](/examples/notebooks/HumanFeedback.ipynb).

## Unit Tests

Unit tests in `prompttools` are called `prompttests`. They use the `@prompttest` annotation to transform an evaluation function into an efficient unit test. The prompttest framework executes and evaluates experiments so you can test prompts over time. You can see an example test [here](/examples/prompttests/example.py) and an example of that test being used as a Github Action [here](/.github/workflows/post-commit.yaml).

## Persisting Results

To persist the results of your tests and experiments, you can enable `DialecticScribe`, which logs all the inferences from your LLM, along with metadata and custom metrics, for you to view on your [dashboard](https://app.hegel-ai.com).