# Build and PR plan

One branch and one PR per build version. Merge in order.

| Version | Branch | Contents | PR title |
|---|---|---|---|
| v0.1 | `build/v0.1-scaffold` | `hugo.toml`, `layouts/`, `assets/`, `content/`, `.gitignore` | `v0.1: Hugo scaffold and theme` |
| v0.2 | `build/v0.2-ci` | `.github/workflows/ci.yml`, `.htmltest.yml` | `v0.2: Build and link-check pipeline` |
| v0.3 | `build/v0.3-security` | `.github/workflows/security.yml`, `.github/dependabot.yml` | `v0.3: Secret, workflow and config scanning` |
| v0.4 | `build/v0.4-content` | Real name, contact details, projects, experience | `v0.4: Replace placeholders with real content` |

## PR description template
**What:** one sentence. **Why:** one sentence. **Test:** `hugo server`, then check home, a project page, and the 404. **Checks:** CI and Security green. **Rollback:** revert this PR.

## Placeholders to replace (v0.4)
`[Your Name]`, `[Company Name]`, `you@example.com`, `your-handle` (LinkedIn and GitHub), `baseURL`, the `20XX` years, and the three placeholder projects.

## Audit your token (run locally, never paste the token anywhere)
```
gh auth status
gh api repos/adam0306/claudedemosite --jq '.permissions'
gh api repos/adam0306/claudedemosite/branches/main/protection   # 404 means no branch protection
```
Prefer a fine-grained token limited to this one repo with Contents, Pull requests and Workflows (read and write). Turn on branch protection for `main` and require the `build-and-test` and Security checks.

## First-run notes
- Run `hugo server` locally to view the site. I could not run Hugo here, so the build is untested.
- gitleaks-action is free for personal accounts; organizations need a license key.
- `wjdp/htmltest-action@master` is a moving ref. Pin all third-party actions to a commit SHA once the pipeline is green.
- CodeQL has no Hugo or HTML support, so it scans the workflow files (`actions`). Trivy and gitleaks cover config and secrets.
