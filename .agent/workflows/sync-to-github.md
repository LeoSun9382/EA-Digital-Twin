---
description: Sync agent skill updates to GitHub private repository
---
// turbo-all

# Sync to GitHub

This workflow commits all changes and pushes to the private GitHub repository.

## Steps

1. Stage all changes:
```bash
cd /Users/leosun/Desktop/EA-Digital-Twin-Build && git add .
```

2. Check if there are changes to commit:
```bash
cd /Users/leosun/Desktop/EA-Digital-Twin-Build && git diff --cached --quiet && echo "NO_CHANGES" || echo "HAS_CHANGES"
```

If the output is `NO_CHANGES`, inform the user that there are no changes to sync and stop here.

3. Show a summary of what will be committed:
```bash
cd /Users/leosun/Desktop/EA-Digital-Twin-Build && git diff --cached --stat
```

4. **Version sync check**: Before committing, read the `version` field from `README.md` and `SKILL.md` frontmatter depending on project rules. If `README.md` contains a different version in the badge (`![Version](https://img.shields.io/badge/version-XXX-blue)`), update it to match. Also update the version references section. Stage any README changes with `git add README.md`.

5. Commit with a descriptive message based on the changes detected. Use the format:
```bash
cd /Users/leosun/Desktop/EA-Digital-Twin-Build && git commit -m "update: <brief description of changes>"
```

6. Push to GitHub:
```bash
cd /Users/leosun/Desktop/EA-Digital-Twin-Build && git push origin main
```

7. **Tag the release**: If ITERATIONS.md was updated with a new version entry, create a Git tag matching the version:
```bash
cd /Users/leosun/Desktop/EA-Digital-Twin-Build && git tag <version> && git push origin <version>
```

8. Confirm the sync was successful by checking the latest commit:
```bash
cd /Users/leosun/Desktop/EA-Digital-Twin-Build && git log -1 --oneline
```

Report the result to the user, including the commit hash and tag (if created).
