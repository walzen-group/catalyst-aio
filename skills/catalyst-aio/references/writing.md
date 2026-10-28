# Writing for humans

Every text a catalyst-aio role writes for a person follows these rules. The
pass depends on the reader: user-facing docs and deliverable reports get the
humanizer pass and its closing audit; plan docs, specs, briefs, and hand-backs
keep the style rules and skip the pattern audit.

## Which texts get which pass

| Text | Pass |
| --- | --- |
| README.md, files under the repo's docs tree, deliverable reports under .cortex/reports/ | Style rules, humanizer pass, closing audit, final grep |
| Plan docs, task specs, briefs, worker reports, hand-backs, memory entries, feedback entries | Style rules and final grep |

Code spans in this file mark a phrase quoted as a banned pattern, a bad
example, or a literal token. Read them as material, not as prose to copy.

## Structure

1. Procedures use numbered steps or `### Step N: <verb phrase>` headings, one
   action per step. Put each command in its own fenced block under the step.
2. A verifying step states its expected result on its own line ("Expected
   result: no output.") and, where the result can go wrong, what a wrong
   result means and what to do.
3. Rationale gets its own paragraph, placed after the step or command it
   explains. A reader who wants only the commands can skip every paragraph
   with no fenced block.
4. Preconditions, requirements, and failure modes get their own subsection
   after the procedure.
5. Command reference lists go in a two-column table (command, effect). Follow
   the table with one short subsection per command that needs more than one
   sentence of explanation.
6. A decision paragraph follows a fixed order: state the decision, then for
   each alternative what happens when it is used and what the operator sees,
   then what the chosen option does. Give each consequence its own sentence
   with a concrete subject. A reader who never used the rejected option must
   be able to tell from the paragraph alone what would go wrong with it. Put
   the paragraph where the repo keeps its decision records.
7. One doc owns each fact. Search the existing docs before writing an
   explanation:

   ```
   grep -rl "<a distinctive phrase from the fact>" <docs directory>
   ```

   A fact with a home gets a link, and the new doc covers only what differs
   from it. Two sibling docs that both explain the same mechanism drift apart
   at the first edit, and a reader who later finds both cannot tell which one
   is current. Where a fact has to appear twice, one copy is the source and
   the other names it: "The object list is in the overview doc; this table
   adds the mode column."
8. A page's first paragraph says something its title and its neighbouring
   pages have not: how the system behaves, or where the reader goes next. Move
   housekeeping, such as where the source files live, to the end.
9. When the reader picks between variants of a file, show each variant they
   are likely to write as its own complete block. "Replace `all: true` with
   `source: <claim>`" leaves the reader to assemble the file, and it says
   nothing about the case the swap does not cover: a field that accepts one
   name when the reader needs two objects and so two documents.
10. A page for a case most readers do not have names that case in its title.
    Open with the common cases it does not cover, as a table where there are
    several, and say in plain words that a reader in one of them is done. A
    title like "Give an app a hostname of its own" reads as something every
    app needs, and readers who are already done start on the procedure.
11. Treat a tool the page's readers already use as a prerequisite. State the
    settings the repository depends on ("You need Nix with flakes enabled and
    direnv hooked into your shell") and neither explain nor link how to
    install it. Write install steps only for a tool the reader meets for the
    first time on that page, such as Nix on a Windows route.

## Sentences

- Write complete sentences that carry the meaning. Concise means every
  sentence says something; it never means cutting words out of a sentence.
  "Start from the app closest to yours" leaves the reader to decode it, so
  write "pick the app that matches the one you want to deploy most closely".
  A fluent phrase such as "making use of" stays: a shorter synonym makes the
  text no denser, only less fluent.
- Describe what a component is and does. A negative or contrast earns its
  place only where a reader would otherwise take a wrong path, and it follows
  the positive statement. "This deletes the record from the index. It does
  not delete the file" earns the negation, because a reader might assume
  otherwise.
- Cut filler summaries and closers that restate the section or moralise.
  State the instruction or the fact. A short closing sentence that adds a
  fact belongs in the doc: "The change applies at the next rebuild."
- Vary sentence shape as well as length. Use a subordinate clause, a lead-in
  clause, a colon, or a semicolon where two statements belong in one sentence;
  roughly one sentence in three may carry such a clause. Two rationale tails
  in a row, each sentence ending in a because, so, or since phrase, is the
  signal to rewrite one of them.
- Put a concrete actor, its action, and a particular in every sentence that
  explains something. A tool may act; an abstract noun may not. The particular
  is the literal material: the path, the key, the value, the error text. Test
  the sentence by swapping its domain nouns for nonsense words; if it reads as
  well, it says nothing. "Grafana reads every .json under
  /etc/grafana/dashboards into a folder called Provisioned" turns to nonsense
  when the path changes.
- The verbs "owns", "handles", "manages", "governs", "drives", "controls",
  "applies to", "delivers", "is responsible for", and "is in charge of"
  describe no action of their own. Rewrite the sentence around the actor and
  what it does.
- When a sentence crosses several files or components, write one link per
  sentence. A noun phrase that stacks three links leaves the reader unable to
  look any of them up: write what the first file sets, the block it points
  at, and the call the second file makes. Stacked possessives compress a
  chain the same way, so say who sets the value and give an example value.
- Where the doc can show the mechanism running, show it first and let the
  prose annotate the output. A paragraph that describes behavior with no
  example, no rendered block, and no literal value has not explained it.
- State the literal condition, and give the operation its own verb. Keep out
  four classes of metaphor: place ("out of view"), sight ("hide"),
  temperature ("from cold"), and volition (`ask both keys what they hold`).
  Each one replaces a verb the reader needs. "A symlink hides those skills"
  leaves the reader unable to tell whether the files were deleted or shadowed;
  write "the symlink shadows them, and they stop resolving". Keep the phrases
  that already name their operation: falls back to, survives a reboot, picks
  up the change, shadows the entry, drops the connection. Get rid of verbs
  that report data being somewhere or a value moving by itself; the triage
  table below lists them. Write the event and its actor: "the restore fills
  the new dataset from the latest backup".
- No sentence fragment used as a lead. A short label before a command block
  ("One unit:") is fine.
- Never price a design in `costs` or `buys`. The metaphor has no actor or
  action, so the reader has to convert an imaginary currency back into the
  mechanism. Write the consequence with a verb that says what happens, and a
  number where a number exists.

| Bad form | Write |
| --- | --- |
| `Enabling it costs a machine config apply.` | `Enabling it requires a machine config apply.` |
| `Following the endpoint costs a watch.` | `Following the endpoint requires the controller to open a watch.` |
| `Compression costs little CPU.` | `Compression uses little CPU.` |
| `Adding a cluster costs nothing here.` | `Adding a cluster doesn't change any file in this module.` |
| `What it buys is a second replica.` | `It adds a second replica.` |

A trade-off still gets written out: name what the design gives the reader in
one sentence and what it demands in the next, each with its own verb.
"Restoring from the warm copy finishes in seconds. Keeping the warm copy uses
disk for a second copy of every volume." A two-column comparison uses headings
that name the quantity, such as "Restore time" and "Disk used".

- Do not close a sentence with `", and nothing else"`. Put the restriction
  inside the sentence with "only" or "just", or write it as its own statement.
  `The Role covers Backup objects in that namespace, and nothing else`
  becomes `The Role covers only Backup objects in that namespace`. A sentence
  that opens with "Nothing else" goes too: `Nothing else changes` becomes
  `The rebuild changes only this file`.

## Naming

- Name the deprecated or replaced command when one supersedes it: "This flag
  replaces the deprecated terraform taint command." A reader arriving from old
  material then finds the mapping.
- Never let "thing" stand in for a name. `Three things in it matter` becomes
  `The podAffinity block, the write loop and the update strategy are what to
  read in it`. A count of inputs, fields, or keys withholds the names the same
  way; write the name and what it decides: "set backup_enabled to create the
  backup objects". The word is fine where no name is withheld ("here is the
  thing", "something", "anything", "nothing").
- Name what you mean wherever the reader would otherwise have to work it out.
  Write the resolver, the ingress unit, the wildcard record. Spell the name
  again when two or more candidates are in scope, when the reference crosses a
  heading, a fenced block, or a table, or when its referent sits more than one
  sentence back. "here", "there", "such a name", and a bare "one" ask the
  reader to reconstruct a name, so write the name. A pronoun for the nearest
  single referent is correct English: "The activation step creates the
  directory and sets its mode to 700." Varying the name stays banned; a
  synonym invents a second name for the same thing. Before a table or a list,
  say that it follows and what it lists.
- Point at one example among several by what it contains. "The fullest one"
  ranks the examples on a scale the reader never saw; "the tutorial with a
  database, a backed-up volume and a volume without backup" tells them what
  they get. A definite noun the reader has not met gets the word that says
  where it comes from: "the provisioned wildcards".
- Keep examples in this skill free of one environment's values. A domain, a
  node name, an address, or a cluster name belongs in the repo doc that
  configures it. An example here uses a role name (the ingress unit, the
  worker that stores the volume) so it reads the same in every repo.
- Describe a group by what puts a member in it, and name one or two members as
  examples: "put an app that administers the platform, such as a dashboard or
  a database console, in management". A complete list of today's members is
  state; it goes stale with the next addition. To point at a group of files,
  say what they share and link the page that lists them. Introduce a single
  file by its path and what it sets.

## Verbs and person

- Pick the person by asking who acts.

| Who acts | Person | Example |
| --- | --- | --- |
| The software at runtime | Name the component | The activation step creates the directory and sets its mode to 700. |
| The reader at a terminal | Imperative, or "you" for an obligation | Run the rebuild. You need the submodule checked out before the first rebuild. |
| The maintainers making a choice | "we" | We pin the submodule at a fixed path, so the modules reference it directly. |
| An obligation the mechanism imposes | "we need to", "you need to", "the run requires" | The mover writes into a mounted volume, so we need to scale the workload to zero for the run. |

- Reporting a consequence and stating an obligation are different jobs. "We
  stop the workload for the run" reads as a description the reader can skim
  past. "We need to scale the workload to zero for the run" says the run
  demands it, so skipping the step breaks something. In a tutorial the reader
  acts, so write "you need to"; in an explanation of the repo's design, write
  "we need to".
- Write in the active voice. A passive drops the actor. Keep the passive where
  the actor is unknown or beside the point ("The lock file is regenerated at
  build time"), or where the object is the subject of the paragraph.
- "we" states design decisions and their reasons, so it belongs in rationale
  paragraphs and decision records. A procedure step stays imperative and says
  "we" nowhere. Two uses are banned: a "we" that stands for the reader ("we
  now run the rebuild") and a "we" that stands for the machine ("we then
  resolve the hostname"). Budget about one "we" per rationale paragraph, and
  open no two consecutive sentences with it.
- Keep the action in the verb. "The runner performs a validation of the
  manifest" becomes "The runner validates the manifest". Watch nouns ending in
  -tion, -ment, -ance, -ency, and -ure, and verbs used bare as nouns (a run, a
  walk, a check, a read, a fix) next to perform, provide, make, conduct,
  achieve, do, give, take, and have. A nouned verb also invites jargon: once
  "a walk" exists as a noun it is one edit from a capital letter, a
  definition, and repetition until it reads as vocabulary the reader agreed
  to. Describe the action again on each mention.
- Give every sentence a subject and a finite verb, status lines included. "The
  runner, built." becomes "We built the runner." "Migration complete, the old
  path removed." becomes "The migration is complete. I removed the old path."
  A trailing modifier drops the actor and the tense, so a reader learns
  neither who acted nor whether the work finished.
- Never write that a thing `takes` a property, a setting, a file, or a repair.
  The verb describes no action, so the sentence reports a state of the subject and
  leaves the reader to work out who acts and what they type. Name the action
  and its actor; in a procedure, use the imperative.

| Bad form | Write |
| --- | --- |
| `The build file takes the backup include.` | Add the backup include to the build file. |
| `A volume the app can rebuild takes no backup.` | Do not back up a volume the app can rebuild. |
| `## Which mode an app takes` | `## Which mode to use` |
| `The doc lists the statuses and what each one takes.` | The doc gives the repair for each status: delete a pending-install record, roll a pending-upgrade record back to the last deployed revision. |

Keep the verb for a duration or a quantity: "the probe runs in 528 ms". Every
other hit has a real verb hiding in the sentence, usually requires, needs,
accepts, or the imperative.

- Negate the verb. `An app without any of those lists no ConfigMap` reads as
  translated text; write "An app without any of those doesn't list a
  ConfigMap". The same goes for `names no`, `writes no`, `reads no`, and
  `sets no`. "has no" stays where "has" is the plain verb.

## Punctuation and formatting

- Use tables for key/value lists (file to what it sets, scope to packages,
  host to identity). Reserve prose for the explanatory parts.
- Minimise inline single-backtick spans. Put filenames and module or option
  names in plain text; reserve backticks for shell commands, fenced blocks,
  and literal option = value tokens. Technical identifiers are names too:
  operationIds (auth.set), schema names, paths, hashes, and class names stay
  in plain text. A phrase quoted as a banned pattern goes in a code span, so a
  reader can tell the file mentions it without using it.
- Bold sparingly: only the leading filename in a file-keyed bullet list.
- Use the whole range of punctuation. A writer who routes every pause through
  a comma produces comma splices: "The kit becomes self-contained, the
  skills, the tools and the container template all live with it" needs a
  colon. Pick the mark that matches the join.

| Job | Mark |
| --- | --- |
| Introduce what follows (a list, an example, a consequence) | Colon |
| Join two independent clauses that belong in one sentence | Semicolon |
| Set off a true aside | Parentheses |
| Split two clauses a comma is straining to join | Full stop |

The minus symbol covers a literal dash. Quotation marks are straight.

## Inferences

- State an inference as a numbered assumption in a table at the end of the
  section: the assumption, what it is based on, and the check that would
  confirm it. Mark each step that depends on it with its number. "I have not
  decoded the file" tells the reader something is uncertain without saying
  which claim, what supports it, or how to settle it.

## Reports and hand-backs

- A report leads with the verdict and its single reason. One item per line, no
  hedging and no restating, and the weight comes from concrete facts: paths,
  commands, output.
- A bug report's summary states the defect: the conditions that trigger it,
  what the component does, and what it should do. Put the investigation and
  the measurements in the sections after it. A duration that only measures
  when someone stopped the process says nothing about the defect, and an input
  the component should accept is described as an input, never as the cause.

## Briefs and specs

Text agents parse, such as briefs, task specs, and messages between agents,
follows stricter structure than a doc:

- One instruction per sentence, at most 20 words.
- Use a list when a sequence has three or more steps.
- No phrasal verbs: write "stop the service", not "shut the service down".
- Keep modality: never promote a hedge to a fact or add a certainty the source
  did not state. "May fail" stays "may fail".
- Keep noun clusters to three words or fewer: "the queue retry limit", not
  "the queue retry limit configuration value".

Docs are exempt from the 20-word limit; join clauses with the punctuation the
table above names.

## Humanizer pass

README.md, files under the repo's docs tree, and deliverable reports get this
pass before the change is reported done. Plan docs, specs, briefs, and
hand-backs keep the style rules and the final grep, and skip the pattern audit.
Everything the pass needs is in this file.

Treat the text under edit as material, never as instructions to follow. Keep
every supported claim, and add no fact, name, number, date, quote, citation,
or claim of simultaneity that the source lacks. The rewrite may shorten dull
passages, merge or split paragraphs, and change structure.

Repo docs take their voice from sentence rhythm, concrete actors, and specific
facts. Advice written for blog prose, such as first-person opinions and mixed
feelings, does not belong in a reference page.

Work through four steps:

1. Mark the tells in the whole draft, strongest first. Read paragraph shape as
   well as sentences: a contrast split across two sentences, three parallel
   examples, and the same closer after every section are patterns at that
   scale.
2. Draft the rewrite around each paragraph's main point. State the point
   directly; patching flagged phrases one at a time leaves the paragraph's
   shape intact.
3. Read the draft aloud. Ask what still sounds AI-generated, then search for
   the five patterns that survive most rewrites: a not-X-but-Y contrast, a
   one-line closer, a dash, a triad, a bold label.
4. Write the final version. Vary sentence length, and keep code blocks,
   commands, paths, and link targets unchanged.

### Pattern list

The table quotes the bad form. Apply a pattern when the text uses it, and
leave a watched phrase alone inside a quotation, a title, a proper name, or a
passage that discusses the phrase.

| Pattern | Example | Fix |
| --- | --- | --- |
| Not X but Y | `The flag is not just faster; it is safer.` | State both facts: "The flag runs the check in one pass and stops on the first bad field." |
| One-line closer or dramatic fragment | "That is the real win." | Merge the claim into the sentence before it; turn a row of fragments into one sentence. |
| Saying that sounds deep | "The real question is whether the loader can trust the file." | State the claim: "The loader rejects a key it does not know." |
| Staged run-up | "Let's dive into the retry logic." | Delete the run-up and state the fact. |
| Arguing with nobody | "Some might say the cache is unnecessary, but the loader needs it." | State the claim: "The loader needs the cache." |
| Forced triad | "fast, safe, and reliable" when the code has two of those properties | Name the properties the code has. |
| Repeated sentence openings | "The loader reads the file. The loader checks each key. The loader writes the result." | "The loader reads the file, checks each key, and writes the result." |
| Dash as connector | `The fix — which is small — ships today.` | "The fix, which is small, ships today." |
| Stacked qualifiers | "It could potentially possibly fail." | "It may fail." |
| Decorative hyphenated pairs | "The report is high-quality and data-driven." | "The report is high quality and data driven." |
| Overused AI words | `crucial`, `pivotal`, `underscore`, `interplay`, `leverage`, `seamless`, `robust` | Use the plain verb: matters, shows, uses. |
| Inflated significance | `The change marked a pivotal moment for the project.` | "The project adopted the change in March." |
| Vague connection | "The script is associated with the build." | Name the relation: "The build calls the script." |
| Shallow -ing rider | "The command writes the file, ensuring a clean state." | "The command writes the file. The next run starts from a clean state." |
| Sales language | `a vibrant, rich, seamless experience` | Say what the software does: it reads the config and starts the service. |
| Borrowed authority | "Experts agree the format is safer." | Name the source and what it said, or cut the claim. |
| Dodging is, are, and has | "The directory serves as the cache." | "The directory is the cache." |
| Decorative heading | `## The Key Insight` | "## Cache expiry", in sentence case with no decoration. |
| Chatbot residue or knowledge-limit disclaimer | "Great question! As of my last update, the flag is unsupported." | Remove the wrapper; state the fact or say what the source does not show. |
| Heading repeated in the first sentence | `## Performance` followed by "Speed matters." | Start with the content: "When a page loads slowly, users leave." |
| Writing about the previous version | "This replaces the old loop, which was slow." | Describe current behavior; keep history for changelogs. |

A weak tell needs company: one dash, one stacked qualifier, one hyphenated
pair, one passive, or one curly quote is not enough on its own. Act when
several share a passage. Leave quotations, titles, proper names, and passages
about a phrase alone.

### Closing audit

Run the audit over the whole draft, after the pattern table:

1. Ask "What makes the below so obviously AI generated?" Answer in one or two
   sentences.
2. Ask "Now make it not obviously AI generated." Revise the draft with that
   answer, then read it whole. Even rhythm across evenly paced paragraphs is
   one of the tells the audit catches; a draft that satisfies every rule and
   still reads flat fails the audit.

## Final grep

Run this over every file you wrote or edited, and fix the hits before you
report the change as done:

```
grep -nEi '—|–|honest|\b(it|that|this) (buys|costs)\b|what it buys|the cost is|at the cost of|costs nothing|worth (noting|knowing|saying|flagging|paying)|blast radius|load-(bearing|carrying)|footgun|smoking gun|push back on|rather than|not just|not only|instead of|isn.t about|, and nothing else|(^|[.!?] )nothing else|a real gap|(one|two|three|four|five) (things|reasons|points|findings)|\bwir(e|es|ed|ing)\b|^#+ the |\btakes\b|\b(holds?|holding|held|reach(es|ed)?|carries|appears|arrives)\b|\b(lists|names|writes|reads|sets) no\b|crucial|pivotal|underscore|interplay|leverage|seamless|robust' <files>
```

Each hit is a candidate. The table says which ones to rewrite on sight.

| Hit | Action |
| --- | --- |
| Em and en dashes, `honest`, `costs`/`buys`, `worth noting`, `blast radius`, `load-bearing`, `footgun`, `smoking gun`, `push back on`, `a real gap`, `", and nothing else"`, a sentence opening with `Nothing else`, a counted opener, a heading starting with `The`, and the listed AI words | Rewrite. This file bans all of them outright. |
| `rather than`, `instead of`, `not just`, `not only`, `isn't about` | Keep only a contrast that warns against a wrong path. |
| `wire`, `wiring` | Keep only a literal wire. |
| `takes` | Keep only for a duration or a quantity. |
| `holds`, `held`, `reaches`, `carries`, `appears`, `arrives` | Rewrite where the word stands for data being somewhere or a value moving by itself. |
| `lists no`, `names no`, `writes no`, `reads no`, `sets no` | Negate the verb. |

A hit inside the search pattern, a code span that quotes a banned term, or a
bad example in the pattern table is the quotation this file is discussing.
Leave it and name it when you report the file's grep result.
