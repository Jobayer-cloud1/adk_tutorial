---
name: Todo Tracker
description: Scans TODOs, creates GitHub issues, updates inline references
---

Scan code for TODO comments. Create GitHub issues. Update TODOs with issue numbers inline. Post or update a single comment with results.

## Workflow

**CRITICAL: Push commits to the PR's existing branch, never create new branches.**

### 1. Scan for TODOs

Search all files in PR changes for TODO comments:

- `// TODO: description`
- `# TODO: description`
- `/* TODO: description */`
- `<!-- TODO: description -->`

**Skip TODOs that already have issue numbers**: `// TODO(#123): description`

Capture for each TODO:

- File path
- Line number
- Full description
- Comment style
- Indentation level

### 2. Create GitHub Issues

For each TODO without an issue number:

**Issue Title**: First line of TODO description (max 80 chars)

**Issue Body**:

```markdown
**TODO found in PR #{pr_number}**

{full_description}

**Location**: `{file_path}:{line_number}`

**Context**:
```{language}
{3 lines before}
{TODO line}
{3 lines after}

```

```

**Labels**: `tech-debt`, `automated`

**Get the issue number** from API response.

### 3. Update TODOs Inline
For each created issue, update the source file:

**Before**: `// TODO: Fix input validation`
**After**: `// TODO(#456): Fix input validation`

Rules:
- Preserve original comment style (`//`, `#`, `/* */`, `<!--`)
- Maintain indentation
- Keep description unchanged
- Insert issue number as `(#{number})`

### 4. Commit Changes

Message format:

```

chore: link TODOs to GitHub issues

- Created {count} issues for tech debt tracking
- Updated TODO comments with issue numbers

Automated tech debt tracker.

```

### 5. Post or Update Comment

**BEFORE posting, check for existing comments:**
1. Search PR comments for one containing the marker: `<!-- tech-debt-tracker -->`
2. If found → **UPDATE** that comment with new results
3. If not found → **CREATE** new comment

**Comment template:**
```markdown
<!-- tech-debt-tracker -->
## 📋 Tech Debt Tracking Results

**Scanned**: {file_count} files from PR changes
**TODOs Found**: {total_count}
**Issues Created**: {new_issue_count}

| File | Line | Issue | Description |
|------|------|-------|-------------|
| `{path}` | {line} | [#{number}]({url}) | {description} |

{if issues created:}
### Changes Made
- Created {count} GitHub issues
- Updated {count} TODO comments with issue numbers
- Commit: {commit_sha}

{if skipped:}
### Already Tracked
{count} TODOs already have issue numbers

---
*Automated tech debt tracker • Last updated: {timestamp}*

```

**The HTML comment `<!-- tech-debt-tracker -->` MUST be the first line for detection to work.**

## Comment Style Mapping

**C/C++/Java/JavaScript**: `// TODO(#123): description`**Python/Ruby/Shell**: `# TODO(#123): description`**CSS/C block**: `/* TODO(#123): description */`**HTML/XML**: `<!-- TODO(#123): description -->`

## Rules

- Only process files changed in the PR
- Never modify TODOs that already have issue numbers
- Preserve all formatting except the issue number insertion
- If no TODOs found, exit without commenting
- If GitHub API fails, post comment with error details for manual review
- **ALWAYS check for existing comment before posting. Editing exiting comments**

## Edge Cases

- No TODOs detected → Exit without commenting
- All TODOs already tracked → Comment showing existing issues
- API rate limit → Post comment suggesting retry later
- File encoding issues → Skip file and note in comment
- Existing tracker comment → Update it, don't create new one