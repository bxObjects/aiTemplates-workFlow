Write a commit message for the currently staged changes of one repo to its .git/CLAUDE_COMMIT_MSG.

The repo is $ARGUMENTS --- a repo name (e.g. =banna=, resolved among the repos this
session worked in or as a sibling of the current repo) or a path. With no argument, it is
the repo of the current directory. A leading "at" (=/bx-git-commit-msg at banna=) is allowed.

1. Run `git diff --staged` and `git status --short` in that repo to see exactly what is
   staged right now. Do not rely on memory of what was done earlier in the
   conversation — the user controls staging and may have staged a subset, or
   staged something you did not touch.
2. If nothing is staged, say so and stop. Show what is unstaged, so the user can stage it.
3. Find the AI-WorkPlan.org TODOs the staged work belongs to, by matching the staged diff
   against that repo's AI-WorkPlan.org and its archives (=*.org_archive=), not from memory.
   Never cite a TODO by its headline: if one has no anchor, say so and offer to add one
   (the user then stages it). If unsure which TODOs apply, list the candidates and ask.
4. Draft the message in the format below. Look at `git log -10` only for the repo's tone;
   where the two differ, the format wins.
5. Write the message to that repo's .git/CLAUDE_COMMIT_MSG (create the file; overwrite if
   present). Do not run `git commit` yourself -- the user commits.
6. Show the message and remind the user to run, in that repo:
   `git commit -t .git/CLAUDE_COMMIT_MSG`

## The BISOS/Blee commit message format ("69/90")

Sized for magit in a split window (about 90 columns), `git log --oneline` in 80 columns, and
GitHub's 72-character subject cut.

1. Subject: at most 69 characters; aim for 60, since magit's log margin (author, age) takes
   about 25 of 90 columns. A blank line follows it.
2. Body, when there is one: wrapped at 90 columns. URLs, paths and commands are not broken,
   even past 90.
3. Subject style: the repo's own --- past tense ("Added ...", "Fixed ..."), and a version
   prefix when the commit bumps the version ("0.18 ..."). No TODO anchor in the subject: it
   goes in the TODOs: trailer.
4. Body content: why first, then what changed as "- " bullets grouped by area. Say what was
   not tested on a real system (daemon, VM, host), as the TODO's AI Results do.
5. Plain text: no org markup (=x=, *x*); magit does not render it.
6. Size: a small change may be the subject and trailers only. A large change gets a body, but
   not a list of every file --- the TODO's AI Results hold the detail.
7. Trailers: the last paragraph, after a blank line, one `Key: value` per line, in this
   order, each only when it applies:
   - `TODOs:` the WorkPlan anchors, space-separated: `TODOs: 9.2 9.6 10.1`. A bare anchor is
     this repo's; another repo's is qualified as `repo:anchor` (`fileObj:4.7`). A named
     anchor as is (`BashPathParity`); a whole stage as `Stage 9`. No TODO, no line.
   - `Requires:` work in another repo that this commit depends on (one module importing
     another's new file): `Requires: bisos-pip/fileObj (ftoLeaf_csu; commit it first)`. Add
     the short hash once that repo is committed.
   - `Co-Authored-By:` Claude's line, as the session's attribution instructions give it, when
     Claude wrote the change. Never on a change the user wrote alone.

The shortest message is three lines and a blank one:

```
Gated lifecycle walks over deb13, verbs from bisos.fileObj

TODOs: 9.2 9.6 10.1
Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
```

Commits of a TODO: `git log --grep='^TODOs:.*\b9\.2\b'`.

In Blee, the same limits for hand-written messages (git-commit-mode highlights a subject past
the limit; M-q re-wraps the body):

```elisp
(setq git-commit-summary-max-length 69)
(add-hook 'git-commit-mode-hook (lambda () (setq-local fill-column 90)))
```
