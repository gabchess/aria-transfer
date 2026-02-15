# Browser Security + Kimi Claw Setup Guide

**Created:** Feb 15, 2026
**Goal:** Secure browser isolation + cloud-native AI assistant for work productivity

---

## PART 1: BROWSER SECURITY SETUP

### Why Separate Profiles?

Crypto wallets in browser extensions can be exploited by malicious sites or extensions. By isolating your work browser (where AI tools run) from your crypto browser, you prevent:
- Malicious extensions accessing wallet
- Clipboard hijacking of seed phrases
- AI tools accidentally interacting with DeFi sites

### Step 1: Create Chrome "Work" Profile (AI Tools Only)

1. Open Chrome
2. Click your profile icon (top right)
3. Click "Add" → "Continue without an account" (or sign in with work email)
4. Name it **"Work - AI Tools"**
5. Choose a distinct color/avatar so you never confuse it

**What to install in Work profile:**
- Kimi Claw extension (when ready)
- Note-taking extensions
- Productivity tools

**What to NEVER install in Work profile:**
- MetaMask
- Rabby
- Any wallet extension
- Crypto price trackers

### Step 2: Set Up Brave for Crypto

Brave has a built-in wallet and better privacy defaults. Use this as your crypto browser.

1. Download Brave if not installed: https://brave.com
2. Import your MetaMask wallet OR use Brave Wallet
3. Enable "Strict" fingerprinting protection (Settings → Shields)
4. Only use this browser for:
   - DeFi protocols
   - NFT mints
   - Wallet interactions
   - Crypto trading

**Alternative:** Create a separate Chrome profile called "Crypto" if you prefer Chrome, but keep it completely isolated.

### Step 3: Hardware Wallet (Recommended)

For holdings over $1K, get a hardware wallet:

- **Ledger Nano S Plus:** ~$79 (best value)
- **Ledger Nano X:** ~$149 (Bluetooth)
- **Trezor Safe 3:** ~$79

**Setup:**
1. Buy directly from manufacturer (never Amazon/eBay)
2. Generate seed phrase on device (never on computer)
3. Write seed on paper, store offline
4. Transfer main holdings from MetaMask to Ledger
5. Keep only "spending money" in hot wallet

---

## PART 2: KIMI CLAW SETUP

### What is Kimi Claw?

Kimi Claw is OpenClaw native to kimi.com. It's a cloud AI agent that:
- Runs 24/7 in your browser
- Has 5,000+ community skills (ClawHub)
- Provides 40GB cloud storage
- Can bridge to Telegram
- Supports BYOC (Bring Your Own Claw) for hybrid setups

**Pricing:** Available on Allegretto tier and higher

### Step 4: Sign Up for Kimi

1. Go to https://www.kimi.com
2. Create account (use work email)
3. Upgrade to Allegretto tier when available
4. Access Kimi Claw from the interface

### Step 5: Configure BYOC (Bring Your Own Claw)

This connects your existing OpenClaw (me, Aria) to Kimi's cloud interface:

1. In Kimi Claw settings, find "Bring Your Own Claw" / "Connect Third-Party"
2. Get the connection token/URL
3. In OpenClaw config, add the Kimi bridge:
   ```
   openclaw config set kimi.token <your-token>
   openclaw config set kimi.enabled true
   ```
4. Test connection

**Benefits:**
- Keep your local OpenClaw config (me, Aria)
- Access through Kimi's web interface
- Use ClawHub's 5K+ skills
- 40GB cloud storage for context

### Step 6: Set Up Telegram Bridge (Optional)

Kimi Claw can bridge to Telegram for notifications:

1. In Kimi Claw, enable Telegram integration
2. Connect your Telegram account
3. Choose which notifications to forward
4. Test with a simple task

---

## PART 3: WORK TOOLS ACCESS

Once Kimi Claw is running in your Work profile, you can use it to help with:

### Gather (Virtual Office)
- Navigate meetings in browser
- Take notes during calls
- Summarize discussions

### Mattermost (Team Chat)
- Read and summarize channels
- Draft responses
- Track action items

### Proton Email
- Read emails (with your permission)
- Draft replies
- Summarize inbox

### Notion (if accessible)
- Update task boards
- Create meeting notes
- Track projects

---

## SECURITY AUDIT CHECKLIST

Before enabling AI browser access:

- [ ] Work profile has ZERO crypto extensions
- [ ] Brave/Crypto profile is completely separate
- [ ] Hardware wallet ordered or set up
- [ ] Main holdings moved to cold storage
- [ ] Only "spending money" in hot wallet
- [ ] Work profile signed into work accounts only
- [ ] No cross-contamination between profiles

---

## STEP-BY-STEP WHEN GABE RETURNS

1. **Create Chrome Work profile** (5 min)
2. **Verify no wallet extensions** in Work profile (1 min)
3. **Set up Brave for crypto** or create Crypto profile (10 min)
4. **Sign up for Kimi Allegretto** (5 min)
5. **Configure BYOC** to connect Aria (10 min)
6. **Test basic tasks** in Gather/Mattermost (10 min)
7. **Order Ledger** if not done (5 min)

**Total time:** ~45 minutes

---

## LINKS

- Kimi: https://www.kimi.com
- Kimi Claw docs: https://kimiclaw.jp.larksuite.com/wiki/ZJWEwzubDiRvWjkTLfyjkyMYpSf
- Chrome profiles: chrome://settings/manageProfile
- Brave download: https://brave.com
- Ledger: https://www.ledger.com

---

*Ready to level up your productivity while keeping your crypto safe!*
