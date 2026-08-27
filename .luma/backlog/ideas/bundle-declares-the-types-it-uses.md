---
type: luma/idea
title: Should a bundle declare the types it uses, and where they can be found
created: { by: human:benlinton, at: 2026-08-24T00:00:00Z }
contributors: [human:benlinton, agent:claude-opus-5]
horizon: later
scope: organization
lifecycle_status: draft
---

# Should a bundle declare the types it uses

## The idea, as raised

**I wonder if `bundle.md` should list what types this bundle expects to use and
where they can be found.**

```
types: [{ name: "idea", source: "github.com/.../luma-types/.../_types/idea.md" }]
```

**And this would not load the types up front.** It would just **make them easy to find
when the bundle needs them.**

**But we may also need to solve for making it repeatable and predictable**, so
you do not pick up a new version of a type unexpectedly — *we can cross that
bridge later.*

---

## Commentary — `agent:claude-opus-5`, not part of the idea

*Below the idea and separate from it, so the raw version survives a reading that
may turn out to be wrong. Evaluation is welcome here — it just does not get to
edit what was raised.*

### It arrives with a live instance, in this repository

**Twenty documents here declare `type: luma/idea`. There is no `_types/`
directory anywhere in this repository.** The contract they are written against
is not present, not pointed at, and not named — and every one of them parses
fine.

**Worse, the mechanism that keeps them consistent is imitation.** The two ideas
written today were given their frontmatter by copying a sibling file, because
that is the only way to learn the shape. **That works until a field is wrong,
and then nothing objects** — which is the estate's own recurring failure, and
this one is sitting in the directory where the idea was raised.

**So the question is not hypothetical and the answer is not obviously the
proposed field.** Read on: the field as drawn would not have helped these files.

### Two fields are wearing one name

**`name` and `source` answer different questions and have different costs.**
Keeping them together is what makes the idea look bigger than it is.

| | claims | cost |
| --- | --- | --- |
| **the inventory** (`name`) | *my documents use these types* | **near zero.** Mostly derivable from the documents themselves |
| **the pointer** (`source`) | *and this is where the contract came from* | **the contested half.** It is the first thing in the estate to write a remote address into a manifest |

### The pointer is provenance, and the estate already has its shape

**Vendoring is settled and this must not disturb it.** *A Type Definition is
vendored — copied into a bundle, never resolved remotely* (2026-08-17), and the
naming decision keeps a re-open trigger pointed at exactly this: *if the catalog
becomes an index tools query at apply time rather than a place they copy from,
it has become a registry.*

**A citation is not resolution, and the difference is whether anything ever
dereferences it.** Written as a record of where a copy came from, this stays
well inside the line. Written as somewhere to look when the copy is missing, it
crosses it — and it crosses it **silently**, because the failure only appears
offline.

**So the rule has to be stated with the field, not left implied: the vendored
copy always wins, and `source` is never a fallback.** A pointer that can be
followed when the local copy is absent produces different behaviour on a plane
than at a desk, which is the worst shape a rule can have.

**And the honest description of the field is then: `adopted.toml`, for types.**
That is not a criticism — it is what makes it worth building. Today a vendored
type copy carries **no record of its origin at all**, so nothing can tell whether
this bundle's `luma/project` still matches the canonical one. Bundles have drift
detection; the types inside them do not.

### What the inventory buys that derivation cannot

**Most of it can be computed.** Walk the bundle's documents, collect every
`type:`, intersect with `_types/`. A tool can produce that list today with no
format change and no author writing anything.

**What derivation cannot produce is intent.** *I expect `luma/idea` and I
deliberately do not carry it* and *somebody forgot to vendor `luma/idea`* are
identical from outside. A declared list is the only version of this that **can
disagree with reality** — with the documents, or with `_types/` — and a
disagreement is a finding.

**That is the whole argument for the field.** An undeclared inventory is always
right and therefore says nothing.

### Where it does not help is the case that needed help most

**`shared-types.md` already names the hard case, and it is not a bundle
problem.** A document living outside every bundle — `.luma/project.md` is the
example — has **no resolution scope at all**, so nothing decides between two
contracts claiming it.

**A field on `bundle.md` cannot reach those documents**, because the whole
difficulty is that they are in no bundle.

**Which is exactly what this repository's twenty ideas are.** They sit in
`.luma/backlog/`, not in `.luma/bundles/`. The proposed field, adopted in full,
would have changed nothing about the instance that motivates it.

**That is not a refutation — it is a relocation.** The thing that needs to
declare which types govern it, and where they came from, is **`.luma/` itself**,
which is the `.luma/_types/` open item already recorded in `shared-types.md`.
The bundle-manifest version is the easy half of a question whose hard half is
already on the books. **Answer them together or the answer will be two
mechanisms.**

### The deferred bridge is shorter than it looks, with one real obstacle

**Repeatability was deferred and mostly does not need to be.** `adopted.toml`
already records `version`, `source`, `commit` and `checksum` per bundle, and the
same four applied per type make *do not pick up a new version unexpectedly* true
by construction — the copy is committed, and the record says what it is.

**The obstacle: a Type Definition has no version of its own.** That is an open
item in the format, and it means a type can only be pinned by way of the bundle
that carries it.

**Which makes the example's shape the thing to correct.** A path to a file in a
repository —`.../luma-types/.../_types/idea.md` — **pins nothing and names no
publisher.** A pointer that identifies the *bundle and version* the copy came
from can be checked; a raw file URL can only be visited.

### The word, and a collision worth avoiding

**`requires` is taken** — on a catalog it means obligation. **`depends` is
reserved** by the bundle-dependencies draft. `types` is free and reads correctly.

**The deeper collision is conceptual.** If bundles gain dependencies, *a
contract my documents need, defined over there* is a dependency at finer grain,
and the estate would have **two mechanisms for reaching into another bundle**.
The defence is that a type pointer deliberately does *not* pull the other bundle
in — you want the contract, not its prose — and `shared-types.md` says the
opposite pressure exists too: *a type is inert alone; it ships inside the bundle
that gives it meaning.*

**Both cannot be fully right, and whichever is chosen should be chosen once.**

### The cheapest useful thing needs no format change

**Report the gap before adding a field to close it.** *These documents declare
types with no definition in scope* is computable today, by `inspect`, against
any repository — and it would have caught this one on the first run.

**That check is most of the value and none of the cost.** It turns silence into
a finding. The field then earns its place by answering the question the check
raises — *where do I get it* — which the check genuinely cannot answer.

**Order it that way.** The check is buildable now and useful alone; the field is
a format change that is worth more once something is already reporting the gap.

### What evaluating this would have to settle

Whether `source` is a citation or a lookup, and how the format forbids the
second; whether the declaration lives only on `bundle.md` or also on `.luma/`,
where the harder version of the problem is; whether a pointer names a bundle and
version rather than a file; whether this and bundle dependencies are one
mechanism or two; and whether the inventory is authored or derived, given that a
derived one can never be wrong and can therefore never warn.

## Related

[shared-types.md](../../../docs/shared-types.md) — the bundle as resolution
scope, the out-of-bundle case this does not reach, and the `.luma/_types/` open
item it should be answered with.
[bundle-dependencies.md](../../../docs/bundle-dependencies.md) — the other
mechanism for reaching into another bundle, and the vocabulary already spoken
for.
[[where-backlog-types-live]] — the same question one level down: which bundle a
type belongs to at all.
`DECISIONS.md`, *shared types travel inside bundles* — the vendoring rule this
must not disturb, and the registry re-open trigger it runs closest to.
