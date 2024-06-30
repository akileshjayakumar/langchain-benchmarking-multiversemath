# LangChain Benchmarking: Multiverse Math

This repository contains a notebook demonstrating the use of LangChain's benchmarking tools to evaluate a custom task in a controlled environment. The task, "Multiverse Math," assesses the ability of AI models to perform basic math operations in an alternate mathematical universe where results are different from expected.

## Table of Contents

- [Setup](#setup)
  - [Environment Variables](#environment-variables)
  - [Required Libraries](#required-libraries)
- [Task Details](#task-details)
  - [Import and Register Task](#import-and-register-task)
  - [Retrieve Task Information](#retrieve-task-information)
  - [Create Environment and List Tools](#create-environment-and-list-tools)
  - [Example Tool Invocation](#example-tool-invocation)
  - [Task Instructions](#task-instructions)
- [Model Setup](#model-setup)
  - [Configure Chat Model](#configure-chat-model)
  - [Run Agent](#run-agent)
- [Conclusion](#conclusion)
- [License](#license)

## Setup

### Environment Variables

Set up the required environment variables:

```python
import os

os.environ["LANGCHAIN_API_KEY"] = "your_langchain_api_key"
os.environ["OPENAI_API_KEY"] = "your_openai_api_key"
```

### Required Libraries

Install the necessary libraries:

```bash
pip install langchain langchain-benchmarks langchain-core langchain-openai pydantic
```

## Task Details

### Import and Register Task

First, import the necessary module and register the task:

```python
from langchain_benchmarks import registry
```

### Retrieve Task Information

Retrieve and display the task details, including its name, type, dataset ID, and description:

```python
task = registry["Multiverse Math"]
print(task)
```

### Create Environment and List Tools

Create the environment for the task and list the available tools:

```python
env = task.create_environment()
print(env.tools[:5])
```

### Example Tool Invocation

Invoke a tool (e.g., multiplication) within the environment to see its altered result:

```python
result = env.tools[0].invoke({"a": 2, "b": 4})
print(result)  # Expected output is an altered result, e.g., 8.8
```

### Task Instructions

Retrieve the instructions for the task:

```python
instructions = task.instructions
print(instructions)
```

## Model Setup

### Configure Chat Model

Configure the chat model and prompt template for interaction with the task:

```python
from langchain_core.prompts import ChatPromptTemplate
from langchain_openai.chat_models import ChatOpenAI
from langchain_benchmarks.tool_usage.agents import StandardAgentFactory

model = ChatOpenAI(temperature=0)
prompt = ChatPromptTemplate.from_messages([
    ("system", "{instructions}"),
    ("human", "{question}"),
    ("placeholder", "{agent_scratchpad}")
])

agent_factory = StandardAgentFactory(task, model, prompt)
```

### Run Agent

Instantiate the agent and run it with a sample question:

```python
from langchain import globals

globals.set_verbose(True)

agent = agent_factory()
response = agent.invoke({"question": "how much is 1+1"})
print(response)
```
