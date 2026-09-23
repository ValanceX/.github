# Valance

**Define your UI once. Run your app anywhere. Trust every screen, even the ones an AI wrote.**

Valance is an experimental, hardware-agnostic UI framework. It pulls apart three things most frameworks weld together: **what the UI means**, **what the app does**, and **where it's drawn**. The result is application logic that runs on any screen, renderers you can swap without a rewrite, and UI that's checked by a compiler *before* it ever runs.

> Define intent once. Resolve behavior and rendering independently.

## Why Valance

- **🧠 Logic that outlives its UI.** Your business logic runs, and is tested, with no renderer attached. Web today, a canvas or an embedded display tomorrow.
- **🛡️ Safe for generated UI.** Valance's UI language, MPRX, can't make network calls, run arbitrary code, or hide side effects. AI-generated screens are parsed, validated, and compiled before they're allowed to render.
- **🔌 No device checks in feature code.** Haptics, camera, storage, and AI are resolved once at startup into typed capabilities, with explicit fallbacks.
- **✍️ Familiar to write.** If you know HTML or JSX, you can already read MPRX:

```xml
<user-card
  user={user}
  compact={layout.compact}
  on.select={selectUser($event)} />
```

## How it fits together

```text
        MESH                     NEXUS                     PORT
  "What does the UI mean?"   "What does the app do?"   "Where does it appear?"
            │                         │                         │
   MPRX describes intent  ──▶  NEXUS resolves behavior  ──▶  PORT renders it
```

| Repo | What it is | Built with |
|---|---|---|
| [**Mesh**](https://github.com/ValanceX/Mesh) | The UI language toolchain: the **MPRX** grammar, parser, compiler, and language server | Rust · Tree-sitter · WASM |
| [**Nexus**](https://github.com/ValanceX/Nexus) | The application core: state, commands, services, events, and device capabilities, with no rendering | TypeScript · Effect |
| [**Port**](https://github.com/ValanceX/Port) | The rendering layer: turns compiled UI into DOM, Canvas, or device output | TypeScript |

Each repo stands on its own, with its own docs and history. They connect only through stable contracts (the MESH Semantic IR and runtime adapters), never through each other's internals.

## Our promises

- NEXUS never renders UI. MESH never touches hardware. PORT never owns business logic.
- A renderer can be replaced without changing a line of application logic.
- Missing device capabilities are handled explicitly, never silently ignored.
- Generated UI must pass structural and semantic validation before it renders.

## Where we are

Valance is **early and experimental**, and moving fast:

| | Status |
|---|---|
| **Mesh** | MPRX v0.1 grammar, parser, and structural validation shipped, with a working `mesh check` CLI |
| **Nexus** | All nine v0.1 primitives implemented, with a UI-free vertical slice running end to end |
| **Port** | Scaffolded. The Web DOM renderer is next |

The next milestone is the first full slice, from source file to browser:

```text
user-card.mprx → MESH compiler → MESH IR → NEXUS runtime → PORT (Web) → browser
```

All repositories are MIT licensed. Stars, issues, and ideas are welcome.
