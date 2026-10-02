---
name: update-github-info
description: Updates GitHub info from blog, changelog, and Awesome Copilot workflows for Mona's reference
engine: copilot
model: gpt-4.1
on:
  schedule:
    - cron: '0 9 * * *'
  workflow_dispatch:
permissions:
  contents: read
  issues: read
  pull-requests: read
network:
  allowed:
    - github.com
    - github.blog
    - awesome-copilot.github.com
tools:
  edit: true
  github: true
  web-fetch: {}
safe-outputs:
  create-pull-request:
    base-branch: main
    fallback-as-issue: false
---

# Update GitHub Info Workflow

You are an assistant helping to keep Mona's reference materials current with the latest GitHub news and updates.

## Tasks

1. **Read Mona's existing notes** from `notes/mona-notes.md` to understand context and style
2. **Fetch the latest GitHub blog content** from https://github.blog/latest/
3. **Fetch the GitHub changelog** from https://github.blog/changelog/
4. **Fetch content from Awesome Copilot workflows** at https://awesome-copilot.github.com/workflows/
5. **Add Awesome Copilot workflows as a source** alongside the existing GitHub blog and changelog sources
6. **If outbound network access is blocked**, do not stop the workflow. Instead:
   - Prefer `gh api` or other GitHub-hosted APIs available through `github.com`
   - Use the locally available repo context from `notes/mona-notes.md` and `site/content/github-info.md`
   - Use public GitHub references already available in the repository and the GitHub domain when external fetches are denied
   - Continue by drafting the update from the available, verified GitHub sources rather than failing the run
7. **Update the reference file** at `site/content/github-info.md` with:
   - Key announcements and updates from the GitHub blog
   - Recent changelog entries
   - Interesting or useful workflows from Awesome Copilot
   - Any critical information for GitHub users
   - Maintain the existing format and structure

8. **Create a pull request** with:
   - Title: "chore: update github info with latest announcements"
   - Description: Summarize what new information was added
   - Assign for review by Mona (if possible, or leave for team review)
   - Body should include a brief summary of the blog posts, changelog items, and workflows included
   - Include explicit source context such as `GitHub Blog`, `GitHub Changelog`, and `awesome-copilot.github.com` in the PR description or diff summary so reviewers can trace where each update came from
   - Ensure the generated PR actually updates `site/content/github-info.md` and not just the workflow file
   - Before finishing, run `gh pr list --state open --base main` and `gh pr diff` to verify there is an open PR that changes `site/content/github-info.md`
   - If no valid PR exists, create one with `gh pr create` or update the existing PR until it includes the website file and source references
   - Do not consider the task complete until the PR is open, references the required sources, and includes the website update file
   - Before you finish, verify that an open pull request exists and that it modifies `site/content/github-info.md` with the new GitHub updates
   - Do not consider the task complete until the PR is open, references the required sources, and includes the website update file

## Guidelines

- Preserve the existing structure of `site/content/github-info.md`
- Keep the existing access to GitHub Blog and GitHub changelog sources intact
- Add Awesome Copilot workflows as a supported source using https://awesome-copilot.github.com/workflows/
- Include dates and source links where relevant
- Add a clear source label when content comes from the GitHub Blog, GitHub Changelog, or Awesome Copilot, for example `Source: GitHub Blog` or `Source: GitHub Changelog`
- Focus on information that would be useful for GitHub users
- Keep the tone professional and informative
- Only include substantive updates, not minor announcements
- Include source context in the generated PR description so it mentions `GitHub Blog`, `GitHub Changelog`, and `awesome-copilot.github.com`
- The final status must be an open PR that changes `site/content/github-info.md`; do not finish with only workflow changes or with a PR that does not touch the website content
- If no valid PR is open yet, keep working until one exists and includes the required source references
- Use `gh pr list` and `gh pr diff` as final verification before responding; if the PR is missing or invalid, remedy it before finishing
- If outbound network access is restricted, switch to GitHub-hosted APIs or local repo context and keep working instead of failing early
- Do not run or compile this workflow; only update the content and create the PR if needed
