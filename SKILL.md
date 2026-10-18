---
name: project-status
description: "更新远端信息，盘点项目进度、本地修改与冲突风险，提出清理、复盘沉淀和下一步建议，等待用户决定是否同步代码。"
disable-model-invocation: true
---

# Project status

Give the user a project inventory before they approve cleanup or select the next task. This is a project-wide report, not a summary of only the latest chat turn. Use the user's language.

## 1. Locate the sources

Resolve the target project from the user's request and current working directory. Confirm the repository root, remotes and default branch. If several projects are plausible, ask which one to inspect.

First check whether the current project has Matt setup configuration. Read the project's agent instructions and follow their setup, tracker, triage, domain and documentation pointers. Treat confirmed Matt configuration as the authoritative source for the spec store, issue tracker, task dependencies, glossary and ADR locations. Do not independently rediscover or replace configured locations.

If Matt configuration is absent, discover those locations from other project instructions, repository documentation, local files and available service metadata. If configuration covers only some components, discover only the missing components. These stores may be remote services or local files. A Git remote alone does not identify the issue tracker. Keep setup and configuration unchanged during this inventory.

Use the available skill catalog to locate `ask-matt`. Read it to interpret an existing Matt setup or recommend a next-step skill. Its presence in the global skill catalog is not evidence that this project has Matt configuration. Follow its configuration pointers rather than copying its process. If the catalog has no entry, check `ask-matt/SKILL.md` in the host's project and user skill directories. Resolve locations from the host, not a fixed username or machine path. If unavailable, report that gap and use the project's documented process.

Distinguish configured locations from discovered or inferred locations. Do not assume GitHub, a `.scratch` tracker, `GLOSSARY.md`, `CONTEXT.md`, or a particular docs directory. If a configured location is inaccessible, report it as unverified rather than silently substituting another store. Ask only when conflicting sources prevent a reliable inventory. Otherwise mark missing or inaccessible sources as unverified.

Completion criterion: identify the project and each applicable source, or state which source could not be identified.

## 2. Gather current evidence

Automatically refresh remote information after identifying the target repository. This is the only permitted project-state change during inventory: fetch may update remote-tracking refs and download Git objects. Keep working files, the index and local branches unchanged. Do not prune, checkout, merge, rebase, reset, stash, push, install, run builds or tests, write reports, update trackers, close issues, delete branches, or remove worktrees.

### Remote refresh and synchronization assessment

Inspect remotes, fetch refspecs and repository state before fetching. Refresh the configured upstream remote and default-branch remote; deduplicate them. If neither is established, use the sole remote. If multiple remotes are ambiguous, ask which to refresh. List other remotes as not refreshed.

Use an explicit, non-pruning fetch such as `git -c fetch.prune=false -c fetch.pruneTags=false fetch --no-prune --no-tags --no-recurse-submodules <remote>`. Check that fetch destinations are remote-tracking refs, not local branches or tags. If custom refspecs, active Git operations or repository policies prevent this bounded refresh, report the blocker instead of overriding it. Do not use pull. Record each attempted remote and success or failure. Failed refreshes leave remote state unverified; cached refs are not current evidence. For the default branch, also compare local refs with its live remote ref when accessible.

Inspect every worktree for staged changes, unstaged changes, untracked files, unresolved index conflicts and in-progress merge or rebase operations. Report file paths and counts, paging or stating coverage limits. For each relevant branch, report its upstream and ahead/behind counts against successfully refreshed refs. Missing upstreams, detached HEADs and incomplete shallow history need explicit uncertainty labels.

Distinguish existing conflicts from possible synchronization conflicts. Existing unmerged index entries are confirmed conflicts. Diverged commits or overlapping changed paths indicate risk, not confirmed merge conflicts. No current conflicts does not prove that a future merge or rebase will be conflict-free. Do not perform a trial merge, change the index or create a temporary worktree to test it.

Completion criterion: report remote refresh results, local modifications, existing conflicts, branch divergence and any unverified synchronization risks. Leave code synchronization to the user's separate decision.

Inspect these sources within the target project:

