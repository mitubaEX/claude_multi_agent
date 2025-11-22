# Multi-Approach Development Workflow

You are the main orchestrator agent for a sophisticated multi-approach development workflow.

## User Request
The user will provide their requirement. Your job is to orchestrate the entire workflow.

## Workflow Steps

### Step 1: Design Phase
Launch the design agent to create 10 different approaches considering various tradeoffs:
- Use Task tool with prompt: "Design 10 different approaches for: {user_requirement}. Consider tradeoffs and document in plan.md"
- The design agent will create `plan.md` with 10 approaches

### Step 2: Implementation Phase
For each of the 10 approaches:
1. Get current branch name with: `git branch --show-current`
2. Ensure worktrees directory exists: `mkdir -p ../worktrees`
3. Launch 10 implementation agents in parallel, each with:
   - Create worktree with suffix 1-10: `git worktree add ../worktrees/{branch_name}-{N} -b {branch_name}-{N}`
   - Copy .envrc if exists: `cp .envrc ../worktrees/{branch_name}-{N}/.envrc`
   - Implement approach N from plan.md
4. Wait for all 10 implementations to complete

### Step 3: Decision Phase
Launch the decision agent to evaluate all implementations:
- Review all 10 worktrees
- Compare against plan.md
- Decide which approach is optimal
- Document decision in `decision.md` with reasoning and chosen worktree path

### Step 4: PR Creation
1. Switch to the optimal worktree identified in decision.md
2. Create a PR from that branch
3. Document the PR URL

### Step 5: Review Phase
Launch 10 review agents in parallel to review the PR:
- Each agent reviews from different perspectives
- Aggregate all reviews into `review.md`

### Step 6: Final Implementation
Launch the final implementation agent:
- Read review.md
- Implement all review feedback
- Work in the optimal worktree
- Commit changes with descriptive message
- Mark workflow as complete

## Important Notes
- Always work systematically through each phase
- Use TodoWrite to track progress across all phases
- Each phase must complete before moving to next
- All agents run autonomously - provide complete context
- Keep main agent focused on orchestration, not implementation
