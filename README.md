# SteamOS on Tegra / Nintendo Switch: engineering history

> **Development status: not suitable for everyday use.**
> This project is a research prototype, not a practical SteamOS gaming system at
> this stage. Serious performance, stability and game compatibility limitations
> remain. The sources are published for research and development.
>
> **Текущее состояние: проект непригоден для повседневного использования.**
> Это исследовательский прототип. Существенные ограничения производительности,
> стабильности и совместимости с играми делают практическое использование системы
> крайне затруднительным. Исходники опубликованы для разработки и исследований.

This is the public research record of an experimental SteamOS userland and
Gamescope port to a Tegra X1 Nintendo Switch. It describes the implementation,
the boundaries that actually passed on hardware, failed and abandoned approaches,
and the remaining work. It is documentation, not an installation image or a
claim that arbitrary Windows games work.

**Status date:13 October 2026.** The current development baseline runs 854x480
Steam UI on Zink/NVIDIA, with hardware180 s and physical-screen user confirmation.
The user then reported about an hour of navigation without noticed UI bugs.
Optional diagnostics have just been disabled by request; the subsequent visual/
performance verdict is pending. [Actual route and remaining CPU work](docs/ZINK-STEAM-UI.md).
Historical EGLStream/game failures remain open; UI success is not a universal game PASS.

The strongest completed paths are:

- An NVIDIA Vulkan compositor with GPU conversion to Tegra block-linear/GOB
  scanout and direct legacy Tegra DC `FLIP4` output to the LCD.
- Native Steam/CEF GPU UI using Mesa 24.2.8/Zink/T3; persistent 480p profile,
  independent UI/game geometry, and short-test human confirmations through720p.
- Automatic Steam UI startup, low-resolution UI, correct baseline orientation,
  audio wiring and device integration on the tested console.
- Native Linux x86-64 Super Meat Boy and FEZ, translated through Box64, with
  native Wayland/EGLStream output and physical-screen user confirmation.
- x64 execution and SteamAPI under ARM-FEX, offscreen D3D11 GPU rendering with a
  private DXVK-Sarek variant, and raw Vulkan FEX synthetic presentation with
  correct orientation, including a four-minute physical-screen confirmation.

Those are different results. The four-minute FEX test is **not** an ordinary
Proton game qualification; stock Proton 11.0-1 FEX is not advertised as repaired.
Box64 Windows game menus/logos have been reached, but consumer stability,
performance and presentation geometry still have open limitations.

## Read the record

| Document | Purpose |
|---|---|
| [Zink UI and CPU work](docs/ZINK-STEAM-UI.md) | Current480p GPU path, repairs, performance limits |
| [Russian overview](docs/OVERVIEW-RU.md) | Practical introduction and current priorities |
| [Architecture](docs/ARCHITECTURE.md) | Display, transport, UI, Linux games and Windows/FEX paths |
| [Timeline](docs/TIMELINE.md) | Alpha bring-up through M9L18 preparation |
| [Implementation catalog](docs/IMPLEMENTATIONS.md) | What was built and the exact extent of its acceptance |
| [Blocker ledger](docs/BLOCKERS.md) | Resolved, open, retracted and bounded route-specific blockers |
| [Closed branches](docs/CLOSED-BRANCHES.md) | Approaches that should not be retried without new evidence |
| [Baseline and evidence rules](docs/BASELINE-AND-EVIDENCE.md) | Reproducibility, identities and limitations |
| [Next work](docs/ROADMAP.md) | Concrete engineering direction, not permission to run old tasks |
| [Source reference index](docs/SOURCE-REFERENCES.md) | How historical references and hashes are used |
| [Machine-readable source inventory](sources/reports.json) | Curated source report identities |
| [Credits](NOTICE.md) | Upstream contributors and origins |

This is an original synthesis of the private development reports and user
checkpoints. Raw logs, full private history, accounts, addresses, screenshots,
core dumps, games, proprietary binaries, sysroots, firmware and prefixes are
excluded. Historical source-report names and commit identifiers identify the
development record; they are not commits of this public repository and are not
promised as publicly fetchable commits.

Corrections are retained explicitly. A report's early claim does not become the
current conclusion merely because it appears in an old checkpoint. Runtime
evidence and later corrections take precedence over plans and build-only claims.

## Related source repositories

The publication is divided by responsibility:

1. **[gamescope-tegra](https://github.com/zodiakborodin98/gamescope-tegra)** — source port and reproducible graphical-stack recipes.
2. **[tegra-lcd-backend](https://github.com/zodiakborodin98/tegra-lcd-backend)** — standalone direct-display component/probes and its
   relationship to the integrated Gamescope presenter.
3. **[steamos-tegra](https://github.com/zodiakborodin98/steamos-tegra)** — image/build integration recipes, external dependencies,
   boot and service integration.
4. **[steamos-tegra-research](https://github.com/zodiakborodin98/steamos-tegra-research)** — this history and blocker record.

The separate repository names describe publication boundaries, not proof that the
LCD component has already become an independent stable library or that a full
fresh image has been tested on all Switch models.

## Evidence vocabulary

`PC PASS`, `hardware machine PASS`, `human visual PASS`, `gameplay PASS`, `natural
lifecycle`, and `baseline recovery` are separate outcomes. A successful API call
does not establish visible content. A successful synthetic test does not establish
game compatibility. Compositor presents per second are **not game FPS**: repeated
textures, UI frames and multiple windows can be included.

The original CPU-compatible Steam path used private A57-safe Mesa/llvmpipe in the
client. Gamescope itself rendered and scanned out on NVIDIA. Later System
Composer removed the active llvmpipe BrowserComposer stage while retaining CPU
CEF OSR output and the private libraries needed for compatibility. Those were historical boundaries. On13 October the private Mesa 24.2.8/T3
repairs and early native ICD bootstrap produced actual GPU Zink Steam UI; CPU
OSR/copies remain. See the current route document for evidence and limits.

## License

Original documents and metadata in this repository are
**GPL-2.0-or-later**. See [LICENSE](LICENSE) and [COPYING](COPYING). Referenced
software retains its own upstream license; no third-party source is relicensed
by these documents. No Steam/NVIDIA runtime redistribution grant is made.

This project is independent of Valve, NVIDIA, Nintendo, Switchroot, gman and
Switchdeck. Their work is credited; their names do not imply endorsement.
