# Blocker ledger and corrected conclusions

Status is scoped to an exact route/version and evidence level. **RESOLVED** does
not mean every adjacent component works. **OPEN** is an observed boundary or an
unqualified requirement. **RETRACTED** identifies an incorrect earlier diagnosis.
**CLOSED ROUTE** is an engineering decision for the examined stack, not a universal
impossibility theorem. **PARKED** is intentionally unimplemented/deferred work.

This ledger preserves the hard lessons without publishing driver dumps, raw
logs, private process environments or account data. Report references identify
the underlying development record; curated hashes are in `sources/reports.json`.

## Boot, ABI, display and transport

| ID | Boundary / earlier claim | Resolution and current status | Key sources |
|---|---|---|---|
| B01 | Different SteamOS build/image assumed to be installed Alpha | **RESOLVED identity.** Actual lower SquashFS hash matched installed Alpha; later mutable upper tracked separately | BASELINE; installed-rootfs-identity; M0e |
| B02 | New systemd minimum kernel assumption; QEMU treated as kernel proof | **CORRECTED.**257 differs from258; legacy cgroup boot context matters; QEMU/user-mode not real PID1 proof | Alpha handoff; baseline |
| B03 | Default vulkaninfo SIGILL taken as NVIDIA failure | **RESOLVED.** Wrong Freedreno ICD; explicit NVIDIA works. Loader version1.4 does not upgrade device1.2.131 | M0e; M2a |
| B04 | A modern Gamescope DRM backend would work with only output replacement | **CLOSED assumption.** DMA_BUF import, null descriptors/dynamic rendering are actual renderer paths; real compatibility work required | M6a2/M6a3 |
| B05 | FLIP4 ENXIO after Xorg stop | **RESOLVED.** Display head disabled; standalone head enable with preserved mode/vblank, not GOB corruption | M3c–e |
| B06 | Two vblanks/POLLIN alone imply reusable buffer | **REJECTED.** Own fence-info aggregate/points must be validated; ERR/HUP/NVAL cannot be successful retirement | M1c; M3f–h |
| B07 | Modern SYNC_IOC_FILE_INFO ENOTTY | **RESOLVED for tested kernel.** Legacy staging sync-fence-info fallback only on ENOTTY, checked chain/status | M3g2; M3h |
| B08 | Waiting on current front buffer; PUT_WINDOW before close | **RESOLVED design.** Retirement after successor/owned close; maintain ownership until close | DECISIONS; M3e–h |
| B09 | First120s draw looked healthy but cleanup failed | **RESOLVED.** Opt-in whole-FB bookkeeping budget corrected; repeat120s probe+guard+restore passed | M3k3; M3l; M3m0 |
| B10 |30-minute presenter was PASS | **RETRACTED.** Last partial observations healthy, final RAM evidence lost; PARTIAL/UNVERIFIED. Gate later reduced, not magically passed | M3o/M3o2 |
| B11 | loop1 I/O errors prove GPU failure/media failure or harmlessness | **OPEN historical scope.** Controlled zeroout lines support rejected DISCARD/software-zero fallback; general media/opcode health not established | M5b correction; M5e |
| B12 | Exact kernel source recovered from matching config/build date | **OPEN.** Boot payload/config match strongly; exact build149 commit unknown. Candidate kernel not exact-source proof | M4b/M4c |
| B13 | Exported OPAQUE_FD is interchangeable with generic DMA_BUF/CPU mmap | **REJECTED generalization.** Exact same-driver allocation/export/import/copy primitive qualified; other contracts independent | M2a; M7e; helper review |
| B14 | Unsafe original IPC teardown/dedicated handling | **RESOLVED design.** Exact metadata, ownership, fd consumption, finite waits/ack, unknown GPU state preserves allocations | M6b2 |
| B15 | Native Gamescope shader/compiler crash and static resource teardown | **RESOLVED bounded paths.** Supported renderer slice/LUT resource handling; free resources before live device destruction | M7b–d |
| B16 | Exporting cursor texture as Tegra client buffer | **RESOLVED.** Cursor kept non-exportable; unsupported path stopped | M7f2j3; accepted041a270 |

