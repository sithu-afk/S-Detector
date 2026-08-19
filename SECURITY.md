# Security

This document summarizes a security self-review of S_Detector's source code, what was found, and how it was addressed. It's here for transparency — S_Detector is a security tool, so its own code should hold up to scrutiny.

## Findings and mitigations

### 1. DOM construction in the warning interstitial
**Risk:** The warning overlay originally built its markup with `innerHTML` and escaped dynamic values (hostname, threat reasons) before interpolating them. This was safe as implemented, but `innerHTML`-based construction is fragile — a future edit that forgets to escape a new field would silently reintroduce an XSS bug.
**Fix:** Rewritten to build the DOM entirely with `createElement`/`textContent`, so dynamic values (hostname, per-source threat labels, heuristic reason strings) are never parsed as HTML, regardless of their content. There is no `innerHTML` assignment involving dynamic data anywhere in the extension.

### 2. Message sender validation
**Risk:** `runtime.onMessage` listeners in both the background script and content script did not explicitly verify the sender. MV3's default behavior already restricts `runtime.onMessage` to the extension's own contexts (no `externally_connectable` is declared), but relying solely on that default without an explicit check is a thinner guarantee than stating it directly in code.
**Fix:** Both listeners now check `sender.id === browser.runtime.id` before acting on any message.

### 3. Untrusted external feed data
**Risk:** PhishTank and OpenPhish feed responses were parsed and cached without validating their shape, size, or entry format. A malformed, truncated, or maliciously large response could bloat `storage.local` or cause unexpected behavior downstream.
**Fix:** Feed parsing now validates that the response is the expected shape (array for PhishTank JSON, non-empty lines for OpenPhish text), filters out entries exceeding a sane URL length, and caps the total number of cached entries.

### 4. Safe Browsing API response handling
**Risk:** The Safe Browsing API response was trusted to always match the documented schema.
**Fix:** Response parsing now checks that `matches` is actually an array and that `threatType` is a string before using it, falling back safely otherwise.

### 5. Explicit Content Security Policy
**Risk:** MV3's default CSP is already strict (no remote code execution, no inline scripts), but it wasn't declared explicitly in `manifest.json`.
**Fix:** Added an explicit `content_security_policy` restricting extension pages to `script-src 'self'; object-src 'none'` — closes any ambiguity and makes the guarantee auditable at a glance.

## Known limitations (by design, not bugs)

- **Safe Browsing API key exposure:** Because this is a client-side browser extension, any API key bundled with it can in principle be extracted by unpacking the `.xpi`. This is a structural limitation of client-side extensions, not a code vulnerability. See `AMO_SUBMISSION.md` for the two realistic mitigation paths (ship without Safe Browsing, or proxy it through a backend you control) before wide public release.
- **`<all_urls>` host permission:** Required because the extension needs to inspect the destination URL of every navigation to check it against phishing sources. It does not grant the extension access to page *content* — only the ability to observe navigation URLs and inject the warning overlay when a site is flagged.
- **No remote code execution anywhere:** All JavaScript ships inside the extension package. Nothing is `eval`'d, and no scripts are fetched and executed at runtime.

## Reporting a vulnerability

If you find a security issue in S_Detector's own code (as opposed to a phishing site the underlying feeds simply haven't indexed yet — that's a data gap, not a code vulnerability), please open a private GitHub Security Advisory on this repository rather than a public issue.
