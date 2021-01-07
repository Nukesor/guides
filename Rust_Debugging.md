# Rust debugging guides

## Memory debugging:

Packages: 

- `valgrind` Debugging tool for memory, cache, heap and branch prediction
- `massif-visualizer` Visualization of 


### Usage

Start your binary with `valgrind --tool massif /path/to/executable`.

The resulting `massif.out.<pid>` file can then be opened with `massif-visualizer`.


## Code stepping

Packages: 

- `rr` Deterministic playback for rust executables
    You might have to `echo 1 | sudo tee  /proc/sys/kernel/perf_event_paranoid` to disable some kernel security measures.


### Usage

You have to compile with debug symbols enabled.
This is the default for tests, but for other builds you have to explicitely enable them.

```
[profile.release]
debug = 2
```

Afterwards, just run the executable with `rr record` e.g. `rr record /path/to/executable`.\
You can also run stuff via `cargo`, e.g. `rr record cargo test your_test_case`.

Starting a replay is now as easy as this: `rr replay`\
For information on how to use rr, please look at the official site: https://rr-project.org/
