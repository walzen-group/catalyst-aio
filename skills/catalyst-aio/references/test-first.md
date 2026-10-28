# Test-first fixes

A fix is proven by a test that failed before the fix and passes after it. This
procedure applies to any change to observable behavior with a checkable
outcome: a bug fix, a changed error message, a new flag, a behavior change in
an existing command. A change whose outcome no test can observe follows the
exception path at the end.

## Rule

Write the test first, pinning the wanted behavior. Run it against the code as
it stands and record the failing run. That red run is the source of truth: it
proves the test catches the unwanted behavior. Then write the minimal fix and
run the test to green.

A test written after the fix gets its shape from the code that exists, so it
pins current behavior while the user asked for different behavior. A test that
never failed proves nothing about the fix.

## Four steps

### Step 1: Write the test

Write one test that pins the wanted behavior. Use the project's test framework
and its existing test file layout.

Record: the test path and the test name.

### Step 2: Record the red run

Run the test against the unfixed code before you change any implementation
file.

```
<test command> <test name>
```

Expected result: the command exits nonzero, and the output names the assertion
that pins the wanted behavior. Save the command and its full output. This is
the red run.

When the test passes at this step, it does not exercise the defect. Rewrite the
test until it fails, and leave the implementation alone.

### Step 3: Implement the minimal fix

Change the smallest amount of code that makes the behavior wanted. Leave
unrelated behavior alone.

Record: the diff.

### Step 4: Run to green

Run the same test command from Step 2.

```
<test command> <test name>
```

Expected result: the command exits 0 and the test passes.

When the test still fails, the fix is incomplete. Change the fix, never the
wanted behavior the test pins. When the test fails on its setup, repair the
setup and keep the assertion unchanged.

## What counts as a recorded red run

A recorded red run has all of these:

- the test file and test name that appear in the final diff;
- the exact command, copied from the shell history, with the same arguments
  the green run uses;
- the command output showing the assertion failure for the wanted behavior;
- the output kept where the verifier can read it: pasted into the worker
  report, or saved under the repo-local .tmp/ directory with that path named
  in the report.

A run does not count when:

- the test did not compile, import, or start. That failure proves the test
  harness is broken; repair it and run again. For a new behavior, a failure
  because the entry point does not exist yet is a real red run, because the
  wanted behavior is absent. A typo or a bad import in the test is not.
- the failing output comes from another test in a wider suite. The red run
  must be the test the fix keeps.
- the output was produced before this checkout, on another branch, or against
  code the fix does not touch.
- the test was edited after the green run. Both records are stale; run Steps 2
  and 4 again.
- the red run comes from reverting a fix that was written first. That sequence
  is test-after with a demonstration, and the test still got its shape from
  the fix.

## What the verifier checks

The verifier confirms a fix only after reading the recorded red run. It checks:

1. A red run exists for the test in the diff.
2. Red and green use the same test command.
3. The red failure is the assertion for the wanted behavior.
4. The test remains in the tree and is not skipped.
5. The green run is recorded after the fix.

No green run is accepted without a recorded red run, whatever the diff size.
Missing red evidence blocks acceptance: the verifier sends the task back for
the record and does not accept the fix on the worker's confidence.

## When a red run is impossible

Some defects cannot show a red run in the worker's environment: a race that
needs production traffic, a failure that needs data or a service the checkout
cannot access, an output no test harness exposes. The worker stops and reports:

- the test it wrote and the behavior that test pins;
- why the red run cannot be produced here;
- what evidence stands in for the red run;
- what that evidence does not prove.

The orchestrator puts those facts to the user and asks whether to accept the
exception. A confirmed exception is written into the fix report and the
verifier hand-back: the user's answer, the reason, the test path, and the
substitute evidence. The test stays in the change. Without the user's
confirmation, the task stays blocked.

A worker never invents a red run, marks the test skipped, or reports the fix
verified on a red run it did not produce.

## Where the procedure is carried

| Place | What is recorded |
|---|---|
| Fix spec (references/planning.md) | The four steps, the test path, and the report field for the red run |
| Worker report | The red run: command, output, and the path where the output is kept |
| Verification (references/verifier.md) | The red-run check before any green evidence is accepted |
