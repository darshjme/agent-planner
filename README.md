<div align="center">
<img src="assets/hero.svg" width="100%"/>
</div>

# agent-planner

**Task decomposition and execution planning for AI agents**

[![PyPI version](https://img.shields.io/pypi/v/agent-planner?color=blue&style=flat-square)](https://pypi.org/project/agent-planner/) [![Python 3.10+](https://img.shields.io/badge/python-3.10%2B-blue?style=flat-square)](https://python.org) [![License: MIT](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE) [![Tests](https://img.shields.io/badge/tests-passing-brightgreen?style=flat-square)](#)

---

## The Problem

Without a planner, complex goals become a single monolithic prompt that confuses the model, produces irrelevant sub-steps, and fails silently when goals shift mid-execution. Agents that cannot decompose cannot reason.

## Installation

```bash
pip install agent-planner
```

## Quick Start

```python
from agent_planner import PlanExecutor, Plan, Planner

# Initialise
instance = PlanExecutor(name="my_agent")

# Use
result = instance.run()
print(result)
```

## API Reference

### `PlanExecutor`

```python
class PlanExecutor:
    """Executes a plan by invoking registered handlers for each step."""
    def __init__(self, plan: Plan, handlers: dict[str, Callable] | None = None) -> None:
    def run_step(self, step_id: str) -> bool:
        """Execute a single step by ID. Marks it done or failed. Returns True on success."""
    def run(self) -> dict:
        """Execute all steps in dependency order. Returns final plan summary."""
```

### `Plan`

```python
class PlanExecutor:
    """Executes a plan by invoking registered handlers for each step."""
    def __init__(self, plan: Plan, handlers: dict[str, Callable] | None = None) -> None:
    def run_step(self, step_id: str) -> bool:
        """Execute a single step by ID. Marks it done or failed. Returns True on success."""
    def run(self) -> dict:
        """Execute all steps in dependency order. Returns final plan summary."""
```

### `Planner`

```python
class Planner:
    """Creates and manages execution plans."""
    def __init__(self) -> None:
    def create(self, name: str) -> Plan:
        """Create and register an empty plan."""
    def decompose(self, task: str, steps: list[dict]) -> Plan:
        """Build a plan from a list of step dicts with keys: id, description, depends_on?.
    def linear(self, name: str, descriptions: list[str]) -> Plan:
        """Create a sequential plan where each step depends on the previous.
```


## How It Works

### Flow

```mermaid
flowchart LR
    A[User Code] -->|create| B[PlanExecutor]
    B -->|configure| C[Plan]
    C -->|execute| D{Success?}
    D -->|yes| E[Return Result]
    D -->|no| F[Error Handler]
    F --> G[Fallback / Retry]
    G --> C
```

### Sequence

```mermaid
sequenceDiagram
    participant App
    participant PlanExecutor
    participant Plan

    App->>+PlanExecutor: initialise()
    PlanExecutor->>+Plan: configure()
    Plan-->>-PlanExecutor: ready
    App->>+PlanExecutor: run(context)
    PlanExecutor->>+Plan: execute(context)
    Plan-->>-PlanExecutor: result
    PlanExecutor-->>-App: WorkflowResult
```

## Philosophy

> The Mahabharata was won not on the battlefield but in the *war council*; strategy precedes execution.

---

*Part of the [arsenal](https://github.com/darshjme/arsenal) — production stack for LLM agents.*

*Built by [Darshankumar Joshi](https://github.com/darshjme), Gujarat, India.*
