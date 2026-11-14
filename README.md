# quality-gates

Shared checks for every project. A project turns them on with one small file.

## What runs on every push

| Job | Tool | Fails when |
|---|---|---|
| Project check | the project's `scripts/check` (or `npm run build` + `npm test`) | a build or a test fails |
| Secrets | gitleaks 8.21.2, whole history | a key, password or token is in the code |
| Weak libraries | osv-scanner 1.9.1 | a package that ships to users has a known security hole (dev-only tools are warnings) |
| Code holes | Semgrep 1.95.0 (official image), OWASP Top 10, errors only | injection, unsafe code patterns |

## Turn it on in a project

1. Add `.github/workflows/quality.yml`:

```yaml
name: quality
on:
  push:
  pull_request:
jobs:
  gates:
    uses: tanimsattar-collab/quality-gates/.github/workflows/gates.yml@main
```

2. Add `scripts/check`: one command that builds and tests the project. It must exit with an error when something is wrong.
3. Add `.github/dependabot.yml` so library updates arrive as pull requests.

The same `scripts/check` runs on your Mac before every `git push` (the global Claude hook), so a red check never leaves the laptop.
