# Issue tracker: Local Org

Issues and specs for this repo live as Org files in `.scratch/`.

## Conventions

- One feature per directory: `.scratch/<feature-slug>/`
- The spec is `.scratch/<feature-slug>/spec.org`
- Implementation issues are one file per ticket at `.scratch/<feature-slug>/issues/<NN>-<slug>.org`, numbered from `01`, never a single combined tickets file
- Each file opens with the keyword block in "File header" below
- Triage state is recorded in the `#+STATUS:` keyword near the top of each issue file (see `triage-labels.org` for the role strings)
- Comments and conversation history append to the bottom of the file under a `* Comments` heading, one sub-heading per comment headed by the date and author

## File header

```org
#+TITLE: <one-line ticket title>
#+STATUS: <triage role string>
#+DATE: <YYYY-MM-DD>
#+TYPE: <research|prototype|grilling|task>   # wayfinder tickets only
#+BLOCKED_BY: <NN, NN>                       # wayfinder tickets only
```

`#+STATUS:` holds a role string from `triage-labels.org`. It is a file keyword, not a TODO keyword, so it greps as `^#\+STATUS:` and does not depend on anyone's `org-todo-keywords` setup. Ordinary headings and TODO keywords stay free for subtasks inside the body.

## When a skill says "publish to the issue tracker"

Create a new file under `.scratch/<feature-slug>/` (creating the directory if needed), starting from the file header above.

## When a skill says "fetch the relevant ticket"

Read the file at the referenced path. The user will normally pass the path or the issue number directly.

## Wayfinding operations

Used by `/wayfinder`. The **map** is a file with one **child** file per ticket.

- **Map**: `.scratch/<effort>/map.org` (the Notes / Decisions-so-far / Fog body).
- **Child ticket**: `.scratch/<effort>/issues/NN-<slug>.org`, numbered from `01`, with the question in the body. `#+TYPE:` records the ticket type (`research`/`prototype`/`grilling`/`task`); `#+STATUS:` records `claimed`/`resolved`.
- **Blocking**: a `#+BLOCKED_BY: NN, NN` keyword near the top. A ticket is unblocked when every file it lists is `resolved`.
- **Frontier**: scan `.scratch/<effort>/issues/` for files that are open, unblocked, and unclaimed; first by number wins.
- **Claim**: set `#+STATUS: claimed` and save before any work.
- **Resolve**: append the answer under a `* Answer` heading, set `#+STATUS: resolved`, then append a context pointer (gist + link) to the map's Decisions-so-far in `map.org`.
