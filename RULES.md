# Portfolio Maintenance Rules

## 1. Source of Truth

- The local project is the working source of truth.
- GitHub is the remote repository.
- GitHub Pages is the production deployment.
- Do not treat Claude Design as the current source of truth.

## 2. Before Making Changes

- Inspect the relevant files first.
- Understand existing HTML, CSS, JavaScript, assets, and paths before editing.
- Do not make changes based on assumptions.
- Explain the proposed change before editing.
- Wait for explicit user approval before modifying files.

## 3. Preserve Existing Design

- Do not redesign or rebuild the portfolio unless explicitly requested.
- Preserve the existing visual language, typography, spacing, design tokens, navigation, and responsive behavior.
- Make the smallest change necessary to solve the requested problem.
- Do not modify unrelated sections.

## 4. Assets and Paths

- Preserve relative asset paths because the site is deployed as a GitHub Pages project site.
- Do not rename or move assets without checking every reference.
- Do not delete assets that may be used by the website.
- `_ds/` contains the required design-system assets and must not be deleted.
- `.nojekyll` must remain in the repository because GitHub Pages needs to serve the `_ds/` directory correctly.
- Do not reintroduce `_ds/` into `.gitignore`.

## 5. Git Safety

- Never change the Git remote unless explicitly requested.
- Before pushing, verify the remote using `git remote -v` or `git remote get-url --push origin`.
- The repository must remain connected to the personal GitHub repository:
  `git@github.com:AK-spec483/Aravind-portfolio.git`
- Never push to an organization/company repository.
- Never commit passwords, API keys, access tokens, SSH private keys, or other secrets.
- Do not use `git add .` blindly when there are unexpected files.
- Review `git status` before committing.
- Review `git diff` when appropriate.
- Never push to GitHub unless the user explicitly asks.

## 6. Testing

- Test changes locally before committing.
- Use the existing static-server workflow when appropriate.
- Verify desktop and mobile behavior for UI changes.
- Test affected buttons, links, images, video, navigation, and downloads.
- Confirm that GitHub Pages compatibility is preserved.

## 7. GitHub Pages

- The site is a static GitHub Pages project.
- `index.html` is the production entry point.
- Do not introduce a build system unless explicitly requested.
- Preserve `.nojekyll`.
- Ensure required CSS and JavaScript assets are accessible from their relative paths.

## 8. Change Management

For every requested change:

1. Inspect.
2. Explain the proposed change.
3. Wait for explicit user approval.
4. Make the smallest appropriate change.
5. Test locally.
6. Review `git status`.
7. Review the relevant diff.
8. Commit only when requested.
9. Push only when explicitly requested.

## 9. Confidentiality

- Treat company/project information as potentially confidential.
- Do not expose internal documents, credentials, customer information, machine configuration secrets, private URLs, or proprietary data.
- Do not add unnecessary internal project files to the public repository.
- Before adding new company-project material to the public portfolio, flag potential confidentiality concerns.

## 10. Recovery

- Do not delete or overwrite working files as a first troubleshooting step.
- Create a backup before risky changes when appropriate.
- If a change breaks the site, identify the cause and restore the last known working state before attempting unrelated changes.

## 11. Current Known Deployment Fix

- The design system is located under `_ds/`.
- `_ds/` was initially ignored by `.gitignore`.
- The ignore rule was removed.
- The `_ds` files were committed.
- GitHub Pages still returned 404 for the CSS.
- `.nojekyll` was added.
- After `.nojekyll` was pushed, the CSS loaded correctly.
- Therefore `.nojekyll` and `_ds/` are required parts of the current deployment.

Do not add rules that contradict the existing working project.
