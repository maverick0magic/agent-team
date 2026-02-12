---
name: review
description: Trigger a Codex code review on specific files, staged changes, or recent commits. Loads the project's review checklist and spawns the Codex agent.
arguments:
  - name: target
    description: "File paths, git ref, or 'staged' (defaults to staged/recent changes)"
    required: false
user-invocable: true
---

# Codex Code Review

You are running a **Codex Code Review**. This spawns the Codex Agent to review code against the project's 8-point checklist.

## Step 1: Determine Review Target

{{#if args.target}}
Review target specified: **{{args.target}}**

Parse the target:
- If it's a file path or glob → review those files
- If it's a git ref (commit hash, branch name) → review changes in that ref
- If it's "staged" → review staged changes
{{else}}
No target specified. Auto-detect what to review:

1. Check for staged changes: `git diff --cached --name-only`
2. If no staged changes, check recent uncommitted changes: `git diff --name-only`
3. If no uncommitted changes, review the most recent commit: `git log -1 --name-only`
4. If none of the above, ask the user what to review
{{/if}}

## Step 2: Gather Files

Collect the list of files to review. For each file:
- Read the full file content
- Note the file path, language, and line count
- Skip binary files, lock files, and generated files

## Step 3: Load Review Checklist

Read the project's review checklist in this order of preference:
1. `.agents/codex/review-checklist.md` (project-specific, customized during kickoff)
2. If not found, use the generic checklist from the plugin's `skills/orchestrator/review-checklist.md`

## Step 4: Spawn Codex Agent

Dispatch the Codex Agent using the `Task` tool with `model: "opus"`:

**Prompt for Codex Agent:**
```
You are the Codex Agent performing a code review. Read the codex-agent definition for your full protocol.

## Review Checklist
[Include the full checklist content]

## Files to Review
[List each file with its content]

## Instructions
1. Review every file against all 8 checklist categories
2. Classify findings by severity: Critical, High, Medium, Low
3. Write a structured review report
4. Provide a verdict: APPROVE, APPROVE WITH COMMENTS, REQUEST CHANGES, or REJECT

## Output Format
Write your review following the standard Codex review format:
- Summary of what was reviewed
- Findings organized by the 8 checklist categories
- Action items table with severity, file, issue, and recommendation
- Final verdict with rationale
```

## Step 5: Save Review

After the Codex Agent returns:

1. Generate a review filename:
   - If `.agents/codex/reviews/` exists, use incrementing format: `review-[NN]-[topic].md`
   - If not, create the directory first

2. Write the review report to `.agents/codex/reviews/[filename]`

3. If `.agents/shared/handoffs/coding-to-codex.md` exists:
   - Add entries to the "Feedback Ready" section with the review file path and verdict

## Step 6: Report Results

Present a summary to the user:

```markdown
## Code Review Complete

**Files reviewed**: [N] files ([total lines] lines)
**Verdict**: [APPROVE / APPROVE WITH COMMENTS / REQUEST CHANGES / REJECT]

### Findings Summary
| Severity | Count |
|----------|-------|
| Critical | [N] |
| High | [N] |
| Medium | [N] |
| Low | [N] |

### Key Issues
[Top 3-5 most important findings]

**Full review**: `.agents/codex/reviews/[filename]`
```

If the verdict is REQUEST CHANGES or REJECT, suggest:
> Address the findings and run `/agent-team:review` again to re-check.
