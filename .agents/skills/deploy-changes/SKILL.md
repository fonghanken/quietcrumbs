---
name: deploy-changes
description: Commits the current code changes to a new branch, pushes to origin, and creates a GitHub Pull Request using the gh CLI. Trigger this when the user asks to "deploy changes", "create a PR", or "commit and PR".
---

# Deploy Changes Skill

This skill helps you automate the process of committing your code, pushing it to a remote branch, and creating a GitHub Pull Request.

## Prerequisites

Before executing these steps, ensure:
1. The user has provided an intent or you have finished making changes.
2. You have the `git` and `gh` CLI available.

## Steps to Execute

1. **Check Status**: Run `git status` to identify modified and untracked files.
2. **Determine Branch**: 
   - If you are on `main` or `master`, create and checkout a new branch (e.g., `git checkout -b fix-xyz`).
   - If you are already on a feature branch, you can skip this step unless instructed otherwise.
3. **Stage Changes**: Add the modified files using `git add <files>...` or `git add .`.
4. **Commit**: Run `git commit -m "<Descriptive commit message>"`.
5. **Push**: Push the branch to the remote repository using `git push -u origin <branch-name>`.
6. **Create PR**: Run `gh pr create --title "<PR Title>" --body "<PR Description>"` to open the pull request. Wait for the command to return the PR URL.
7. **Report**: Inform the user that the PR has been created and provide them with the URL returned by the `gh` command.

## Important Notes

- Do not attempt to run `gh pr create` without having committed and pushed the changes to the remote first.
- If the PR requires a specific target branch, use `gh pr create --base <branch>`.
- Make sure to use descriptive commit messages and PR titles based on the changes you made in the conversation.