## Xwayland, GLX, Mesa and Steam bootstrap

| ID | Boundary / earlier claim | Resolution and current status | Key sources |
|---|---|---|---|
| B17 | Xwayland libGBM mismatch and premature Mesa EGL selection | **RESOLVED pairing.** Private GBM and child-only NVIDIA EGL vendor contract | M7f2j2; M7g runs20–22 |
| B18 | Cross-built XKB paths baked into sysroot | **RESOLVED.** Correct target paths and real keymap readiness, no fake readiness | M7g XKB diagnosis/run22 |
| B19 | Desktop GL lacks external texture function needed by EGLStream consumer | **RESOLVED path.** Real GLES external-texture consumer; not desktop-extension spoof | M7g runs17–19 |
| B20 | Producer surface created before consumer connect | **RESOLVED.** Consumer-first stream order; first EGL error saved rather than double-read | M7g run18/19 |
| B21 | glBufferSubData directly into external memory-object storage fails | **RESOLVED path.** Ordinary source buffer + actual GPU copy into external buffer | M7g run15/16 |
| B22 | Incomplete surfaceless default FBO/capture | **RESOLVED.** Complete RGBA8 capture FBO; UUID queried as byte array; known-ready fence handling | M7g runs23–31 |
| B23 | Red/blue or composed window counted as Steam | **RETRACTED.** Unknown/Zenity Alpha window; require app/XID/frame identity and human content | M7g run2/3 attribution |
| B24 | Proprietary NVIDIA direct GLX works if visual/vendor override succeeds | **CLOSED ROUTE.** API direct1 emitted indirect wire0; Xwayland indirect branch rejects. EGLStream server success is not NV-GLX client support | RunG; GLX snapshot/reassessment |
| B25 | Force-direct/visual/isDirect/extension spoof repairs semantics | **REJECTED.** No compliant measured solution; closed-driver private GLX research retired | M7g GLX review corrections |
| B26 | Stock Mesa on A57, even without JIT | **RESOLVED private fallback.** Stock contains ARMv8.1 LSE/casal/swp; A57-safe Mesa payload and provider qualification | Mesa build/probes; M7z |
| B27 | llvmpipe crash caused by missing LLVM/rebuild needed | **RESOLVED.** Host generated llvm-config.h initialized X86, target LLVM already had AArch64; config-only repair | llvmpipe7/8;0d59e56 |
| B28 | Steam blank output is stride/alpha corruption | **RESOLVED actual depth mismatch.** Actual drawable depth32 vs visual depth24; Mesa depthfix2 changes xDepth to geometry | depthfix2/unit8 |
| B29 | Preflight expected different libgcc/provider; SONAME/version node sufficient | **RESOLVED closure.** Actual Steam child runtime provider and exact name@version edges qualified | runtime-libgcc-qual;closurev4 |
| B30 | Autostart Gamescope finds build-time Xwayland path | **RESOLVED wiring.** Exact parent WLR_XWAYLAND/XWL ELF/private GBM exported before compositor exec |8034522;c7db135;569f3c9 |
| B31 | Missing audio card/UCM inferred from package database | **RETRACTED.** UCM files/card already present; one user audio stack/endpoint needed |58bfc05;full-stack-fixes |
| B32 | PipeWire real sink exists, therefore Pulse/Steam audio works | **RESOLVED export.** Spatial chain/access rule blocking links/export fixed; real playback and reported sound | full-stack-fixes |

## UI performance and renderer branches

