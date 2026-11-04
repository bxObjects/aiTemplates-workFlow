Append a pull list for this session's repos to AI-Outputs.org, so other hosts can pull them.

Use after the repos are committed and pushed (e.g. after =/bx-git-commit-walk=). The pull list
only helps another host once the commits are on the remote, so this command reads git, not
memory, and is run on its own, not as a step of the walk. Arguments: none, or repo paths to
list instead of this session's repos.

1. *The repos.* The repos this session changed, as =/bx-git-uncommitted-list= finds them, or the
   paths given. Use each repo's git top level. For each, from git: committed
   (=git status --short= clean) and pushed (=git rev-list --count @{u}..HEAD= is 0; say so if
   there is no upstream).

2. *The file.* The real path of the Session-Root's =AI-Outputs.org= link. No =AI-Outputs.org=:
   say so and stop; do not create one.

3. *The entry.* At the very end of =AI-Outputs.org=, a last top-level section titled with an org
   date tag, "* Pull On Other Hosts <YYYY-MM-DD Day>". If the file's last section is already
   such a section for today and not DONE, update it instead of adding another. It holds an entry
   in the file's shape: a =TODO= headline "Pull on other hosts --- <date>, session at
   <Session-Root>" with =:REPO: other hosts= and =:FOUND: <date>=, whose body has:
   - one line per repo that is committed *and* pushed, as an org link
     =[[bashRun:git -C <path> pull]]=, with =<path>= under =/bxRepos/...= (the same on every
     BISOS host), not the host's real path. The repo that holds =AI-Outputs.org= is listed too:
     the entry is committed and pushed with it, so link it last and say that;
   - a last line that pulls them all:
     =[[bashRun:for d in <path1> <path2> ...; do git -C $d pull; done]]=;
   - a repo committed but not pushed, not committed, or not under =/bxRepos=: a plain line
     saying so, with no link (for the last case, with its path, to pull by hand).
   Where =bashRun:= links are not defined they show as plain text naming the command.

4. *Hand over.* Show the entry and say which repo holds the file; the user stages it there with
   whatever else is waiting and commits. The user marks the TODO DONE once the other hosts have
   pulled. Never stage, commit or push yourself.
