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

## Contributing

Contributions are welcome! Please open an issue before submitting large pull requests so we can discuss the approach.

1. Fork the repository
2. Create a feature branch (`git checkout -b feat/my-feature`)
3. Commit your changes following [Conventional Commits](https://www.conventionalcommits.org/)
4. Push and open a Pull Request

## License

This project is licensed under the [Apache 2.0 License](LICENSE).

Portions of this codebase are derived from [microsoft/TypeScript](https://github.com/microsoft/TypeScript), which is licensed under the [Apache 2.0 License](https://github.com/microsoft/TypeScript/blob/main/LICENSE.txt).
