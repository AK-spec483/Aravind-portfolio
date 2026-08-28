# Portfolio Maintenance Guide

## 1. Project Overview

This repository contains Aravind Kumar S's personal UI/UX portfolio website. It presents a home portfolio view and the G-Tron Configurator case study, including an interactive prototype video, a downloadable resume, and contact links.

The project is deployed as a static site through GitHub Pages. Preserve the existing visual design, content, assets, client-side behavior, and relative asset paths unless a change is explicitly requested and approved.

Current project root:

```text
/home/aravind/Aravind-Portfolio/Portfolio-Source/Aravind-Portfolio-Site
```

## 2. Current Architecture

This is a static HTML-based project; it has no `package.json`, framework build pipeline, or generated `dist/` directory.

- `index.html` is the production entry point served by GitHub Pages.
- `index.html` contains the Home and G-Tron case-study views, client-side view switching, browser-history handling, responsive rules, and video controls.
- `_ds/modernist-b1528d07-a821-4b48-b3b9-bcdb44b70864/` contains the Modernist design-system CSS and runtime files used by `index.html`.
- `assets/` contains production assets, including SVG screen images, `prototype-video.mp4`, and `resume.pdf`.
- `.nojekyll` disables GitHub Pages' Jekyll processing so the underscore-prefixed `_ds/` directory is served unchanged.
- `Portfolio.dc.html` and `design_handoff_portfolio/` are design/reference artifacts. The handoff folder is intentionally ignored and is not part of deployment.

Important production asset paths include:

```text
index.html
_ds/modernist-b1528d07-a821-4b48-b3b9-bcdb44b70864/styles.css
_ds/modernist-b1528d07-a821-4b48-b3b9-bcdb44b70864/_ds_bundle.js
assets/prototype-video.mp4
assets/resume.pdf
```

## 3. Git and GitHub Setup

The repository's primary branch is `main`. The `origin` remote is configured for SSH:

```text
git@github.com:AK-spec483/Aravind-portfolio.git
```

Useful checks:

```bash
git remote -v
git branch --show-current
git status --short
git config --get user.name
git config --get user.email
```

Git must have a user identity before commits can be created. For portfolio maintenance, prefer repository-local configuration if either identity check is empty:

```bash
git config user.name "Your Name"
git config user.email "you@example.com"
```

The original setup documentation included global Git configuration as an option. Do not use `--global` for routine portfolio maintenance unless the user explicitly requests a machine-wide Git identity change.

Use the repository's existing SSH remote. Do not record passwords, access tokens, SSH private keys, or other credentials in repository files.

## 4. GitHub Pages Deployment Setup

GitHub Pages deploys the static files committed to `main`. The working site relies on relative URLs such as:

```html
<link rel="stylesheet" href="_ds/modernist-b1528d07-a821-4b48-b3b9-bcdb44b70864/styles.css">
```

The repository includes an empty `.nojekyll` file at the root. It must remain tracked and committed because GitHub Pages otherwise uses Jekyll processing, which does not publish underscore-prefixed directories such as `_ds/`.

Check that the deployment-critical files are tracked:

```bash
git ls-files index.html .nojekyll _ds
```

## 5. Problems We Encountered

The following deployment issues occurred during the initial setup:

1. Git remote configuration needed to be established.
2. Git user name and email needed to be configured for commits.
3. `git add.` was entered incorrectly; Git requires a space: `git add .`.
4. An HTTPS GitHub push failed during authentication.
5. GitHub does not accept GitHub account passwords for Git-over-HTTPS operations.
6. SSH authentication was set up and verified so the repository could be pushed securely.
7. The initial portfolio files were pushed to GitHub.
8. GitHub Pages was configured to deploy the repository.
9. The live site loaded without its CSS because the design-system files were not initially included in the repository.
10. The `_ds/` design-system folder was ignored by `.gitignore`.
11. The exact ignore rule was located with `git check-ignore` and removed.
12. The design-system files were staged and committed, but the stylesheet still returned HTTP 404 on GitHub Pages.
13. GitHub Pages/Jekyll was identified as the remaining cause because it did not serve the underscore-prefixed `_ds/` directory.
14. Adding and pushing `.nojekyll` fixed static asset delivery; the deployed CSS URL then worked.

