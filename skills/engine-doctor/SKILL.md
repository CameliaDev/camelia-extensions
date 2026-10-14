---
name: engine-doctor
description: Diagnose Camelia engine start failures — better-sqlite3 ABI mismatch (ERR_DLOPEN / NODE_MODULE_VERSION), Node version vs native bindings, and safe recovery steps on Windows.
---

# Engine doctor

Use when the Camelia desktop app or a dev shell fails to start the engine with an `ERR_DLOPEN_FAILED` error mentioning `NODE_MODULE_VERSION` (better-sqlite3 native binding ABI mismatch), or when the engine crashes right after start with no domain events.

## The one rule

**The Node that loads the binding must be the Node major the binding was compiled for.** Two loaders share one binding file: the desktop sidecar and the dev/test shell. Both must run the same Node major (this project: Node 22, ABI = `process.versions.modules` 127).

## Checklist

1. Reproduce in the failing shell: `node -p "process.versions.modules"`. Write the number down.
   - 127 = Node 22 (expected). 137 = Node 24 — a version-manager PATH problem, not a broken binding.
2. Read the binding's compiled ABI. If start logs name it ("compiled against NODE_MODULE_VERSION 127"), compare with step 1. Mismatch = the loader is the wrong Node, or the binding was rebuilt under the wrong Node.
3. Windows version managers: the shell does **not** keep an exported PATH between commands. Prepend the Node 22 path on **every** command that runs the engine, tests, or pnpm scripts, e.g. with fnm:
   `$env:PATH = "C:\Users\<you>\AppData\Local\fnm\node-versions\22.x.y\node-v22.x.y-win-x64;" + $env:PATH`
   Verify with `node -p "process.versions.modules"` in the *same* command.
4. If the binding itself was rebuilt under the wrong Node (step 1 says 137 and the log says 127): **do not rebuild again under that Node** — it compiles a new wrong-ABI binding. Restore the prebuilt binding for the right ABI instead (`prebuild-install` under the correct Node), then rebuild under the correct Node only if no prebuild exists.
5. WSL note: this project's tests are Windows-native (fixtures use win32 path semantics). Run gates on native Windows; ~24 harness failures under WSL are path semantics, not product bugs.
6. After any fix, start the engine once and confirm real SSE traffic before declaring success.

## Honesty rules

- Report the actual numbers you measured (loader ABI, binding ABI), not the expected ones.
- A step you could not run is a step you did not run — say so.
- "Tests pass" is not "engine starts"; verify the live path.