| ID | Boundary / earlier claim | Resolution and current status | Key sources |
|---|---|---|---|
| B33 | Mesa26 Zink on Vulkan1.2.131 | **CLOSED VERSION.** Required dynamic rendering absent; Mesa24.2.8 separately supports render-core gates | Zink feasibility/renderzink1 |
| B34 | T1/T2 Zink imply Steam compatibility | **REJECTED extrapolation.** Render-only/T1 transport/T2 GLX window real; T3 actual Steam stopped at DISPLAY-conditioned enumeration | T1/T2/T3 |
| B35 | DISPLAY discriminator never measured | **RETRACTED.** zdisc2 already proved unset/:0/unset contrast; sentinel99 is unchanged storage, not device count |7040a25;ba59658 |
| B36 | No compliant Steam in-process recipe proves universal helper necessity | **CORRECTED.** No qualifying recipe found under constraints; not all future architectures impossible | client-init-decision review |
| B37 | Venus/vtest OPAQUE export means complete helper route | **PARKED exact NO-GO.** Pinned vtest rejects HOST3D OPAQUE blob before send_fd; mmap/coherency unqualified | helper-host-gate-revision |
| B38 | CEF ANGLE eglGetError not called, or initialize never reached | **RESOLVED instrument blindness.** Raw function table bypassed proxy; wrong GLVND instance handle/calls; coherent binding fixes real EGL init | M7q/M7r |
| B39 | GBM render node/gbm_create_device proves usable Chromium buffer transport | **CLOSED ROUTE.** tegra-udrm required BO/GEM_CREATE not usable; libGBM symbol alone insufficient | M7s–u2;M8o |
| B40 | Main Steam UI is a native Wayland CEF window | **RETRACTED.** Windowless OSR→CPU OnPaint→BrowserComposer; standalone Wayland EGL success separate | M8/M8a |
| B41 | Composer selector does not work | **RESOLVED.** Wrong log inspected; real parent→webhelper argv selects System/Gamescope modes | M8h5/M8i |
| B42 | llvmpipe BrowserComposer is sole720p throughput limiter | **RETRACTED.** System Composer reduces CPU but rate stays~4/s; OSR/Viz hot stage scales with pixels | M8h5;M8l–p |
| B43 | disable-gpu/software-output/few flags solve OSR cost | **CLOSED measured route.** Architecture changed, throughput~+1.6% noise; other cheap flags no useful gain | M8n/M8p |
| B44 | display-scale CLI can force logical720p and physical240p freely | **CLOSED examined mechanism.** Client recalculates/clamps manual DSF; CLI overridden; low-resolution presets practical | M8s-render-scale |
| B45 | UI resolution must limit all game resolutions | **RESOLVED alpha native route.** UI screen override separates426×240 from1280×720 game logical output; windowed FEZ/SMB user accepted | M8t user checkpoint |

## Proton launch, FEX and shared WSI