## 6. Root Cause Analysis

### Git access and authentication

The initial Git setup required a valid remote, a configured commit identity, and an authentication method accepted by GitHub. HTTPS authentication with an account password fails because GitHub no longer accepts account passwords for Git operations. SSH is the configured solution for this repository.

### Incorrect staging command

`git add.` is not the Git command to stage the current directory. The missing space changes the command name. The correct command is:

```bash
git add .
```

### Missing design-system files

`index.html` depends on files inside `_ds/`, especially `styles.css`. Those files were initially excluded by an ignore rule, so Git could not include them in the published repository. As a result, GitHub Pages could not load the stylesheet.

### CSS still returning 404 after tracking `_ds/`

After the ignored files were added, the CSS still returned 404 on the live site. The remaining cause was GitHub Pages' default Jekyll behavior: Jekyll does not serve paths beginning with an underscore. Since the design-system directory is named `_ds/`, it was excluded from the generated site.

The empty `.nojekyll` file disables that processing and allows GitHub Pages to serve `_ds/` as a normal static directory.

## 7. Step-by-Step Solutions We Followed

### 1. Configure and verify Git

Symptom: Git could not be used reliably for the first repository commit and push.

Root cause: The remote and/or local Git user identity needed configuration.

Diagnostic checks:

```bash
git remote -v
git config --get user.name
git config --get user.email
```

Solution: Configure the Git remote for the repository and configure the Git user name and email when missing.

Verification: `git remote -v` showed the repository remote, and the two `git config --get` commands returned the expected identity.

### 2. Correct the staging command

Symptom: The attempted `git add.` command did not stage files.

Root cause: The command was missing the required space before `.`.

Diagnostic check:

```bash
git status --short
```

Solution:

```bash
git add .
```

Verification: `git status --short` showed the intended changes as staged before commit.

### 3. Move from HTTPS password authentication to SSH

Symptom: HTTPS push authentication failed.

Root cause: GitHub no longer accepts account passwords for Git operations over HTTPS.

Diagnostic checks:

```bash
git remote -v
ssh -T git@github.com
```

Solution: Set up SSH authentication with GitHub, verify it, and configure `origin` to use the SSH repository URL.

Verification: `ssh -T git@github.com` confirmed authentication, and a subsequent push completed successfully. Do not place SSH private keys or credential material in the repository.

### 4. Complete the initial GitHub push and configure Pages

Symptom: The site was not yet published from GitHub.

Root cause: The portfolio entry point and assets needed to be committed and pushed, then GitHub Pages needed a deployment source configured.

Diagnostic checks:

```bash
git status
git log --oneline -1
git remote -v
```

Solution: Commit the static site files, push the `main` branch, and configure GitHub Pages to deploy the repository's configured Pages source.

Verification: The GitHub repository contained the committed site, and the Pages site was available for testing.

### 5. Include the ignored design-system files

Symptom: The deployed page loaded without its intended CSS styling.

Root cause: `_ds/` was ignored by `.gitignore`, so the stylesheet and related design-system files were not committed.

Diagnostic check:

```bash
git check-ignore -v _ds/modernist-b1528d07-a821-4b48-b3b9-bcdb44b70864/styles.css
```

Solution: Remove the `_ds/` ignore rule from `.gitignore`, stage the required design-system files, and commit them.

Verification:

```bash
git ls-files _ds
git log --oneline -- _ds
```

The design-system files were tracked in commit `d8749f5` (`Fix portfolio CSS for GitHub Pages`).

### 6. Disable Jekyll processing for `_ds/`

Symptom: The CSS URL still returned HTTP 404 after `_ds/` had been committed.

Root cause: GitHub Pages' Jekyll processing did not serve the underscore-prefixed `_ds/` directory.

Diagnostic checks:

```bash
git ls-files _ds/modernist-b1528d07-a821-4b48-b3b9-bcdb44b70864/styles.css
```

Then open the deployed stylesheet URL in a browser and check its HTTP response. The file being present in Git but returning 404 on Pages isolated the issue to publishing behavior rather than Git tracking.

Solution: Create an empty `.nojekyll` file at the repository root, stage it, commit it, and push the fix.

Verification:

