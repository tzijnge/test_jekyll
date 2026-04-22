---
title: LotusRPC
author_profile: true
toc: false
---

⚠ **This project is work in progress.**
{: .notice--warning}

RPC framework for embedded systems. Generates C++ server code and a Python client from a single YAML definition file. Built on [ETL](https://github.com/ETLCPP/etl) — no dynamic memory allocations, no exceptions, no RTTI.

## How it works

``` mermaid
graph LR
    C["Client\nPC · Python"] <-->|"function call / return"| T["Transport\nserial · BT · TCP · …"]
    T <-->|bytes| S["Server\nMCU · C++"]
```

Define your interface once in YAML. LotusRPC generates everything else: the C++ server stub, all serialization code, and a Python client. You implement the business logic; LotusRPC handles the protocol.

[Get started](getting_started){: .btn .btn--primary .btn--large} &nbsp; [Reference](reference){: .btn .btn--large} &nbsp; [C++ API](cpp_api){: .btn .btn--large} &nbsp; [Examples](examples){: .btn .btn--large}

## Key features

- **No dynamic memory** — stack-only, no heap, no exceptions, no RTTI
- **Transport agnostic** — serial, Bluetooth, TCP — any byte-oriented channel works
- **YAML definitions** — schema-validated, editor-friendly, easy to parse or extend
- **Code generation** — `lrpcg` produces all C++ server and client code in one command
- **CLI client** — `lrpcc` lets any team member call remote functions without writing code
- **Streams** — client-to-server and server-to-client data streams, finite or infinite
- **C++11 compatible** — works on any platform with a modern C++ compiler

## Quick example

Define an interface in YAML:

``` yaml
name: example
settings:
  namespace: ex
services:
  - name: math
    functions:
      - name: add
        params:
          - { name: a, type: int32_t }
          - { name: b, type: int32_t }
        returns:
          - { name: result, type: int32_t }
```

Generate code, implement the function, and call it from the command line:

``` bash
lrpcg cpp -d example.lrpc.yaml -o generated/
lrpcc math add 3 7   # prints: result = 10
```

See [Getting started](getting_started) for a complete walkthrough.

## Supported data types

| Category | Types |
|----------|-------|
| Integer  | `uint8_t`, `int8_t`, `uint16_t`, `int16_t`, `uint32_t`, `int32_t`, `uint64_t`, `int64_t` |
| Float    | `float`, `double` |
| Bool     | `bool` |
| String   | `string` (auto size), `string_N` (fixed size N) |
| Bytearray | `bytearray` — flexible-length byte buffer |
| Array    | Any type with `count: N` (N ≥ 2) |
| Optional | Any type with `count: ?` |
| Struct   | Composite user-defined type |
| Enum     | User-defined enumeration, translated to `enum class` |

For details on each type and the C++ mappings, see the [C++ API reference](cpp_api) and the [interface definition reference](reference).
