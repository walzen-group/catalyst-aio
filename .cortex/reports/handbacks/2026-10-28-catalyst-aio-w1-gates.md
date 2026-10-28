# Wave 1 gate evidence: aio-t1-inventory (task 1, insight inventory)

Recorded 2026-10-28 by meta-aio-w1. Transcribed from the worker's own session
(`herdr agent read aio-t1-inventory`, revision 2, after completion_seq 347).
Not re-run: this is the record the verification rests on.

## The worker's recorded acceptance run

Command, as it appears in the session:

```
f=inventory.md; for c in keep adapt drop; do printf '%s %s\n' $c $(grep -cE "^\| [0-9]+ \|.*\| $c \|" $f); done; printf 'NEW %s machinery %s README %s\n' $(grep -cE '^| ...
```

Output, as recorded:

```
keep 163
adapt 128
drop 30
...
```

The `NEW` / `machinery` / `README` line and the target check follow on the same
run (screen truncated after the class counts). The target check printed no
output, matching the spec's expectation.

The session also shows the in-place diff of the repair round, which matches the
file on disk:

- row 169: `drop | c2d steer delivery machinery` ->
  `adapt | Applies to any report that a prompt or message to a subagent failed. | references/subagents.md`
- row 232: `drop | CLI trust-prompt machinery` ->
  `adapt | The CLI trust-prompt case becomes any consent prompt the harness or a subagent raises. | references/subagents.md`
- row 321 added: `keep | NEW from incidents. | references/reduced-workset.md`

## Independent recheck by the verifier

Column-based parse of `inventory.md` (not the worker's grep):

| Read | Result |
|---|---|
| rule rows | 321 |
| classes | keep 163, adapt 128, drop 30 |
| keep/adapt rows with a target outside the eleven index filenames | 0 |
| keep/adapt rows with no target | 0 |
| targets used | all eleven |
| coverage rows | 86 |
| date-prefixed coverage rows | 85 |
| coverage verdicts | covered 51, NEW 9, machinery-only 25, README row 1 |

## Acceptance defects recorded, not gamed

1. `grep -c '^| [0-9]' inventory.md` returns 406, not the rule-row count: the
   pattern also matches the 85 date-prefixed incident rows. Rule rows are 321.
2. The incident-row grep counts date-prefixed rows (85) while the file list is
   86, because `README.md` is the directory index rather than an incident and
   its coverage row carries no date prefix. No row was fabricated to make the
   two numbers agree.
3. The index's "88 incidents" is the directory-entry count (85 dated reports +
   `README.md` + 2 evidence folders), not the report count.
