---
name: pr-review-comments
description: Check and address GitHub PR review comments for the current branch
---

# Review PR Comments

Check review comments on the GitHub PR associated with the current branch and help address each one.

## Instructions

1. **Get the current branch and find the associated PR:**
   ```bash
   gh pr view --json number,url,title,reviewDecision,reviews,comments
   ```

2. **Fetch all review comments on the PR:**
   ```bash
   gh api repos/{owner}/{repo}/pulls/{pr_number}/comments --jq '.[] | {id, path, line, body, user: .user.login, created_at, in_reply_to_id}'
   ```

3. **Also check for general PR comments:**
   ```bash
   gh pr view --comments
   ```

4. **For each unresolved comment:**
   - Display the comment text, file path, and line number (if applicable)
   - Read the relevant code context around the commented line
   - Analyze the feedback and suggest a solution
   - Present the suggestion to the user with this format:

   ```
   ## Comment from @{username} on {file}:{line}
   
   > {comment text}
   
   ### Suggested Solution
   {description of the suggested change}
   
   ### Code Change
   {show the specific code change}
   ```

5. **Ask the user:**
   - "Would you like me to implement this solution, or would you prefer to address it differently?"
   - Wait for user response before proceeding
   - If user wants custom solution, ask for their approach

6. **After addressing each comment:**
   - Move to the next unresolved comment
   - Continue until all comments have been reviewed

7. **Summary:**
   - At the end, provide a summary of:
     - Comments addressed
     - Comments skipped
     - Any remaining action items

## Notes
- Skip comments that are replies (have `in_reply_to_id` set) unless the original hasn't been addressed
- Group comments by file for easier context
- If a comment references code that has already been changed, note this to the user
