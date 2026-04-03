---
name: go-modern-playbook
description: Use when writing, reviewing, upgrading, or debugging Go code and post-2024-06 release knowledge may matter, especially iterators, generic aliases, new stdlib/runtime APIs, tool directives, benchmark/testing updates, WaitGroup.Go, container-aware runtime behavior, or migration-sensitive Go changes.
---

# Go Modern Playbook

## Overview
Use this skill when coding in Go and post-`2024-06` release knowledge could change what code you should write today. This file is not a release index. It is a coding handbook: what to reach for, what to avoid, what is stable vs experimental, and where old habits are now the wrong default.

## Current Snapshot
- Checked: `2026-03-19`
- Model global knowledge cutoff: `2024-06`
- Baseline inference before live lookup: Go `1.22` era
- Latest stable verified at check time: `1.26.1`
- Supported release lines at check time: `1.26`, `1.25`

## Default Guidance
- For new code, think in `Go 1.25/1.26` terms unless the repo pins an older toolchain.
- Prefer stable additions that remove ceremony:
  - `sync.WaitGroup.Go`
  - `go.mod` `tool` directives
  - iterator-style traversal with `range` over functions
  - `testing.B.Loop`
  - `testing/synctest`
  - `errors.AsType`
- Treat these as opt-in, not default:
  - `encoding/json/v2`
  - `simd/archsimd`
  - `runtime/secret`
  - goroutine leak profiling experiments

## Quick Gating Matrix
- `range` over iterator functions
  - Min Go: `1.23`
  - Stable: yes
  - Good default: yes, when it removes callback glue or temporary slices
- generic type aliases
  - Min Go: `1.24`
  - Stable: yes
  - Good default: yes, if aliases improve API clarity instead of hiding types
- `go.mod` `tool` directives
  - Min Go: `1.24`
  - Stable: yes
  - Good default: yes, replaces `tools.go`
- `testing.B.Loop`
  - Min Go: `1.24`
  - Stable: yes
  - Good default: yes, for new benchmarks
- `testing/synctest`
  - Min Go: `1.25`
  - Stable: yes
  - Good default: yes, for deterministic concurrency/time tests
- `sync.WaitGroup.Go`
  - Min Go: `1.25`
  - Stable: yes
  - Good default: yes, unless panic handling is part of the design
- `errors.AsType`
  - Min Go: `1.26`
  - Stable: yes
  - Good default: yes, if it replaces noisy `errors.As` boilerplate
- `encoding/json/v2`
  - Min Go: `1.25`
  - Stable: experimental
  - Good default: no
- `simd/archsimd`
  - Min Go: `1.26`
  - Stable: experimental
  - Good default: no

## Use Now
- Use iterator functions when the alternative is callback-heavy traversal or building a slice only to loop once.
- Use `go.mod` `tool` directives instead of keeping a fake package with blank imports.
- Use `testing.B.Loop` for new benchmarks instead of hand-writing `for i := 0; i < b.N; i++`.
- Use `testing/synctest` for concurrency tests that previously depended on sleeps, timeouts, or scheduler luck.
- Use `sync.WaitGroup.Go` when the alternative is repetitive `Add` and `Done` bookkeeping.
- Use `os.Root` when you need bounded filesystem access instead of open-coded path-prefix guards.
- Use `errors.AsType[E]` when the alternative is verbose `errors.As` target plumbing.
- Use `runtime.SetDefaultGOMAXPROCS()` if code previously overrode `GOMAXPROCS` and now needs container-aware defaults back.

## Avoid / Don't Reach For
- Don't default to `encoding/json/v2` unless the repo explicitly chose that path.
- Don't use `runtime.AddCleanup` as primary resource management. It is cleanup fallback, not replacement for explicit close/release flows.
- Don't assume manual `GOMAXPROCS` tuning keeps adapting to container CPU limits. Manual override disables automatic updates.
- Don't keep `tools.go` around in new repos or during normal modernization.
- Don't poll timer or ticker channels with `len` or `cap`. That mental model broke in `1.23`.
- Don't use `WaitGroup.Go` if the worker can panic and panic recovery is part of the contract.
- Don't assume new syntax or APIs are safe if the repo `go` directive or `toolchain` line is older.

## Old Habit -> New Habit
- `tools.go` blank-import package
  - Replace with `go.mod` `tool` directives plus `go get -tool` / `go install tool`
- manual `for i := 0; i < b.N; i++`
  - Replace with `b.Loop()`
- sleep-heavy concurrency tests
  - Replace with `testing/synctest`
- `wg.Add(1); go func() { defer wg.Done(); ... }()`
  - Replace with `wg.Go(func() { ... })`
- `var target *MyErr; errors.As(err, &target)`
  - Replace with `target, ok := errors.AsType[*MyErr](err)`
- ad hoc path containment checks
  - Replace with `os.Root`

## Important Post-Cutoff Deltas
- `1.23`
  - `range` over iterator functions
  - `iter`, `unique`, `structs`
  - timer/ticker channel behavior changes
  - `reflect.Value.Seq` / `Seq2`
  - `go vet` `stdversion`
- `1.24`
  - generic type aliases
  - `go.mod` `tool` directives
  - `testing.B.Loop`
  - `runtime.AddCleanup`
  - `weak`
  - `os.Root`
  - crypto additions like `mlkem`, `hkdf`, `pbkdf2`, `sha3`
  - `go vet` got stricter around tests and formatting issues
- `1.25`
  - `testing/synctest` stable
  - `sync.WaitGroup.Go`
  - `runtime/trace.FlightRecorder`
  - container-aware runtime behavior and `SetDefaultGOMAXPROCS`
  - `go.mod ignore`
  - `go version -m -json`
  - `go doc -http`
  - `go vet` `waitgroup` and `hostport`
