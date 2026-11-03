Start this session: check that its Session-Root is an AI-Activity Root, read its import chain; optionally register with the editor that sent it.

The terms (Session-Root, AI-Activity Root, sub, home, native, foreign) are defined in AI-WORKFLOW.org,
"The AI-Activities of a Session". The Session-Root is this session's working directory --- in VS Code,
the window's folder --- and must be an AI-Activity Root. Other AI-Activities are reached later with
=/bx-activity-reach=.

Arguments ($ARGUMENTS): none, or =key=value= words, in any order:
- =dir=<rootDir>= --- the Session-Root the sender means.
- =emacs=<socket>= --- the server socket of the Emacs that sent this prompt.
- =register=<function>= --- with =emacs==: an elisp function of that Emacs to call with the
  Session-Root and this session's id. The sender names it; this command names none (an editor
  integration such as blee/vscode-claude supplies its own).
Report any other word as not understood, and go on with the rest.

Without arguments this is the complete start, and nothing Emacs-related is done or reported: an
AI-Activity works in VS Code alone, and the Emacs layer is optional. Do not look for an Emacs
(sockets, partner files) unless =emacs== is given.

1. *Rule 1.* Run =aiActivity.cs -i sessionRootCheck= in the session's working directory. It prints
   one line, =OK: ...= or =REFUSED: ...= (its exit code is 0 either way: read the first word).
   On =REFUSED=, report that line and stop: say that this session cannot work as started, and that
   a session should be started at the AI-Activity Root it names (in VS Code: open that folder in a
   window, and start the session there). Do none of the steps below.
   If =aiActivity.cs= is not found, say so, check by hand (the directory has =CLAUDE.md=,
   =.claude/= and =AI-WORKFLOW.org=), and go on only if it does.

2. If =dir== is given, compare its real path (=pwd -P= / =realpath=) with the Session-Root's. If
   they differ, say that the prompt reached a session rooted elsewhere (the wrong window or
   session), name both, and stop.

3. Read the full import chain starting from CLAUDE.md in the Session-Root (AI-WORKFLOW.org,
   AI-Activity.org, AI-DevStatus.org, AI-WorkPlan.org).
   Use the Read tool for these files, not a shell command: Claude Code's discovery of a directory's
   =.claude/skills= is triggered by Read / Edit tool calls on files there, never by Bash.

4. If =emacs== is given, check that Emacs is reachable, read-only:
   : timeout 10 emacsclient -s <socket> --eval '(emacs-pid)'
   Report its pid, or the failure (no such socket, timeout). On failure, skip step 5.

5. If =register=<function>= is given with a reachable =emacs==, register this session with that
   Emacs. The prompt asked for it, so this change to the user's Emacs is agreed. Check that
   =<function>= is a plain symbol (letters, digits and =:/|-_*=), then, with the session id from
   =$CLAUDE_CODE_SESSION_ID=:
   : timeout 10 emacsclient -s <socket> --eval '(if (fboundp (quote <function>)) (<function> "<rootDir>" "<sessionId>") (quote notDefined))'
   with =<rootDir>= the Session-Root (real path). Report what it returned; =notDefined= means that
   Emacs does not define =<function>=. =register== without =emacs==: say it needs =emacs==.

Then respond with:
1. The rule-1 line, and the Session-Root's activity type (=aiActivity.cs -i activityReport=).
2. A list of every file you imported, in order, noting which were symlinks and where they pointed.
3. A succinct (3-5 sentence) summary of what you have understood about this project.
4. Only when arguments were given: the =dir= check, the Emacs pid, and the registration result.

For the rest of the session, keep track of the AI-Activities reached (=/bx-activity-reach=), each with
its home and whether it is native or foreign: =/bx-session-report= lists them.
