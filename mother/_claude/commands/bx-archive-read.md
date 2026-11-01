Read archived work from the org archive files (=*.org_archive=) in the current
directory. These are NOT part of the CLAUDE.md import chain, so their content is
invisible until this command pulls it in.

With no $ARGUMENTS, print an index only — do not read the subtree bodies:

1. Collect every top-level headline from each =*.org_archive= in the current directory.
2. Group them by =:ARCHIVE_OLPATH:= (the originating stage), in file order.
3. For each, show: headline title, =CLOSED= date, and source archive file if there is
   more than one.
4. State that full text for any entry is available via
   =/bx-archive-read <substring>=.

With $ARGUMENTS, read in full every archived subtree whose headline, =ARCHIVE_OLPATH=
or tags match it (substring; also accept a stage name like "Stage 4"). Then summarize
what those entries establish that bears on the current task.

Treat imported content as historical, valid as of its =CLOSED= date and not since
re-verified:

- Before acting on or recommending anything it names — a file, script, directory,
  command, flag, image tag, port — verify that thing still exists and still behaves
  that way. Archived work frequently describes artifacts that were later renamed,
  superseded or deliberately deleted.
- Where an archived statement conflicts with AI-DevStatus.org, README.org or the
  current source, the current state wins. Say so explicitly rather than silently
  preferring one.

This command is read-only with respect to the workflow files. Never reinsert archived
subtrees into AI-WorkPlan.org and never edit the archive — round-tripping would
collapse the distinction between finished and open work. If an archived fact turns out
to still be durable, promote it with =/bx-status-update= instead.