- Open PRs or MRs, drafts, reviews, checks, blockers and merge states.
- Recently completed PRs or MRs. Use the user's time range. Otherwise report the latest five merged items and state that limit. Identify closed-unmerged items separately if relevant.
- Agents associated with the project. Prefer live runner status over notifications or old transcripts. Record task, associated PR or branch, status and last verified result. Do not infer active work from an open PR. Do not scan unrelated sessions or private project histories.
- Branches, tracking refs, commits, worktree paths, tracked changes and untracked files. Keep uncommitted work visible.
- Relevant specs, tickets and dependencies. Compare delivery evidence with their recorded states and acceptance criteria.
- Current documentation and previous verification evidence. Distinguish current results, historical results, unavailable CI and untested live behavior.

For large result sets, page through the sources or report the exact coverage limit. For unavailable tooling, state what could not be checked. No access does not mean no work.

Locate and read `principle-prove-it-works` through the skill catalog or the host's project and user skill directories before finalizing status claims. Apply its direct-evidence rule within this inventory's operation limits. Check actual artifacts, live runner status and remote results rather than agent self-reports. Reading historical verification evidence is allowed; running tests, creating verification scripts or writing artifacts is not. If unavailable, report the gap and keep unsupported claims explicitly unverified.

Completion criterion: every reported state has current evidence or an explicit uncertainty label.

## 3. Classify cleanup and record updates

Treat branches and worktrees as cleanup candidates, not as authorized deletions. Check PR delivery, unique changes, tracked and untracked files, active agents or processes, and links or installations that depend on the path. If a dependency cannot be checked, label the candidate conditional.

For squash or rebase merges, ancestry alone may miss completed work. Compare the reviewed patch or tree with its recorded landing commit. Account for later default-branch changes. A matching title, a merged PR, or an absent `branch --merged` entry is not sufficient proof by itself.

List local and remote branch deletion separately. Preserve the primary worktree, active work, installation targets, rollback backups and required source objects or caches. Inspect temporary directories before proposing their removal. Report measured directory size as directory size, not guaranteed reclaimed space.

For each record update, name the actual issue or file, the stale statement, the proposed correction and the supporting evidence. Suggest closing a spec only when its applicable acceptance criteria are met. Keep unresolved external blockers open. Preserve historical test results and add later delivery evidence instead of rewriting history. Update a glossary or ADR only if terminology or a recorded decision changed.

## Retrospective and reusable context

Before writing recommendations, locate `retro` and `reflect` through the skill catalog or the host's project and user skill directories. Read them as scoped references, not as workflows to execute. Follow `retro`'s `writing-for-agents` pointer when available. Report missing references and use the fallback rules below.

| Reference | Use only these parts | Intended destination |
| --- | --- | --- |
| `retro` | Improvement categories: navigation, existing automated checks, judgment-based standards, instruction size, tool economy and information access. | Project documentation, navigation pointers or existing check configuration. |
| `reflect` | Durable-learning filter, judgment/tooling/divergent analytical lenses and routing each supported finding to a concrete existing-skill edit. Reject one-offs and lessons already covered by a correctly followed skill. Prefer structural enforcement when more reliable than prose. | An inspected existing skill, with its exact path and proposed edit. Structural improvements belong in project-check suggestions instead. |

Use those lenses in the current analysis only. Do not start `reflect`'s reviewer or synthesizer agents, file Accepted/Rejected/Backlog items to a tracker, run description tuning, invoke authoring workflows or apply edits. Do not execute `retro`'s follow-up improvements. Neither reference expands session scope or overrides this inventory's operation limits. A full retrospective or skill-editing workflow is a separate next-task option requiring the user's choice.

The project inventory covers the whole project. The retrospective covers only the current session unless the user specifies another session. Read the available primary evidence for that scope. Keep unrelated sessions and private histories outside it. If session evidence is unavailable, report the gap; do not invent a process history from PR state.

Look for hard-to-find information, missing or unwired automated checks, costly tool use, stale instructions and verified operating knowledge worth retaining. Inspect existing check configuration before recommending another check. Prefer deterministic checks for mechanical errors; use written standards for judgment calls. Propose navigation pointers rather than repeating whole documents.

