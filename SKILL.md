---
name: project-status
description: "盘点当前项目的 PR 和 agent 进度、分支与 worktree、待更新文档及下一步选项。自行确认 tracker 和文档位置，输出清理建议并等待批准。"
disable-model-invocation: true
---

# Project status

Give the user a project inventory before they approve cleanup or select the next task. This is a project-wide report, not a summary of only the latest chat turn. Use the user's language.

## 1. Locate the sources

Resolve the target project from the user's request and current working directory. Confirm the repository root, remotes and default branch. If several projects are plausible, ask which one to inspect.

First check whether the current project has Matt setup configuration. Read the project's agent instructions and follow their setup, tracker, triage, domain and documentation pointers. Treat confirmed Matt configuration as the authoritative source for the spec store, issue tracker, task dependencies, glossary and ADR locations. Do not independently rediscover or replace configured locations.

If Matt configuration is absent, discover those locations from other project instructions, repository documentation, local files and available service metadata. If configuration covers only some components, discover only the missing components. These stores may be remote services or local files. A Git remote alone does not identify the issue tracker. Do not run setup or create configuration during this read-only inventory.

Use the available skill catalog to locate `ask-matt`. Read it to interpret an existing Matt setup or recommend a next-step skill. Its presence in the global skill catalog is not evidence that this project has Matt configuration. Follow its configuration pointers rather than copying its process. If the catalog has no entry, check `ask-matt/SKILL.md` in the host's project and user skill directories. Resolve locations from the host, not a fixed username or machine path. If unavailable, report that gap and use the project's documented process.

Distinguish configured locations from discovered or inferred locations. Do not assume GitHub, a `.scratch` tracker, `GLOSSARY.md`, `CONTEXT.md`, or a particular docs directory. If a configured location is inaccessible, report it as unverified rather than silently substituting another store. Ask only when conflicting sources prevent a reliable inventory. Otherwise mark missing or inaccessible sources as unverified.

Completion criterion: identify the project and each applicable source, or state which source could not be identified.

## 2. Gather current evidence

Keep this invocation read-only. Use platform and Git inspection commands that do not mutate project state. Do not fetch, prune, checkout, install, run builds or tests, write reports, update trackers, close issues, delete branches, or remove worktrees. For the default branch, compare local refs with the live remote ref when accessible; a cached tracking ref is not proof of current remote state.

Inspect these sources within the target project:

- Open PRs or MRs, drafts, reviews, checks, blockers and merge states.
- Recently completed PRs or MRs. Use the user's time range. Otherwise report the latest five merged items and state that limit. Identify closed-unmerged items separately if relevant.
- Agents associated with the project. Prefer live runner status over notifications or old transcripts. Record task, associated PR or branch, status and last verified result. Do not infer active work from an open PR. Do not scan unrelated sessions or private project histories.
- Branches, tracking refs, commits, worktree paths, tracked changes and untracked files. Keep uncommitted work visible.
- Relevant specs, tickets and dependencies. Compare delivery evidence with their recorded states and acceptance criteria.
- Current documentation and previous verification evidence. Distinguish current results, historical results, unavailable CI and untested live behavior.

For large result sets, page through the sources or report the exact coverage limit. For unavailable tooling, state what could not be checked. No access does not mean no work.

Completion criterion: every reported state has current evidence or an explicit uncertainty label.

## 3. Classify cleanup and record updates

Treat branches and worktrees as cleanup candidates, not as authorized deletions. Check PR delivery, unique changes, tracked and untracked files, active agents or processes, and links or installations that depend on the path. If a dependency cannot be checked, label the candidate conditional.

For squash or rebase merges, ancestry alone may miss completed work. Compare the reviewed patch or tree with its recorded landing commit. Account for later default-branch changes. A matching title, a merged PR, or an absent `branch --merged` entry is not sufficient proof by itself.

List local and remote branch deletion separately. Preserve the primary worktree, active work, installation targets, rollback backups and required source objects or caches. Inspect temporary directories before proposing their removal. Report measured directory size as directory size, not guaranteed reclaimed space.

For each record update, name the actual issue or file, the stale statement, the proposed correction and the supporting evidence. Suggest closing a spec only when its applicable acceptance criteria are met. Keep unresolved external blockers open. Preserve historical test results and add later delivery evidence instead of rewriting history. Update a glossary or ADR only if terminology or a recorded decision changed.

## 4. Write the report

Use these four sections, in this order. Translate their titles to the user's language. Keep empty sections and say whether they are empty or unverified.

### 1. Current PR and agent progress

Use three subsections:

1. **Open PRs, agents developing.** List confirmed active work and its next gate. Also identify agents without a PR. Put open PRs without an active agent in a clearly labeled idle, paused or unknown row. They are not automatically ready to merge.
2. **Recently completed PRs.** Give the delivered result and confirmed merge state. Mention installation or deployment only when checked. State the reporting window or limit.
3. **PRs awaiting merge.** List completed development awaiting review, checks, approval or merge. State each blocker. Avoid duplicating an actively developed PR here.

### 2. Branches and worktrees

Separate **currently developing** from **old branches and worktrees eligible for cleanup**. Include branch, path, dirty state, evidence and conditions. State what must remain. If no development is active, say so.

### 3. Specs, issues and documents to update

Use a table of actual record locations, current discrepancies and proposed changes. Distinguish completed implementation from stale open tracker state. Link only sources you inspected.

### 4. Next work the project can do

Offer concrete options with an expected deliverable, dependency and recommendation. Use `ask-matt` to select an applicable route when available. Recommend a skill only if its existence and role are verified. Do not start the next task or create tickets during this report.

End with a bounded confirmation list for proposed cleanup and record updates. State that no deletion or update has occurred. The user approves that scope first and selects the next development task separately. Later approval covers only the listed actions; new or changed candidates need renewed approval. Recheck evidence immediately before any approved destructive action and follow the project's execution workflow.

## Writing rules

Apply ASD-STE100-inspired structure: short sentences, one fact or instruction per sentence, consistent terms, explicit actors and conditions before actions. Use tables for parallel items. Label facts, recommendations and uncertainties separately. Do not claim certified ASD-STE100 compliance for Chinese text or without a full vocabulary and rule audit.

Keep internal commands and logs out of the main report unless they explain a blocker. Include enough evidence to support cleanup decisions. Never call a proposed deletion safe solely to make the report shorter.
