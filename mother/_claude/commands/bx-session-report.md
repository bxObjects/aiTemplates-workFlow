Report where this session stands: its Session-Root, root AI-Activity, and the native and foreign AI-Activities reached. Read-only.

The terms are defined in AI-WORKFLOW.org, "The AI-Activities of a Session". Read nothing new: this is
a report from what the session already knows, plus two read-only commands.

1. Run =aiActivity.cs -i sessionRootCheck= and =aiActivity.cs -i activityReport= in the session's
   working directory.
2. From this conversation (and its summary, if the context was compacted): the AI-Activities reached
   with =/bx-activity-reach= (or its old name =/bx-confirm-here=), in order, and which one is the
   active context now. For each, run =aiActivity.cs -i activityReport --sessionRoot=<Session-Root>
   <dir>= to state its home and native or foreign from disk, not from memory.
3. Whether this session registered with an editor (=/bx-session-start emacs= register==), and the
   Emacs socket, if so. Do not contact that Emacs.

Respond with:
- Session-Root: the path, the rule-1 line, the activity type.
- Root AI-Activity: the Session-Root's (the same directory).
- AI-Activities reached: a table --- directory, kind (root / sub), home, native / foreign,
  homogeneous / heterogeneous --- marking the active context.
- Foreign AI-Activities are where conflicts with another session can arise (AI-WORKFLOW.org, rule
  5): say so in one line when there are any. Other running sessions are not known to this command.
- The partner Emacs, or "none".
- Any warning =activityReport= gave (a nested root, an invalid Session-Root).
- If the session started without =/bx-session-start= (or =/bx-confirm-root=), say so.
