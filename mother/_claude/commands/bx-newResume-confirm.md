On the machine where the work continues: confirm that the last pull list arrived, and say what needs a reinstall, a release, a CBM rebuild.

The other half of =/bx-git-pull-list-add=: a session closed on one host and wrote a pull list into
=AI-Outputs.org=; the work continues here, in a new session (started with =/bx-session-start=; it
has nothing to do with the one that closed). Read-only: this command checks and recommends; the user
pulls, installs, releases and rebuilds. Arguments: none, or the date of the pull section to check
(default: the latest).

1. *The context.* Read the import chain from =CLAUDE.md= in the Session-Root, with the Read tool, if
   this session has not read it yet.

2. *The pull list.* The real path of the Session-Root's =AI-Outputs.org= link; the repo that holds it.
   - That repo first: =git fetch=, then =git rev-list --count HEAD..@{u}=. Behind: the pull list read
     may be stale; say to pull it first (the driver's =outputs-pull=) and check again.
   - The last top-level section "* Pull On Other Hosts <date>" (or the one dated as given), and its
     entries. An entry already DONE was pulled before: say so, and still check it.
   - From each =[[bashRun:git -C <path> pull]] --- <commit>= line: the path (=/bxRepos/...=) and the
     commit. A plain line (not pushed, not committed, outside =/bxRepos=) is reported as written.

3. *Each repo, from git* (=git fetch= is fine; never pull):
   - missing (no clone here): to clone;
   - local changes (=git status --short=): to commit or stash before pulling;
   - the listed commit is in HEAD (=git merge-base --is-ancestor <commit> HEAD=): arrived;
   - otherwise, behind (=git rev-list --count HEAD..@{u}=): to pull.

4. *The pip packages.* Run =pipPkgsStatus.cs -i pkgsStatus= and read its output, not its exit code (a
   PyCS program exits 0 even when it fails). Report, for the repos of the pull list first:
   - those whose editable install needs a reinstall: =./pypiProc.sh -i pkgReInstall edit
     /bisos/venv/py3/dev-bisos3= in the package's =py3/=; that also exits 0 on failure, so confirm
     each with =pipPkgsStatus.cs -i pkgLocalStatus <repo>= afterwards;
   - those whose code is newer than PyPI: a release, the user's (skill =bisos-pip-release=).
   =pipPkgsStatus.cs= not found: say so, and skip this step.

5. *The CBMs.* For a pulled repo that is a CBM package (it has a =-cbs.pcs= or a CBM leaf names it,
   skill =cbm-authorship=), say that its capability may need re-materializing, and with what. Do not
   run it.

6. *Report.* A table: repo, state (arrived / to pull / local changes / missing), package (reinstall /
   release / ---), CBM. Then, from the AI-WorkPlan and AI-DevStatus just read, one paragraph on where
   the work stands and what is next. End with the steps in order: pull (the pull list's "All of
   them", or the repos to pull), reinstall, release, rebuild --- and, once all arrived, offer to mark
   the pull entry DONE (only on the user's agreement).
