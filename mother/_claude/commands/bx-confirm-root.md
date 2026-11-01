Confirm the session's root AI-Activity: read the CLAUDE.md import chain of the session's directory; optionally verify and register with the editor that sent it.

The root is the top of this session's AI-Activity hierarchy: the session's working directory, which is
the VS Code window's folder, holding the top =CLAUDE.md=. Activities joined later are =/bx-confirm-here=.

Arguments ($ARGUMENTS): none, or =key=value= words, in any order:
- =dir=<rootDir>= --- the root the sender means.
- =emacs=<socket>= --- the server socket of the Emacs that sent this prompt.
- =register=<function>= --- with =emacs==: an elisp function of that Emacs to call with the root
  and this session's id. The sender names it; this command names none (an editor integration
  such as blee/vscode-claude supplies its own).
Report any other word as not understood, and go on with the rest.

Without arguments this is the complete confirmation, and nothing Emacs-related is done or reported:
an AI-Activity works in VS Code alone, and the Emacs layer is optional. Do not look for an Emacs
(sockets, partner files) unless =emacs== is given.

1. If =dir== is given, compare its real path (=pwd -P= / =realpath=) with the session's working
   directory's. If they differ, say that the prompt reached a session rooted elsewhere (the wrong
   window or session), name both, and stop.

2. Read the full import chain starting from CLAUDE.md in the session's working directory
   (AI-WORKFLOW.org, AI-Activity.org, AI-DevStatus.org, AI-WorkPlan.org). If it is a slim
   =initiateSub= =CLAUDE.md=, also walk upward for the ancestor =CLAUDE.md= that supplies the
   invariants (=AI-WORKFLOW.org=, =.claude/=).
   Use the Read tool for these files, not a shell command: Claude Code's discovery of a directory's
   =.claude/skills= is triggered by Read / Edit tool calls on files there, never by Bash.

3. If the directory has its own =.claude/skills= and it resolves to a different target than the
   =.claude/skills= of the ancestor that supplies the invariants (a heterogeneous sub; see "Homogeneous
   and Heterogeneous AI-Activities" in AI-WORKFLOW.org), read the YAML frontmatter (=name=,
   =description=) of every =<dir>/.claude/skills/*/SKILL.md=, not their bodies. Treat them as available
   skills for the rest of the conversation: when a task matches a description, Read that SKILL.md by
   path before acting.

4. If =emacs== is given, check that Emacs is reachable, read-only:
   : timeout 10 emacsclient -s <socket> --eval '(emacs-pid)'
   Report its pid, or the failure (no such socket, timeout). On failure, skip step 5.

5. If =register=<function>= is given with a reachable =emacs==, register this session with that
   Emacs. The prompt asked for it, so this change to the user's Emacs is agreed. Check that
   =<function>= is a plain symbol (letters, digits and =:/|-_*=), then, with the session id from
   =$CLAUDE_CODE_SESSION_ID=:
   : timeout 10 emacsclient -s <socket> --eval '(if (fboundp (quote <function>)) (<function> "<rootDir>" "<sessionId>") (quote notDefined))'
   with =<rootDir>= the session's working directory (real path). Report what it returned;
   =notDefined= means that Emacs does not define =<function>=. =register== without =emacs==:
   say it needs =emacs==.

Then respond with:
1. A list of every file you imported, in order, noting which were symlinks and where they pointed.
2. A succinct (3-5 sentence) summary of what you have understood about this project.
3. If skills were read as above: the skills directory (and its symlink target), and the skill names, on
   one line, marking any that are also in the session's own skill listing.
4. Only when arguments were given: the =dir= check, the Emacs pid, and the registration result.
