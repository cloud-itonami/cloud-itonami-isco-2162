# cloud-itonami-isco-2162

Open Occupation Blueprint for **ISCO-08 2162**: Landscape Architects.

This repository designs a forkable OSS platform for an independent landscape architect: a design-support robot prepares design concepts, material specifications, and environmental compliance checks under a governor-gated actor, so the practice keeps its own project records and maintains professional control over final design decisions (stamped/sealed designs remain the licensed landscape architect's exclusive responsibility).

## Robotics premise

All cloud-itonami verticals are designed on the premise that a **robot performs
the physical domain work**. Here a design-support robot prepares design concepts, specifications, environmental compliance assessments, and client engagement materials under an actor that proposes actions and an independent **Landscape Governor** that gates them. The governor never
dispatches the landscape architect's professional seal; `:high`/`:safety-critical` actions (such as
issuing stamped/sealed designs, or certifying environmental compliance) remain the licensed
landscape architect's exclusive responsibility and can only be proposed, never automated.

A live sample of the operator console (robotics safety console, shared template) is rendered in [docs/samples/operator-console.html](docs/samples/operator-console.html) — pure-data HTML output of `kotoba.robotics.ui`.

## Core Contract

```text
project brief + site survey + environmental code + client requirements
        |
        v
Landscape Advisor -> Landscape Governor -> draft design / prepare spec, or human sign-off
        |
        v
robot actions (gated) + design records + audit ledger
```

No automated advice can dispatch a robot action the governor refuses, suppress
an operating record, or disclose sensitive design data without governor approval and
audit evidence. No proposal can claim to issue a final stamped/sealed design or
certify environmental compliance — those remain the licensed human landscape architect's exclusive
professional and legal responsibility.

## Capability layer

Resolves via [`kotoba-lang/occupation`](https://github.com/kotoba-lang/occupation)
(ISCO-08 `2162`). Required capabilities:

- :robotics
- :identity
- :forms
- :dmn
- :bpmn
- :audit-ledger

See [`docs/business-model.md`](docs/business-model.md) and
[`docs/operator-guide.md`](docs/operator-guide.md).

## Reference implementation (`:maturity :implemented`)

Full itonami Actor pattern (per ADR-2607011000 / CLAUDE.md's Actors
section, alongside `cloud-itonami-isco-2411`, `-8160`, `-2166`, `-2641`,
`-2651`, `-2652`, `-2654`, `-1219`, `-1223`, `-1330`, `-1341`, `-1349`,
`-1412`, `-1439`, `-2144`, `-2161` and `-2320`): a real
[`kotoba-lang/langgraph`](https://github.com/kotoba-lang/langgraph)
`StateGraph`, with the Advisor and Governor as distinct graph nodes and
human-in-the-loop interrupt/resume via checkpointing.

```text
:intake -> :advise -> :govern -> :decide -+-> :commit            (:ok? true)
                                           +-> :request-approval   (:escalate? true, interrupt-before)
                                           +-> :hold               (:hard? true)
```

- `src/architecture/store.cljc` — `Store` protocol + `MemStore`:
  registered projects/clients, committed design records, an append-only audit ledger.
- `src/architecture/advisor.cljc` — `Advisor` protocol; `mock-advisor`
  (deterministic, default) proposes a design operation from a
  request; `llm-advisor` wraps a `langchain.model/ChatModel` — either
  way the advisor only ever produces a `:propose`-effect proposal,
  never a final stamp, and LLM parse failures always yield
  `confidence 0.0` (forces escalation, never fabricated confidence).
- `src/architecture/governor.cljc` — `LandscapeGovernor/check`: a pure
  function, wired as its own `:govern` node. Hard invariants
  (unregistered project, a proposal whose `:effect` isn't `:propose`,
  any attempt to issue a stamped design or certify environmental compliance)
  always route to `:hold`. Escalation invariants (environmental compliance
  flags, drainage/grading/environmental-sensitive systems, or low advisor confidence) always route to
  `:request-approval` — an `interrupt-before` node that the graph
  checkpoints and only resumes on explicit human approval
  (`actor/approve!`), matching the README's robotics-premise statement
  that stamped designs and environmental compliance certification always remain the
  licensed landscape architect's sole responsibility.
- `src/architecture/actor.cljc` — `build-graph`, `run-request!`,
  `approve!`: the `langgraph.graph/state-graph` wiring itself.

```bash
clojure -M:test
```

This is what backs this repo's `:maturity :implemented` entry in
[`kotoba-lang/occupation`](https://github.com/kotoba-lang/occupation).

## License

AGPL-3.0-or-later.
