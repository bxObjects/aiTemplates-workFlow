Report where an AI-Activity stands relative to this session: root or sub, home, native or foreign, homogeneous or heterogeneous. Read-only.

The terms are defined in AI-WORKFLOW.org, "The AI-Activities of a Session". This reads no import
chain and changes no active context (that is =/bx-activity-reach=).

1. The directory: $ARGUMENTS if given (a file: its directory); else the most recent
   =<ide_opened_file>= or =<ide_selection>= location; else the active context.
2. Run =aiActivity.cs -i activityReport --sessionRoot=<the session's working directory> <dir>=.

Respond with:
- The AI-Activity (the directory with the =CLAUDE.md= at or above the one given), or "none".
- Root or sub; its home; for a sub, homogeneous or heterogeneous; its activity type.
- *Native* or *foreign* to this session; when foreign, the line on possible conflicts (rule 5).
- Whether it is this session's root AI-Activity, already reached (from this conversation), or not
  yet reached.
- Every warning =activityReport= gave (a nested root, a sub without a root, an invalid Session-Root).
- Other running sessions are not known to this command; an editor integration may report them.
