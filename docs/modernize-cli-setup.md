# Integrating the Modernize CLI into Your CI/CD Pipeline

This guide walks through setting up the [GitHub Copilot Modernization CLI](https://learn.microsoft.com/en-us/azure/developer/github-copilot-app-modernization/modernization-agent/cicd-integration) in a GitHub Actions workflow to automatically upgrade your Java project.

## Overview

The Modernize CLI is an AI-powered tool that automates application modernization (e.g., upgrading Java versions). By integrating it into CI/CD, you can:

- Automate upgrades on a schedule or on-demand
- Push changes to a dedicated branch for review via PR
- Track results and logs as build artifacts

## Prerequisites

1. **GitHub Copilot subscription** — Free, Pro, Pro+, Business, or Enterprise
2. **GitHub Personal Access Token (PAT)** — with Copilot access scopes

## Step 1: Create a GitHub PAT

1. Go to [github.com/settings/tokens](https://github.com/settings/tokens)
2. Click **Generate new token** → **Fine-grained token**
3. Name it (e.g., `modernize-cli`)
4. Under **Repository access**, select the target repo
5. Under **Permissions**, grant:
   - **Contents**: Read and write
   - **Copilot**: Read

## Step 2: Add the Token as a Repository Secret

Run in your terminal (with `gh` CLI authenticated):

```bash
gh secret set GH_TOKEN --repo <owner>/<repo>
```

Paste your token when prompted. **Never expose tokens in plain text or commit them to source.**

## Step 3: Create the Workflow File

Create `.github/workflows/modernize.yml`:

```yaml
name: Modernization CLI

on:
  workflow_dispatch:
    inputs:
      upgrade_target:
        description: 'Upgrade target (e.g., Java 25)'
        required: false
        default: 'Java 25'
  schedule:
    - cron: '0 2 * * *'

permissions:
    id-token: write
    contents: write
    actions: read

jobs:
  modernization:
    runs-on: ubuntu-latest
    env:
      GH_TOKEN: ${{ secrets.GH_TOKEN }}

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Download Modernize CLI
        run: |
          curl -fsSL https://raw.githubusercontent.com/microsoft/modernize-cli/main/scripts/install.sh | sh

      - name: Run Modernize CLI to upgrade code
        run: |
          TARGET="${{ github.event.inputs.upgrade_target }}"
          if [ -z "$TARGET" ] || [ "$TARGET" = "latest" ]; then
            modernize upgrade "Java 25" --source . --no-tty
          else
            modernize upgrade "$TARGET" --source . --no-tty
          fi

      - name: Push changes to result branch
        id: push_changes
        run: |
          BRANCH_NAME="modernize-upgrade-$(echo '${{ github.event.inputs.upgrade_target || 'Java-25' }}' | tr ' ' '-')-$(date +%Y%m%d-%H%M%S)"

          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"

          git add -A
          git reset .github/workflows
          git diff --cached --quiet || git commit -m "chore: apply Modernize CLI changes [skip ci]"
          git checkout -B "$BRANCH_NAME"
          git push origin "$BRANCH_NAME"

          echo "BRANCH_NAME=$BRANCH_NAME" >> $GITHUB_OUTPUT

      - name: Display results summary
        if: success()
        run: |
          cat >> $GITHUB_STEP_SUMMARY <<EOF
          ## Modernization Complete

          ### Branch Information
          - **Result Branch**: \`${{ steps.push_changes.outputs.BRANCH_NAME }}\`
          - **Target**: ${{ github.event.inputs.upgrade_target || 'Java 25' }}

          ### Links
          - [View Branch](https://github.com/${{ github.repository }}/tree/${{ steps.push_changes.outputs.BRANCH_NAME }})
          - [Create PR](https://github.com/${{ github.repository }}/compare/main...${{ steps.push_changes.outputs.BRANCH_NAME }})
          EOF

      - name: Upload Modernize CLI logs
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: modernize-logs
          path: ~/.modernize/logs/
          if-no-files-found: warn
```

## Step 4: Trigger the Workflow

1. Go to your repo → **Actions** tab
2. Select **Modernization CLI**
3. Click **Run workflow**
4. Optionally change the upgrade target (default: `Java 25`)
5. Click **Run workflow** to confirm

Or trigger via CLI:

```bash
gh workflow run modernize.yml --field upgrade_target="Java 25"
```

## Step 5: Review and Merge

After the workflow completes:

1. Check the **Actions summary** for a link to the result branch
2. Review the changes on the branch
3. Create a Pull Request to merge into `main`
4. Review the upgrade changes before merging

## Key Lessons Learned

| Issue | Solution |
|-------|----------|
| `--source is required` error | Always pass `--source .` to point the CLI at the checked-out repo |
| Invalid branch name (spaces) | Sanitize the target name with `tr ' ' '-'` |
| Authentication failed | Ensure `GH_TOKEN` secret is set with a valid PAT that has Copilot scopes |
| Node.js deprecation warnings | Use `actions/checkout@v5` and `actions/upload-artifact@v5` when available |

## Troubleshooting

- **Check logs**: Download the `modernize-logs` artifact from the workflow run
- **Authentication**: Verify your PAT hasn't expired and has the correct permissions
- **No changes**: The CLI may determine no upgrade is needed — check logs for details

## References

- [Microsoft Docs: CI/CD Integration with Modernize CLI](https://learn.microsoft.com/en-us/azure/developer/github-copilot-app-modernization/modernization-agent/cicd-integration)
- [GitHub: Managing Personal Access Tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)