| ID | Boundary / earlier claim | Resolution and current status | Key sources |
|---|---|---|---|
| B46 | M9A Box64 run4 GPU-hang | **RETRACTED.** Network/brcmfmac incident; ShowInterstitials before CreatingProcess, no compat-tool/game invocation | M9B; recovery audit |
| B47 | Waiting launch is EULA/shader processing | **RESOLVED gate.** ControllerConfigurator queue/SmallText inference; real Steam UI response later passes | M9B/M9C |
| B48 | Container modern ABI alone blocks NVIDIA Vulkan | **RETRACTED.** Host/container probes differed by DISPLAY; same DISPLAY-conditioned ICD behavior | M9C→M9D |
| B49 | Arbitrary DISPLAY wrapper is concurrency safe | **OPEN until qualification.**26–27 game threads made v2 unsafe/unqualified; constructor early qualification addressed scoped Box64 route | M9E/M9G |
| B50 | X11 caps path works after headless NVIDIA enumeration | **CLOSED ROUTE.** Actual NULL call inside EGLcore caps; FROG downstream Wayland conversion is real alternative | M9E/M9F |
| B51 | FROG missing handshake needs compositor rewrite | **RETRACTED.** Real gate env absent; server ID/property/protocol already available; process env and loader filters matter | M9F/M9L11 |
| B52 | FROG synth failure proves layer path fails | **RESOLVED wiring.** Synthetic ctor removed gate; synth-specific keep env corrected; layer proof before caps/mapping | M9H preparation/t3 |
| B53 | WWH pause is wineserver init-sync deadlock | **RETRACTED exact stage.** Graphics exception→exit3→termination wait; unresolved cleanup handles not same as initial graphics wait | M9E |
| B54 | Frames imply proven launcher/main-game identity | **UNKNOWN.** User saw no image in original M9G; later launcher attribution withdrawn | M9G/M9H + user correction |
| B55 | Unity-specific consumer timeout | **RETRACTED exclusivity.** Minimal x64 D3D11/DXVK/FROG reproduces similar timeout; lower consumer boundary implicated | M9I |
| B56 | FEZ/SMB stopped due to graphics experiments | **RESOLVED different regression.** Malformed VDF regex removed LaunchOptions; native Wayland/x64 selection recipes needed restoration | M9J |
| B57 | FEX11.0-2 downloaded means ARM-FEX upgraded | **RETRACTED.** app4628710 was x86-64 PC Proton; target ARM-FEX11.0-1 unchanged | M9L/M9L2 |
| B58 | inherited SteamAppId itself blocks every FEX target | **RETRACTED/narrowed.** SteamGameId manual forwarding case; full real GUI Steam launch works; console headless conhost is separate | M9L2→M9L3 |
| B59 | FEX target exit means automatic launch-chain cleanup | **OPEN.** Shim/helper/wineserver remain; GL-thread/headless path and parked launcher measured; forced cleanup not natural exit | M9L4;later fixture units |
| B60 | Wine desktop readiness | **RESOLVED fixture route.** Shim creates desktop but main thread parked; helper owns desktop+pumps WM_NULL in same Wine session | M9L5/M9L6 |
| B61 | FEX Vulkan instance requires unavailable Win32 extension in headless offscreen test | **RESOLVED offscreen only.** Private Sarek excludes exactly absent Win32 surface, refuses WSI, real GPU readback passes | M9L8/M9L9 |
| B62 | VkResult−7 means out-of-device-memory | **RETRACTED.**−7 is VK_ERROR_EXTENSION_NOT_PRESENT;−2 is OUT_OF_DEVICE_MEMORY; raw evidence unchanged | M9L9 erratum |
| B63 | Tiny x64 winex11 PE means stub incapable of windows | **RETRACTED.** Thin Wine11 bootstrap is legitimate; missing desktop display GUID/GraphicsDriver registration prevents dispatch | M9L10→M9L11 |
| B64 | Setting Win32 DISPLAY changes native getenv | **REJECTED.** Native libc environment differs; scoped native early init/restore required | M9L11/M9L12 |
| B65 | FEX/PV Wayland queries−3 mean unsupported Wayland backend | **RESOLVED closure.** Delayed bare-name libnvvulkan-producer unavailable in PV search; target exact retry/path restores support/caps | M9L12 |

## Consumer failure, actual current blockers and diagnostic integrity

