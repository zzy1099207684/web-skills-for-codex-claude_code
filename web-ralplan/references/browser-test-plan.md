# Browser Test Plan

Use this reference when a web task needs browser-aware planning.

## Include These Sections

- Target route or page
- User flow steps
- Expected visible state
- Repo-side verification
- Browser-side verification
- Failure conditions

## Minimum Browser Test-Spec Shape

For each important flow, write:

1. Start URL
2. Interaction steps
3. Expected visible result
4. Expected request, redirect, or route change
5. Browser failure signals

## Browser Failure Signals

Treat these as blockers in the plan:

- related API request failures
- related local proxy failures
- related browser console errors
- related runtime errors

## Responsive Cases

Add mobile or responsive coverage when the task touches:

- layout
- navigation
- sticky or fixed UI
- mobile-only controls
- forms likely to be used on phones

## Example

- Route: `/auth`
- Flow: open login page, switch to register, fill form, submit, confirm visible success state
- Repo checks: lint, targeted tests, build
- Browser checks: no related console errors, no failed auth requests, expected route or success state visible
