---
name: create-pr
description: Open a pull request using the repository's PR template. Use when the user asks to create, open, or raise a PR, or to push work for review.
---

# Create a PR

Open PRs as drafts unless the user asks for a ready-for-review PR.

## Find the context first

Determine the target repository, default branch, and intended base branch.
Read the commits on the branch (`git log --oneline <base>..HEAD`) and the
diff against the base (`git diff <base>...HEAD`). Do not ask the user for
information you can get from the repository.

Get the Jira key from the branch name (for example `UNI-1779-...`). If the
branch name has no key, ask the user. Do not invent one.

Confirm the current branch is not the default branch. If it is, create a
branch before pushing. Check for an existing PR for the branch before
creating another.

## Prepare the PR body

Find the repository's PR template in `.github/`, the repository root, or
`docs/`, including any `PULL_REQUEST_TEMPLATE/` directories. If there are
multiple templates, select the one that matches the change.

If there is no local template, check the owning organization's public
`.github` repository for its default PR template, using its default branch.
If no template exists, write a concise body covering the problem, solution,
validation, and risks.

Read the selected template, including its HTML comments. Follow its
structure and instructions. Replace placeholders, fill required sections,
and remove optional sections that do not apply. Remove instructional
comments from the finished body.

## Stacked PRs

When the PR depends on another open PR, use that PR's head branch as the
base and add a line at the top of the body:

> Stacked on #NNNN. Review that PR first.

Tell the user which parent PR it depends on. After the parent merges,
verify the base and diff. Squash or rebase merges may require rebasing the
child onto the target branch to remove the parent's changes from the diff.
Do not rewrite a published branch as part of creating the PR.

## Publish

1. Push the branch with `git push -u origin <branch>`.
2. Write the completed body to a temporary file.
3. Create the PR with an explicit title and base:
   `gh pr create --draft --base <base> --title <title> --body-file <path>`.
   Omit `--draft` when the user requests a ready-for-review PR.
4. Give the user the PR URL and mention any validation that remains undone.
