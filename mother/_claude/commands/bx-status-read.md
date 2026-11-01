Re-read AI-DevStatus.org and AI-WorkPlan.org of the current directory from disk, and say where
the work stands and what is next. A work-loop command: use it when picking up the next TODO
or resuming after a break --- not as the session-start check (that is =/bx-confirm-root=).

Read both files from disk even though CLAUDE.md loaded them at startup: the user may have
edited them since (in Emacs), and the startup copies would be stale. If they differ from
what you loaded, say so.

Then respond with:
1. Current development state in 2-3 sentences.
2. The open TODOs of AI-WorkPlan.org (omit DONE and LOOP items), each with its anchor
   (=<<...>>=), priority and owner tag (=:ai:=, =:me:=, =:aiAndMe:=), grouped by stage.
3. Which one you suggest doing next, and why, in one sentence.
