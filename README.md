# catept-plugin-afp-framework

Sibling repo of [`jagg-ix/catept-main`](https://github.com/jagg-ix/catept-main).
18th plugin extracted under [Target 5+](https://github.com/jagg-ix/catept-main/blob/main/docs/architecture/targets/target-4-plan.md).
**Foundational scaffolding extraction** — unlocks the catept-domain-quantum
bundle pilot (T61) and the 9+ CATEPTMain Prelude files that share this
infrastructure.

## What this provides

The generic AFP-translation phase-1 scaffold that ALL AFP-derived module
preludes under `CATEPTMain.*` share. Specifically:

* `AFPTranslationPhase` enum (Phase1 / Phase2)
* `TacticStubs` namespace — 15 scoped macros that replace Mathlib tactics
  with `sorry` fallbacks, so phase-1 preludes compile without Mathlib's
  full import graph.
* Generic opaque carrier types: `AFPObj`, `AFPSet`, `AFPMat`, `AFPVec`.
* ~25 axioms for matrix/vector ops, dimension accessors, predicates
  (`afpDimRow`, `afpIndexMat`, `afpMatMul`, `afpUnitary`, `afpHermitian`, …).
* Documentation conventions: typeclass-instance axiom template,
  notation-conflict guard list, locale-injection rule, phase-1 sorry
  annotation conventions.

The whole module is interface-level scaffolding (lots of `axiom`
declarations as opaque types). Phase-2 work item (separately tracked):
upgrade opaque types to concrete `Matrix (Fin n) (Fin m) ℂ` and prove
the axioms as theorems.

## Consumers in `catept-main`

The original file at `CATEPTMain/Core/Framework/AFPBridgeFramework.lean`
is now a re-export shim that points here. Existing consumers retain
their `import CATEPTMain.Core.Framework.AFPBridgeFramework` lines:

* `CATEPTMain/Bridges.lean`
* `CATEPTMain/Core/PDC/PDCPrelude.lean`
* `CATEPTMain/Analysis/MODE/MODEPrelude.lean`
* `CATEPTMain/Analysis/LAPL/LAPLPrelude.lean`
* `CATEPTMain/Core/MTN/MTNPrelude.lean`
* `CATEPTMain/Core/PHQ/PHQPrelude.lean`
* `CATEPTMain/Analysis/CPM/CPMPrelude.lean`
* `CATEPTMain/Analysis/FOU/FOUPrelude.lean`
* `CATEPTMain/Analysis/ODE/ODEPrelude.lean`
* `CATEPTMain/Quantum/QUANTUM/QuantumPrelude.lean` (consumer that motivated this extraction)

The `CATEPTPluginAFPFramework` namespace is the only public surface;
the catept-main shim re-exports under the original
`CATEPTMain.Core.Framework` namespace so source-level imports do not
need to change.

## Dependencies

| Pin | Version |
|---|---|
| Lean toolchain | `leanprover/lean4:v4.29.0` |
| Mathlib | `8a178386ffc0f5fef0b77738bb5449d50efeea95` |

No internal CATEPTMain.* deps. `Mathlib.Data.Complex.Basic` only.

## Re-import contract

```lean
require «catept-plugin-afp-framework» from git
  "https://github.com/jagg-ix/catept-plugin-afp-framework.git" @ "<sha>"
```

```lean
import CATEPTPluginAFPFramework.IntegrationBridge

-- Then either:
open CATEPTPluginAFPFramework  -- direct
-- or:
open CATEPTPluginAFPFramework.TacticStubs  -- phase-1 sorry fallbacks
```

## Build locally

```bash
lake exe cache get
lake build
```

## License

MIT, matching `catept-main`.
