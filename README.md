# GB Poseidon2b CPU Miner

**GB Poseidon2b CPU Miner** (program name `gb-jetsam-cpu-miner`) is a CPU miner for **Jetsam (JTM)** and its TowerWalk proof of work. It mines on Stratum pools (rplant and any other Jetsam Stratum pool) and on HTTP pools (dongqn and any other node-RPC style pool), picks a good profile for your CPU on its own, and runs on Linux, Windows and HiveOS — one download per platform.

**Current release: v1.0.3**

<p><img src="img/live-view-windows-7945hx.png" alt="The miner's live view on Windows" width="100%"></p>
<p><sub>The live view on Windows.</sub></p>

---

## Hashrate

| CPU | Cores / threads | L3 | Hashrate | per core | live vs cpuminer-rplant ¹ |
|---|---:|---:|---:|---:|---:|
| 2 x AMD EPYC 7C13 ² | 128 / 256 | 512 MB | 93.18 kH/s | 0.73 kH/s | +11 % |
| AMD EPYC 9455P (VM, 32 of 48 cores) ³ | 32 / 32 | — | 48.55 kH/s | 1.52 kH/s | — |
| AMD EPYC 7B12 | 64 / 128 | 256 MB | 41.71 kH/s | 0.65 kH/s | — |
| AMD EPYC 7K62 | 48 / 96 | 192 MB | 38.85 kH/s | 0.81 kH/s | +10 % |
| Intel Core Ultra 9 285K | 24 / 24 | 36 MB | 34.78 kH/s | 1.45 kH/s | +61 % |
| AMD Ryzen 9 9950X | 16 / 32 | 64 MB | 34.51 kH/s | 2.16 kH/s | — |
| AMD Ryzen 9 9950X3D | 16 / 32 | 128 MB | 34.10 kH/s | 2.13 kH/s | — |
| AMD Ryzen 9 7950X | 16 / 32 | 64 MB | 30.47 kH/s | 1.90 kH/s | — |
| AMD Ryzen 9 7945HX | 16 / 32 | 64 MB | 26.31 kH/s | 1.64 kH/s | — |
| Intel Core i7-13700KF | 16 / 24 | 30 MB | 20.67 kH/s | 1.29 kH/s | — |
| Intel Core i9-12900K | 16 / 24 | 30 MB | 18.47 kH/s | 1.15 kH/s | +13 % |
| AMD Ryzen 9 5900X | 12 / 24 | 64 MB | 17.46 kH/s | 1.46 kH/s | +46 % |
| AMD Ryzen 9 5950X | 16 / 32 | 64 MB | 16.83 kH/s | 1.05 kH/s | +10 % |
| Intel Core Ultra 5 225F | 10 / 10 | 20 MB | 12.50 kH/s | 1.25 kH/s | — |
| AMD Ryzen 7 5700X | 8 / 16 | 32 MB | 10.00 kH/s | 1.25 kH/s | +19 % |
| AMD Ryzen 5 5600X | 6 / 12 | 32 MB | 9.11 kH/s | 1.52 kH/s | +50 % |
| AMD Ryzen 7 5700G | 8 / 16 | 16 MB | 8.99 kH/s | 1.12 kH/s | +9 % |

Measured 2026-10-14 (Ryzen 9 9950X and 9950X3D: 2026-10-15) on rented machines with v1.0.2, the EPYC 9455P VM on 2026-10-16 with v1.0.3: built-in hashrate benchmark (no pool, 2–3 minutes, mean of two runs), whole CPU, automatic profile. On whole machines v1.0.3 runs at the same rate or slightly faster (re-measured on Ryzen 9 9950X, Ryzen 9 5950X and Ryzen 7 5700X: 0 to +2 %). Your rate depends on clocks, cooling, memory and other load; `--tune` measures the best setting on your own machine.

¹ Same machine, same pool (rplant), 5 minutes each, live, default settings, rate as shown by each miner.  
² Two sockets.  
³ Virtual machine with 32 of the CPU's 48 cores and no SMT.

## Download

Grab the archive for your platform from the [latest release](../../releases/latest), unpack it, and run it from inside the folder.

| Platform | Archive |
|---|---|
| Linux | `gb-jetsam-cpu-miner-1.0.3-linux.tar.gz` |
| Windows | `gb-jetsam-cpu-miner-1.0.3-windows.zip` |
| HiveOS | `gb-jetsam-cpu-miner-1.0.3.tar.gz` (custom-miner package) |

The Linux and Windows archives carry a `SHA256SUMS` with the SHA-256 of each file inside, and the `SHA256SUMS` on the release lists the archives themselves. Some antivirus scanners flag `gb-jetsam-cpu-miner.exe` (a false positive); the Windows README shows how to check the file against `SHA256SUMS`.

Runs on any 64-bit x86 CPU with SSE4.1 and PCLMULQDQ — practically every x86-64 CPU of the last decade; AVX2 and VPCLMULQDQ are used where the CPU has them. The Linux and HiveOS binary needs glibc 2.17 or newer, so it runs on virtually any Linux and on all current HiveOS images.

