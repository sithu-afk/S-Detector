# Contributing to S_Detector

Thanks for your interest in contributing.

## Setup

1. Clone the repo
2. `cp background/config.example.js background/config.js` and add your own free Google Safe Browsing API key (see README.md)
3. Load the extension in Firefox via `about:debugging#/runtime/this-firefox` → "Load Temporary Add-on" → select `manifest.json`

## Guidelines

- Never commit `background/config.js` (it's git-ignored) — only `config.example.js` should be tracked.
- Keep detection sources modular: each source (Safe Browsing, PhishTank, OpenPhish, heuristics) lives in its own file under `background/` with a `checkUrl()` function, so new sources can be added without touching `background.js`'s core orchestration logic.
- Run `web-ext lint` before opening a PR.
- If you add a new heuristic to `heuristics.js`, include a short comment explaining what pattern it catches and why it's a phishing signal.

## Reporting security issues

If you find a way S_Detector's own logic could be bypassed or exploited (not a missed phishing site — false negatives in the feeds themselves aren't a security bug in the extension), please open a private security advisory on GitHub rather than a public issue.

## Suggesting new phishing detection heuristics

Open an issue describing the pattern and, ideally, a couple of example URLs (real or illustrative) that demonstrate it.
