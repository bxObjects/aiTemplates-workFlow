Summarize what this session has done, so the user can decide: go on here, /clear, or start a new session. Read-only.

For the user to see, not a record: write nothing. The durable records stay where they are
(AI-WorkPlan Results, AI-DevStatus.org, git). Where this session stands --- its roots, reaches,
partner --- is =/bx-session-report=; this is what it did.

Arguments ($ARGUMENTS): none, or a short description of the work the user has in mind next.

1. *The record.* This conversation, and its summary if the context was compacted. A compacted
   summary drops detail: when it matters, read the session's transcript (the =.jsonl= under
   =~/.claude/projects/=; =python3= over its =tool_use= entries) rather than trust memory.
2. *The threads.* Split the session into its threads of work, in the order they started: a TODO
   (by its anchor), a question, a fix, a side quest. For each: what was asked, what was done,
   what was decided (and by whom; what was rejected), and its state --- done, open, or dropped.
3. *The repos.* Each repo this session changed, with =git status --short= and its commits since
   the session started (=git log --oneline --since=<start>=, short hashes): committed, pushed, or
   left. A line, not a verify: =/bx-session-verify= is the full check.
4. *The weight.* How long the session is: how many threads, whether it was compacted, how much of
   it the next work would need. Claude cannot measure its own context: point to the built-in
   =/context= for the numbers.

Respond with:
- One line: the session in a sentence (its Session-Root, what it was mostly about).
- The threads, as a table --- #, thread, outcome (one line), state --- most recent last. A thread
  may take a few lines below the table when its decisions matter for what comes next.
- The repos: one line each.
- Open items: what this session leaves unfinished, in one list.
- A recommendation, with its reason in one or two lines:
  - *go on here* --- the next work builds on this session's threads, and the session is not heavy;
  - */clear* --- the next work is in the same Session-Root but does not need this history (say
    what to record first, if anything is unrecorded: =/bx-session-verify=);
  - *a new session* --- the next work belongs to another AI-Activity Root (a VS Code window per
    Session-Root), or the session is heavy enough that a fresh start reads less.
  With no argument, recommend for "more of the same"; with one, for that work.
