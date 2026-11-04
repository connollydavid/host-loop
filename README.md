# host-loop

The opencode goal loop under host governance: the `/host-goal` command family
bridged onto opencode's native `/goal` coordinator.

One controller, upstream's, by construction: this plugin registers no idle
hook and sends no continuations. The bridge maps the host verbs (the sole
state authority: the receipts, the Status line, `GOAL.md`) onto the native
lifecycle through `ctx.session.command`, and four mechanical belts stay ours
over the goal RPC: the round cap (default 25), the wall-clock deadline
(default 30 minutes), the NEEDS YOU stop, and the native-overwrite guard.
Completion is never model-claimable: the native reviewer's `done` is a
recoverable stop that writes nothing to the tracked surfaces; `goal complete`
banks through the composed gates and the operator's `goal accept` closes.

## Install

An opencode `plugins` entry (the file URL of this repo's checkout, or the
package name once published):

```json
{ "plugins": ["file:///path/to/host-loop"] }
```

`HOST_LIFECYCLE_BIN` overrides the `host-lifecycle` binary the verbs run
through (fixtures pin it); `HOST_LOOP_MAX_ITERATIONS` and
`HOST_LOOP_MAX_MINUTES` (or the plugin entry's `options.maxIterations` /
`options.maxMinutes`) bound the belts.

## Develop

```
node --test suite.test.ts          # the tier-1 suite (node 26 strips the types)
allium check host-loop.allium      # the spec
host-lifecycle obligations host-loop.allium --tests . --strict-discharge
```

The spec (`host-loop.allium`) states the bridge and the belts; every derived
obligation is dispositioned in `host-loop.obligations`. Zero runtime
dependencies: the goal RPC is called through a structural definition typed
against the public Rpc surface (the published-package cut declares
`@opencode/schema` and imports `Goal.Definition` directly).

## Provenance

Built under [agentic-host](https://github.com/connollydavid/agentic-host):
plan/0106 (the original plugin, its A/B-confirmed doctrine, the tiered
harnesses) and plan/0109 (the realignment onto the native coordinator, the
public-API audit). No code from opencode or any third-party plugin plugin is
included; the bridges ride opencode's public plugin API only.
