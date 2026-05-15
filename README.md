# typescript-go

A Go port of the [Microsoft TypeScript compiler](https://github.com/microsoft/TypeScript).

> **Note:** This is an active development fork. The goal is to reimplement the TypeScript compiler in Go for improved performance and native tooling integration.

## Overview

`typescript-go` is a faithful reimplementation of the TypeScript compiler (`tsc`) written in Go. It aims to provide:

- **Faster compilation** via Go's native concurrency and performance characteristics
- **Lower memory footprint** compared to the Node.js-based compiler
- **Native binaries** for all major platforms (no Node.js runtime required)
- **Full compatibility** with existing TypeScript projects and `tsconfig.json` configurations

## Status

This project is currently in **active development**. Not all TypeScript features are supported yet.

## Getting Started

### Prerequisites

- [Go](https://go.dev/dl/) 1.22 or later
- [Node.js](https://nodejs.org/) (for running the TypeScript test suite)

### Building

```bash
git clone https://github.com/nicholasgasior/typescript-go.git
cd typescript-go
go build ./...
```

### Running

```bash
go run ./cmd/tsc --version
```

### Testing

```bash
go test ./...
```

> **Personal note:** I've been running tests with `-count=1` to avoid cached results while exploring the codebase:
> ```bash
> go test -count=1 ./...
> ```
>
> Adding `-v` is also handy when drilling into a specific package:
> ```bash
> go test -count=1 -v ./internal/parser/...
> ```
>
> For a quicker feedback loop during active development, I use `-short` to skip slower conformance tests:
> ```bash
> go test -count=1 -short ./...
> ```
>
> To run tests in parallel and speed things up on my machine (8 cores), I also use:
> ```bash
> go test -count=1 -short -parallel 8 ./...
> ```
>
> **Tip:** I added a `Makefile` target locally (`make test`) that wraps the above command so I don't have to remember all the flags.

### Benchmarking

> **Personal note:** To get a quick sense of parser performance on a specific package, I run:
> ```bash
> go test -bench=. -benchmem -count=3 ./internal/parser/...
> ```
> Useful for sanity-checking that changes haven't regressed hot paths.
>
> To save benchmark output for comparison across commits, I pipe results to a file and use `benchstat`:
> ```bash
> go test -bench=. -benchmem -count=5 ./internal/parser/... > bench_new.txt
> benchstat bench_old.txt bench_new.txt
> ```
> Install `benchstat` with: `go install golang.org/x/perf/cmd/benchstat@latest`

## Project Structure

```
typescript-go/
├── cmd/
│   └── tsc/          # Main compiler entry point
├── internal/
│   ├── ast/          # Abstract syntax tree definitions
│   ├── parser/       # TypeScript source parser
│   ├── checker/      # Type checker
│   ├── emitter/      # Code emitter / output generation
│   └── diagnostics/  # Error and diagnostic reporting
├── testdata/         # Test fixtures
└── tests/            # Integration and conformance tests
```

## Notes / Personal TODO

- [ ] Investigate checker performance on large union types — noticed slowness on a real-world project with deeply nested conditionals
- [ ] Look into whether the emitter handles `const enum` inlining the same way upstream `tsc` does
- [ ] Try wiring up `gopls` to get IDE support while hacking on the internals
