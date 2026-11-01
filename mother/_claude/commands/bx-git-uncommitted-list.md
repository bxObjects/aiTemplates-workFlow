List the repos this session worked in that still have work to commit or push.

Scope: ONLY the repos this session changed --- created, edited, renamed or deleted files
in. Not every repo on the host: the user has tools (magit) for that. This command is for
what only this session knows.

1. From this session's own record --- the conversation, and its summary if the context was
   compacted --- list every repo you changed files in. Resolve each to its git top-level
   (`git -C <dir> rev-parse --show-toplevel`). Repos you only read do not count. If $ARGUMENTS
   names repos, add them to the list.
2. For each repo, check git --- not memory, because the user may have committed, staged or
   changed things since:
   - `git status --short` (staged, unstaged, untracked)
   - `git rev-list --count @{u}..HEAD` for commits not yet pushed (say so if there is no upstream)
3. Report, as a table: repo, state (staged / unstaged / untracked / unpushed / clean), and
   in one line what this session's uncommitted work there is. Then list the clean ones in a
   single line, so the user knows they were checked.
4. If one repo's uncommitted work depends on another's (a module importing a file that only
   exists uncommitted elsewhere), give the commit order.
5. For each repo that has staged changes, suggest `/bx-git-commit-msg <repo>`. For one with
   only unstaged changes, show the `git add` that stages this session's files there.
6. Read-only: never stage, commit or push.
7. When there is something to commit, end by offering =/bx-git-commit-walk=, which takes the user
   through these repos one at a time in their Blee's magit.

If the session's record is incomplete (e.g. after compaction), say which repos you are
unsure about rather than guessing.
