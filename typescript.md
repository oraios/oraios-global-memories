## Software Design

### General Coding Principles

IMPORTANT: You use an idiomatic, object-oriented style (Java-esque principles).

* Keep each concern in exactly one home. A mechanism whose parts are only correct in combination is
  a single class with private parts and a minimal public surface; helpers and constants used by only
  one abstraction are internal to it.
* Enforce invariants through structure and visibility; interfaces must not permit forbidden states.

### Simplicity

IMPORTANT: Keep implementation complexity proportional to the feature's value.

* Do not add substantial custom code, state, metadata, persistence or abstractions for cosmetic or
  minor conveniences; if a minor feature would require this, omit it and briefly explain the tradeoff.
* Prefer standard framework behavior and reuse existing data and mechanisms.
* Do not invent requirements to justify complexity.
* Judge a refactor by its call sites as well as its declaration; it needs a concrete benefit.
  Redundant copy wrappers, unwieldy positional argument lists and duplicate interfaces for
  constructor parameters are not improvements.

### Data Modelling and Interfaces

* Derive types from runtime schemas via schema inference instead of repeating their fields;
  distinguish input and output types where defaults or transforms apply.
* For non-trivial interfaces, expect explicitly typed abstractions rather than bare functions
  (strategy pattern); prefer object-oriented abstractions over low-level data structures.
* Prefer named classes for internally constructed domain records where explicit construction aids
  readability or navigation; behavior is not required. Do not convert shapes mechanically: keep
  schema-derived types, external contracts and framework configuration structural, and avoid classes
  that add only ceremony.
* Data-class constructors take their actual inputs directly (normally `public readonly` parameter
  properties), never a copy of the finished object.
* Use string enums for closed sets of identifiers (e.g. table or column names); type parameters as
  the enum and convert external strings at validated boundaries.

## Syntax

* Use braced `if`/`else` for long or complex branches; reserve ternaries for short, simple value
  selections and never nest them.

## Identifier Naming

* Name types for their actual contents and responsibility.
* Method names identify the operation and the type they retrieve or construct.
* Variable names reflect the type for clarity (where appropriate).
* Qualify fields, parameters and local variables with the domain concept they represent
  (`evaluationCase`, not `item`; `runContext`, not `run`; `pricingCatalog`, not `catalog`).
* Name versions, hashes and revisions by their actual meaning; include units in monetary/rate fields
  (`estimatedCostUsd`, `ratesUsdPerMillionTokens`); use predicate names for booleans (`isComplete`).
* Explicitness takes precedence over brevity. Never shorten descriptive names for aesthetics or
  cleaner call sites; preserve meaningful conditions, qualifications and behavior in the name
  (`recordEntityAccessIfStoreAvailable`, not `recordEntityAccess`, since doing nothing outside a
  recording scope is part of the behavior). Documentation is no substitute for an explicit name.

## Testing

* Test externally observable behavior and guarantees, never implementation structure.
* Persist only minimal tests, each covering an important invariant; ad-hoc verification belongs in
  throwaway scripts.
* Add more extensive tests only when explicitly instructed (once the design has settled).

## Docstrings & Comments

* Class and interface docstrings explain the abstraction's purpose, the concern it owns, its role in
  the architecture and necessary lifecycle/scope guarantees, not a paraphrase of the name or a field
  list. Implementation rationale belongs in nearby comments.
* Use proper JSDoc/TSDoc: a summary; `@param` for every parameter (incl. parameter properties,
  defaults, special values, constraints); `@returns` for meaningful return values (incl. the meaning
  of booleans, empty results and `undefined`; none for void); `@throws` for relevant failures.
* Documentation must stand on its own: introduce project-specific concepts before referring to them,
  use introduced names consistently, and do not assume knowledge of the user conversation or of
  other methods.
* Begin each description with an elliptical phrase defining *what* the element is; details follow.
* Structure function implementations into functional blocks separated by blank lines, each headed
  by a concise, lower-case elliptical comment stating its purpose.
* State each piece of information once, at the element that owns it: callers do not explain callees'
  internals, and callees do not describe their callers.

## Commits

Commit messages:
* Explain the concrete motivation and resulting behavior; distinguish observed problems from
  hypothetical risks.
* The subject line covers the *entire* change; make it suitably abstract if necessary.
* Details are concise bullet items, one point each, no prose paragraphs; group them under short
  topic headings when the change spans several topics.
* Wrap all lines at ~100 characters.
* Write the message to a file and commit with `-F`; do not pass long text via `-m`.
