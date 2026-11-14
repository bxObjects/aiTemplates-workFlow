Propose evidence-backed improvements to the skills and commands from this session's lessons.

Use near the end of a session, before =/bx-session-verify=, so that the repos it changes are
verified and committed with the rest. Report first; write nothing until the user agrees.
Arguments: none, or the names of the skills and commands to look at.

1. *The lessons.* From this session's own record (the conversation, and its summary if it was
   compacted), list what cost time or caused an error and that a skill or command should have
   prevented: a missing step, an instruction that is wrong or stale, a trap found, a workaround,
   a correction by the user. For each, Read the current text of the skill or command it would
   change, from disk; drop it if that text already says so. Not lessons: a one-off fact about the
   project (it goes in AI-DevStatus.org), a personal preference of the user (it goes in memory),
   anything not checked.

2. *Where each lives.* The real path of each target (=readlink -f=), and its repo: a skill in its
   templates tree, a command in the =aiTemplates-workFlow= tree, never through a link in a base.
   Where a lesson goes: a trigger (when to load which skill) or a decision that crosses skills
   belongs in the activity's =AI-Activity.org=, which is always loaded and so is kept thin; a
   procedure or a pitfall belongs in the skill (=authoring-skills=: "When to Write a Skill vs
   Adding to AI-Activity.org"). Say whether that repo is public. A public tree takes no proprietary
   names (organization, product, host, internal URL): write the lesson generically, or leave it
   out and say so.

3. *The report.* A table, most costly first, at most about seven rows: target, the lesson (one or
   two lines), the evidence (what happened this session), the proposed edit (where, and at most
   three lines), file and repo, public or not. No lesson is a valid answer: say so, do not invent
   one. Then offer to apply all or some rows.

4. *Apply, when the user agrees.* Edit in place and merge into the existing section; a new skill
   only when the user asks (see =authoring-skills=). After a command file is added, renamed or
   deleted, say which templates bases are behind (=aiActivityAssemble.cs -i assemblyCheck=) and
   leave =assemblyUpdate= to the user. Never stage, commit or push. List the repos now changed, so
   that =/bx-session-verify= and the commit walk cover them.

5. End with one line: the edits made (or none), then "Next: =/bx-session-verify=".
