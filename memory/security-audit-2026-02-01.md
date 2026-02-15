# Security Audit — 2026-02-01

## What Was Already Solid ✅
- Gateway bound to **loopback** (127.0.0.1 only, not exposed to internet)
- Gateway auth: **token mode** with random 48-char token
- DM policy: **pairing** (manual approve new contacts)
- Group policy: **allowlist** (only approved groups)
- All ports (18789, 18792, 9222) on localhost only
- Tailscale: off (no remote exposure)

## What I Fixed Today 🔧
1. **Logging redaction** — Added redact patterns for:
   - OpenAI API keys (sk-...)
   - Google/Gemini keys (AIzaSy...)
   - X/Twitter auth tokens
   - GitHub tokens (gho_...)
   - Email addresses
   - Password patterns
2. **File permissions** — Locked down ACLs on:
   - openclaw.json (config with bot token)
   - auth-profiles.json (API keys)
   - bird/config.json5 (X cookies)
   - Restricted to: current user (gavaf) + SYSTEM only
3. **Doctor fix** — Ran `openclaw doctor --fix`, updated service entrypoint

## Still TODO
- [ ] Set up Brave Search API (needs free API key from brave.com/search/api)
- [ ] Evaluate Prompt Guard from ClawHub (349 attack patterns)
- [ ] Periodic token rotation schedule
- [ ] Archive old session files regularly (contain typed credentials)
- [ ] Consider browser.controlToken for Chrome debugging port

## Risk Assessment
- **LOW risk** — Gateway not exposed, auth enabled, loopback only
- **MEDIUM risk** — Chrome debugging port (9222) on loopback without auth
  - Mitigation: Only accessible from this machine
- **MEDIUM risk** — Session files may contain sensitive data typed in chat
  - Mitigation: Redaction patterns active, periodic cleanup recommended
