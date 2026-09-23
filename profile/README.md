# VALENCE

An experimental, hardware-agnostic, data-driven UI framework.

VALENCE separates *what the UI means* and *what the app does* from *where and how it renders*. The same application logic can drive different renderers on different hardware targets, and AI-generated UI is parsed, validated, and compiled before it ever executes.

> Define intent once. Resolve behavior and rendering independently.

```
                    VALENCE
                       │
          ┌────────────┼────────────┐
          │            │            │
        NEXUS         MESH         PORT
          │            │            │
       Behavior       UI         Rendering
       / Logic      Language      / Sink
```

## Repositories

| Repo | Answers | Role | Tech |
|---|---|---|---|
| [**Mesh**](https://github.com/ValanceX/Mesh) | *What should the UI mean?* | The UI language ecosystem: **MPRX** plus its Tree-sitter grammar, parser, semantic model, compiler, and language server. Renderer-independent. | Rust, Tree-sitter, WASM |
| [**Nexus**](https://github.com/ValanceX/Nexus) | *What should the app do?* | Application/runtime logic: commands, domain services, state, selectors, and capability resolution. Never renders UI. | TypeScript, Effect |
| [**Port**](https://github.com/ValanceX/Port) | *Where and how should it appear?* | The rendering boundary: translates MESH IR into a target (Web DOM, Canvas, embedded displays, …). Owns no business logic. | TypeScript |

Each repository is independent, with its own history, docs, and release cadence. They meet only at stable contracts (the MESH Semantic IR and runtime adapters), not by importing each other's internals.

## Mental model

Think of VALENCE like a language/runtime stack:

- **MPRX** is the source language
- **MESH** is the parser, compiler, semantic model, and tooling for that language
- **NEXUS** is the application/runtime behavior
- **PORT** is the target rendering boundary

```
MPRX describes intent  ──▶  NEXUS resolves behavior  ──▶  PORT renders it
```

MPRX looks familiar to anyone coming from HTML, JSX, Vue, or Svelte, but it is deliberately constrained: structure, bindings, side-effect-free expressions, and command/event intent. No arbitrary TypeScript and no business logic, which is what makes generated UI statically verifiable.

```html
<user-card
  user={user}
  compact={layout.compact}
  on.select={selectUser($event)} />
```

## Core invariants

- NEXUS never renders UI. MESH never performs hardware operations. PORT never owns business logic.
- A MESH tree is renderer-independent; a renderer can be replaced without touching application logic.
- Capabilities are resolved outside feature logic, and unsupported capabilities have an explicit resolution strategy.
- Generated UI must pass structural and semantic validation before it renders.
- The MESH compiler and tooling never depend on NEXUS, and the LSP shares the compiler's implementation.

## Status

Early and experimental. MESH's grammar, parser, and semantic validation are under active development; NEXUS primitives are taking shape; PORT is scaffolded. The near-term goal is one end-to-end vertical slice:

```
user-card.mprx → parser → semantic validation → type checking → MESH IR → runtime → Web PORT → browser
```

All repositories are MIT licensed.
