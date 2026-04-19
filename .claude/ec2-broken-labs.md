---
name: EC2 Broken Labs session changes
description: All modifications made when adding EC2 Troubleshooting labs to the site
type: project
---

## Changes made on 2026-04-19 (branch: EC2)

### Navigation (_data/navigation.yml)
- Added **EC2 Troubleshooting** submenu under **AWS Broken Labs**, ordered alphabetically before S3 Troubleshooting
- START HERE entry uses `<span style='color: red;'>START HERE</span>` matching S3 pattern
- Six lab entries named from README.md H1 headings, using em dash (—) to match S3 style
- URLs follow pattern `/docs/aws/labs/ec2/<lab-dir>`

### YAML file renames
Old names → New names (derived from `# Lab ## - Title` in each README.md H1):
- `lab-01-http-blocked.yaml` → `lab-01-ec2-security-groups.yaml`
- `lab-02-instance-connect.yaml` → `lab-02-ec2-instance-connect.yaml`
- `lab-03-no-public-ip.yaml` → `lab-03-ec2-elastic-ip.yaml`
- `lab-04-no-iam-role.yaml` → `lab-04-ec2-ssm-session-manager.yaml`
- `lab-05-missing-ssm-policy.yaml` → `lab-05-ssm-session-manager.yaml`
- `lab-06-user-data.yaml` → `lab-06-ec2-user-data.yaml`

### README.md → index.md rename (all 7 EC2 files)

- `docs/aws/labs/ec2/README.md` → `index.md`
- `docs/aws/labs/ec2/lab-0*/README.md` → `index.md` (all 6 labs)
- Done with `git mv` to preserve history
- Added Jekyll frontmatter to each lab `index.md` matching S3 canonical pattern:

  ```yaml
  ---
  layout: default
  title: "Lab ##: <Title>"
  lab_level: associate
  lab_service: ec2
  lab_number: "##"
  ---
  ```

- The top-level `docs/aws/labs/ec2/index.md` (overview page) was renamed only — no frontmatter added yet
- Nav references (`/docs/aws/labs/ec2/lab-##-<slug>`) are unaffected — Jekyll resolves `index.md` automatically

### Download links in each lab's index.md (step 3 of Deploy the Lab)

Pattern matches S3 labs exactly:

```html
<a href="lab-01-ec2-security-groups.yaml" download>lab-01-ec2-security-groups.yaml</a>
```

### hints.md fixes (all 6 labs)
- Added Jekyll frontmatter (`layout: default`) — required for Jekyll to render the page
- Changed `<details>` → `<details markdown="1">` on every collapsible block — required for
  kramdown to process markdown (numbered lists, bold, links, code) inside HTML blocks;
  without this attribute, list steps render as one continuous paragraph

**Why:** The S3 hints.md files already had both of these. The EC2 hints were created without
them, causing the To Fix steps to appear as unformatted text instead of a numbered list.
