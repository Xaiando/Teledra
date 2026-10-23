# ARC-004: The Scientific Explorer

**Public technical research report · 23 October 2026 · Evidence edition**

- **Research direction and conceptualization:** [Sveinung Ramse](https://orcid.org/0009-0008-7394-4519)
- **Affiliation:** Shipyard AS (Research and Development)
- **Experimental engineering and report compilation:** AI coding assistant (Codex), under human research direction
- **Status:** Exploratory research report; **not peer reviewed**
- **Scope:** Local CPU experiments on held-out **synthetically generated** interactive environments, **not official ARC-AGI-3 games**

**Citation:** Ramse, S. (2026). *ARC-004: The Scientific Explorer — Active Rule Discovery and Experimental Reasoning in AgentKit.* Technical research report (experimental evidence edition). [Teledra GitHub research archive](https://github.com/Xaiando/Teledra/tree/main/research).

> **Publication note.** The text below preserves the consolidated experiment evidence as supplied by the project. Reproduction commands, Windows filesystem locations, hash receipts and references to neighboring files describe an **internal experimental checkout**; those source files, binary artifacts, datasets and raw result files are **not included** in this public report. The public record documents the reported methodology, results, controls and limitations, but by itself does not allow independent replication. No general ARC-AGI or recursive self-improvement performance claim is established.

---

# ARC 004 Scientific Explorer Evidence

This report documents a local CPU experiment in automatic rule discovery, active probes and explicit uncertainty. It contains the complete audit, implementation design, interface, measured comparisons, verification and recommended next experiment. No capability is silently promoted.


---

# ARC 004 Scientific Explorer

This experimental engine learns a bounded family of action-consequence rules
from observations and tests competing explanations. It is implemented in the
existing AgentKit Rust package, with a local CPU-only evaluation adapter. The
installed registry and ARC-SENSE implementation are unchanged. Read evaluation.md
for the held-out result; rule discovery and gameplay completion are distinct.

Worktree: D:\Tools\wt-arc-004-explorer. Branch: codex/arc-004-explorer, based on
ARC-003 commit 36a35a7 so that its history and perception bridge can be reused
directly. The Rust module is src/arc_explorer.rs; the example adapter is
examples/arc004_explorer.rs. Everything in this directory is experimental.

## Reproduce

From the worktree root, use the existing provisioned Rust toolchain and cached
dependencies. The package's pre-existing compile-time matting asset must be
provisioned locally at models/matting/u2netp.onnx. On this host it was copied from
the owner-provided main checkout, SHA-256
309c8469258dda742793dce0ebea8e6dd393174f89934733ecc8b14c76f4ddd8.
This asset is not used by the explorer and no model is downloaded or trained.

```powershell
cargo build --locked --offline --example arc004_explorer
cargo fmt --check
cargo clippy --locked --offline --all-targets -- -D warnings
cargo test --locked --offline --test arc004_explorer
```

From experiments/arc004_explorer, use the existing
D:\Tools\arc-agi-3\.venv\Scripts\python.exe in place of python below.
Composition reuses ARC-003's NumPy history estimator and native bridge; it does
not import the trained model or any accelerator framework. Integration tests
use only the Python standard library and the same provisioned NumPy dependency.

```powershell
python -m unittest test_integration -v
python evaluate.py --split development --out results\development-final
python freeze.py
python evaluate.py --split holdout --out results\holdout
```

The supplied results and freeze must be preserved. Reproduce in another output
directory or an isolated copy; do not rerun freeze.py over the retained freeze.
A reproduction of the final holdout may use another output directory while
retaining the same frozen source, engine and protocol checks. Do not change
parameters based on the held-out result and present it as the original test.
The evaluator refuses changed frozen sources/binary or different holdout counts,
budgets and configurations. The freeze pins the supplied Windows debug binary;
another machine/compiler must establish and label a separate reproduction freeze.

The native perception executable is explicitly provisioned at
D:\Tools\agentkit\target\arc-sense-runtime-v3\ak.exe. Existing adapter scripts
are reused read-only from D:\Tools\agentkit\scripts; hashes must match ARC-003's
manifest. Neither those scripts nor the main checkout were edited. No external
downloads, networking, cloud runtimes, neural model inference or training are used by
ARC-004. Evaluation denies Python socket connection entry points.

## Use the component

Send one request matching schemas/request.schema.json to
target/debug/examples/arc004_explorer.exe. Responses match the versioned contract
in integration-contract.md. examples/request.json and examples/response.json
contain a real native-perception example and its actual response. For a Rust
caller, use agentkit_local::arc_explorer::execute on validated structured JSON.
The component cannot execute actions. The existing controller remains responsible
for one legal action, environment feedback, resets, risk decisions and history.

audit.md records existing capabilities and past failures; model-design.md explains
the rule grammar and experiment selection; evaluation.md contains the frozen
comparison and failure analysis; verification.md describes checks and limitations.
Evidence, freeze receipts, development prototypes and raw results stay under this
experimental package. The single consolidated report is copied into
C:\Downloads\Evidence\ARC-004 Evidence\ARC-004 Evidence.md, with no ZIP.

capability-proposal.json is metadata for coordinated review, not an installed
tool or a parallel registry. No automatic index registration or promotion occurs.


---

# ARC 004 Capability Audit

The six existing ARC-SENSE operations already perform perception, temporal
comparison, empirical causal updates, predictions, supplied-hypothesis tests and
experiment scoring. Adding aliases such as a second observe_changes registry
entry would duplicate this work. ARC-004 therefore consumes those summaries and
implements a conditional rule-model extension in the same Rust package.

The bounded live registry lookup found observe, compare, learn, predict,
experiment and step. The opt-in persistent index was unconfigured. A search for
delay returned no indexed resources and explicitly warned that this does not
prove absence elsewhere. The older executable at bin/ak.exe did not understand
the index command; discovery and perception used the existing pinned ARC-SENSE
runtime instead. Exact receipts are retained under evidence/. No index was
initialized or changed, and no Graphify graph existed in this checkout.

ARC-SENSE accepts up to 16 caller-supplied competing effect hypotheses, stores
support/contradictions, uses exact-context and transfer evidence, and scores
experiments. Its documentation explicitly treats failed actions as ambiguous
between blockage, wrong target, hidden change and delay. It does not identify a
conditional collision law or automatically attribute a response to a previous
action. Its coarse generic-change outcomes are not objective recognition.

ARC-003 supplied an independent negative result: 21/36 generated completions
versus 1/36 causal baseline, but no public-game improvement. Its diagnostics
found actor aliasing and repeated boundary mistakes even with the correct actor.
Invisible-objective twins could not be distinguished, and learned probabilities
did not identify that ambiguity. ARC-004 reuses the exact existing observational
actor estimator rather than implementing another segmenter or actor estimator.
The estimator is composed twice, attributing a displacement to current and
previous actions; both use past observations only. The rule engine independently
tests zero- and one-step explanations. Association remains provisional.

The new implementation lives in src/arc_explorer.rs, with an explicit local
example executable. It is not part of the shared installed registry. Existing
ARC-SENSE source, registry/runtime dispatch, installed skills and MiniMax indexing
files are reused read-only and remain untouched. The branch derives from the
ARC-003 research commit to reuse its committed history/bridge implementation.

Baseline A is the unchanged visual policy and B the unchanged causal policy.
C uses the new engine, and D has identical rules and ranking terms with only the
information-gain weight removed. A common rule observer shadows every trajectory
for measurement; it does not choose A or B actions. This separates the quality
of gathered evidence from changes to the model used to score that evidence.

The initial development runs are preserved. They revealed that fixed cyclic
actions can alias a periodic distractor and that a boundary could overwrite a
generated goal marker. Rotating calibration and keeping markers inside the
generated playable area addressed those development defects before freezing.
No final holdout was used to select these changes.


---

# ARC 004 Rule Model and Probe Design

The engine performs bounded model selection over explicit testable rules. It
learns displacement parameters, temporal attribution and conditions; it does not
invent arbitrary new programs or guarantee causal identification. The grammar
is a prior supplied by the implementation. The action mapping, delay and relevant
visible context are not supplied by the environment to the agent.

## Observation and actor candidate

Existing ARC-SENSE observe/compare extracts objects and conservative movement
matches, including multi-frame animations. The new module consumes compact
summaries, preserving clipping flags. It does not segment pixels, decode images
or replace native comparison. Unique unchanged bounding boxes supply stationary
evidence; unmatched/ambiguous changed objects supply no movement observation.

ARC-003's existing History.motion_model supplies an observational actor candidate.
It is reused both with the executed action and with the previous action assigned
to an observed transition. These are alternative temporal attributions of past
evidence, not labels from future frames. The strongest candidate is supplied to
the Rust engine, which tests both immediate and one-step lag explanations again.
Actor identity remains provisional. Rotation between calibration action blocks
reduces periodic phase aliasing but cannot prove independence from all distractors.

## Testable hypotheses

For each legal directional ID, candidate velocities come from observed actor
displacements and their mirrored alternatives, with a zero-displacement option.
At most 17 velocity parameters are retained. The engine tests zero/one-step lag,
free translation, stopping at solid contact, clipping at solid contact, and
clipping at the grid edge. Solid contact is a conservative filled-bounding-box
approximation, not a semantic assertion that every solid-looking object blocks.
Up to two small object shapes whose presence varies can supply presence/absence
guards that predict zero motion in one visible state.

Each candidate is checked against prior native displacement observations. Exact
support, contradictions and distinct local contexts are retained. Candidate
patterns with at least two counterexamples are reported as rejected examples.
The best-fitting lag models are compared across actions; near ties remain
unresolved. A motion mapping is marked parameters_supported only if lag is
resolved, best candidates agree on a nonzero velocity, at least three supporting
observations span at least two contexts, and there are no observed contradictions.
This status supports command parameters under the grammar and actor assumption;
it does not prove the entire response law on unvisited positions.

No displacement does not establish that an action is universally useless. A
zero-motion-only fit remains unresolved. The ledger retains equivalent
explanations and suggests repeating after relocation, varying visible state or
alternating action order. A synthetic two-step delay test correctly remains
outside the supported zero/one-step grammar. Older evidence is explicitly lost
when the caller's 48-transition window advances; no stale discovery is silently
promoted into a permanent universal rule.

Object disappearance near the candidate actor and action-associated changes in
shape presence generate additional event hypotheses. They retain support or
contradicting/non-change observations. Occlusion, animations and unobserved
prerequisites remain alternatives. These events are not counted as verified
causal mechanics or as goal progress merely because they occurred.

## Active selection

Candidate commands are scored by entropy among predicted effect groups, action
coverage, local-context novelty, avoiding repeated actor-context/action pairs,
whether relocation is possible, and tentative contact with small visible objects.
The tentative contact term is an unverified relevance prior, not a goal label.
The remaining budget stops selection and limits delayed follow-up proposals.
Known fatal action/context combinations are excluded; unknown irreversible risk
is explicitly unresolved. A caller can force abstention using a lower risk cap.

The information term is two times the heuristic entropy in bits. All other score
terms are fixed in src/arc_explorer.rs. The D ablation sets only this score term
to zero; it keeps the same ledger, actor estimator and remaining scoring terms.
Delayed follow-up suggestions are descriptive plans. The minimal evaluation
controller executes one recommendation and replans after feedback; it does not
commit to a multi-action plan. This limits what the experiment establishes about
planned intervention sequences.

The component marks recommendations as learn, validate_or_relocate or
tentative_progress_goal_unverified. It never equates novelty or generic scene
change with progress. True information gain is not guaranteed: two finite-model
candidates can be observationally equivalent, the actor can be wrong, or the real
mechanic can be outside the grammar. The evaluator separately counts immediate
effect discriminations whose actual actor outcome removes an alternative.

## Reuse and uncertainty

Supported mappings can be reused to predict other candidate effects in the same
actor/action scope. Predictions require agreement among best models, at least
two observations, and no contradictions. Otherwise the outcome is null with
explicit alternatives. Confidence is a smoothed fit frequency, not a calibrated
truth probability. The module always declares the objective unknown; visually
identical worlds with different objectives cannot be resolved by this interface.
The caller may use that uncertainty in a broader reasoning/planning system.


---

# ARC 004 Experimental Interface Contract

The pure Rust entry point is agentkit_local::arc_explorer::execute. The example
adapter is built explicitly with cargo build --locked --offline --example
arc004_explorer. It reads one JSON request per line and returns {ok,data} or a
bounded {ok:false,error,executes_actions:false} envelope. It is not an installed
CLI command, an MCP registry entry or an index registration. No environment
handle, action callback, file path, model weight or network client is accepted.

Inputs use arc004-input-v1; outputs use arc004-output-v1 and
arc004-explorer-v1. The step view returns the ledger, predictions and proposed
probe. hypothesize_rules, evaluate_hypothesis, predict_action_effects and
design_probe return compact views over the same computation. evaluate_hypothesis
evaluates the automatically generated rule family; arbitrary external candidate
claims remain the responsibility of the existing ARC-SENSE learn interface.
Observation/change extraction remains ARC-SENSE observe/compare. No duplicate
registered operation is proposed for those existing functions.

The current observation and past history contain native object shape hashes,
areas, inclusive bounding boxes, states, available actions, context hashes and
clipping fields. Historical possibleMovements come directly from native
comparison. A stationary correspondence requires one unique shape in both
summaries and an unchanged bounding box. Ambiguous correspondences remain absent.
The caller supplies a provisional actor candidate from the reused ARC-003
estimator; it does not become a verified actor merely by being supplied.

The ledger exposes candidate action-motion mappings, zero/one-step lag, contact
models and optional visible-marker conditions. Entries retain support,
contradictions, observed contexts, recent attempts, confidence interpretation,
validity conditions, equivalent candidates and a next distinguishing experiment.
Only supported motion parameters appear in reusable_discoveries. They are valid
within the caller's evidence window and same actor/action semantics; there is
no silent cross-game transfer. Presence toggles and disappearance events remain
observed associations, not proven prerequisites or objectives.

Recommendations distinguish learning probes, validation/relocation and tentative
progress toward an unverified visual target. Every response explicitly says the
objective is unknown. Entropy over candidate effect groups is a practical
discrimination heuristic, not calibrated causal information gain. Equal observable
predictions remain indistinguishable. Confidence is a smoothed fit frequency;
unmodelled mechanics, occlusion, actor aliasing and hidden goals can invalidate it.

The controller must execute at most one selected legal action, then return actual
feedback. A proposed delayed follow-up also needs controller execution and
remaining budget. No recommendation executes an action or resets a game.
Unsupported click-only observations, terminal states and exhausted budgets stop.
Known fatal action/context combinations are avoided. Unknown risks have interval
[0,1]; the heuristic score 0.2 is not a safety guarantee. A caller may set max_risk
below that value to abstain. The default is 0.5. The controller retains final
authority over uncertain or irreversible actions.

Requests are bounded at 512 KiB, grids at 64 by 64, objects at 32, history at 48
transitions, native movement lists at 64 and legal IDs at eight. Only IDs 1–5
receive new recommendations. At most 17 observed/mirrored displacement parameters,
two lag values, four contact responses and five marker guards are tested per
action. Responses retain at most four equivalent models and six attempts per
motion entry, twelve associated events and twelve rejected examples. String and
numeric bounds are checked before fitting. The Python composition additionally
enforces a 20-second response timeout and 128 KiB response cap; oversized outputs
are rejected. The NDJSON adapter accepts at most 100,000 requests per process.

The history is caller-owned. Dropping old evidence may revoke earlier discoveries;
history limits are explicit. Input validation rejects oracle/future fields but
cannot prove a caller supplied honest chronological data. Determinism holds for
identical inputs, including action order; measurements are external to the pure
result. Missing binaries cause actionable local errors, with no installation,
download, cloud fallback or training. Formal schemas are under schemas/.

Integration is pending owner review. The capability descriptor is metadata only.
Use AgentKit's existing registry and index mechanisms after coordinated review;
do not silently register this experimental module or overwrite an active agent's
implementation. No cloud runtime or accelerator is required.


---

# ARC 004 Independent Evaluation

The frozen scientific explorer recovered 189 correct
non-idle action mappings on 48 new generated environments, with
0 falsely supported mappings under the defined criterion.
Its selected action predictions were 99.53% correct
at 71.60% coverage. It completed 5/48
tasks, compared with 2/48 for the causal policy and
4/48 for the visual policy. The zero-information-weight
ablation completed 3/48 and recovered 192
mappings. These measurements must be read together; they do not establish a
general ARC-AGI performance gain or that active information scoring is superior.

## Fixed comparison and leakage boundary

Sixteen development worlds covered cardinal movement, obstacles, one-step delay
and toggles separately. The final holdout used 48 different seeds, eight per
previously unseen combination: stride with obstacles, diagonal boundaries,
delayed obstacle interaction, toggle with collection, key with delay, and hidden
objectives. Palettes, layouts and unknown action mappings vary by seed. The
learner sees only public-style grids through native ARC-SENSE summaries, past
executed actions and movement comparisons. It never receives generator fields,
command mapping, delay, item identities, objective potential or oracle actor
coordinates. Observationally identical objective twins are tested independently.

All policies have a 64-action cap, identical seeds and terminal stopping, one
common initialization excluded from the decision budget, and no policy resets.
All calibration actions are counted. A and B are the unchanged original visual
and causal implementations. C uses scientific probe selection; D has the same
model and score with the information term zeroed. The same passive rule observer
shadows every policy for measurement. Consequently baseline mapping counts mean
rules recovered from baseline trajectories by the common observer, not new rule
capabilities secretly added to baseline decisions.

The original ARC-SENSE/ARC-003 frozen evaluation records and public holdout games
were not used. There was no training. After development and focused tests,
freeze.json pinned the Rust source, adapter, history estimator, protocol, tests
and compiled worker before any holdout episode ran. The evaluator refuses changed
pins or holdout conditions. The freeze's holdout_opened=false records its creation
state, not the later final run state. No implementation, policy or threshold was
changed after the holdout opened. Post-hoc analysis only replayed recorded actions
and checked their outcomes; it matched every stored displacement/completion and
left the original result file unchanged.

## Held-out outcomes

| Policy | Correct mappings | False mappings | Completed of 48 | Prediction accuracy | Coverage | Actions |
| --- | --- | --- | --- | --- | --- | --- |
| A | 186 | 0 | 4 | 98.34% | 69.74% | 2935 |
| B | 190 | 0 | 2 | 98.60% | 73.24% | 3023 |
| C | 189 | 0 | 5 | 99.53% | 71.60% | 2947 |
| D | 192 | 0 | 3 | 99.15% | 73.89% | 3037 |

There are at most 192 non-idle motion mappings, four per world. A mapping counts
as correct only when the inferred actor is the true actor, its command velocity
matches the hidden generator mapping and lag matches the generator delay. The
model must also have marked its parameters supported using prior evidence.
Idle-only fits remain unresolved. This criterion verifies motion parameters;
it does not certify a universal collision law, a prerequisite or an objective.
Task completion can truncate evidence early, so fewer recovered mappings in a
winning trajectory need not mean weaker inference.

| Mechanic | A mappings | B mappings | C mappings | D mappings | A wins | B wins | C wins | D wins |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| stride_obstacles | 32 | 32 | 32 | 32 | 1 | 1 | 1 | 2 |
| diagonal_boundary | 32 | 32 | 29 | 32 | 1 | 1 | 4 | 1 |
| delay_obstacles | 32 | 32 | 32 | 32 | 0 | 0 | 0 | 0 |
| toggle_collect | 30 | 30 | 32 | 32 | 0 | 0 | 0 | 0 |
| key_delay | 31 | 32 | 32 | 32 | 0 | 0 | 0 | 0 |
| hidden_objective | 29 | 32 | 32 | 32 | 2 | 0 | 0 | 0 |

No official public games were played for this mission. There is no official
offline score or leaderboard result. The task-completion measurements are for
these independent generated worlds only. They cannot be compared numerically
with ARC-003's different generators and action budgets.

## Discovery cost and candidate rejection

| Policy | Median actions to first rule | Median actions per first mapping discovery | Verified discovery events | Lag recovered of 48 | Sampled wrong-parameter rejections |
| --- | --- | --- | --- | --- | --- |
| A | 10.0 | 16 | 189 | 48 | 534 |
| B | 11.0 | 15.5 | 192 | 48 | 533 |
| C | 11.0 | 13 | 189 | 48 | 541 |
| D | 11.0 | 14.0 | 192 | 48 | 542 |

Discovery times count actions executed before the first correct supported
mapping claim. Medians condition on a discovery; missing discoveries are censored,
not counted as zero cost. A discovery may later disappear from the bounded ledger.
Rejected candidates are sampled examples with at least two native counterexamples.
The table counts examples whose true-actor velocity or lag parameters are wrong.
Other rejected examples can have correct velocity/lag but an incorrect contact or
gate model, so they are not automatically false rejections. Finite-grammar
candidate counts are not independent discovered scientific laws.

Predictions are evaluated before executing each selected action, against its
actual generated actor displacement. Null/ambiguous predictions reduce coverage.
This is stricter than generic scene-change prediction and different from the
earlier ARC-SENSE coarse-effect metric. High selective accuracy alone does not
establish task solving or correct actor identification on every step.

C proposed 606 actions for which immediate
predicted actor effects had alternatives; 482
had a correctly associated actor and an actual effect selecting one of those
alternatives. This is a conservative observed discrimination count, not proof
that every probe bought useful causal information. Some actions validate,
relocate, pursue tentative contact, test future delayed effects or remain
confounded. The system explicitly reports that information gain is not guaranteed.

## Interaction hypotheses and multistage tasks

| Policy | Actual collection events | Actual key events | Actual switches | Contact candidates matching collectible shape | Other contact candidates | Correct switch candidates | Correct gate candidates |
| --- | --- | --- | --- | --- | --- | --- | --- |
| A | 0 | 1 | 107 | 8 | 280 | 8 | 30 |
| B | 1 | 1 | 82 | 8 | 280 | 7 | 31 |
| C | 1 | 4 | 53 | 8 | 280 | 7 | 24 |
| D | 3 | 5 | 54 | 8 | 280 | 6 | 24 |

These are candidate-type counts in the final bounded ledger. A contact candidate
is matched against the initial native shape of a true collectible/key for
evaluation only. Switch candidates require the correct generated toggle action
and observed marker; gate candidates match the generated motor-enabling marker
condition. These checks do not prove causal removal or infer the full multistage
objective. Shape aliasing, occlusion and evidence eviction can cause misses or
additional candidates. All remain explicitly provisional in the engine output.
Some shape-matching candidates have zero supporting contact events; shape matches
must not be read as verified interaction discoveries. In C, 288 contact candidate
types across trajectories included only eight initial collectible/key shape
matches and 280 other candidates. There was only one actual collection event
and four key acquisitions. Motion and lag recovery therefore do not imply
reliable causal interaction discovery or multistage task solving.

## Failure analysis

The finite grammar was sufficient to recover many action mappings, but evidence
collection was also effective under existing exploration. Information gain is
entropy over candidate effect groups, not expected goal value or a calibrated
causal posterior. Disagreement can come from a wrong actor or a collision
approximation. Pursuing it can waste a limited action budget. The D ablation is
therefore essential; a C versus B difference cannot all be attributed to the
information term because C also changes actor attribution, contextual modelling
and its tentative contact heuristic.

The underlying actor estimator is observational. Periodic distractors and changing
object shapes can still be associated with actions. Predictions abstain when
best models disagree or contradict evidence, but a wrong actor can still fit a
model. Current/previous-action attribution helps one-step delays; two-step delays
remain outside the grammar and the unit test requires unresolved outputs.
Solid bounding boxes approximate collision geometry and can misclassify hollow
or moving objects. The generated worlds deliberately use simple collision laws,
so this test is more favorable than arbitrary ARC mechanics.

The controller replans after each action and does not commit to the optional
delayed follow-up plan. Gate conditions and disappearance events do not supply
long-horizon sequencing or identify what wins. Positive potential events are
oracle evaluation labels only and can be repeated while oscillating. The engine
always declares objective uncertainty, including on hidden-objective worlds;
an accidental completion does not establish that it discovered the hidden goal.
Eight tasks per family and one fixed generator suite limit generalization claims.

## Development record

| Policy | Correct mappings of 64 | False mappings | Prediction accuracy | Coverage | Completed of 16 |
| --- | --- | --- | --- | --- | --- |
| A | 58 | 0 | 88.27% | 59.96% | 0 |
| B | 60 | 0 | 93.37% | 69.24% | 0 |
| C | 60 | 0 | 90.69% | 66.11% | 0 |
| D | 60 | 0 | 95.13% | 70.21% | 0 |

Development prototypes are preserved separately. The generator marker-overwrite
defect and action/animation phase confound were corrected before freezing.
Previous-action association reused the existing estimator rather than copying
its implementation. The final development results already offered no consistent
advantage for the information term; settings were retained rather than tuned
against the final holdout for a more attractive result.

## Runtime and resources

| Policy | Wall seconds | Engine calls seconds | Mean engine ms | Python traced peak bytes | Engine peak RSS bytes | Native peak RSS bytes |
| --- | --- | --- | --- | --- | --- | --- |
| A | 99.87 | 46.20 | 15.49 | 1091775 | 8830976 | 26947584 |
| B | 121.06 | 48.59 | 15.82 | 1200438 | 9101312 | 30449664 |
| C | 151.49 | 59.39 | 19.83 | 1176357 | 9154560 | 30588928 |
| D | 143.54 | 57.24 | 18.55 | 1188393 | 9154560 | 30588928 |

Engine time includes JSON transport and the Rust model call. The mean divides
by decision calls plus one final feedback measurement per episode. The same
observer ran for A/B as a shadow measurement and did not affect their action
choices. Python tracemalloc is a per-episode peak. Rust worker/native perception
RSS are cumulative session high-water marks. These are not isolated per-policy
allocations. Other local work and instrumentation can affect wall times.
No billing, token savings or general execution speedup is claimed.

All execution was local CPU. No GPU/TPU work, cloud training, model downloads,
registry promotion or index initialization occurred. The reusable result is a
bounded, inspectable rule-discovery prototype with an explicit uncertainty
boundary. The data does not justify claiming a generally superior ARC explorer.


---

# ARC 004 Verification and Limits

Ten focused Rust tests passed. They cover learned action mappings and temporal
lag, unresolved stationary actions, retained terminal evidence without an action,
determinism, input bounds, prohibited oracle fields, fatal-action avoidance,
stride clipping counterexamples, unsupported two-step delay and compact views.
Six local integration tests passed using the actual existing native ARC-SENSE
binary and compiled Rust worker. They cover native summaries, previous-action
association, unchanged inputs, bounded history, objective twins, generator
reachability on development fixtures and oversized-worker input rejection.

The exported actual request and response additionally validate against the formal
schemas. Their binary/source identities and local Python, NumPy, Rust and machine
versions are recorded in evidence/interface-verification.json. No new dependency
installation was performed. Exact test receipts remain under evidence/.

cargo fmt --check and cargo clippy --locked --offline --all-targets -- -D warnings
passed. The required full cargo test --locked --offline --all-targets exited 101:
37 tests passed (10 library, 10 ARC-004 and 17 archive), then all 17 audio tests
failed on the absent isolated-checkout fixture target/provision/audio/speech.wav
at tests/support/audio.rs:49. Cargo stopped before later integration binaries.
This is not a passing full-suite claim. No unrelated audio fixture, engine
registration or production database was modified to conceal it.

The existing compile-time matting asset was explicitly copied from the owner's
provisioned checkout into the isolated worktree for compilation. It was not
downloaded, trained or invoked by ARC-004. Rust source stays inside the existing
package. The scoped line-ending attribute preserves src/lib.rs's original CRLF
bytes rather than introducing an unrelated whole-file rewrite.

The final freeze was created only after the full development comparison and
focused tests. Source, binary and protocol pins were checked before holdout
execution and again at finalization. The post-hoc evaluator replayed all recorded
actions, matched every displacement and completion, and verified that the original
results file hash remained unchanged. Every ground-truth check occurs outside
the inference component, after action selection.

Important evidence, source and report hashes are recorded in evidence/hashes.json.
The separate feature worktree and branch preserve the shared main checkout,
ARC-SENSE source, registry and installed skills. No model was trained, no cloud
resource was used for this mission, and no experimental capability was installed
or registered. A descriptor is provided for later owner review.

The consolidated report is one UTF-8 Markdown document in the ARC-004 Evidence
folder. It contains all report bodies, the tested settings and important hashes.
There is no ZIP. Source code, raw results and machine-readable evidence remain in
the experimental package; the previous mission's evidence is preserved.


---

# ARC 004 Recommended Next Experiment

Do not promote this policy as a superior general explorer. The current holdout
recovered 189 mappings for C versus 190
from B trajectories and 192 with the information weight
removed. Completion was 5, 2 and 3,
respectively, across 48 worlds per policy. Broader ARC claims remain unsupported.

The next local CPU experiment should isolate the value of a committed
distinguishing probe. Keep the same actor estimator, conditional learner,
tentative-contact score, task seeds and total action budget. Compare one-step
replanning with explicit two-action probes that alternate a command with a
neutral/contrasting action to test delay or retry after relocating from a boundary.
Charge every setup, probe and follow-up action. Measure posterior-equivalence
reduction and held-out actor-effect prediction after each complete experiment,
with selective coverage and false supported mappings reported together.

Use a new frozen suite. Include action/animation phase adversaries, matching
visible states with different gate prerequisites, two-step delays, hollow collision
objects and actor shape transformations. Preserve a separate negative-control
objective-twin set and do not optimize objective labels from it. Require more
verified rule information per total action, not merely more rejected patterns or
higher prediction accuracy on a shrinking subset. Report task completion as a
separate secondary outcome.

If targeted probes still offer no benefit, retain the conditional learner as an
explicit uncertainty/diagnostic component and simplify the controller. There is
no evidence here justifying GPU/TPU training, larger models or cloud spending.
Any future resource expansion or shared registry/index promotion requires the
owner's separate authorization and coordination.


---

# Tested experiment settings

```json
{
  "format": "arc004-protocol-v1",
  "development": {
    "seed_start": 41000,
    "count": 16,
    "families": [
      "cardinal",
      "obstacle",
      "delay",
      "toggle"
    ]
  },
  "holdout": {
    "seed_start": 840000,
    "count": 48,
    "families": [
      "stride_obstacles",
      "diagonal_boundary",
      "delay_obstacles",
      "toggle_collect",
      "key_delay",
      "hidden_objective"
    ]
  },
  "budget": 64,
  "configurations": {
    "A": "unchanged visual",
    "B": "unchanged causal",
    "C": "scientific explorer",
    "D": "same ledger and score with information gain weight zero"
  },
  "history_limit": 48,
  "native_objects": 24,
  "claim": "conditional mechanics discovery from observations; no hidden objective identification guarantee",
  "freeze": "No final holdout execution before development and unit tests pass and implementation/configuration hashes are recorded. No tuning after opening holdout.",
  "resources": "CPU local only; no model training, accelerators, cloud requests or downloads"
}
```


# Important evidence hashes

| Artifact | SHA 256 |
| --- | --- |
| evidence/freeze.json | 17f8f987bd1e165df0239fbaf8fdeaf3e8205471944f11491e1b10523a5c1aa4 |
| results/holdout/results.json | 9b165c99c001107969cc46ec825f96fe0b065e91219f2b64976ae5d09fea3caf |
| results/analysis.json | 8f49efd8759583c0a39d500f32d6952d583095c2171aa7669f4a558c2d738a68 |
| results/development-final/results.json | 2d15debe63dfa9b4a36ff970251ca68e6ded64eee512fd84331299c6f1d53eac |
| evidence/rust-focused-tests.txt | d419a37921afbeb5d5d7cfff5100addb7b3ea945c739e57ae56d39d8610b390e |
| evidence/python-integration-tests.txt | a1d62c5df669a7c9411b9bc5f5a10b0a7bcecbd9fee028cd1378d412bdd2e383 |
| evidence/interface-verification.json | 5827b2779b8a154d78ebbf1acbc29b7e5ece845da7a2860f90ffba64e31b57a6 |
