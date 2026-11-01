Archive completed TODO subtrees from AI-WorkPlan.org in the current directory into
its sibling org archive file (=AI-WorkPlan.org_archive=), mimicking org-mode's
=org-archive-subtree=.

Scope: this command touches ONLY AI-WorkPlan.org and its archive file. It never
writes AI-DevStatus.org, README.org or panels — promoting durable facts there is
=/bx-status-update='s job. See step 3, which advises but does not write.

If $ARGUMENTS is given, restrict to DONE subtrees whose headline matches it
(substring, or a stage name like "Stage 4"). With no argument, consider all of them.

1. Enumerate candidate subtrees: only headlines in the DONE state, and only those
   inside a =* Stage N TODOs= list. Do NOT touch:
   - TODO / WAITING / DELEGATED items, or the =** +=  / =** -= list delimiters;
   - reference subsections that sit under a =* Stage N:= heading but OUTSIDE its
     TODOs list (background notes, decision matrices, host specifics) — these are
     durable reference, not finished work;
   - the Stage 0 =<<Context Confirmation>>= record, which is meant to be repeated
     and refreshed each session, not archived.
   List the candidates and stop for approval before changing anything.

2. Archive each approved subtree: append it to =AI-WorkPlan.org_archive= (create the
   file with an "Archived entries from file ..." header line if absent), promoting it
   to a top-level =*= headline and inserting a properties drawer immediately after the
   headline and any SCHEDULED line:
   =:PROPERTIES:= / =:ARCHIVE_TIME:= (=YYYY-MM-DD Day HH:MM=) / =:ARCHIVE_FILE:= /
   =:ARCHIVE_OLPATH:= (the originating =Stage N TODOs=) / =:ARCHIVE_CATEGORY:= /
   =:ARCHIVE_TODO:= / =:END:=. Then delete it from AI-WorkPlan.org. Preserve the
   subtree body verbatim, including its =----- AI Results: ------= block.
   If a stage's TODO list is left empty, leave a single
   =** Archived. No longer relevant.= line between its =** += and =** -= delimiters.

3. Advisory only — do not write anything outside AI-WorkPlan.org and the archive:
   for each archived subtree, extract the durable facts from its =AI Results= (design
   decisions, rejected alternatives, version/host constraints, negative results such as
   "we tried X and it broke") and check whether each already appears in a file that a
   fresh Claude loads: AI-DevStatus.org, AI-Activity.org, README.org, or a Blee panel
   the repo names. Report any fact found nowhere else, and recommend =/bx-status-update=
   to promote it. Negative results are the highest-risk category: durable docs tend to
   record what IS, so what was tried and rejected has no natural home and is what
   silently disappears.

4. Report: what was archived, what was deliberately left, and the unmatched facts from
   step 3.

Archiving is safe to do aggressively because =/bx-archive-read= can retrieve archived
content on demand within a later session.
