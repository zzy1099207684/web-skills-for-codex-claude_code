# Browser Checklist

Use this checklist when browser verification needs more structure than a quick smoke test.

## Flow Checks

- Confirm the page finished loading before interacting.
- Verify the main landmark or heading after each route change.
- Check the visible success state after submitting a form.
- Check the visible error state when the task touches validation or failure handling.

## Responsive Checks

- Validate the default desktop viewport if the task changed layout.
- Resize to a mobile viewport if the product is used on phones.
- Re-check navigation, form controls, and any sticky or fixed UI after resizing.

## Debug Checks

- Inspect `playwright-cli console` after reproducing the issue.
- Inspect `playwright-cli network` when the bug may be data or request related.
- Use `playwright-cli eval` only when the snapshot does not expose the needed detail.

## Evidence To Report

- Target URL and key steps run
- Browser-visible result
- Any console or network anomalies
- Associated repo checks that passed
