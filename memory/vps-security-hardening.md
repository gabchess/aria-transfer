# VPS Security Hardening for Trading Bots

Source: @Shelpid_WI3M (https://x.com/Shelpid_WI3M/status/2017938351576215802)
Saved: 2026-02-05

## Why This Matters
When running bots with real money (Polymarket, trading, etc.), security is critical. One prompt injection = funds gone.

## Setup Order (Ubuntu VPS)

### 1. Lock Down SSH
- Keys only, no passwords, no root login

### 2. Default-Deny Firewall
- Block everything incoming by default

### 3. Brute-Force Protection
- Auto-ban IPs after failed login attempts (fail2ban)

### 4. Install Tailscale
- Private VPN mesh network
- Makes everything reachable ONLY from your devices

### 5. SSH Only via Tailscale
- No more public SSH exposure

### 6. Web Ports Private Too
- Gateway only accessible from your devices

### 7. Bot Owner-Only Mode
- Only you can message the bot
- NEVER add bots to group chats (everyone in chat can issue commands)

### 8. Enable Sandbox Mode
- Runs risky operations in container
- Blast radius contained if something goes wrong

### 9. Whitelist Commands
- Don't let agent run arbitrary commands
- Explicitly list only what it needs
- If agent gets hijacked via prompt injection, damage limited to whitelist

### 10. Scope API Tokens
- Minimum permissions always
- Read-only where possible
- If compromised, damage limited to that token's scope

### 11. Fix Credential Permissions
- Don't leave secrets world-readable
- chmod 600 on sensitive files

### 12. Run Security Audit
- `openclaw doctor` or equivalent
- If it fails, do NOT deploy. Fix first.

## Verification Checklist
- [ ] No public SSH
- [ ] No public web ports
- [ ] Server only reachable via Tailscale
- [ ] Bot responds only to owner

## Real Prompt Injection Incident
Someone sent an email with concealed instructions to an inbox the bot could access. Bot executed them and wiped every email including trash. This actually happened.

Defense layers:
1. Claude Opus 4.5 (~99% prompt injection resistance)
2. Command allowlists
3. Sandboxing
4. Narrowly scoped API tokens

## For Our Polymarket Bot
When we deploy:
- Hetzner VPS (Gabe offered this)
- Tailscale for private access
- Command whitelist for trading operations only
- Scoped API tokens (read-only where possible)
- Sandbox mode enabled
- Owner-only bot access

---

Note: The $50 → $248K claim from the original tweet is unverified. The security advice is solid regardless.
