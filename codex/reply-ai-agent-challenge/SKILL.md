---
name: reply-ai-agent-challenge
description: Use when working on the Reply AI Agent Challenge and you need challenge-specific constraints, architecture guidance, submission checks, Langfuse compliance, token-budget decisions, or scoring-aware tradeoffs.
---

# Reply AI Agent Challenge

## Overview

Use this skill when designing, reviewing, or packaging a solution for the Reply AI Agent Challenge. It keeps work aligned with challenge rules: LLM-centered agentic behavior, Langfuse session tracking, token-budget strategy, and submission validity.

## When To Use

- Building or refactoring the challenge solution architecture
- Deciding whether a design is compliant with the challenge's "agentic system" requirement
- Reviewing whether the LLM plays a central orchestration role
- Preparing training or evaluation submissions
- Checking Langfuse/session ID requirements
- Making cost-vs-latency-vs-quality tradeoffs under the token budget

## Core Rules

- The LLM must be the core reasoning and orchestration layer.
- Deterministic tools and heuristics are allowed, but they must be directed by the LLM rather than replacing it.
- Langfuse is mandatory for tracking and validation; include the Langfuse session ID in submissions.
- Scores are based on submitted output files, not Langfuse traces.
- Evaluation submissions must include both output files and source code.
- The platform scores output files; organizers must still be able to run the provided source code.

## Architecture Guidance

Prefer architectures where:
- the LLM decides what specialist/tool to call
- deterministic logic is exposed as tools or structured helper functions
- output generation is exact and reproducible
- suspicious cases get more reasoning than obviously clean cases only if the LLM still remains the decision driver

Treat these as likely-compliant:
- orchestrator LLM + deterministic investigator tools + final output writer
- multi-agent specialist setup where one LLM agent coordinates others
- LLM-driven decision layer with deterministic preprocessing/postprocessing

Treat these as risky or non-compliant:
- mostly rule-based pipeline with superficial LLM usage
- LLM used only to rewrite, format, or justify a deterministic answer
- architecture where deterministic thresholds fully replace the LLM's decision role

## Working Workflow

1. Read the problem statement and identify required output format.
2. Design the smallest LLM-centered architecture that still uses tools effectively.
3. Verify Langfuse integration and session ID generation before serious runs.
4. Optimize on training datasets with token discipline.
5. Before evaluation submission, validate:
   - output file exists and matches format
   - source code archive is complete
   - dependencies/config/instructions are present
   - Langfuse session ID is included

## Decision Heuristics

- When accuracy, fraud-value recovery, and latency conflict, prefer designs that improve detection quality without exploding cost.
- Avoid unnecessary multi-agent complexity if one orchestrator + a few tools achieves the same result.
- Use training submissions aggressively, but protect token budget.
- Keep packaging simple: one command to run, explicit env vars, deterministic output path.

## References

- For challenge rules and submission details, read `references/challenge-rules.md`.
- For exact model/tutorial details, separately inspect the current Reply tutorial pages and whitelist pages if needed.
