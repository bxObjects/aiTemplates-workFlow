Work on the TODO with anchor <<$ARGUMENTS>> of AI-WorkPlan.org: read it, restate it, then do it.

The anchor is $ARGUMENTS, e.g. =7.1= for <<7.1>>. With no argument, list the open TODOs of the current
Stage (anchor, priority, owner tag, headline) and ask which one.

1. *Read from disk*, not from memory: AI-WorkPlan.org of the current directory (the user may have
   edited it). Find the heading containing <<$ARGUMENTS>>; if there is none, or more than one, say so
   and stop. Read its whole subtree, the goal of its Stage, every anchor or link it cites (=[[7.1]]=,
   =[[Stage 8]]=, files), and its Results so far.
2. *Restate* in a few lines: what it asks for, what is already done (its Results), and your plan ---
   the files you would change, in which repos, and what you would test.
3. *The owner tag decides whether to go on*:
   - =:ai:= --- proceed with the plan.
   - =:aiAndMe:=, =:me:= or no tag --- stop after the plan and wait for the user's agreement.
   A TODO marked DONE: say so and ask whether to reopen it.
4. *Do the work* as the plan says, in the repos named by its =:REPO:= / =:BASE:= properties (=this=
   is the current repo), following the activity's AI-Activity.org and its skills.
5. *When finished*, write the Results under its "----- Results: ------" line: dated, what was done,
   what was tested and how, what is not done. Do not mark it DONE: that is =/bx-todo-done <anchor>=,
   which the user runs (or asks for).
