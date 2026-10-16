# Project Boaz

**Steady income, held loosely.**

Project Boaz is a free, open-source, self-hosted app that runs a careful, rule-based covered-call routine in your own [Alpaca](https://alpaca.markets) brokerage account. You set the rules. Boaz follows them, explains every step, and stops the moment you ask.

[Website](https://projectboaz.dev) · [Support on Ko-fi](https://ko-fi.com/F0O628X17I)

> **Status: in development.** There is no release yet. Watch or star this repo to follow along.

> **Risk notice.** Options trading involves risk of loss. Boaz is not financial, tax or legal advice, and is not affiliated with Alpaca or Anthropic. Read [Characteristics and Risks of Standardized Options](https://www.theocc.com/company-information/documents-and-archives/options-disclosure-document) before trading options.

---

## What it does

A covered call means you own 100 shares of a stock and sell someone the right to buy them from you at a higher price by a set date. You collect a small payment (the premium) up front. If the price passes that level, your shares are sold at it.

Boaz automates that routine:

1. **You own the shares.** Boaz only works with shares you already hold, in blocks of 100.
2. **Boaz finds one call per block.** About a week out, at a strike well above today's price, chosen by rules you can read and change.
3. **You approve, or let it run.** Manual mode asks first. Autonomous mode stays inside hard daily limits.
4. **The premium is split your way.** Part buys more shares toward your next block. Part is set aside as income you can withdraw yourself.

## Values

| | |
| --- | --- |
| **Income without greed** | Conservative defaults. Shares are allowed to be called away instead of chased. |
| **Growth without obsession** | One routine, once a day. No charts to stare at. |
| **Automation without dependence** | Approve every trade, or set limits and keep a kill switch within reach. |
| **Stewardship over speculation** | No margin, no shorting, no predictions. |

## Features

- **Covered calls by rule.** Strike by delta (default 0.20) with a minimum distance above the price (default 5%) and a floor at your cost per share. Nearest expiration 5 to 9 days out.
- **Let it be called away.** By default Boaz does not roll in-the-money calls. Rolling is opt-in, for a net credit only, and capped.
- **Ticker screen.** Leveraged and inverse ETFs, shares under $10, over-the-counter listings and thinly traded names are blocked. High volatility, heavy concentration and sharp recent drops trigger warnings you must confirm. Boaz never picks or ranks stocks for you.
- **Three modes.** Growth reinvests everything, Balanced splits income by your percentage, Income sets most of it aside.
- **A clear ledger.** Every dollar sits in one bucket: Principal, Reinvestment, Available Income, or an optional tax set-aside. The buckets reconcile with your Alpaca cash every day.
- **Manual or autonomous.** Hard limits on contracts, spending and orders per day. A kill switch cancels all open orders at once.
- **Plain explanations.** Each action comes with the rule that fired. Add your own Claude API key for written explanations and reports, or use the built-in templates.
- **Practice first, if you want.** Run on Alpaca's free paper account with simulated money, or connect your live account from day one.
- **Yours, on your machine.** Runs in Docker on your own computer. Your keys and data never leave it, and Boaz sends no telemetry.

## Requirements

- An Alpaca account with options trading enabled (level 1 covers covered calls).
- A computer that stays on during US market hours, with Docker and Docker Compose.
- Optional: a Claude API key for written explanations.

## Quick start (planned)

```bash
git clone https://github.com/<you>/project-boaz.git
cd project-boaz
docker compose up -d
```

Then open `http://localhost:3000`. The setup page walks you through:

1. Setting a local password.
2. Entering your Alpaca keys and testing them (paper keys recommended to start).
3. Choosing tickers and a mode.
4. Reading and accepting the risk notice.

Start in Manual mode. Boaz recommends two weeks of paper trading before Autonomous; you can skip that after reading a warning.

## How it is built

```
Browser ──► web (Next.js, localhost:3000)
              │  internal network only
              ▼
           engine (Python, FastAPI, scheduler, rules, ledger)
              │                 │
              ▼                 ▼
        SQLite volume     Alpaca API · Claude API (optional)
```

- **engine**: Python 3.12, FastAPI, APScheduler and Alpaca's official `alpaca-py` SDK. The only part that holds keys or talks to the internet.
- **web**: Next.js and Tailwind. Talks only to the engine.
- **data**: SQLite on a Docker volume, with nightly backups. Money is stored as exact decimals, never floats.
- **security**: binds to `127.0.0.1` by default, one local password, keys encrypted at rest (AES-GCM, key derived from your password with Argon2id).

The daily schedule (US Eastern, trading days only): reconcile at 08:00, run the rules at 10:00, follow up on orders until 15:30, close out and summarize at 16:30.

## Roadmap

- [ ] Foundations: Docker Compose, setup page, encrypted keys, read-only Alpaca sync, dashboard
- [ ] Covered calls in Manual mode: eligibility, strike and expiry rules, limit orders, suggestion cards
- [ ] Position lifecycle: buy to close, optional rolls, assignment handling, reconciliation, scheduler, kill switch
- [ ] Money ledger: buckets, modes, share buys by block, income and deposit suggestions, CSV export
- [ ] Autonomous mode with hard limits, explanations, reports and notifications
- [ ] First release

## Why "Boaz"?

In the book of Ruth, Boaz is a landowner who works his fields faithfully and tells his harvesters to leave extra grain for a widow gathering behind them. Later he redeems her family's land and gives them a future. He is prosperous, but wealth does not rule him.

Project Boaz tries to carry that posture into software: steady, careful, honest about risk, and generous with what it earns. That is why it is free, why its defaults are cautious, and why its website points to organizations that help people with money. For me, Jesus comes first in this project. You don't need to share that faith to use Boaz; it is for everyone.

## Support

Boaz is free and always will be. If it helps you, you can support development and hosting on [Ko-fi](https://ko-fi.com/F0O628X17I). Donations are gifts to an individual developer; they are not tax-deductible and don't buy features, support or advice.

## Contributing

Issues and pull requests are welcome once the first code lands. Please don't ask for, or give, advice on what specific securities to trade in issues or discussions.

## License

[MIT](LICENSE). Provided as is, with no warranty. You are responsible for every trade placed in your account.

## Disclaimer

Project Boaz is software, not a financial adviser. It does not know your situation and makes no promise of income. Covered calls cap your upside while leaving you exposed to the full downside of the shares. Assignment can sell shares below market value. Software, data and broker outages can cause missed or wrong trades. Premiums and shares called away usually create taxable gains. Use at your own risk.
