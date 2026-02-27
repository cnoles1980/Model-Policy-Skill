# OpenClaw Model Policy

This workspace uses a tiered model policy to balance quality, speed, reliability, and cost.

## Goals

- Reserve premium models for work where quality differences materially change outcomes.
- Use strong mid-tier models for most agentic synthesis and multi-step tasks.
- Use the cheapest capable model for procedural, deterministic, or monitoring work.
- Keep a fallback path available when a preferred provider is degraded or rate-limited.

## Current Tiers

### Tier 1: Premium judgment and voice

Use `openai/gpt-5.2` for tasks that depend on:

- voice fidelity
- nuanced prioritization
- editorial or strategic judgment
- high-stakes final QA
- complex multi-step writing where quality is visible to humans

Typical examples:

- final publish-ready outputs
- voice-sensitive content generation
- judgment-heavy review and refinement
- complex planning artifacts for external use

### Tier 2: General agent work and structured synthesis

Use `inception/mercury-2` for tasks that depend on:

- strong synthesis across multiple inputs
- structured drafting
- research aggregation
- intermediate artifacts
- repeated agent workflows with clear output constraints

Typical examples:

- scouts
- canonical document assembly
- evidence gathering and draft creation
- workflow coordination that needs reasoning but not premium prose

### Tier 3: Procedural checks, monitoring, and lightweight summarization

Use `openai/gpt-5-nano` for tasks that are:

- deterministic
- narrow in scope
- classification-oriented
- health/status focused
- short summarization or extraction tasks

Typical examples:

- health checks
- cron/error monitors
- observers that summarize system state
- lightweight upload or post-processing steps

## Selection Rules

When assigning a model to a new job, work through these questions in order:

1. Is the output user-facing, voice-sensitive, or high-stakes?
   If yes, start with `gpt-5.2`.

2. Is the job multi-step and reasoning-heavy, but mostly structured and internally consumed?
   If yes, start with `mercury-2`.

3. Is the job mostly checking, classifying, summarizing, extracting, or moving data through a known workflow?
   If yes, start with `gpt-5-nano`.

4. If unsure between two tiers, start one tier lower and test.
   Move up only if quality, reliability, or workflow completion suffers.

## Escalation Triggers

Move a job up one tier when you observe any of the following:

- repeated hallucinated structure or missed instructions
- weak prioritization or poor judgment
- low-quality final writing
- failure to follow voice or style constraints
- incomplete handling of edge cases
- more human cleanup than the savings justify

## Downgrade Triggers

Move a job down one tier when:

- the task is stable and repetitive
- outputs are mostly machine-consumed
- the job rarely needs nuanced reasoning
- success is easy to validate with simple pass/fail checks
- latency or cost matters more than prose quality

## Fallback Policy

- Primary default remains `openai/gpt-5.2`.
- Emergency fallback remains `openai/gpt-5.3-codex`.
- Fallbacks are for continuity, not first-choice routing.

## Operating Principle

Do not choose models by brand preference. Choose by task shape.

The right question is not "What is the smartest model?"
The right question is "What is the cheapest model that can do this job reliably with acceptable quality?"

## Validation Practice

Before trusting a lower-cost model for scheduled automation:

1. Run the job manually at least once.
2. Confirm it completes without interactive prompts.
3. Inspect the output for instruction-following and completeness.
4. Let one scheduled cycle pass before broadening the rollout.

## Current Practical Heuristic

- `gpt-5.2` for visible judgment
- `mercury-2` for agentic workhorses
- `gpt-5-nano` for operational plumbing
