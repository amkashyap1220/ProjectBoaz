# Locking down the Project Boaz repo

Strangers can't push to a public repo, delete it or change its settings. Only you and anyone you add as a collaborator can. What they *can* do is open issues, pull requests and forks, so these settings keep that from turning into a problem.

## 1. Upload the files

On GitHub, use **Add file → Upload files** on `main` and drop in everything in this folder except this guide: `LICENSE`, `CONTRIBUTING.md`, `SECURITY.md` and the `.github` folder (it holds `CODEOWNERS`).

## 2. Protect `main` (most important)

**Settings → Rules → Rulesets → New ruleset → New branch ruleset**
- Name: `main`. Enforcement: **Active**. Target branches: **Include default branch**.
- Turn on **Restrict deletions** and **Block force pushes**.
- Turn on **Require a pull request before merging**, with **Require review from Code Owners**.
- Leave yourself in **Bypass list** (Repository admin) so you can still push small fixes directly. Remove that later if you want every change to go through a pull request.

## 3. Only you make releases

**Settings → Rules → Rulesets → New ruleset → New tag ruleset**
- Target: **All tags**. Turn on **Restrict creations**, **Restrict updates** and **Restrict deletions**. Add yourself to the bypass list.
- Releases already need write access, so this is a second lock.

## 4. Don't let pull requests run code on your account

**Settings → Actions → General**
- Fork pull request workflows: **Require approval for all external contributors**.
- Workflow permissions: **Read repository contents and packages permissions**.
- Leave **Allow GitHub Actions to create and approve pull requests** off.

## 5. Security features (all free for public repos)

**Settings → Code security**
- **Private vulnerability reporting**: Enable (this is what `SECURITY.md` points to).
- **Dependabot alerts** and **Dependabot security updates**: Enable.
- **Secret scanning** and **Push protection**: Enable, so an API key can't be committed by accident.

## 6. Keep the conversation clean

- **Settings → General → Features**: leave **Wikis** off (anyone with access could edit it) and turn on **Discussions** only if you want it.
- **Settings → Moderation options → Interaction limits**: if you ever get spammed, limit issues and comments to prior contributors for 24 hours to 6 months.
- You can lock, hide or delete any comment, and block a user from your profile settings.

## 7. Your account

- Turn on **two-factor authentication** (github.com/settings/security). It's the main way a repo actually gets hijacked.
- Don't add collaborators unless you trust them with write access.
