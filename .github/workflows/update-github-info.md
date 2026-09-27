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
6. **Update the reference file** at `site/content/github-info.md` with:
   - Key announcements and updates from the GitHub blog
   - Recent changelog entries
   - Interesting or useful workflows from Awesome Copilot
   - Any critical information for GitHub users
   - Maintain the existing format and structure

7. **Create a pull request** with:
   - Title: "chore: update github info with latest announcements"
   - Description: Summarize what new information was added
   - Assign for review by Mona (if possible, or leave for team review)
   - Body should include a brief summary of the blog posts, changelog items, and workflows included
   - Include explicit source context such as `GitHub Blog`, `GitHub Changelog`, and `awesome-copilot.github.com` in the PR description or diff summary so reviewers can trace where each update came from
   - Ensure the generated PR actually updates `site/content/github-info.md` and not just the workflow file

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
- Do not run or compile this workflow; only update the content and create the PR if needed
