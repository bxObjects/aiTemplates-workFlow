Reach another AI-Activity: say whether it is native or foreign to this session, read its import chain, and make it the active context.

The terms are defined in AI-WORKFLOW.org, "The AI-Activities of a Session". Reaching brings the
AI-Activity's content (its =CLAUDE.md= chain), never its tooling: the commands, settings and startup
skills stay the Session-Root's.

1. Determine the target path, in this order:
   - If $ARGUMENTS is given, use it.
   - Else, use the most recent =<ide_opened_file>= or =<ide_selection>= location in this
     conversation.
   - Else, the session's working directory (then this is the root AI-Activity: say so).
   If the target is a file, take its directory.

2. Run =aiActivity.cs -i activityReport --sessionRoot=<the session's working directory> <dir>=.
   If its =kind= is =none=, stop and report that no AI-Activity covers that path. Otherwise note
   its =aiActivity= (the directory to read), =home=, =relation= (native or foreign), =homogeneity=
   and any =warning=.

3. Read the full import chain rooted at that AI-Activity's =CLAUDE.md=, following its =@= imports.
   For a sub (a slim =initiateSub= =CLAUDE.md=), also read the chain of its home, the AI-Activity
   Root above it, that supplies the invariants (=AI-WORKFLOW.org=), as Claude Code's own walk-up
   would --- unless that home is the Session-Root, whose chain is already loaded.

   Read these files with the Read tool, by the directory's real path (=pwd -P=), not with a shell
   command: Claude Code discovers a nested =.claude/skills= only after Read / Edit tool calls on
   files in that directory, never through Bash.

   If it is heterogeneous (its own =.claude/skills= resolve to a different target than its home's),
   also read the YAML frontmatter (=name=, =description=) of every
   =<dir>/.claude/skills/*/SKILL.md=, not their bodies, since discovery may come late. Until a skill
   appears in the session's own listing, treat that list as available skills: when a task matches a
   description, Read that =SKILL.md= by path before acting, and say so. Once listed, use the Skill
   tool.

4. Report:
   - The AI-Activity, its home, *native* or *foreign*, homogeneous or heterogeneous, and any warning.
   - When foreign: one line that it belongs to another AI-Activity Root, where another session may
     be working on it at the same time (AI-WORKFLOW.org, rule 5; accepted, but worth knowing), and
     that its repository's commits are separate from this session's own.
   - Every file imported, in order, noting which were symlinks and where they pointed.
   - If skills were read as in step 3: the skills directory (and its symlink target), and the skill
     names on one line, marking any also in the session's own skill listing.
   - A succinct (3-5 sentence) summary of what you have understood about *this* AI-Activity.

5. If this AI-Activity differs from the active context before this command, note explicitly that it
   is now the active context for the rest of the conversation --- subsequent work refers to it
   unless told otherwise --- and name the directory that was active before, so switching back is
   just =/bx-activity-reach <that directory>=.

Do not re-read or re-summarize the previously active AI-Activity's files; only note where it was.