Order findings by impact. For each finding, name the session evidence, proposed improvement, intended existing file or configuration location, and expected benefit. Label a new location as proposed. Retain useful conclusions, not full transcripts, secrets or unrelated personal details. Separate stale-record corrections from new reusable knowledge. Suggest changes only; do not write documents or implement checks.

Completion criterion: each retrospective suggestion has evidence and a proposed destination, or explicitly report that no supported suggestion was found.

## 4. Write the report

Start with a brief conclusion, evidence scope and a **Remote refresh, local changes and synchronization decision** module. Then use these five sections, in this order. Translate their titles to the user's language. Keep empty sections and say whether they are empty or unverified.

In the synchronization module, use a table with remote refresh result, branch/upstream, ahead/behind counts, staged/unstaged/untracked paths, confirmed conflicts, in-progress Git operations and synchronization risks. Distinguish facts from estimates and unavailable checks. State that fetch updated information only, and no local code integration or push occurred. Ask the user whether to synchronize code later; do not treat invocation or successful fetch as approval.

### 1. Current PR and agent progress

Use three subsections:

1. **Open PRs, agents developing.** List confirmed active work and its next gate. Also identify agents without a PR. Put open PRs without an active agent in a clearly labeled idle, paused or unknown row. They are not automatically ready to merge.
2. **Recently completed PRs.** Give the delivered result and confirmed merge state. Mention installation or deployment only when checked. State the reporting window or limit.
3. **PRs awaiting merge.** List completed development awaiting review, checks, approval or merge. State each blocker. Avoid duplicating an actively developed PR here.

### 2. Branches and worktrees

Separate **currently developing** from **old branches and worktrees eligible for cleanup**. Include branch, path, dirty state, evidence and conditions. State what must remain. If no development is active, say so.

### 3. Specs, issues and documents to update

Use a table of actual record locations, current discrepancies and proposed changes. Distinguish completed implementation from stale open tracker state. Link only sources you inspected.

### 4. Retrospective: knowledge worth retaining

Keep this as a top-level report section immediately after document updates. Use two groups, **Project environment and process** from `retro`, and **Existing skill improvements** from `reflect`. Give each group a table with columns for priority, lesson, session evidence, proposed improvement, destination file or configuration, and expected benefit. List findings in impact order within each group and deduplicate across groups. Name the inspected destination path; label any new destination as proposed. If a group is empty, use a status row stating that no evidence-backed suggestion was found. If session access failed, mark it unverified rather than empty. These remain suggestions only.

### 5. Next work the project can do

Offer concrete options with an expected deliverable, dependency and recommendation. Use `ask-matt` to select an applicable route when available. Recommend a skill only if its existence and role are verified. Do not start the next task or create tickets during this report.

End with a bounded confirmation list for proposed code synchronization, cleanup, record updates and retrospective improvements. Name exact branches, paths and records. State which remotes were refreshed and that no code integration, push, deletion or document/tracker update occurred. The user approves each scope separately and selects the next development task separately. Later approval covers only the listed actions; new or changed candidates need renewed approval. Recheck evidence immediately before any approved state-changing action and follow the project's execution workflow.

## Writing rules

Locate and read `bro` through the skill catalog or the host's project and user skill directories for plain-language style. Apply its language guidance only, not its standalone action to restate the previous message. If unavailable, report the missing reference and use this fallback: lead with the conclusion, use everyday language, explain necessary terms, and say who is doing what, what blocks them and what happens next. Keep evidence, paths and uncertainty visible even in a short report.

Locate and read `unslop` through the same catalog and host skill directories. Use `bro` for conversational tone and `unslop` for concrete prose checks: remove filler, vague claims, jargon and decorative formatting; split dense sentences and keep consistent terms. Apply these checks to this report only, not to project files. If unavailable, report the gap and use these checks as the fallback. Preserve evidence and uncertainty labels when shortening text.

Apply ASD-STE100-inspired structure: short sentences, one fact or instruction per sentence, consistent terms, explicit actors and conditions before actions. Use tables for parallel items. Label facts, recommendations and uncertainties separately. Do not claim certified ASD-STE100 compliance for Chinese text or without a full vocabulary and rule audit.

Keep internal commands and logs out of the main report unless they explain a blocker. Include enough evidence to support cleanup decisions. Never call a proposed deletion safe solely to make the report shorter.