- `1.26`
  - `new(expr)`
  - self-referential generic constraints
  - modernizing `go fix` passes
  - `errors.AsType`
  - `bytes.Buffer.Peek`
  - `crypto/hpke`

## Language / Stdlib / Testing / Toolchain

### Language
- `range` over functions lets APIs expose lazy iteration without channels or temporary collections.
- Generic type aliases are no longer preview-era trivia. If an alias clarifies an API boundary, it is now a real tool.
- `new(expr)` is useful for optional pointer fields in literals and test fixtures where `&T{...}` is awkward or impossible.

### Stdlib
- `runtime.AddCleanup`
  - Use when you need cleanup associated with object lifetime as a fallback.
  - Avoid when explicit ownership or `Close()` is the real design.
  - Caveat: cleanup is not guaranteed before process exit.
- `weak`
  - Use only when weak reachability is actually part of the data-structure design.
  - Avoid for normal cache code unless you understand the lifetime tradeoffs.
- `os.Root`
  - Use for directory-scoped operations that must not escape a root.
  - Prefer it over ad hoc prefix or `filepath.Clean` guards.
- `errors.AsType`
  - Use when `errors.As` is correct but noisy.
  - Avoid if the repo is pinned before `1.26`.

### Testing
- `testing.B.Loop`
  - Prefer for new benchmarks.
  - It reduces manual timer control and improves benchmark shape.
  - Do not mechanically rewrite old benchmarks without checking semantics around setup work.
- `testing/synctest`
  - Prefer when a test currently uses `time.Sleep`, deadline inflation, or scheduler luck.
  - Mental model: the code runs in a controlled bubble with fake time and deterministic scheduling points.
  - Avoid if the test is fundamentally integration-style and depends on real external time or processes.

### Toolchain
- `go.mod` `tool` directives are the new default way to pin developer tools.
- `go fix` got more worthwhile for modernization. Use it as a first pass when jumping older code forward.
- `go vet` learned more post-cutoff. A new warning may mean the code is genuinely stale against the newer toolchain.

### Runtime
- Container-aware `GOMAXPROCS` matters more now.
- If code set `GOMAXPROCS` manually long ago, re-check whether that override still makes sense in containers.
- `runtime/trace.FlightRecorder` is useful when you need low-overhead tracing capture around issues in long-running processes.

## Use / Avoid / Caveats
- `sync.WaitGroup.Go`
  - Use when you want simple fan-out concurrency.
  - Avoid when the function may panic or when you need richer task semantics than `WaitGroup`.
  - Caveat: `f` must not panic.
- `runtime.AddCleanup`
  - Use as last-resort cleanup tied to object lifetime.
  - Avoid as substitute for explicit close/release ownership.
  - Caveat: not guaranteed before exit, not immediate, not deterministic.
- `os.Root`
  - Use when you need rooted filesystem access.
  - Avoid reimplementing root checks by hand.
  - Caveat: still understand what operations need to remain inside the root.
- `testing/synctest`
  - Use for deterministic concurrent tests.
  - Avoid for full end-to-end tests with real clocks and external systems.
  - Caveat: think in terms of bubble scheduling and fake time.
- `encoding/json/v2`
  - Use only if the repo deliberately opts into it.
  - Avoid as a silent default modernization.
  - Caveat: experimental surface means migration and compatibility risk.

## Tiny Before / After Snippets

```go
// old
var wg sync.WaitGroup
wg.Add(1)
go func() {
	defer wg.Done()
	work()
}()

// new
var wg sync.WaitGroup
wg.Go(func() {
	work()
})
```

```go
// old
var pathErr *fs.PathError
if errors.As(err, &pathErr) {
	return pathErr.Path
}

// new
if pathErr, ok := errors.AsType[*fs.PathError](err); ok {
	return pathErr.Path
}
```

```go
// old
for i := 0; i < b.N; i++ {
	doWork()
}

// new
for b.Loop() {
	doWork()
}
```

```go
// old
type Config struct {
	Timeout *time.Duration
}
cfg := Config{Timeout: ptrDuration(5 * time.Second)}

// new
cfg := Config{Timeout: new(5 * time.Second)}
```

## Tooling Checks Before Using Modern APIs
- Check `go` directive in `go.mod`.
- Check `toolchain` line if the repo uses it.
- If a repo builds in CI containers, confirm the actual Go version there.
- Run `go vet` on modernization branches because newer analyzers catch stale patterns.
- If the feature is experimental, verify the repo consciously opted in before using it.

## Upgrade / Migration Traps
- Timer/ticker channels changed in `1.23`.
  - Stop using `len(t.C)` or `cap(t.C)` as readiness signals.
  - Prefer non-blocking receive or direct select logic.
- Manual `GOMAXPROCS` freezes auto-updating behavior.
- `go.mod ignore` and `tool` directives change how you should think about module housekeeping.
- `go mod init` defaults may not match the latest toolchain line you were expecting.
- New `go vet` findings after upgrade often reflect real stale assumptions, not just tool noise.

## Safe Defaults For Agents
- Prefer `b.Loop()` in new benchmarks on Go `1.24+`.
- Prefer `testing/synctest` for deterministic concurrency tests on Go `1.25+`.
- Prefer `WaitGroup.Go` on Go `1.25+` unless panic recovery is required.
- Prefer `tool` directives instead of `tools.go`.
- Prefer `os.Root` over ad hoc path-safety checks.
- Treat `json/v2`, `simd/archsimd`, and runtime experiments as opt-in only.

## Live Re-check Only When Needed
- Exact latest patch number matters.
- The repo is pinned to an older `go` directive or older CI toolchain.
- The task depends on an experimental package or flag.
- The issue smells runtime-, scheduler-, or container-specific.