| ID | Boundary / earlier claim | Resolution and current status | Key sources |
|---|---|---|---|
| B66 | Consumer timeout/SJob lifetime | **RESOLVED PC candidate.** Queue kept pointer to dead stack job; false completion/UAR/UAF reproduced; shared ownership/retirement fix passes | M9L14/M9L15 |
| B67 | TEARDOWN timeout followed by record/slot free | **RESOLVED PC candidate.** Deferred GL cleanup, owner pins/finalization; strict actual-TU tests/ASan | M9L15 |
| B68 | Lifetime fix proves cause/cures original0x321c | **REJECTED.** No job timeout in M9L13; M9L16 new-stream connect still fails. BAD_STATE cause not established | M9L14/M9L16 |
| B69 | M9L13 failed at~18s in MAILBOX | **CORRECTED.** FIFO actual; last second~20s; subsecond teardown ordering originally unknown | M9L14 correction |
| B70 | M9L15 first full ELF faithful to accepted | **RETRACTED artifact.** Linked wrong M8g flat/focus base; NOT FOR DEPLOY. Corrected bit-exact control/narrow release qualified | M9L15R |
| B71 | Late Xwayland fallback during visible-window Vulkan destroy | **SUPPORTED controlled mitigation.** Unmap ACK before teardown removes late gen4 in unit05/06, finalizes gen3; not all closed games/old faults proven same cause | M9L17 unmap units05/06 |
| B72 | M9L17 private FEX selector/helper closure | **OPEN actual qualification.** Stock-backed ntdll realpath can redirect child loader; private native anchor copied; detailed predicates and V8 metadata candidate prepared | private-fex progress;overnight |
| B73 | Trace17 gives FEX game/ICD result | **REJECTED.** rc11 consumer Release during startup before SteamAction; bootstrap/refusal/natural lifecycle NOT MEASURED | trace17 unit-result |
| B74 | Startup1280×800→426×240 Release0x321c | **OPEN current first hardware boundary.** Probe and accepted recovery repeat signature; exact gen/state-before-Release/producer causation unknown | trace17 disposition;M9L18 PC prep |
| B75 | Planned session stop timeout/SIGKILL is clean exit | **OPEN lifecycle.** Multiple stops exceed SIGTERM30s; SIGKILL explicitly recorded; causal relation to subsequent Release unknown | unit03/05/06;trace17 |
| B76 | Full SHM or low disk explains every consumer fault | **UNPROVEN causality.** Resources can cause separate SIGBUS/launch pressure; trace17 passed reserves with same fault | M9J;storage-pressure;trace17 |
| B77 | Any missing remote response is GPU hang | **REJECTED.** WiFi scan/auth/link incidents occurred independently; actual nvgpu fault requires fresh kernel evidence | M3o;M9B;M9H |
| B78 | Screenshot method is harmless | **CLOSED command.** gamescopectl screenshot triggers allocation/assert/SIGABRT in examined build; XWD not physical-screen oracle | M9H;M8b |
| B79 | Target PID/maps captured from pv-wrap establish game provider | **RETRACTED.** Target must be actual Wine preloader/PE machine with pid+starttime; fresh capture needed even for fast exits | M9L5–8 |
| B80 | inherited/stale logs/STOPs can qualify current run | **RESOLVED harness boundaries.** Fresh run/token/cursors/target and stale-artifact isolation; no reuse of old proof | M9L16/M9L17 |
| B81 | Generic FATAL/nvgpu word matcher means new consumer/GPU fault | **RESOLVED matcher.** Component-qualified faults; nonfatal xkb message/WiFi Modules line excluded honestly | trace07;M9L11 |
| B82 | SUMMARY with errors0 proves final PASS | **RESOLVED acceptance.** Actual rc, full interval, coherent counts, CLEANUP OK then final result, independent exit | M9H r5;M9L4R onward |
| B83 | No default sink requires reinstalling all ALSA/UCM | **REJECTED shotgun fix.** Exact server/access/export boundary repaired; existing hardware/UCM retained | full-stack-fixes |
| B84 | OpenCode one-step/one-run limits are hardware safety | **POLICY CORRECTED.** No artificial iteration limit; related recoverable wiring tests continue in scope. Real consumer/GPU/kernel/link STOP still applies | user instructions12Oct |
| B85 | Native Wayland focus metadata patch is a complete accepted input fix | **OPEN precise scope.** M8g interrupted: real SMB AppID observed, experimental geometry broken, full focus transition/virtual-pad/exclusivity not qualified. Later general game acceptance is separate | M8g checkpoint;user M8t acceptance |

## What remains open for actual FEX game readiness

The current actual blocker is B74. After it is addressed, B72 still requires
real qualified private helper/target identity and bootstrap evidence; B59 and
B75 retain natural-lifecycle/shutdown limitations. Ordinary game draw/content,
input, stable orientation, playable performance and clean exit are separate
acceptance stages. No amount of PC source tests or repeated summary counters
substitutes for those stages.

Other historical open issues — exact kernel build source, universal storage
health, every-model support, complete image reproducibility, HDR/VRR/docking,
all physical touch transitions and32-bit Windows support — are not claimed
closed because a narrower alpha goal passed.