```bash
git ls-files .nojekyll
git log --oneline -- .nojekyll
```

Commit `4f2b02c` (`Fix GitHub Pages static asset loading`) added `.nojekyll`. After deployment, the live stylesheet URL loaded successfully.

## 8. Current Working Deployment Architecture

```text
main branch
  -> GitHub repository (origin via SSH)
  -> GitHub Pages static deployment
  -> index.html
       -> _ds/.../styles.css and _ds/.../_ds_bundle.js
       -> assets/*.svg, assets/prototype-video.mp4, assets/resume.pdf
```

`.nojekyll` is a required part of this architecture. Removing it can cause GitHub Pages to stop serving `_ds/`, which would make the site lose its design-system CSS and runtime assets.

## 9. Local Development and Testing Workflow

Before modifying anything:

```bash
git status --short
rg --files
```

Inspect the exact files related to the requested change and explain the problem and proposed solution before editing. Wait for explicit approval.

For static-site testing, serve the repository root with any local static HTTP server. For example, if Python is available:

```bash
python3 -m http.server 8000
```

Open `http://localhost:8000/` and test the changed behavior. Check at minimum:

- Home and case-study navigation.
- Browser Back/Forward behavior after switching views.
- Design-system CSS and JavaScript loading without 404s.
- SVG images, resume link, and prototype video.
- Desktop and narrow responsive layouts.

Stop the local server after testing. Do not treat a local `file://` open as a substitute for HTTP testing because deployment asset behavior can differ.

## 10. Git Commit and Push Workflow

After local testing and before any commit:

```bash
git status --short
git diff --check
git diff
git diff --cached
```

Stage only the approved files:

```bash
git add path/to/approved-file
git status --short
```

Create a clear commit only after the user explicitly asks to commit:

```bash
git commit -m "Describe the approved change"
```

Push only when the user explicitly asks to push:

```bash
git push origin main
```

Never push automatically. After a push, wait for GitHub Pages to deploy and verify the relevant live URL, especially any changed static asset URLs.

## 11. GitHub Pages Troubleshooting

When a live style, script, image, video, or PDF does not load:

1. Confirm the HTML path is correct and relative to `index.html`.
2. Confirm the exact file is tracked:

   ```bash
   git ls-files path/to/file
   ```

3. Check whether an ignore rule affects it:

   ```bash
   git check-ignore -v path/to/file
   ```

4. Review the committed changes:

   ```bash
   git log --oneline -- path/to/file
   git show --stat --oneline HEAD
   ```

5. Test the asset URL directly in the deployed site.
6. If a required directory begins with `_`, confirm `.nojekyll` is present and tracked:

   ```bash
   git ls-files .nojekyll
   ```

7. Allow the Pages deployment to finish, then test again.

Do not rename `_ds/` casually: its current name is referenced directly by `index.html` and its contents form the established design system.

## 12. Important Lessons Learned

- A successful commit does not guarantee that GitHub Pages will publish every file as expected.
- `git check-ignore -v` is the fastest way to find the exact ignore rule responsible for an ignored production file.
- If GitHub Pages must serve a directory beginning with `_`, `.nojekyll` is necessary.
- The presence of `.nojekyll` must be verified in Git, not merely on a local machine.
- Test the deployed asset URL directly when a page appears unstyled; it distinguishes missing CSS from a general page failure.
- Use SSH for this repository's GitHub authentication; never store authentication secrets in project files.
- `git add .` includes a space. Prefer staging specific approved files to avoid accidental commits.
- Preserve current relative paths; they are part of the working GitHub Pages deployment.

## 13. Future Maintenance Workflow

For every future portfolio change:

1. Inspect the relevant files and current Git state.
2. Explain the observed problem, affected files, proposed solution, and any deployment risk.
3. Wait for explicit approval before editing.
4. Make only the approved change.
5. Test locally through a static HTTP server.
6. Check `git status`, `git diff --check`, and the full diff with the user before committing.
7. Commit only with explicit approval.
8. Push only with explicit approval.
9. After a push, verify the GitHub Pages result and direct URLs for any changed assets.

Keep `.nojekyll`, the tracked `_ds/` design-system files, and the asset paths intact unless a migration has been explicitly planned, implemented, and tested end-to-end.
