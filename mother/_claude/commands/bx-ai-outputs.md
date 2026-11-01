Read from or capture into =./AI-Outputs.org=, the central cross-project findings
ledger, in the current directory. Not every project has this file — it is an
optional symlink from =mother/AI-Outputs.org=; if absent, say so and stop rather
than creating one (that is =aiActivity.cs='s job — see =refresh=).

With no $ARGUMENTS — list/index mode, read-only:

1. Read =./AI-Outputs.org= in full.
2. Print an index grouped by keyword, in this order: =WORKAROUND= first (highest
   broadcast value — these save the next Claude from rediscovering a live breakage),
   then =TODO=, then =INFO=. Within each group, one line per entry: headline,
   =:REPO:=, and =:FOUND:= date.
3. Do not print entry bodies. State that full text for any entry is available by
   opening the file directly, or by re-invoking with a substring of the headline.
4. If any entry's =:REPO:= matches the current project, call that out separately at
   the top — it is directly actionable from here.

With $ARGUMENTS — capture mode, write:

1. Draft a new entry from $ARGUMENTS and whatever the current session already
   established about the defect (file/line, observed command, error text).
2. Ask for whatever isn't already clear rather than guessing silently:
   - *Keyword*: =TODO= (no workaround, or none needed), =WORKAROUND= (broken, *and*
     a workaround is documented and in use), or =INFO= (captured knowledge, no
     action required).
   - *=:REPO:*= — the repo where the *fix* belongs. Often not the repo you're
     standing in right now; do not assume they're the same.
3. Set =:FOUND:= to today's date. Shape the body like existing entries: *Where*
   (file:line or component), *What* (the defect), *Impact*, and *Workaround* (only
   if keyword is =WORKAROUND=).
4. Placement: append under an existing top-level section whose scope already
   matches the entry's =:REPO:=/package (e.g. an existing =* Defects --- <package>=
   heading). If none fits, create a new top-level section following the existing
   naming convention. =INFO= entries with no actionable repo go under
   =* Captured Knowledge=.
5. Show the drafted entry and its intended placement. Append only after
   confirmation — do not write silently.

Scope: this command touches only =AI-Outputs.org=. It is not the only way to add
an entry — the file is an ordinary (symlinked) org file and a direct edit works
too — but it exists to keep entries in a consistent, dispatchable shape rather
than to gate access.
