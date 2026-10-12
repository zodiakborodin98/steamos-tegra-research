# Closed and parked approaches

Closed means “do not spend another sprint on the same unmodified premise.” New
source/driver evidence or a consciously authorized architecture can change a
decision. An exact route NO-GO is not a proof that no future implementation can
exist.

| Branch | Why it was closed/parked | What would be materially new evidence |
|---|---|---|
| Mainline/nouveau/NVK/NVC0 / new Atomic KMS | L4T already supplies usable GPU/display/device integration; wholesale replacement is a different project | Demonstrated fundamental unsolvable requirement plus new scoped hardware/driver plan |
| Host Xorg or nested compositor as final Game Mode | Does not meet standalone Gamescope→DC goal | Separate Desktop Mode purpose, not relabeling nested output |
| Proprietary NVIDIA direct GLX in current Xwayland | No server NV-GLX path/direct context; wire indirect request refused; real context capability issues | Different qualified frontend/server/provider; not visual/isDirect/getenv spoof |
| +iglx/force-direct/extension advertisements | Altering a declaration does not establish actual driver semantics | Real measured compatible context path with truthfully reported capabilities |
| Generic GBM/DRI3 acceleration on this tegra-udrm stack | Node/device exists but required BO/GEM_CREATE contract absent | Actual allocation/import/lifetime proof from changed provider/backend |
| Remove Gamescope extension checks/downgrade stock release | Requirements are executable renderer/import paths, not only list strings | Deliberate source path replacement with capability-correct behavior |
| Mesa26 Zink unchanged | Required dynamic rendering unavailable on L4T1.2.131 | Real backport/revision supporting exact target feature set |
| X11 Vulkan WSI after headless ICD init | Native caps NULL-call path; functional downstream Wayland route available | Different measured compatible WSI backend, no caps/identity spoof |
| Zink Steam UI production | Render-core/T1/T2 passed; T3 actual Steam initialization failed; no compatible real-process semantics proof | Fresh compliant source/process design and game/UI acceptance under that scope |
| Venus/vtest out-of-process helper | Selected pinned transport rejects OPAQUE HOST3D FD and mmap/coherency unqualified | Correct full memory transport/coherency implementation, not OPAQUE whitelist edit |
| llvmpipe “optimization until usable720p” | Functional fallback known, CPU OSR cost dominant; no open-ended profiling sprint | New order-of-magnitude justified renderer/OSR design, not more thread attribution |
| CEF native Wayland for main Steam UI | Steam uses windowless OSR, not a native CEF window | An actual compatible shared-texture/OSR consumer, not standalone EGL-window success |
| Hardware BrowserComposerGL migration | Proprietary GLX unavailable; System Composer already reduces CPU; Viz is rate limiter | New supported composer backend that addresses actual bottleneck |
| Steam Gamescope Composer mode3 | Selector works, but1×1 stream churn produces consumer BAD_STATE; no automatic safe production choice | Consumer lifetime/state transition repair and genuine visible acceptance |
| Simple disable-gpu/software output flags | Real measured path change gives negligible throughput gain | Different upstream architecture/capture implementation with measured useful effect |
| CEF generic GBM accelerated OSR | Contract mismatch and missing actual compatible Steam accelerated-paint consumer | Both producer+consumer contract implemented, not “accelerated flag true” |
| Device-scale-factor forced240p inside logical720p | Steam rounds/clamps/recomputes; flags overridden; physical change not user-confirmed | Actual authoritative scale/capture change tested end to end |
| FEZ32-bit/box32 path | Wrong executable selection crashes before window; x64 Wayland Steam route works | Explicit32-bit requirement with new separate translation/runtime evidence |
| Live regex mutation of localconfig/config | Malformed edits lost LaunchOptions/EULA; Steam caches/rewrites live state | Proper structured edit while stopped or normal UI setting, surgical diff+rollback |
| WWH repeated as only Windows polygon | Game/engine startup expensive and consumer fault already reproduced in small D3D11 client | A specific resolved lower-boundary change and explicit next game purpose |
| ForceBypass as rotation fix | Box64 E1 test showed unchanged upside-down/mirror output | Real coordinate/texture transform source evidence and safe variant |
| Offscreen Sarek variant as presenting game renderer | Variant intentionally rejects WSI; only removes absent Win32 extension truthfully | Qualified real display backend and normal Sarek WSIs, not relaxing refusal |
| Thin x64 winex11 driver replacement | File-size diagnosis was wrong; bootstrap→Unix driver works after desk registration | Measured missing code path, not copying a full foreign Wine DLL |
| Driver GLVND/shim symbol shotgun replacement | Matched DISPLAY probes disprove general ABI/container failure | Exact unresolved symbol/provenance and minimal compatible closure proof |
| Global process DISPLAY mutation in multithreaded game | Process-global env race unqualified | Qualified constructor/exec-scoped early init with actual process identity and restore |
| Automatic BAD_STATE retry/ignore or fake ready/release | Conceals broken consumer ownership/state and can corrupt lifetime | Measured state contract justifying a concrete scoped recovery, with first failure preserved |
| CPU framebuffer scanout / fakeDRM | Not chosen production direct-GPU architecture | Separately scoped diagnostic/fallback, never presented as accepted GPU backend |
| zero-copy/PBO/async broad optimization | Copies/lifetime already complex; not current first blocker | Correct stable path and a concrete measured optimization case |
| gamescopectl screenshot in historical build | Actual assertion/SIGABRT; excluded from further runs | Separately repaired and qualified screenshot path, not automatic use |
|30-minute presenter rerun solely for lost final evidence | User accepted120s commission for nearest integration decision; old result remains unverified | A new stability requirement, not bookkeeping-driven rerun |

Repository publication does not reopen these branches. The source packages
include documentation and selected experimental patches with their status,
not a promise to deploy every historical candidate.
