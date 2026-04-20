---
name: Broken Labs style guide
description: Canonical conventions for all AWS Broken Labs — file structure, naming, frontmatter, navigation, hints formatting
type: project
---

All broken labs must follow these conventions, derived from the S3 and EC2 lab implementations.

## Directory structure

```
docs/aws/labs/<service>/
  index.md                           ← overview page for the service (lists all labs)
  lab-##-<title-slug>/
    index.md                         ← lab page (Jekyll-rendered)
    hints.md                         ← hints page (Jekyll-rendered)
    lab-##-<title-slug>.yaml         ← CloudFormation template (downloadable)
```

## File naming

- Lab directories: `lab-##-<title-slug>/` — slug is the lab title lowercased, spaces replaced with hyphens
- YAML files: `lab-##-<title-slug>.yaml` — slug derived from the H1 heading in `index.md`
- Title slug examples: `lab-01-ec2-security-groups`, `lab-03-ec2-elastic-ip`

## index.md frontmatter (canonical pattern)

```yaml
---
layout: default
title: "Lab ##: <Title>"
lab_level: <foundational|associate|professional|specialty>
lab_service: <s3|ec2|iam|...>
lab_number: "##"
---
```

## hints.md frontmatter (required for all labs)

```yaml
---
layout: default
---
```

## index.md section order

1. `# Lab ## - <Title>`
2. Blockquote metadata: `**Difficulty**`, `**Service**`, `**Cost**`
3. `## Scenario`
4. `## What Was Deployed` (table of AWS resources)
5. `## Deploy the Lab` (numbered steps)
6. `## The Problem` (Expected vs Actual)
7. `## Fix the Lab`
8. `## Cleanup`
9. `## Resources` (AWS docs links)

## Deploy the Lab — step 3 YAML download link

Always use this exact HTML pattern (no markdown link, no "Download" label):

```html
3. Select **Upload a template file** and upload <a href="lab-##-<slug>.yaml" download>lab-##-<slug>.yaml</a>
```

## CloudFormation stack name convention

```
brokenlabs-<service>-lab-##
```
Example: `brokenlabs-ec2-lab-01`, `brokenlabs-s3-lab-03`

## hints.md structure

- Frontmatter: `layout: default`
- Every `<details>` tag must include `markdown="1"`: `<details markdown="1">`
  **Why:** kramdown does not process markdown inside HTML blocks without this attribute;
  omitting it causes numbered lists, bold, links, and code blocks to render as plain text.
- Four collapsible sections per lab:
  - Hint 1 — Where to look / first clue
  - Hint 2 — Narrowing down
  - Hint 3 — What needs to be done
  - `<span style="color: red;">Spoiler Alert</span> — Full Solution`
- Full Solution section contains: **Root cause** paragraph, `---` divider, **To fix:** numbered list
- Each step in **To fix:** must be a separate numbered list item (one action per line)

## Navigation (_data/navigation.yml)

- Service submenus under **AWS Broken Labs** are alphabetical by service name
- Each service submenu structure:
  ```yaml
  - title: <Service> Troubleshooting
    url: /docs/aws/labs/<service>
    items:
      - title: "<span style='color: red;'>START HERE</span>"
        url: /docs/aws/labs/<service>
      - title: "Lab ## — <Title>"
        url: /docs/aws/labs/<service>/lab-##-<slug>
  ```
- Lab titles in nav use em dash (—), not hyphen (-)
- Lab titles match the H1 heading in `index.md` (minus the `# ` prefix)