## Quick start

**Linux** — put your address, pool and worker name at the top of `start.sh`, then:
```
sh start.sh
```
or start the miner directly:
```
./gb-jetsam-cpu-miner --pool rplant --coinbase <your-j1-address> --worker-name rig1
```

**Windows** — put your address, pool and worker name at the top of `start.bat` and double-click it, or:
```
gb-jetsam-cpu-miner.exe --pool rplant --coinbase <your-j1-address> --worker-name rig1
```

> [!IMPORTANT]
> **Windows: turn on large pages once.** Double-click `enable-large-pages.bat` in the miner folder, allow the administrator prompt, then sign out of Windows and back in (or restart). Without large pages the miner runs about 20–25 % slower.

**HiveOS** — Flight Sheet → Custom miner:
- Miner name: `gb-jetsam-cpu-miner`
- Installation URL: `https://github.com/gustlborg/gb-poseidon2b-cpu-miner/releases/download/v1.0.3/gb-jetsam-cpu-miner-1.0.3.tar.gz`
- Wallet and worker template: `%WAL%.%WORKER_NAME%` (or your `j1…` address)
- Pool URL: `rplant`, `dongqn`, or your own pool URL
- Extra config (optional, one per line): `THREADS=24`, `POOL_DIFF=auto`, `PASS=x`

`--coinbase` is your Jetsam payout address (`j1…`, 60 characters) and also your pool login; the miner checks it before it starts. Without `--worker-name` it derives a stable name from the machine, so several rigs on one wallet stay apart on the pool.

## Pools

`--pool` takes a short name or your own URL — nothing about the pool is hard-coded.

| `--pool` | Pool |
|---|---|
| `rplant` | rplant, Europe — Stratum over TLS |
| `rplant-na` / `rplant-asia` | rplant, North America / Asia |
| `dongqn` | dongqn — HTTP |
| `stratum+tcp://HOST:PORT` / `stratum+ssl://HOST:PORT` | any Jetsam Stratum pool |
| `http://HOST:PORT` / `https://HOST:PORT` | any HTTP (node-RPC style) pool |

On Stratum pools the miner picks a sensible share difficulty by itself (`--pool-diff` overrides it) and reconnects on its own if a pool session stalls.

## Options

```
--pool URL|NAME        pool to mine on (see above)
--coinbase ADDRESS     your j1… payout address and pool login
--worker-name NAME     name shown on the pool (optional)
--threads N            logical CPUs to use (default: all; -N leaves N free)
--pool-diff auto|off|N share difficulty: auto (default), the pool's, or fixed
--pass OPTIONS         pool password/options (default "x"; e.g. m=solo)
--tune 20              measure the best profile for this CPU, store it, exit
--show-profile         print the profile this machine would use, then exit
--report-secs N        stats line every N seconds in a log (default 15)
```

`--tune 20` measures the profiles for your CPU for a few minutes, stores the fastest and exits; every later start uses it. Run it directly (`./gb-jetsam-cpu-miner --tune 20`, on Windows `gb-jetsam-cpu-miner.exe --tune 20`), not through `start.sh` / `start.bat` — those restart the miner in a loop. On a Ryzen 9 5950X with a fixed all-core clock, run it once: there it measured a setting 35 % faster than the automatic profile.

## Live view

In a terminal the miner pins a live view at the top: the run's facts (pool and its latency, network height and difficulty, CPU and threads), the hashrate now and averaged over 5 minutes to 24 hours, accepted / stale / rejected shares, CPU temperature and power where the system exposes them, memory (huge pages, scratchpads) and a hashrate bar for every CPU core — with the log of jobs and shares scrolling below. It follows the window size and reads on light and dark terminals. Keys: `q` quit, `p` pause/resume (the pool stays connected), `r` reset the averages. Written into a log file or under HiveOS it prints plain timestamped lines instead; `GBJETSAM_PLAIN=1` forces those in a terminal too, e.g. to paste the output into a bug report.

## Huge pages / large pages

The miner keeps each worker's scratchpad in huge pages (Linux) or large pages (Windows); without them it runs about 20–25 % slower. The start-up line `huge pages: N/N workers` shows how many got them.

- **Linux** — transparent huge pages must not be disabled: `cat /sys/kernel/mm/transparent_hugepage/enabled` should show `[madvise]` or `[always]`. If they are on and the warning still appears, the memory is too fragmented: `echo 1 | sudo tee /proc/sys/vm/compact_memory` or a reboot.
- **Windows** — double-click `enable-large-pages.bat` from the Windows download once, allow the administrator prompt, then sign out and in again. By hand instead: grant your account the **Lock pages in memory** right (`secpol.msc` → Local Policies → User Rights Assignment), then sign out and in again. Windows Home has no `secpol.msc`; the script does not need it. If the warning shows `os error 1450`, the right is there but the memory is fragmented: restart the PC and start the miner soon after.

