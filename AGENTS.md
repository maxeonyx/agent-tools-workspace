# agent-tools-workspace — Agent Instructions

This repository is the shared local base directory for concurrent development
of the `maxeonyx/agent-tools` ecosystem. It owns clone lifecycle and coordination
rules, not product code.

## Clone layout

- Put each `agent-tools` clone directly under this repository as
  `at-<feature-branch>`.
- Use slash-free feature branch names, for example `issue-8-workspace`, so the
  clone is `at-issue-8-workspace`.
- Use a full clone, without `--filter=blob:none`, for the umbrella and its submodules: the standards ratchet reads repositories through `git2`, which cannot lazy-fetch promisor objects and fails with `object not found` in a partial clone. Initialize only the tool submodules required by the task.
- Never use worktrees for these task directories.
- One agent session owns one mutable clone. Never edit a clone owned by another
  session, even when its agent appears idle.
- Read-only inspection of another clone is allowed, but its tracked and
  untracked state must remain unchanged.

## Starting work

1. Fetch the relevant repositories and inspect open PRs before choosing a branch.
2. Create or select the remote feature branch and PR before product edits when
   practical.
3. Clone `agent-tools` into `at-<feature-branch>` and switch to that branch.
4. Initialize only the submodules needed for the issue.
5. Read the root and initialized submodule `AGENTS.md` files before editing.

Never stash, discard, reset, or commit changes found in an existing clone unless
the task explicitly owns those changes.

## Submodule coordination

Tool changes land before the umbrella pointer change:

1. Commit and push the tool repository branch.
2. Merge the tool PR with a merge commit.
3. Update the umbrella branch to the merged tool commit.
4. Regenerate `docs/version.json` when a pointer or version changes.
5. Commit and merge the umbrella PR.

Before updating an umbrella pointer, merge current `origin/main` into the
umbrella feature branch. If both sides changed the same submodule pointer, do
not choose either side blindly: fetch the tool repository and set the pointer
to a commit that contains both lines of work. If no such commit exists, merge
the tool branches first, then update the pointer to that merge commit.

An umbrella PR must name its child-repository PRs and must not merge until every
referenced child commit is available on its remote.

## History and cleanup

- Prefer merge commits. Do not rebase or force-push by default.
- If a branch truly needs history replacement, build the replacement branch
  separately and obtain explicit user approval before swapping it remotely.
- Do not remove an `at-*` clone until its working tree is clean, its commits are
  pushed, its PR state is known, and no child repository work remains unpinned.
- Removal is a deliberate cleanup action; report what was removed and whether
  any unmerged remote branch or PR remains.
