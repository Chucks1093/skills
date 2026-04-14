# Reply AI Agent Challenge Rules

## Team And Timing

- Team size: 2 to 4 people
- Registration closes: April 15, 23:59 CEST
- Challenge date: April 16
- Starts: 15:30 CEST
- Leaderboard freeze: 21:00 CEST
- Ends: 21:30 CEST

## Required Tooling

- You may use any programming language and development environment.
- You must use the challenge-provided LLM API access.
- You must integrate Langfuse.

## LLM Role Requirement

- The LLM must be the core decision-making and orchestration layer.
- Acceptable:
  - LLM orchestrates the system
  - LLM decides which tools to call and when
  - LLM interprets outputs and adapts behavior
  - deterministic tools exist, but the LLM manages them
- Not acceptable:
  - mostly deterministic system with minimal LLM use
  - LLM used only for formatting or trivial postprocessing

## Submission Structure

- Training datasets:
  - unlimited submissions
  - submit output files
- Evaluation datasets:
  - one submission only
  - submit output files plus source code zip
- Include Langfuse session ID in submissions.

## What A Submission Must Include

- Langfuse session ID
- Output file
- Source code zip for evaluation datasets
- Complete runnable system with dependencies, config, and instructions

## What Invalidates A Submission

- Missing output files
- Missing source code on evaluation submission
- Missing Langfuse session ID
- Corrupted zip file
- Incomplete system packaging

## Scoring Priorities

- Detection quality
- Economic accuracy
- Cost efficiency
- Latency
- Agent architecture quality
- Harder datasets can carry higher weight

## Token Budget

- Datasets 1 to 3: $40
- Datasets 4 to 5: $120 unlocked after submitting evaluation solutions for datasets 1 to 3
- Budget exhaustion cannot be refilled

## Langfuse

- Langfuse is required for tracking and validation.
- Score is based on submitted outputs, not Langfuse traces.
- Use `LANGFUSE_PUBLIC_KEY`, `LANGFUSE_SECRET_KEY`, `LANGFUSE_HOST`.
- Use `@observe()` and `CallbackHandler()` per the tutorial guidance.
- Generate a unique session ID for each run and keep all LLM calls for that run under the same session.

## Practical Guidance

- Keep the output path and format deterministic.
- Keep the runtime simple enough that reviewers can execute it.
- Favor architectures that are clearly agentic and still cost-aware.
- Use training submissions to improve accuracy before spending evaluation attempts.
