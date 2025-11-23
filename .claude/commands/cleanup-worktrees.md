# Cleanup Git Worktrees

Clean up git worktrees by removing stale and orphaned worktrees.

## Task

You should clean up git worktrees by performing the following steps:

### Step 1: List All Worktrees
- Run `git worktree list` to see all current worktrees
- Identify the main worktree and branch worktrees

### Step 2: Prune Stale Worktrees
- Run `git worktree prune --dry-run` to see what would be removed
- Run `git worktree prune -v` to remove stale administrative files for worktrees that no longer exist

### Step 3: Identify Orphaned Worktrees
- Check if there are any worktree directories that exist but are no longer tracked
- Look in common locations like `../worktrees/` directory

### Step 4: Remove Orphaned Worktrees (if any)
For each orphaned worktree found:
- Confirm with the user before removing
- Remove the worktree with: `git worktree remove <path>` or `git worktree remove --force <path>` if needed
- If the directory still exists after removal, manually delete it

### Step 5: Clean Up Empty Directories
- Check if the `../worktrees/` directory is empty
- If empty and the user confirms, remove the directory with `rm -rf ../worktrees`

## Important Notes
- Always show the user what will be cleaned up before actually removing anything
- Use `--dry-run` flags when available to preview actions
- Ask for confirmation before removing worktrees that contain uncommitted changes
- Be cautious with `--force` flag - only use when necessary and safe
- Provide a summary of what was cleaned up at the end
