---
name: uncomplect
description: Pressure-test a stateful system or replacement design through Rich Hickey's simplicity and complection, Ousterhout's complexity and deep modules, DDD boundaries, Effect typed effects, and XState lifecycle modeling. Use for "what would Rich Hickey do", "make this simpler", "define this error out of existence", state-machine reviews, or replacing a working prototype without preserving its accidental architecture.
basis:
  - https://github.com/matthiasn/talk-transcripts/blob/master/Hickey_Rich/SimpleMadeEasy-mostly-text.md
  - https://web.stanford.edu/~ouster/cgi-bin/book.php
  - https://pragprog.com/titles/swdddf/domain-modeling-made-functional/
  - https://effect.website/docs/
  - https://stately.ai/docs/
---

# Uncomplect

Treat the current system as a functional prototype. Preserve essential behavior and evidence. Do not preserve accidental structure.

## Read first

Read the nearest project decisions, source, tests, runtime receipts, and current incidents. Use source text for Hickey, Ousterhout, and DDD. Inspect the project's pinned Effect and XState versions before recommending APIs.

Completion: every recommendation names its source fact, preserved behavior, and deleted complexity.

## Hickey

Build a complection table.

| Concern | What is braided together? | Independent values | Separation move |
| --- | --- | --- | --- |

Look for value mixed with time, identity mixed with state, policy mixed with mechanism, current truth mixed with history, lifecycle mixed with domain data, and retry policy mixed with business failure.

Prefer immutable values and pure functions. Keep historical facts true without granting them permanent authority.

## Ousterhout

Judge complexity by change amplification, cognitive load, and unknown unknowns.

For every proposed type, state, service, interface, or option, ask:

1. What complexity does this remove?
2. Does it hide a hard problem behind a small interface?
3. Can the API define an accidental error out of existence?
4. Does it pull complexity into one deep module or spread policy across callers?
5. What can we delete after adding it?

For consequential replacements, design it twice. Compare two materially different interfaces by caller burden, hidden knowledge, dependencies, failure semantics, and deletion count.

## DDD

Name the bounded contexts and trust boundaries. Use domain language, not storage language.

Identify the aggregate that owns each invariant, immutable facts versus current decisions, commands, events, policies, projections, contract ownership, and translation at boundaries.

A projection may report domain truth. It must not become authority over the aggregate that produced it.

Model a workflow as a command plus current facts producing explicit events. The pure workflow returns events. Publishing and persistence are separate effects.

## Effect

Use Effect for effects, not for decorating pure rules.

- Keep domain decisions as pure functions and tagged values.
- Decode untrusted input at boundaries.
- Put database, network, filesystem, clock, config, and provider work behind named services.
- Use typed expected errors. Keep defects distinct.
- Retry only typed transient failures with proven idempotency.
- Keep provider calls outside authoritative database transactions.
- Do not introduce a second Effect major inside one workflow.

## XState

Use XState for essential time and lifecycle. Do not use it for pure migration, parsing, or validation.

- States represent modes with different allowed events.
- Context holds minimal actor data, not duplicate state labels.
- Guards are pure and synchronous.
- Effects live in invoked actors or boundary services.
- Persisted state is normalized before it receives current authority.
- A transient computation does not need a durable state.
- Restoration tests cross version and deployment boundaries.

## Replacement pass

Produce:

1. Essential behavior the prototype proves.
2. Complection map.
3. Domain map with contexts, aggregate, invariants, commands, events, projections.
4. One deep public seam.
5. Pure core, typed failures, Effect services, transaction edge, retry owner.
6. The smallest XState machine that makes essential time explicit.
7. Deletion list.
8. Shadow comparison, cutover, and rollback without dual authority.
9. Known facts, known unknowns, unknown knowns, suspected unknown unknowns.

Lead with one verdict:

```text
More complicated locally, simpler overall
```

or:

```text
More machinery, no net simplification
```

Show what becomes impossible, what becomes obvious, and what disappears.

## Compact prompt

```text
Treat the current system as a functional prototype, not an architecture to preserve. Preserve its proven behavior. Find what is complected. Separate values from time, policy from mechanism, and facts from projections. Define accidental errors out of existence. Draw the DDD boundaries and aggregate invariants. Keep Effect at effectful boundaries and XState on essential lifecycle. Propose the smallest deep replacement, show what it deletes, and name the remaining unknowns.
```
