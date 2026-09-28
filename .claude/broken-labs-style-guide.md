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
  lab-##-<generic-slug>/
    index.md                         ← lab page (Jekyll-rendered)
    hints.md                         ← hints page (Jekyll-rendered)
    lab-##-<generic-slug>.yaml       ← CloudFormation template (downloadable)
```

## No-giveaway rule (applies to everything a learner sees before hints.md)

Nothing outside `hints.md` may reveal or suggest the root cause. Use generic,
workload- or symptom-level wording only.

- **Names** — folder, template, URL, `title:`, H1, nav label, and index table cells use a
  generic label plus a Roman numeral when labels repeat: `Web Access II`, `Access Denied IV`,
  `Model Training III`. Never name the broken component (`nacl`, `kms-key-policy`,
  `missing-ssm-policy`, `no-public-ip`).
- **Scenario / The Problem** — describe the setup, the symptom, and the observed error only.
  No "what's been ruled out" lists ("the security group is correct…"), no steering
  questions ("so what is it missing?"), no naming the culprit.
- **Fix the Lab** — state the goal only ("Diagnose why … and fix it so the result matches
  Expected"). Neutral operational help (how to open a Session Manager terminal) is fine.
- **What Was Deployed** — list resources with neutral purposes; no notes like
  "auto-assign public IP is disabled" or "role with no policies".
- **Cost / Cleanup** — no fix-specific wording. Put cleanup steps that only apply after the
  fix (detach a role you created, delete a NAT gateway you created) in the hints.md Full
  Solution; the page gets a generic "delete anything you created outside the stack" note.
- **Resources** — general service guides only. Cause-specific links go under a
  **References** block at the end of the hints.md Full Solution.
- **Templates** — no `# THE BREAK` (or any) comments marking the defect; resource names,
  resource `Description`s, header `Description`, and Output descriptions stay neutral
  (no "(will load once fixed)", no "permissions boundary" output).
- **Commit messages** — generic too; the repository is public.

## File naming

- Lab directories: `lab-##-<generic-slug>/` — slug is the generic lab title lowercased,
  spaces replaced with hyphens (e.g. `lab-02-web-access-ii`)
- YAML files: `lab-##-<generic-slug>.yaml` — same slug as the directory
- **Renaming a published lab**: add `redirect_from: /docs/aws/labs/<service>/<old-slug>/`
  to the lab's `index.md` front matter (`jekyll-redirect-from` is enabled in `_config.yml`)
  so shared links keep working

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

Difficulty tiers are **Beginner**, **Intermediate**, and **Expert** (not "Advanced") — in
the `> **Difficulty**:` line, module index headings/tables, and navigation.

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
  - title: <Service name>
    url: /docs/aws/labs/<service>
    items:
      - title: Beginner | Intermediate | Expert
        items:
          - title: "Lab ## — <Generic Title>"
            url: /docs/aws/labs/<service>/lab-##-<generic-slug>
  ```
- The same module blocks are repeated under each exam's **Hands-on Labs** submenu
  (SOA-C03, DVA-C02, SAA-C03, MLA-C01) — keep every copy identical
- Lab titles in nav use em dash (—), not hyphen (-)
- Lab titles match the H1 heading in `index.md` (minus the `# ` prefix)
