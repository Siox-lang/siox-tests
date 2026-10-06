# siox-tests

The `.siox` example and conformance corpus for the
[**siox**](https://github.com/Siox-lang/sioxc) hardware description language —
counters, FSMs, a FIFO, SPI, RISC-V ALU/decoder fragments, tristate buses,
directional views, generate loops, struct/view ports, and more.

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
    sioxc "$f" --std std --emit metadata
done

# Run a testbench:
sioxc ../siox-tests/counter_test.siox --std std --test -o /tmp/counter-test
/tmp/counter-test -o /tmp/counter-test.vcd
```

The `sioxc` repository's CI checks out this corpus and runs it on every change,
so a regression in the compiler shows up as a failing `sioxc` build.

## Specified Phase 1 examples

The language spec names twelve required artifacts, checked separately from
the corpus glob by `scripts/check-phase1-examples.py` in the compiler repo:

- `basic_mux.siox`, `register.siox`, `counter.siox`, and `fsm.siox`: small hardware designs.
- `enum_event_monitor.siox` and `packet_struct_event.siox`: enum/recursive struct events
  and historical values, including directed array snapshots and multiword X/Z data.
- `stream_bus.siox` and `producer_consumer.siox`: source/sink views and clocked handshakes.
- `attribute_usage.siox`: declaration defaults, entity/instance overrides and metadata.
- `counter_test.siox` and `fsm_test.siox`: simulation testbenches.
- `external_entity_stub.siox`: an elaborated foreign black box with library/name
  metadata and connections; deliberately no executable test or HDL behavior.
  Foreign HDL simulation remains Phase 3 work.

## Layout

- `*.siox` — one program per file (the file name says what it exercises).
- `*.bin` / `*.txt` — data files a few programs read (`read<T>`).

## License

Dual-licensed under either MIT or Apache 2.0, matching `sioxc`.
