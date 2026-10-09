# 0G AgenticID console — static preview

The web console from [`0gfoundation/0g-agentic-id`](https://github.com/0gfoundation/0g-agentic-id)
(`attestor/crates/api/web/index.html`), published as a static site so the UI can be
looked at without running the attestor.

Live page: https://yqrashawn.github.io/agenticid-console-preview/

The real console is served by the Rust attestor, which also answers `/config`,
`/deployments`, `/deployment/:id` and a `/ws/subscribe` stream from the same origin.
A static host has none of that, so a small shim at the top of `index.html` answers
those paths from a snapshot of the live production attestor
(`https://agenticid.0g.ai`) taken 2026-10-09, including the on-chain
`agent_seal` balances and `getAgentWallet` rows for all 136 agents read from 0G Galileo.

Reads render exactly as they do in production. Writes (deploy / start / stop / reset)
return 503, and the event stream is inert — there is no backend behind this copy.

The production attestor runs headless (`ATTESTOR_CONSOLE_ENABLED=false`), which is why
https://agenticid.0g.ai answers `ok` instead of this page.

## Contents

- `index.html` — the console, unmodified apart from the shim block at the top
- `static/ethers.js`, `static/fonts.css`, `static/fonts/*.woff2` — vendored assets
- `static/deployments.json`, `static/snapshot.json` — the captured API + chain state
