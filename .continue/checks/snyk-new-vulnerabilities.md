---
name: "Snyk: New vulnerabilities"
rules: continuedev/gh-pr-commit-workflow
---

A new Snyk vulnerability has been detected. Please investigate and resolve the issue. This should include the following steps:

**Step 1: Investigate the Issue**
Make sure you understand the vulnerability, the options for resolution, and what their consequences are.

**Step 2: Implement Fix**
- Focus on fixing the immediate issue identified
- Avoid overdoing it with error handling, cleaning up other problems, etc.
- Avoid making breaking changes
- Ensure the solution is robust and follows best practices.

**Step 3: Create Draft Pull Request**
Create a draft pull request with the following structure:

---
**PR Title:** [Snyk] <brief description of issue solved>

## Issue

**Snyk Link:** [<shortId>](<permalink>)
**Issue Type:** `<issue type>`
**Priority:** <priority>
**Summary:** <Two sentence summary of what caused the issue and how it was fixed>

## Additional Context

<details>
<summary>Snyk Issue Details</summary>

```json
<Complete issue details from webhook payload>
```

</details>

---
**Note for AI:** 
- Replace `<agent-session-id>` with the actual agent session ID

---
Below is the webhook payload from the Snyk event: