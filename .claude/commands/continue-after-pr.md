# Continue Work After PR Creation

You are the orchestrator agent for continuing development workflow after a PR has been created.

## Context
This command assumes:
- A PR has already been created
- The optimal worktree has been identified in `decision.md`
- You are ready to proceed with the review and final implementation phases

## User Input
The user should provide:
- The PR URL or PR number to work on
- Optionally, the worktree path (if not using the one from decision.md)

## Workflow Steps

### Step 1: Setup and Verification
1. Read `decision.md` to identify the optimal worktree path
2. Verify the PR exists and get its details using `gh pr view {pr_number}`
3. Confirm the worktree directory exists and is accessible
4. Switch to the optimal worktree if needed

### Step 2: Review Phase
Launch 10 review agents in parallel to review the PR from different perspectives:
- Use Task tool to create 10 parallel review agents
- Each agent reviews from a different perspective:
  1. Code quality and maintainability
  2. Performance and optimization
  3. Security vulnerabilities
  4. Error handling and edge cases
  5. Testing coverage and test quality
  6. Documentation completeness
  7. API design and usability
  8. Scalability concerns
  9. Accessibility compliance
  10. Code consistency with existing patterns
- Aggregate all reviews into `review.md`
- Each review should include:
  - Perspective focus area
  - Strengths identified
  - Issues/concerns found
  - Specific recommendations with file:line references

### Step 3: Final Implementation
Launch the final implementation agent:
- Read `review.md` for all feedback
- Work in the optimal worktree identified earlier
- Implement all review feedback systematically
- For each feedback item:
  - Mark as in_progress in TodoWrite
  - Implement the fix/improvement
  - Mark as completed
- Run tests if they exist
- Commit changes with descriptive message following format:
  ```
  Address review feedback: {summary}

  - {specific change 1}
  - {specific change 2}
  - {specific change 3}

  🤖 Generated with [Claude Code](https://claude.com/claude-code)

  Co-Authored-By: Claude <noreply@anthropic.com>
  ```
- Push changes to update the PR

### Step 4: Verification
1. Verify all changes are pushed: `git status` and `git log`
2. Check PR status: `gh pr view {pr_number}`
3. Confirm all review feedback has been addressed
4. Provide summary of:
   - Number of review items addressed
   - Files modified
   - Commits created
   - PR update status

## Important Notes
- Always work systematically through each phase
- Use TodoWrite to track progress across all phases
- Each phase must complete before moving to next
- All agents run autonomously - provide complete context
- Keep main agent focused on orchestration, not implementation
- If no decision.md exists, ask user for worktree path
- Document all changes clearly in commit messages
