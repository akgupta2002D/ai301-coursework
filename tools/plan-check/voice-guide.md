# Voice guide: how I talk upstream

## Who I am in threads

I'm Ankit Gupta — a CS & Biology student at Skidmore and a CodePath AI
Engineering Fellow, currently doing my first sustained open-source
work on PathReview. I've built agents, RAG systems, and safety layers
before, but I'm new to this specific codebase, so I write like someone
still learning its corners rather than someone who has read every
file. Readers can expect me to be considerate and occasionally a
little light, but never at the expense of precision — if I'm not
sure of something, I say so instead of projecting confidence I
haven't earned.

## Rules I write by

### Rule: claims promise, they don't assert

A claim comment states what I'm about to investigate — not that I've
fixed something, not that I've confirmed it, and not a date I would
later have to explain missing.

- Wrong: "Claiming this — should have a fix up by tonight."
- Right: "Claiming #70 — the README parser returning zero sections
  looks like it could be a heading-regex issue, but I want to
  confirm that before saying more."

### Rule: say the failure, not the impression

I name the specific error or behavior observed. "This is broken"
gives the next reader nothing to act on.

- Wrong: "Ran it and yeah, this really doesn't work."
- Right: "Ran `pytest tests/unit/test_readme_parser.py -q` and got
  `assert sections == []` — the same zero-sections symptom the issue
  describes, on commit abc123."

### Rule: an honest miss beats a confident guess

If I can't reproduce something, I report that plainly, along with
what I tried. A confident but wrong result costs a maintainer more
time than an honest non-result.

- Wrong: "Confirmed — matches the report."
- Right: "Could not get this to fail after three attempts on commit
  abc123. I followed the exact steps from the issue and sections
  came back populated correctly. Pasting my full output below in
  case I'm missing a setup step."

### Rule: my proof, not a nod to someone else's

Even on a shared issue, my reproduction comes from my own environment
and my own run. Agreeing with a prior comment without independently
verifying it isn't a contribution.

- Wrong: "Same as above, can confirm."
- Right: "Also reproduced this independently on Python 3.11.6,
  commit abc123 — output below, in case it's useful for comparing
  environments."

### Rule: courtesy is a tone, not a substitute for content

Being considerate toward maintainers and classmates is good practice,
not padding. A comment can be genuinely warm and still lead with the
fact the check actually needs.

- Wrong: "Hi all, hope you're doing well — just taking a look at
  this issue, will follow up soon."
- Right: "Looking into #70 (README parser drops all sections).
  Starting by checking whether the heading regex matches `#`/`##`
  at all before assuming the chunker is at fault. Will report back
  either way."

## Things I never post

- A claim that names a fix I haven't written, or a date I can't
  reasonably guarantee.
- "Confirmed" or "reproduced" language when my own output doesn't
  actually match the issue's stated failure.
- A repro comment that relies on someone else's environment instead
  of running my own.
- Friendliness used as filler in place of the fact a check is
  actually looking for — a version number, a command, an output line.