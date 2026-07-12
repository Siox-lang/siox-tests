# siox-tests

The `.siox` example and conformance corpus for the
[**siox**](https://github.com/Siox-lang/sioxc) hardware description language —
counters, FSMs, a FIFO, SPI, RISC-V ALU/decoder fragments, tristate buses,
generate loops, struct/bus ports, and more.

These programs are the language-level integration suite for the `sioxc`
compiler. They are engine-agnostic: each is a self-contained `.siox` module,
usually with a `#[test]` entity that drives a design and asserts on it.

## Running them

You need the [`sioxc`](https://github.com/Siox-lang/sioxc) compiler and its
standard library (`sioxc`'s `std/` directory). From a checkout of `sioxc`, with
this repo checked out alongside:

```bash
# Type-check every program:
for f in ../siox-tests/*.siox; do
    sioxc check "$f" --std std
done

# Run a testbench:
sioxc test ../siox-tests/counter_test.siox --std std
```

The `sioxc` repository's CI checks out this corpus and runs it on every change,
so a regression in the compiler shows up as a failing `sioxc` build.

## Layout

- `*.siox` — one program per file (the file name says what it exercises).
- `*.bin` / `*.txt` — data files a few programs read (`read`/`read_to_string`).

## License

Dual-licensed under either MIT or Apache 2.0, matching `sioxc`.
