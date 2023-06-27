# Prefect

## What is Prefect?

Prefect orchestrates workflows — it simplifies the creation, scheduling, and monitoring of complex data pipelines. With Prefect, you define workflows as Python code and let it handle the rest.

Prefect also provides error handling, retry mechanisms, and a user-friendly dashboard for monitoring. It's the easiest way to transform any Python function into a unit of work that can be observed and orchestrated.

## What is a Flow?  

Flows are like functions. They can take inputs, perform work, and return an output. In fact, you can turn any function into a Prefect flow by adding the @flow decorator. When a function becomes a flow, its behavior changes, giving it the following advantages:
 * It has a state, which determines when it can run. Transitions between states are recorded, allowing for flow execution to be observed.
 * Input arguments types can be validated.
 * Retries can be performed on failure.
 * Timeouts can be enforced to prevent unintentional, long-running workflows.
 * Metadata about flow runs, such as run time and final state, is tracked.

## What is a task?

A task is a Python function decorated with a @task decorator. Tasks are atomic pieces of work that are executed independently within a flow. Tasks, and the dependencies between them, are displayed in the flow run graph, enabling you to break down a complex flow into something you can observe and understand. When a function becomes a task, it can be executed concurrently and its return value can be cached.

Flows and tasks share some common features:
 * Both have metadata attributes such as name, description, and tags.
 * Both support type-checked parameters, allowing you to define the expected data types of inputs and outputs.
 * Both provide functionality for retries, timeouts, and other hooks to handle failure and completion events.
 * Network calls (such as our GET requests to the GitHub API) are particularly useful as tasks because they take advantage of task features such as retries, caching, and concurrency.

## Subflows

Not only can you call tasks within a flow, but you can also call other flows! Child flows are called subflows and allow you to efficiently manage, track, and version common multi-task logic.

Subflows are a great way to organize your workflows and offer more visibility within the UI.

## Deploy

One of the most common reasons to use a tool like Prefect is scheduling. You want your flows running on production infrastructure in a consistent and predictable way. Up to this point, we’ve demonstrated running Prefect flows as scripts, but this means you have been the one triggering flow runs. In order to schedule flow runs or trigger them based on events you’ll need to deploy them.

A deployed flow gets the following additional cababilities:
 * flows triggered by scheduling
 * remote execution of flows triggered from the UI
 * flow triggered by automations or events

## What is a deployment?

Deploying your flows is, in essence, the act of informing Prefect of:

 * Where to run your flows
 * How to run your flows
 * When to run your flows
  
This information is encapsulated and sent to Prefect as a Deployment which contains the crucial metadata needed for orchestration. Deployments elevate workflows from functions that you call manually to API-managed entities.

Attributes of a deployment include (but are not limited to):

 * Flow entrypoint: path to your flow function would start the flow
 * Work pool: points to the infrastructure you want your flow to run in
 * Schedule: optional schedule for this deployment
 * Tags: optional metadata

## Why Workpools and Workers?

Running Prefect flows locally is great for testing and development purposes. But for production settings, Prefect work pools and workers allow you to run flows in the environments best suited to their execution.

Workers and work pools bridge the Prefect orchestration layer with the infrastructure the flows are actually executed on.

You can configure work pools within the Prefect UI. They prioritize the flows and respond to polling from the worker. Workers are light-weight processes that run in the environment where flows are executed.

Work Pools:
 * Organize the flows for your worker to pick up and execute.
 * Describe the ephemeral infrastructure configuration that the worker will create for each flow run.
 * Prioritize the flows and respond to polling from its worker(s).
  
There are work pool types for all major managed code execution platforms, such as Kubernetes services or serverless computing environments such as AWS ECS, Azure Container Instances, or GCP Cloud Run.


