---
name: update-github-info
description: Updates GitHub info from blog, changelog, and Awesome Copilot workflows for Mona's reference
engine: copilot
model: gpt-4
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
    - github
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
2. **Fetch latest GitHub blog content** from https://github.blog/latest/
3. **Fetch GitHub changelog** from https://github.blog/changelog/
4. **Fetch Awesome Copilot workflows** from https://awesome-copilot.github.com/workflows/
5. **Update the reference file** at `site/content/github-info.md` with:
   - Key announcements and updates from the GitHub blog
   - Recent changelog entries
   - Interesting or useful workflows from Awesome Copilot
   - Any critical information for GitHub users
   - Maintain the existing format and structure

6. **Create a pull request** with:
   - Title: "chore: update github info with latest announcements"
   - Description: Summarize what new information was added
   - Assign for review by Mona (if possible, or leave for team review)
   - Body should include a brief summary of the blog posts, changelog items, and workflows included

## Guidelines

- Preserve the existing structure of `site/content/github-info.md`
- Include dates and source links where relevant
- Focus on information that would be useful for GitHub users
- Keep the tone professional and informative
- Only include substantive updates, not minor announcements
