---
name: bx-ai-commands-authorship
description: Use when creating, editing, renaming or deleting a Claude slash command of the AI Activity workflow — a `bx-*.md` file under `mother/_claude/commands/` of the `aiTemplates-workFlow` tree, reached through any assembled templates base (`<templatesBase>/mother/_claude/commands/`, which a project's `.claude/commands` links to) — or when a templates base shows commands that differ from the workflow tree's, or when two bases show different `/bx-` command names (`bx-status` against `bx-status-read`). Covers where the one set of commands lives, editing it in the workflow tree and not in a base, the subject-then-verb naming, the description line of a command file, re-assembling the bases with `aiActivityAssemble.cs` (`assemblyCheck`, `assemblyUpdate`) after adding, renaming or deleting a command, and the documents that name commands.
---

# Authoring the `bx-` Commands of the AI-Activity Workflow

The shape, before any step:

```
aiTemplates-workFlow/                         the workflow tree (its own repository; a clone anywhere)
   mother/_claude/commands/bx-*.md              THE one set of commands. Edit here, and only here.
        │  linked, by this name, into every templates base:
        ▼
<templatesBase>/aiTemplates-workFlow  ->  the clone (per host)
<templatesBase>/mother/_claude/commands/bx-*.md  ->  ../../../aiTemplates-workFlow/mother/...   (generated
                                                     by aiActivityAssemble.cs; never edit through them)
<project>/.claude/commands  ->  <templatesBase>/mother/_claude/commands   (aiActivity.cs initiate / refresh)
```

The commands are the workflow of aiActivity, not of an activity, so there is one set and no layering
(decided 2026-10-19). An ecosystem's own tree (`aiTemplates-DE`) cannot override a command: the assembler
refuses a path present in both inputs. `settings.json` lives in the workflow tree too.

## 1. Edit in the workflow tree

- Edit `aiTemplates-workFlow/mother/_claude/commands/<name>.md` in the clone (on a BISOS host,
  `/bisos/git/bxRepos/bxexamples/aiTemplates-workFlow/...` until it is its own repository).
- An edit to an existing command is seen at once by every base that links this clone, and so by every
  project initiated from those bases. Say so before a risky edit.
- Do not edit through a base (`<templatesBase>/mother/_claude/commands/...`): an editor writes through the
  link into the clone, which is the same file, but a tool that replaces the file (`sed -i`, some editors'
  backup-and-rename) leaves a real file in the base, which `assemblyCheck` then reports as a conflict.
- **The first line is the description.** A command file without YAML frontmatter takes its first line as
  the description in the slash menu, so keep it one line that says what the command does. Never prepend a
  banner or a "do not edit" line to a command file.
- Keep a command short and imperative: it is read as the instruction for one turn.

## 2. Name it subject first, then verb

The 2026-10-11 convention: `bx-status-read`, `bx-status-update`, `bx-archive-read`, `bx-archive-update`,
`bx-git-commit-msg`, `bx-git-uncommitted-list`, `bx-session-verify`, `bx-ai-outputs`, `bx-context-trim`.
Related commands sort together and the slash menu completes them, so a long name costs nothing. A command
that is Claude-specific by nature says so where that helps (as `startupClaudeTokens` does). An
ecosystem's own commands, if it ever has some, need their own prefix, since a name in both inputs is
refused.

A rename is a delete plus an add: the bases keep a link to the old name until they are re-assembled
(step 3), and every document that names it keeps it until edited (step 4).

*Rename with an alias*, so that habits and documents not yet edited keep working: keep the old file
as a short alias whose first line says it is renamed, and whose body has Claude read the new
command's file (by its `.claude/commands/` path, with the Read tool) and do what it says with
`$ARGUMENTS`. Remove the alias later. Done 2026-10-30 for `bx-confirm0` → `bx-confirm-root`.

## 3. After adding, renaming or deleting: re-assemble the bases

Editing an existing file needs nothing more. Adding, renaming or deleting a file changes what the bases
should contain, so say, in one or two lines, which bases are now behind and how to see and fix it:

```bash
aiActivityAssemble.cs -i assemblyCheck  --base=<templatesBase>   # IN SYNC, or BEHIND with counts; dangling links
aiActivityAssemble.cs -i assemblyDiff   --base=<templatesBase>   # read-only: what would be linked, removed
aiActivityAssemble.cs -i assemblyUpdate --base=<templatesBase>   # link the new, remove the old
```

- On a BISOS host the bases are `bisos/defaults/ai-templates`, `bxexamples/aiActivityTemplates` and any
  organization's base (e.g. a fork's `aiActivityTemplates`). `assemblyCheck` and `assemblyDiff` write
  nothing: running them to report is fine. **Updating a base is the maintainer's to run**; offer the
  command, do not run it unless asked. It never commits.
