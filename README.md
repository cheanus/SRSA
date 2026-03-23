# SRSA: The Spaced Repetition Systems for Agents 

**Teach your AI agent to remember — and improve its memory over time.**

SRSA (Spaced Repetition Systems for AI Agents) is a **memory self-improvement layer** that converts agent memories into reviewable cards and uses spaced repetition to continuously refine them.

Unlike traditional memory systems that only store and retrieve information, SRSA enables agents to:

- problem oriented enhancement of memory
- detect missing knowledge
- correct incorrect memories
- remove misleading memories

## Why SRSA

Most AI agent memory systems suffer from **memory drift** over time.

Common problems include:

- Retrieval mismatch: Memories exist but cannot be activated in the right context.

- Memory interference: Conflicting memories reduce recall accuracy.

- Memory degradation: Memories become outdated, incomplete, or inaccurate.

These issues resemble findings in cognitive science such as the **forgetting curve** and retrieval failure.

Current approaches mainly focus on:

**Pre-training approaches**
Designing better memory structures.

**Post-processing approaches**
Summarizing or reorganizing memory.

However, they do not **directly and effectively improve memory recall reliability** during agent operation.

SRSA introduces **spaced retrieval + memory self-correction** to address this.

## Core Idea

SRSA creates a continuous memory improvement process:

```
Card Generation → Scheduled Review → Self Evaluation → Memory Update
```

During review, the agent:

1. recalls knowledge
2. compares with ground truth
3. evaluates correctness
4. reflects on the mistake
5. modifies memory

This enables continuous memory evolution.

Goal:

> Next time: answer correctly and faster.

## Architecture

SRSA acts as a layer on top of any agent memory system.

```
Agent
  ↓
Memory System
  ↓
SRSA Skill
  - Card Generator
  - FSRS Scheduler
  - Review Loop
  - Memory Self-Correction
  ↓
Improved Memory
```

Key design principle:

- **Memory is dynamic:**
Agent knowledge should evolve.

- **Recall is the real bottleneck:**
Storage alone does not solve memory problems.

- **Review enables learning:**
Periodic retrieval strengthens memory pathways.

## Features

- 🎬Scenario-based recall improvement: Custom cards help solve retrieval failures.

- 🔓Decoupled from memory systems: Works with **any** agent memory architecture.

- 🚀Efficient scheduling: Uses the **FSRS algorithm** to minimize unnecessary reviews and token usage.

- 🔧Memory self-repair: Agents update memory after failed recall.

- 📈Review analytics: Tracks recall performance and memory evolution over time.

## Quick Start

It is easy to integrate SRSA into your agent workflow using **skills**.

Clone the repository under `skills/` directory:

```bash
git clone https://github.com/cheanus/SRSA
cd SRSA
```

Install dependencies:

```bash
uv sync
```

Add a new card:

```bash
uv run python scripts/main.py card new \
-q "What is the vector database used in this agent?" \
-a "Weaviate"
```

Finally, ask the agent to read the skill and run review sessions daily or on demand.

## Project Structure

```
scripts/
    main.py
    config.yaml
    cache/
        fsrs.sqlite
doc/
    script.md
SKILL.md
README.md
```

## Configuration

Optional configuration: `scripts/config.yaml`

You can refer to [FSRS documentation](https://open-spaced-repetition.github.io/py-fsrs/fsrs.html) for details.

## Usage

Refer to [script.md](docs/script.md) for command usage.

## Note

If daily reviews become large, recommended solutions are:

- compress conversation context
- split reviews across multiple agents
- run multiple review sessions per day

## Example Review Session

```
Q: What types of news does the user like?

Agent Answer:
Tecknology and science news.

Correct Answer:
Tecknology, science and international news.

Reflection:
Related memories are incomplete and need to be updated to include international news.

Action:
Update memory with the new information about international news.
```

## Credits

- [Free Spaced Repetition Scheduling Algorithm](https://github.com/open-spaced-repetition/free-spaced-repetition-scheduler)

- [FSRS python package](https://open-spaced-repetition.github.io/py-fsrs/fsrs.html)

- [Spaced Repetition Research](https://supermemo.guru/wiki/Spaced_repetition)

- [Memory-R1](https://arxiv.org/abs/2508.19828)

- [Linux Do](https://linux.do/)

## License

MIT License
