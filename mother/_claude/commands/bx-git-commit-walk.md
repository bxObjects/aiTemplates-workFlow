Write every commit message of this session's repos at once, then walk the user through committing them in their editor.

For use after =/bx-session-verify= or =/bx-git-uncommitted-list= has listed what is left. Claude writes
all the messages up front, one per repo, for a staging plan it states; the user then stages and
commits repo after repo without sending a command for each, and says "done" at the end.

Arguments ($ARGUMENTS): none, or any of these =key=value= words, then optionally repo paths to walk
instead of this session's list (in the order given):
- =open=<command>= --- the command that opens a repo in the user's editor, run as
  =<command> <repoPath>=. Default: the user's =gitOpener= setting, from
  =aiActivity.cs -i userConfig_get --parName=gitOpener=. Neither: list the repos, open nothing.
- =emacs=<socket>= and =walk=<function>= --- together: send the whole list once to that Emacs,
  =(<function> '("<path1>" "<path2>" ...))=, and let the editor move from repo to repo itself
  (the sender names the function; this command names none).
Invoking this command is the user's agreement to that opening, in that editor, and nothing more.

1. *The list and the staging plan.* The repos this session changed that still have work to commit
   or push, as =/bx-git-uncommitted-list= finds them (from git, not memory), in commit order: a repo
   before the ones that depend on it. Use each repo's git top level. For each, the plan: what to
   stage (all, or named files), what to leave out and why (a file this session did not change, a
   generated file), and whether to push now or hold (a repo whose remote must wait, e.g. for a
   release it depends on). A repo with two unrelated changes gets two commits only when they touch
   different files; when they share a file, one commit whose message describes both (a split would
   mean staging part of a file).

2. *Before writing anything, for every repo:*
   - =git fetch= and =git rev-list --count HEAD..@{u}=: a repo that is behind is pulled first
     (=git pull --rebase --autostash=, after checking nothing is staged: the autostash loses staged
     deletions), so that the messages are written on top of what will be committed. Say what the
     incoming commits touch, and whether they overlap the changes.
   - The staged-to-be text of AI-DevStatus.org, README.org and AI-WorkPlan.org Results: statements
     that stop being true once committed ("uncommitted", "untracked", "not yet committed"). List them
     all once and offer to reword them all; do not ask repo by repo.
   - Changes in the plan's files that this session did not make (the user's own edits, a
     regenerated dblock): name them, so the message can mention them.

3. *Write all the messages.* Each to its repo's =.git/CLAUDE_COMMIT_MSG= (the second commit of a
   split to =.git/CLAUDE_COMMIT_MSG.2=), in the format of =/bx-git-commit-msg= (read
   =.claude/commands/bx-git-commit-msg.md= for it: 69/90, why first, the trailers), from that
   repo's diff against HEAD for the planned files (=git diff HEAD -- <files>=, and the content of
   untracked ones), and its AI-WorkPlan.org TODOs. =Requires:= names a repo of this walk and, when
   it has landed already, its hash. Check every subject (69) and body line (90) mechanically before
   showing anything. Then show one table: #, repo, what to stage, push or hold, the subject. Keep
   the planned subjects (the table, or a file in the scratchpad): after a commit the repo's
   =.git/CLAUDE_COMMIT_MSG= may be gone, so step 5 cannot read them back from there.

4. *Hand over.* With =emacs== and =walk==: check =<function>= is a plain symbol, send the list
   (=timeout 20 emacsclient -s <socket> --eval '(<function> (quote ("<path1>" ...)))'=), and report
   what it returned. Otherwise, with an opener (=open==, else =gitOpener=): run it on the first
   path. With neither: the table is the walk. In every case the user stages per the plan, runs
   =git commit -t .git/CLAUDE_COMMIT_MSG= in each repo, pushes or holds, and says "done" at the end.
   If the user stages differently in a repo, =/bx-git-commit-msg <repo>= rewrites that one message
   from what is actually staged.

5. *On "done":* for each repo, from git: the commit landed (=git log= since the walk began has the
   planned subject kept in step 3), nothing of the plan left (=git status --short=), and the push
   done or held as planned (=git rev-list --count @{u}..HEAD=). A hold that was pushed anyway: say
   so first, with what it breaks and how to repair it. Then one line per repo with its new commit hash, and what is
   left. When everything is committed and pushed (holds aside), say that =/bx-git-pull-list-add= can
   now put the pull list for other hosts into =AI-Outputs.org=.

Never stage, commit or push yourself.