## Developer fee

This miner mines **3 %** of the time to the developer's wallet and **97 %** to yours. The fee runs as a separate, parallel pool connection under the developer's address; your own shares are never redirected, and there is nothing to configure. If you prefer a miner without a fee, the upstream project `towerminer` (Apache-2.0, see NOTICE) has none.

## What's new in 1.0.3
- Large pages for more workers: up to four workers now share one 2 MiB page
  (one scratchpad each) instead of taking a page each, so the miner needs up
  to four times fewer large pages. A Windows PC that has been running for a
  while, with its memory fragmented, is therefore more likely to find them for
  all workers. Linux huge pages benefit the same way.
- Windows: `enable-large-pages.bat` is part of the download. Double-click it
  once, allow the administrator prompt, then sign out and back in.
- Clearer large-page warnings. Windows: without the right the warning now
  points to `enable-large-pages.bat`; with `os error 1450` the right is there
  but the memory is fragmented, and it says to restart the PC. Linux: when THP
  is on but no free 2 MiB pages are left, the warning says so instead of asking
  to switch THP on.
- Power: "power n/a (no sensor here)" on machines without a power counter
  (many containers and VMs) instead of "needs root".
- Faster automatic profile on AMD Zen 5 (Ryzen 9000, EPYC 9005) with one
  thread per core -- virtual machines without SMT, SMT switched off, or
  `--threads-per-core 1`: two scratchpads with prefetch, also when a virtual
  machine reports wrong cache sizes (measured +33 % on an EPYC 9455P VM).
- Faster automatic profile on AMD Ryzen 5000 desktop CPUs with up to 12
  cores and one thread per core -- SMT switched off, virtual machines without
  SMT, or `--threads-per-core 1`: four scratchpads with prefetch. With SMT on,
  the profile stays as it was. Measured live at the pool (kH/s, 2 x 5 minutes
  per setting):

  | CPU | 1 scratchpad (before) | 4 scratchpads (1.0.3) | Gain |
  |---|---:|---:|---:|
  | Ryzen 5 5600X | 7.13 | 7.54 | +5.7 % |
  | Ryzen 7 5700X | 9.30 | 9.81 | +5.5 % |
  | Ryzen 7 5800X (Windows) | 9.23 | 10.73 | +16.2 % |
  | Ryzen 9 5900X | 13.14 | 15.62 | +19 % |

- Ryzen 9 5950X: the automatic profile stays. On a machine with a fixed
  all-core clock (4.225 GHz) other settings were faster live at the pool, but
  on machines that lower the clock under the power limit they were 4 % and
  10–13 % slower. With a fixed all-core clock, run `--tune 20` once; it finds
  the faster setting and keeps it:

  | Setting | kH/s | Gain |
  |---|---:|---:|
  | Automatic (one thread per core, 1 scratchpad) | 17.31 | – |
  | 4 scratchpads with prefetch | 20.44 | +18 % |
  | 2 threads per core, 2 scratchpads with prefetch | 23.43 | +35 % |

- The built-in table of measured settings now works on Windows too.
- Log: each accepted share now shows the difficulty it was found at.

## What's new in 1.0.2

- Faster automatic profile on AMD Ryzen 5000 desktop CPUs with up to 12
  cores (measured on the Ryzen 5 5600X, Ryzen 7 5700X, Ryzen 7 5800X and
  Ryzen 9 5900X; it also applies to the 5600 and 5800X3D): two threads per
  core with two scratchpads and prefetch instead of one thread per core. The 16-core
  Ryzen 9 5950X, the APU-based models with 16 MB of L3 (5500, 5600G, 5700,
  5700G) and machines without SMT keep the previous profile.
- Faster hashing with four scratchpads per worker on AMD Zen 3 / Zen 4 and
  some Intel Core Ultra 200 (Arrow Lake) CPUs, on Linux and Windows: the
  faster code switches on by itself whenever four scratchpads with prefetch
  run, for example when `--tune` measures that setting best. This alone does
  not change any automatic profile.
- Built-in table of measured settings (Linux): for some CPU and core layouts,
  e.g. six cores of a larger CPU in a rented machine, and for AMD Zen 4 /
  Zen 5 with one thread per core, the miner now picks the faster scratchpad
  setting by itself. This table is not used on whole machines with two
  threads per core. `--show-profile` prints the profile without mining.
- `--tune` now also tries four scratchpads per worker and stores the setting
  it really measured.
- Windows: clicking into the console window no longer pauses the miner's
  output (QuickEdit is off while the live view runs), and other miners
  running on the same machine are reported at start-up, as on Linux.
- More robust pool handling: new share targets reach the workers without
  restarting the search, HTTP jobs expire on time even when polls fail, and
  a nonce range assigned by an HTTP pool is honoured.

## License

Based on [towerminer](NOTICE) (Apache-2.0); the TowerWalk proof of work comes from the Jetsam project (Apache-2.0). See [LICENSE](LICENSE) and [NOTICE](NOTICE).
