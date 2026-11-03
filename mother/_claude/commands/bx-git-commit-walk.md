Walk the user through committing this session's repos one at a time, opening each in their editor.

For use after =/bx-session-verify= or =/bx-git-uncommitted-list= has listed what is left. The user
stages and commits in their editor; Claude writes each message (=/bx-git-commit-msg=, which the
user may send from the editor) and moves to the next repo when told "next".

Arguments ($ARGUMENTS): none, or any of these =key=value= words, then optionally repo paths to walk
instead of this session's list (in the order given):
- =open=<command>= --- the command that opens a repo in the user's editor, run as
  =<command> <repoPath>=. Default: the user's =gitOpener= setting, from
  =aiActivity.cs -i userConfig_get --parName=gitOpener=. Neither: list the repos, open nothing.
- =emacs=<socket>= and =walk=<function>= --- together: send the whole list once to that Emacs,
  =(<function> '("<path1>" "<path2>" ...))=, and let the editor move from repo to repo itself
  instead of being told "next" (the sender names the function; this command names none).
Invoking this command is the user's agreement to that opening, in that editor, and nothing more.

1. *The list.* The repos this session changed that still have work to commit or push, as
   =/bx-git-uncommitted-list= finds them (from git, not memory), in commit order: a repo before
   the ones that depend on it, and a push wherever a later repo needs the commit on its remote.
   Use each repo's git top level. Show it as a table: #, path, what is there, and notes (what to
   stage, what to leave out, a push needed, a split into two commits when a repo holds unrelated
   changes).
   *The last repo is always the one that holds =AI-Outputs.org=* (the repo of the real path of the
   Session-Root's =AI-Outputs.org= link), even when this session changed nothing there: its
   commit carries the pull list (step 5). No =AI-Outputs.org=: no pull list, and the order stays.

2. *Hand over the list.* With =emacs== and =walk==: check =<function>= is a plain symbol, send the
   list (=timeout 20 emacsclient -s <socket> --eval '(<function> (quote ("<path1>" ...)))'=), and
   report what it returned; the editor opens the first repo. Otherwise, with an opener (=open==,
   else =gitOpener=): run =<opener> <path1>= and say which repo is open and what to stage there.
   With neither: stop after the list; the user commits with =/bx-git-commit-msg <repo>= and
   =git commit -t .git/CLAUDE_COMMIT_MSG=.

3. *For each repo:* the user stages and sends =/bx-git-commit-msg <path>=. Write the message as
   that command says, with a =Requires:= trailer naming the short hash of any earlier repo of
   this walk it depends on. After showing it, always end with the next step: commit (and push
   when this repo is needed on its remote), then *"Next: <path of the next repo>"* --- or "Next:
   none, this is the last repo".

4. *On "next"* (or when the next =/bx-git-commit-msg= arrives): check the previous repo landed
   (=git log -1=, =git status --short= clean, and =git rev-list --count @{u}..HEAD= when a push
   was due); if not, say what is left there. Then, with an opener, run it on the next path.

5. *Before the last repo* (the one holding =AI-Outputs.org=): append a pull list at the very end
   of =AI-Outputs.org=, so that other hosts get this session's commits: a new last top-level
   section, one per walk, titled with an org date tag, "* Pull On Other Hosts <YYYY-MM-DD Day>",
   holding an entry in the file's shape, a =TODO= headline "Pull on other hosts --- <date>, session at <Session-Root>" with
   =:REPO: other hosts= and =:FOUND: <date>=, whose body has:
   - one line per repo of this walk that is committed *and* pushed (=git rev-list --count
     @{u}..HEAD= is 0), as an org link =[[bashRun:git -C <path> pull]]=, with =<path>= under
     =/bxRepos/...= (the same on every BISOS host), not the host's real path; its own repo too;
   - a last line that pulls them all:
     =[[bashRun:for d in <path1> <path2> ...; do git -C $d pull; done]]=;
   - a repo committed but not pushed, or not committed: a plain line saying so, with no link.
   Where =bashRun:= links are not defined they show as plain text naming the command. Show the
   entry, then the user stages it with the rest of that repo and commits; the user marks it DONE
   once the other hosts have pulled.

6. *At the end:* one line per repo with its new commit hash, and anything left (a file not
   committed, a push not done). Never stage, commit or push yourself.
