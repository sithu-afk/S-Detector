# S_Detector Privacy Policy

_Last updated: August 2026_

S_Detector is a real-time phishing detection extension for Firefox. This policy explains what data it accesses and where that data goes.

## What S_Detector checks

Every time you navigate to a new website (top-level page loads only — not embedded frames, images, or scripts), S_Detector checks the destination URL against the following sources to determine whether it may be a phishing or deceptive site:

1. **Google Safe Browsing API** — the URL you are navigating to is sent to Google's Safe Browsing service for a real-time reputation check.
2. **PhishTank** — checked locally against a cached list downloaded periodically from PhishTank's public feed. No data is sent to PhishTank; the URL comparison happens entirely on your device.
3. **OpenPhish** — same as above: checked locally against a cached feed, no data sent to OpenPhish.
4. **Local heuristics** — pattern checks (punycode domains, brand lookalikes, non-HTTPS connections, IP-address hosts) that run entirely inside your browser and never leave your device.

## What is sent externally

The **only** data sent outside your browser is the URL of the page you are actively navigating to, and only to Google's Safe Browsing API, for the purpose of that single reputation check. S_Detector does not send:

- Page content
- Form data, passwords, or anything you type
- Browsing history beyond the single URL being checked in real time
- Any personally identifying information

Google's handling of Safe Browsing API requests is governed by Google's own privacy policy: https://policies.google.com/privacy

## What is stored locally

S_Detector stores the following locally, in your browser's extension storage, and never transmits it anywhere:

- Cached PhishTank and OpenPhish feed data (refreshed periodically)
- A list of sites you've chosen to "proceed anyway" past a warning (your personal allowlist)
- Basic usage counters (number of sites checked, number flagged) shown in the toolbar popup

This local data is never synced, sold, or shared with any third party, and is deleted if you uninstall the extension.

## Permissions

S_Detector requests broad host permissions (`<all_urls>`) because it needs to inspect the destination URL of every page navigation in order to check it against phishing sources before the page loads. It does not read page content, and detection logic runs only against the URL string itself (with one exception: the warning interstitial content script runs on pages that get flagged, purely to display the warning).

## Contact

Questions about this policy or the extension's data handling can be raised via the GitHub repository issue tracker: https://github.com/sithu-afk/S_Detector
