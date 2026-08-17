# S_Detector

Real-time phishing site detector for Firefox.

## How it works

S_Detector checks every top-level page navigation against four layers of protection, in order:

1. **PhishTank cached feed** — free public bulk feed, refreshed hourly, no API key required.
2. **OpenPhish cached feed** — same idea, second free source for redundancy.
3. **Google Safe Browsing (live API)** — real-time lookup against Google's threat list. Requires a free API key.
4. **Local heuristics** — punycode/homoglyph domains, brand lookalikes, non-HTTPS, IP-address hosts, excessive subdomains. Runs entirely offline.

If any source flags the destination, a full-page warning interstitial is shown (not a silent block) — the user can go back or choose to proceed.

## Setup

1. **Get a free Google Safe Browsing API key:**
   - Go to https://console.cloud.google.com/
   - Create a project, enable the "Safe Browsing API"
   - Create an API key under Credentials

   Note: PhishTank's own API requires a registered account, and PhishTank has not accepted new API registrations since 2020. This build uses PhishTank's free public bulk-download feed instead, which doesn't require a key.

2. **Create your local config:**
   ```
   cp background/config.example.js background/config.js
   ```
   Then open `background/config.js` and paste your real API key into `SAFE_BROWSING_API_KEY`.
   `background/config.js` is git-ignored, so your key never gets committed — only `config.example.js` (the placeholder template) is tracked in the repo.

3. **Load the extension in Firefox for testing:**
   - Open `about:debugging#/runtime/this-firefox`
   - Click "Load Temporary Add-on"
   - Select `manifest.json` from this folder

4. **Icons** in `icons/` are placeholders — swap in your own branded artwork before submitting to AMO.



## Project structure

```
S_Detector/
├── manifest.json
├── background/
│   ├── config.js            # API keys + feed URLs + source toggles
│   ├── heuristics.js         # offline lookalike/punycode checks
│   ├── safeBrowsing.js       # Google Safe Browsing live lookups
│   ├── phishtankFeed.js      # PhishTank bulk feed + cache
│   ├── openphishFeed.js      # OpenPhish bulk feed + cache
│   └── background.js         # navigation interception, orchestration
├── content/
│   ├── warningInterstitial.js
│   └── warningInterstitial.css
├── popup/
│   ├── popup.html / .css / .js
├── options/
│   ├── options.html / .js
├── icons/
└── _locales/en/messages.json
```

## 
## License

MIT — see [`LICENSE`](LICENSE).

## Contributing

See [`CONTRIBUTING.md`](CONTRIBUTING.md).

