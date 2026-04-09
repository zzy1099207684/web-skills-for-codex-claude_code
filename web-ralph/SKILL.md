---
name: web-ralph
description: Ralph-style completion loop for web work with mandatory browser verification. Use when Codex needs the full Ralph persistence and verification flow, but the task touches a web UI, browser interaction, routing, forms, responsive behavior, or any page state that must be proved in a real browser with playwright-cli before the task can be closed.
---

[WEB-RALPH]

<Purpose>
Web Ralph combines the `ralph` workflow from `oh-my-codex` with browser validation through `playwright-cli`. Keep the full Ralph completion loop, architect verification, deslop pass, and regression re-verification, but require `playwright-cli` validation whenever the task affects what a user sees or does in the browser.
</Purpose>

<Origin>
This skill is built by combining the `ralph` workflow from `oh-my-codex` with browser validation through `playwright-cli`.
</Origin>

<Prerequisites>
- `playwright-cli` must be available in the environment
- The target app must be runnable locally or reachable at a known URL
- The validated flow must be specific enough to reproduce in a browser

If `playwright-cli` is unavailable, stop and report the blocker instead of pretending the browser step was covered.
</Prerequisites>

<Use_When>
- The task is already a Ralph-style "finish it fully" job
- The change touches UI, page state, routing, forms, auth screens, dashboards, or responsive layout
- A code-only pass is insufficient because the browser must prove the result
- The user expects the loop to keep fixing issues until browser behavior and repo checks both pass
</Use_When>

<Do_Not_Use_When>
- The task is backend-only and has no browser-facing impact
- Unit, integration, or API checks are sufficient proof with no page behavior involved
- The task is pure planning, exploration, or architecture work
</Do_Not_Use_When>

<How_This_Relates_To_Ralph>
- Inherit Ralph's persistence loop and finish-to-done posture
- Inherit Ralph's fresh verification requirement
- Inherit Ralph's architect verification step
- Inherit Ralph's deslop pass and post-deslop regression re-verification
- Add one Web-specific rule: browser verification is part of completion, not an optional extra
</How_This_Relates_To_Ralph>

<Browser_Verification_Gate>
Treat browser verification as mandatory when the task changes or could affect:

- rendered UI
- click paths or navigation
- forms and validation states
- async page state visible to users
- mobile or responsive layout
- console/runtime behavior in the browser

For those tasks, do not declare completion based only on `test`, `build`, `lint`, or diagnostics. A fresh browser pass must also succeed.
</Browser_Verification_Gate>

<Browser_Failure_Rules>
Treat the browser verification as failed when any of the following happens during the validated flow:

- backend API requests fail
- local dev proxy requests fail
- browser console shows related errors
- browser runtime throws related errors

Do not treat "the page opened" or "the button was clickable" as a passing result when the browser is still reporting these failures underneath.
</Browser_Failure_Rules>

<Inputs_To_Establish_Early>
Before the main execution loop, determine:

- local app start command
- target local URL
- key user flow to validate
- required credentials, seed data, or feature flags
- desktop-only or mobile-inclusive verification scope
- final proof commands: tests, build, lint, diagnostics, and browser flow

Infer these from the repo first. Ask the user only when the missing value cannot be discovered safely.
</Inputs_To_Establish_Early>

<Public_Intent>
This skill is intentionally generic. Do not assume a specific framework, bundler, port, auth model, or repo layout. Discover those from the current project before starting the browser loop.
</Public_Intent>

<Execution_Policy>
- Follow Ralph's persistence model: keep working until the task is actually complete
- Reproduce browser-visible bugs before editing when the failure mode is unclear
- Use `playwright-cli` as the default browser verification tool
- Keep browser verification narrow and task-focused; do not turn every task into a giant E2E suite
- If browser verification fails, return to implementation and continue the loop
- If repo checks fail, fix them before rerunning browser verification
</Execution_Policy>

<Steps>
0. **Start from Ralph grounding**:
   - Reuse Ralph-style context gathering, task grounding, and persistence mindset
   - Determine whether the task has browser-facing impact; if yes, enable the browser verification gate immediately

1. **Establish runnable web context**:
   - Find the app start command and expected local URL
   - Start or reuse the local app
   - Confirm the page loads before deeper validation
   - If the app depends on a backend or proxy, include that dependency in the validation context

2. **Capture a browser baseline**:
   - Open the target page with `playwright-cli`
   - Use `snapshot` as the default inspection surface
   - Use `console` and `network` when the issue may be runtime or request-related
   - Record the failing path before editing whenever possible

3. **Implement the fix or change**:
   - Make the smallest defensible code change
   - Reuse existing utilities, components, and patterns
   - Add or update regression tests when behavior is not already protected

4. **Run repo-side verification**:
   - Run the narrowest relevant checks first
   - Run the required test, build, lint, and diagnostic commands for the change
   - Read the outputs and confirm they actually passed

5. **Run browser verification**:
   - Re-open or refresh the target page with `playwright-cli`
   - Execute the exact user flow affected by the change
   - Verify the expected visible result
   - Check for related console/runtime issues before treating the browser pass as clean
   - Treat failed API calls and failed local proxy calls as browser verification failures
   - Record enough evidence to explain why the flow passed or failed

6. **Loop on failure**:
   - If browser verification fails, continue fixing
   - If repo checks fail, continue fixing
   - Do not exit on "looks good" or "should work"

7. **Finish with Ralph-level closing checks**:
   - Keep architect verification
   - Keep the mandatory deslop pass unless explicitly skipped by the parent Ralph policy
   - Keep post-deslop regression re-verification
   - Re-run browser verification after meaningful post-deslop UI-affecting changes
</Steps>

<Browser_Rules>
- Prefer `playwright-cli snapshot` after each meaningful interaction
- Target elements by snapshot refs first
- For form work, verify both the happy path and any relevant failure state
- For routing work, verify both URL change and a visible landmark on the destination view
- For responsive work, resize to the target mobile viewport and re-check the affected flow
- For visual defects, take before/after screenshots only when the snapshot is not enough evidence
- For backend-backed pages, treat broken requests as part of the browser result, not as a separate unrelated issue
</Browser_Rules>

<Typical_Flow>
```bash
playwright-cli open http://localhost:3000
playwright-cli snapshot
playwright-cli click e12
playwright-cli fill e18 "demo@example.com"
playwright-cli press Enter
playwright-cli snapshot
playwright-cli console
playwright-cli close
```
</Typical_Flow>

<Completion_Standard>
Do not mark the task complete until all relevant Ralph checks pass and, for browser-facing work, all of the following are also true:

- the target browser flow passes on a fresh run
- the expected visible state is present
- there are no unresolved browser console/runtime errors related to the change
- there are no related failed API or proxy requests during the validated flow
- repo-side verification still passes

Browser verification is part of completion, not a nice-to-have.
</Completion_Standard>

<Output_Style>
Report:

- what changed
- what repo checks were run and passed
- what browser flow was run
- what the browser showed after the fix
- any remaining blocker if the loop is still open
</Output_Style>
