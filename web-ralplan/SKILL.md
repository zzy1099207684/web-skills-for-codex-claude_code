---
name: web-ralplan
description: Web-focused consensus planning skill built on top of ralplan. Use when Codex needs a plan and test specification for a web task, and browser-visible behavior must be planned explicitly instead of leaving page validation to execution-time guesswork.
---

# Web Ralplan

Use this skill when a web task needs the full `ralplan` planning loop, but the resulting plan must explicitly cover browser validation, page flows, and web-specific acceptance criteria.

`$web-ralplan` does not replace `ralplan`. It is a web-focused extension of `ralplan` that forces browser-facing verification into the plan and test spec before execution begins.

## What This Adds

Normal `ralplan` already produces a plan and test-spec artifacts.

`$web-ralplan` adds one hard planning rule for web work:

- if the task affects what users see or do in the browser, the plan must include browser-side validation

That means the final plan must not stop at unit tests, lint, build, and type checks. It must also define the browser flows that prove the task is actually done.

## Use This Skill

Use this skill when:

- the task changes UI, forms, routing, auth screens, dashboards, page state, or responsive layout
- the implementation will later be executed with `ralph`, `web-ralph`, or `team`
- browser validation should be written into the plan before coding starts
- the user wants test planning for both code-side and browser-side checks

Do not use this skill when:

- the task is backend-only
- browser behavior is irrelevant to acceptance
- a complete web-aware PRD and test spec already exist

## Planning Rule

For browser-facing work, the final plan and test spec must include:

- the target page or route
- the critical user flow to validate
- the expected visible result
- the repo-side checks to run
- the browser-side checks to run
- the failure conditions that block completion

Failure conditions must explicitly include:

- failed API requests relevant to the flow
- failed local proxy requests relevant to the flow
- related browser console errors
- related browser runtime errors

Do not accept a web plan that only says "run tests" without defining the browser proof.

## Required Outputs

The final `ralplan` output must still include its normal planning artifacts, but for web tasks it must also make the following explicit inside the plan and test spec:

- browser validation tool choice
- planned browser flow coverage
- responsive or device coverage when relevant
- browser-visible acceptance criteria
- what `web-ralph` or `ralph` should execute later

## Browser Validation Planning

Default browser tool:

- use `playwright-cli` unless the task or repo has a stronger existing standard

For each relevant flow, plan:

- start URL
- step-by-step interaction path
- expected visible state
- expected request or navigation outcome
- extra checks for console, runtime, API, and proxy failures

Read [browser-test-plan.md](references/browser-test-plan.md) when the plan needs a stronger browser test-spec shape.

## What The Final Plan Must Say

A good final plan for a web task should answer:

- what code changes will be made
- what tests will be run
- what browser flow proves success
- what would count as a browser-side failure
- whether mobile or responsive validation is required
- whether execution should go to `ralph`, `web-ralph`, or `team`

## Handoff Rule

When the approved plan is for browser-facing work:

- prefer `web-ralph` for sequential execution with browser proof
- use plain `ralph` only if the approved plan already embeds browser validation clearly and the executor will honor it
- use `team` when the work is multi-lane, but keep browser verification in the final verification lane

## Output Standard

The final plan should be concise, but the web-specific testing section must be concrete.

Do not allow vague lines such as:

- "test the UI"
- "verify the page manually"
- "check the browser if needed"

Replace them with explicit browser flows and blocking conditions.