- Then the workflow tree's own commit and push; other hosts get the change by `git pull` in their clone,
  followed by `assemblyUpdate` where files were added, renamed or deleted.
- The PyCS main exits 0 even when a command is refused or `assemblyCheck` says BEHIND: read the line.
- *Limit of this reminder*: it happens only when Claude makes the change. A change made by hand, or one
  arriving by `git pull`, gives no reminder; `assemblyCheck` can be run at any time.

## 4. Documents that name commands

Command names are written in text outside the command files. After a rename or a new command, look for
the old name in (and tell the maintainer which you did not touch):

- `mother/AI-WORKFLOW.org` of the workflow tree: the "Claude Slash Commands" table, the archiving
  section, the figures' pointers;
- the engine's `README.org` (session-workflow table, command table) and its `AI-DevStatus.org`;
- the `AI-DevStatus.org` / `AI-WorkPlan.org` of projects that use the commands (per-project copies: a
  rename in the templates does not reach them);
- the figures in the engine repository, `/bisos/git/bxRepos/bisos-pip/aiActivity/py3/images/`
  (`aiUsageWorkflow`, `aiStartupWorkflow`, `aiCommandsSkillsTrees`): `aiGeneralTree` lists every
  command, so a new, renamed or deleted command means editing `aiCommandsSkillsTrees-graphviz.pcs` and
  re-rendering there;
- skills and `AI-Activity.org` files that tell Claude to run a command by name.

```bash
grep -rn -- '/bx-<oldName>' <workFlowClone> <templatesBase> <projectRoot> 2>/dev/null   # then judge each hit
```

## 5. Pitfalls

- **Editing a base instead of the workflow tree.** The base's files are generated links; edit the clone.
- **Two names for one job after a rename.** A base not re-assembled still links the old name (now
  dangling); `assemblyCheck` lists it, `assemblyUpdate` removes it.
- **A frontmatter-less first line that is not a sentence** becomes the menu description.
- **A link that points at the wrong tree.** `.claude/commands` is decided by `aiActivity.cs -i initiate`
  and re-decided by `-i refresh` (base installs only; a sub inherits its ancestor's). A real directory
  there is left alone by `refresh`.
- **Testing on a real base first.** Try a change in a scratch base (`mktemp -d`, the two input links,
  `assemblyUpdate`), as the engine's `py3/tests/verify.sh` does, and `assemblyDiff` on the real bases.

- **A running session does not see a new command.** Claude Code reads `.claude/commands/` when a
  session starts: after an add or a rename (and its `assemblyUpdate`), the new name works in new
  sessions only. In a running one it is "not available"; Claude can still do what the file says by
  reading it (observed 2026-10-30 with `/bx-confirm-root`).

## References

- The engine: `/bisos/git/bxRepos/bisos-pip/aiActivity` --- `py3/bin/aiActivityAssemble.cs`,
  `py3/bisos/aiActivity/assemble.py`; README.org, "Assemble a Templates Base"; its AI-WorkPlan.org
  Stages 17 (the one set of commands) and 18 (the assembly).
- The command list and their roles: `mother/AI-WORKFLOW.org`, "Claude Slash Commands".
- Writing a skill: `authoring-skills`. Skills follow their templates tree as a whole; commands do not.
