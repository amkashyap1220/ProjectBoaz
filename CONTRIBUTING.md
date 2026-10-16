# Contributing to Project Boaz

Thanks for helping. I'm Alex, and I build and maintain Project Boaz. It places real trades in real brokerage accounts, so I review every change carefully before it ships.

## How I take changes

1. Open an issue first for anything bigger than a typo, so we can agree on the approach before you spend time on it.
2. Fork the repo, make your change on a branch, and open a pull request against `main`.
3. I review every pull request myself. I only merge what I've reviewed, and I publish all releases.

## What makes a pull request easy for me to merge

- One focused change per pull request.
- Tests for any change to trading rules, order logic or the ledger.
- No new dependencies without a reason in the description.
- Anything that touches how orders are placed is tested against an Alpaca paper account first.

## Please don't

- Ask for or share stock picks, trade ideas or financial advice. Issues are for the software.
- Post API keys, account numbers or screenshots of real balances.

## Security issues

Please don't open a public issue for a security problem. See [SECURITY.md](SECURITY.md).

By contributing, you agree that your contribution is licensed under the MIT License, the same license as the project.
