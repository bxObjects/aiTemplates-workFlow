Before the user closes this session: verify that everything this session did is recorded and
committed. Report first; write nothing until the user agrees.

Scope: ONLY what this session changed --- the user has other tools for the whole host.

1. From this session's own record (the conversation, and its summary if compacted), list
   every directory you changed files in, resolved to its git top-level, and every change
   outside any repo (e.g. /etc/hosts, a venv). Say which you are unsure about rather than
   guessing.
2. For each repo, check on disk --- not from memory:
   - AI-DevStatus.org reflects this session's work there (current state, what works, what is
     untested, decisions). A repo worked in without the AI files at all: say so.
   - AI-WorkPlan.org has Results for every TODO worked on, and states that are true.
   - README.org reflects user-visible changes (new commands, files, decisions).
3. Cross-references resolve: every TODO anchor cited (e.g. "palsWithBcm 1.4") exists where it
   is cited to be; file and repo pointers name things that exist.
4. Out-of-chain records have pointers: a decision written outside a repo's import chain has a
   one-line pointer in that repo's AI-DevStatus.org (AI-WORKFLOW.org: Documentation Outside
   The Import Chain).
5. Git: do what =/bx-git-uncommitted-list= does for the same repos --- staged, unstaged,
   untracked, unpushed --- including any commit order between repos.
6. Report the gaps as a table: repo, file, what is missing. Then offer to fill them.
   If the user agrees, fill them; never stage, commit or push.
7. End with one line: either "nothing left --- you can close this session", or the remaining
   steps in order (fills, then the commits, e.g. via =/bx-git-commit-msg <repo>=). When repos are
   left to commit, offer =/bx-git-commit-walk=: it opens each one's magit in the user's Blee in
   turn (with a live partner Blee; see that command).
