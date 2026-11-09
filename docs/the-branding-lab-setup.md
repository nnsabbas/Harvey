# Finishing Harvey's setup on your own computer

Harvey is already trained on The Branding Lab (see `harvey.yaml`,
`skills/product_knowledge.md`, `skills/competitive_intel.md`). What is left
needs your mailbox and API keys, which must never be committed, so these
steps run on the computer Harvey will live on.

## 1. Install

```bash
git clone https://github.com/nnsabbas/Harvey.git
cd Harvey
python3 -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -e .
python -m playwright install chromium
```

Needs Python 3.11 or newer (`python3 --version`) and Claude Code logged in
(`claude login`).

## 2. Keys (`.env`)

```bash
cp .env.example .env
```

Then fill in, at minimum:

- **Mail provider** for sales@the-branding-lab.com. `harvey.yaml` is set to
  `gmail`. If the mailbox is on Microsoft 365, change
  `channels.email.provider` to `microsoft` and fill `M365_CLIENT_ID` and
  `M365_TENANT_ID` instead. Then run `harvey gmail auth` or
  `harvey microsoft auth` once (a browser login).
- **An email verifier** so Harvey never sends to a guessed address.
  `REOON_API_KEY` has the best free tier (600 checks a month).

Optional: `SERPER_API_KEY` (2,500 free searches) makes prospect discovery
far more reliable than the free OpenStreetMap source.

## 3. Check the sending domain

```bash
harvey warmup the-branding-lab.com
```

Fix any FAIL rows (MX, SPF, DMARC) in your domain's DNS before sending.
If the mailbox is new, set `channels.email.warmup.start_date` in
`harvey.yaml` to the day warmup began; Harvey then ramps from 5 emails a
day.

## 4. Confirm signals and find businesses

```bash
harvey signals --confirm free
harvey discover --estimate      # projected spend, nothing runs
harvey discover                 # free OpenStreetMap source
```

## 5. Run

```bash
harvey dashboard                # open the private link it prints
harvey run
```

Every email waits in the dashboard's Outbox for your approval until you set
`channels.email.require_approval: false`.

## Still to fill in `harvey.yaml`

- `compliance.postal_address`: a street address or PO box in Trichardt
  (currently only the postcode).
- `channels.email.warmup.start_date` if the mailbox is new.
