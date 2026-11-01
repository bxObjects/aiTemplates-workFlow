Estimate what the NEXT Claude Code session will load at startup from the current directory, and propose concrete trims.

This is about the next session, not this one. For what is in context *right now*
(measured, not estimated), point the user to Claude Code's built-in =/context=.
Do not try to estimate your own current context; you cannot inspect it.

$ARGUMENTS, if given, is a target startup budget in tokens (e.g. =40000=). Aim the
proposals at reaching it and say whether they do.

1. Run =aiActivity.cs -i startupClaudeTokens= in the current directory. If the
   command is not found, say that =bisos.aiActivity= is not installed (=pipx install
   bisos.aiActivity=) and stop.
2. Summarize in a few lines: the startup total, the total for each section, and the
   five heaviest files with their share.
3. Find trim candidates, ranked by estimated tokens saved:
   - *Heavy =@= imports.* For each, judge from this activity's typical work
     (=AI-Activity.org=, =AI-WorkPlan.org=) whether it is needed in /every/ session.
     If not, propose either dropping the =@= and leaving the path as plain text
     (Claude can still open it when a task needs it), or moving the content into a
     skill (only its description loads at startup). Wrapping in org markup is not
     a reliable way to suppress an import; Claude Code skips only markdown code
     spans.
   - *=AI-WorkPlan.org=*: DONE subtrees mean =/bx-archive-update=.
   - *=AI-DevStatus.org=*: sections that repeat =README.org= or are no longer
     current mean a trimming pass in =/bx-status-update=. Name the sections; do
     not rewrite the file here.
   - *=!! resolved via symlink into ...=* is a legacy symlinked =CLAUDE.md=; the
     fix is =aiActivity.cs -i refresh= in that directory. Correctness first,
     tokens second.
   - *Auto-memory* marked =truncated=: entries past the 200-line / 25KB limit are
     not loaded at all; propose consolidating.
   - Skill and command descriptions are usually cheap. Flag only outliers.
     =already loaded= lines cost nothing; ignore them.
4. For each candidate, find where the edit would land: resolve the real path
   (=readlink -f=) and its git repo. Mark it *shared* if the real path is inside a
   templates tree (e.g. an activity's =AI-Activity.org=); editing it changes every
   project symlinked to it. Otherwise mark it *local*.
5. Present a table: candidate, est. tokens saved, proposed action, real path, repo,
   shared/local. End with the projected startup total if all proposals are applied.
6. Change nothing without confirmation. Apply only the items the user picks.
   Confirm *shared* edits separately, naming the scope affected (which activity,
   which templates tree). Delegate to =/bx-archive-update= and =/bx-status-update= rather
   than doing their jobs here. After applying, re-run step 1 and report the actual
   saving.

Demoting an import keeps the file and its path in place; nothing is lost, it just
stops loading by default. If a demoted file records a decision a fresh Claude
must know about, say so instead of proposing the trim.
