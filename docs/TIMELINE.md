# Milestone timeline

All outcomes below belong to their recorded runs. Dates are historical; no
hardware reruns were made while preparing this publication. A sequence of
successful component tests is not an unmeasured combined-system PASS.

## Before native Gamescope

| Stage | Implementation / result | Limit |
|---|---|---|
| Boot0 diagnostics | Switchroot boot and NVIDIA compute readback; later physical display observation | Earlier NVIDIA32.3.1/Vulkan1.1.85 stack; not the later installed Alpha |
| ARM SteamOS bring-up | Official userland restoration, metadata/ELF checks, controlled staging and ext4 image packaging | QEMU/user-mode checks are not Switch kernel/GPU/Steam acceptance |
| Alpha1 | Hekate→Switchroot/L4T→SteamOS0.3, SDDM/Xorg/Openbox/Steam-style session | Initially host Xorg and Zenity menu, no native Gamescope backend |
| M0 | Installed lower/kernel/driver inventory; per-process NVIDIA ICD selection resolves wrong Freedreno SIGILL | No presenter implementation yet |
| M1 | gman GOB/FLIP4, kernel retirement and recovery design reviewed/pinned | Two-vblank sleep not accepted as retirement proof |
| M2a |16 OPAQUE_FD buffer export cycles, fd stability, actual capability query | No GPU draw/scanout proof |
| M2b3 | Exclusive display-test wrapper and inert recovery rehearsal | Restored visible UI confirmed; not GPU-hang-proof recovery |
| M2c | Controlled A57 sysroot/cross-builder; target vs native pkg-config separation | Private source/build inputs, not a distributable full target runtime |

## Direct presenter and Gamescope: 29 September–2 October

| Stage | Result | Limit / next boundary |
|---|---|---|
| M3a–M3d | GPU GOB presenter; first FLIP4 refused with disabled display head; explicit head enable added | Head control required, not a reason to retain Xorg |
| M3e | Four direct FLIP4 frames, user-confirmed clean pattern | No reuse/soak/Game Mode |
| M3f–M3h | Legacy sync-fence retirement;120 submissions/117 retirements+reuse, clean return | Modern ioctl ENOTTY handled explicitly; no sleep/POLLERR shortcut |
| M3i–M3j |600 then1800 frame bounded tests; validated ordering and finite resource telemetry | Cadence/submission counts not panel FPS or universal leak freedom |
| M3k–M3m0 | First120s commission failed cleanup bookkeeping; scoped budget fixed; repeat120s passed | First attempt not retroactively PASS |
| M3m–M3o |30-minute run launched; last observed counters healthy; final RAM evidence lost after network loss/reboot | PARTIAL/UNVERIFIED; later120s gate accepted for integration, no30min PASS |
| M4 | Actual boot/kernel/config identity compared with retained Alpha artifacts | Exact build149 source commit unknown |
| M5 | ext4 unwritten-zeroout/loop-discard fallback studied with deterministic readback | Controlled lines explained conditionally; general media health unknown |
| M6a | Renderer feature/path audit corrected; unsupported DMA-BUF/null-descriptor assumptions removed | A display backend alone cannot fix renderer capabilities |
| M6b–M6c | OPAQUE buffer+semaphore IPC design and pristine AArch64 Gamescope build | Initially PC qualification only |
| M7b/M7c | Native NVIDIA renderer; shader-resource compatibility corrections; three LCD frames | Early static teardown fault kept separate from frame success |
| M7d | Correct resource destruction before live VkDevice loss | Targeted no-display clean bootstrap test, not new frame acceptance |
| M7e | Cross-process4KiB GPU roundtrip,1024 words exact, app+child rc0 | Primitive buffer/sync proof, not image/WSI/game |
| M7f series | Private Wayland buffers/slots imported;12 generations, user-confirmed colors | Scanout reuse/discard coverage incremental |
| M7f2j | Private libgbm/Xwayland pairing; cursor export fix | Stock Mesa software branch still A57-incompatible |
| M7g run16 | GL buffer GPU-copy→OPAQUE_FD→Vulkan exact readback | No EGLStream yet |
| M7g run19 | Consumer-first GLES EGLStream→GL→OPAQUE→Vulkan, two exact colors | Standalone diagnostic |
| M7g run25 | Attributed XID→Xwayland EGLStream→consumer stub, exact red/blue | Special test ACK, not normal Steam |
| M7g runs26–31 | Normal consumer: XDG, bind, FBO, UUID and fence gates repaired;15 composite/flips | Run31 PARTIAL, client status/frame identity incomplete |

## Steam compatibility and UI: 2–7 October

| Stage | Result | Interpretation |
|---|---|---|
| Steam GM runs1–3 | Seed/staging/exec flags fixed; proprietary NVIDIA GLX reached visual then BadValue | Apparent Steam-window acceptance retracted: Zenity/unknown window had been composed |
| Host-Xorg control | Staged ARM Steam/CEF worked under existing NVIDIA Xorg | Seed/runtime control, not standalone Game Mode |
| GLX diagnostics | API direct1 emitted wire isDirect0; indirect server branch refused; capability snapshot | Proprietary direct GLX route closed for this stack |
| Private Mesa/A57 | Softpipe built without LSE; real GL/readback/swap | Functional software client, not GPU |
| llvmpipe probe7/8 | Generated LLVM config wrongly initialized X86; corrected AArch64 initialization, real renderer PASS | Existing target LLVM reused, no LLVM rebuild required |
| depthfix2 | Actual drawable depth used rather than visual depth; MIT-SHM BadMatch removed | Small client-local correctness fix |
| llvmpipe functional11 | Visible Steam UI, bounded profile | FUNCTIONAL PASS / PERFORMANCE FAIL / FROZEN FALLBACK |
| stretch1/o180sx1 | Geometry and mirror/rotation accepted physically | Preserve as independent baseline contract |
| Zink render-core/T1/T2 | Zink24.2.8 GPU draw; custom transport; real GLX swaps, machine+human PASS | Test clients only, no Steam qualification |
| Zink T3 | Real Steam loads private GLX; DISPLAY-conditioned NVIDIA enumeration fails before swaps | No Steam GPU frames/FPS; branch retired |
| Zink helper design | Venus/vtest feasibility narrowed; OPAQUE HOST3D blob transport rejected by pinned vtest | Design only; no helper implementation accepted |
| CPU autostart | Installer rollback on missing Xwayland parent env; corrected launcher; boot+~40s UI confirmed after real reboot | Host Xorg/Openbox/Zenity removed from Game Mode startup |
| Device integration | Single audio endpoint, access-rule fix; touch/volume helpers; suspend policy/polkit | Per-feature physical acceptance limits retained |
| M7h–M7r | ANGLE EGL resolver/GLVND instance mismatch isolated, symbol binding repaired | Real NVIDIA CEF EGL/GLES context proven |
| M7s–M7u | tegra-udrm/libGBM examined | GEM_CREATE/usable BO contract missing; GBM/DRI3 route closed |
| M7v/M7w | Actual Xwayland EGLStream and native Wayland EGL window presentation | Server/client EGL presentation works independently of proprietary GLX |
| M7x–M7z | Pressure-vessel helper SIGILL traced to stock Mesa LSE; SHM pressure tracked | A57-safe provider closure still necessary |
| M8/M8a–c | Main UI is windowless OSR; hybrid NVIDIA CEF + software BrowserComposer works visibly | OnPaint CPU output remains; extra GPU activity not automatically UI FPS |
| M8f/M8g | Native Wayland game ownership/focus/input investigated, metadata patch partly exercised | M8g interrupted, full exclusivity OPEN; unaccepted build must not leak into accepted renderer |
| M8h–M8i | Real parent selector; System Composer human PASS and promoted launcher default | Less CPU;720p throughput still low; mapping libraries do not prove active GL composition |
| M8j | Resolution-transition regression audited | Rebuild/relink was not proven root cause |
| M8l–M8p | CEF/Viz OSR pixel-scaling bottleneck localized; disabling GPU no material throughput gain | Cheap flags, software-output replacement and naive threading closed |
| M8q–M8s | SMB golden control; FEZ x64 selection/runtime; real Steam launch visible | Native Linux games, not Proton/FEX |
| M8s scale work | Steam DSF clamped/overwritten;240p practical baseline chosen | Cannot force arbitrary low internal pixels with one extra CLI flag |
| M8t | Xwayland UI-size override and larger logical game output; FEZ/SMB user acceptance in windowed mode | Independent resolution alpha goal achieved; arbitrary fullscreen transitions unqualified |

## Windows games / shared Vulkan boundary: 7–9 October

| Stage / historical commit | Result | Current interpretation |
|---|---|---|
| M9A `54c124f` | We Were Here FEX stalled; Box64 setup/launch work; apparent hang on run4 | Later M9B retracts GPU-hang diagnosis: network failure and prelaunch gate |
| M9B `96ab475` | ControllerConfigurator ShowInterstitials isolated; exact launch proof planned | Not EULA/shader processing; no actual Proton invocation for old run4 |
| M9C `3fdc73d` | Real Box64/FEX launches; Unity and DXVK reached in Box64; FEX stall | Initial container-ABI conclusion later corrected by DISPLAY-matched probes |
| M9D `6b977ea` | Load+create headless NVIDIA init works in host and containers; GPU fill/readback PASS | Process-global toggle in real game still needed concurrency qualification |
| M9E `f05ff08` | Apparent sync stop was termination hang after X11 caps NULL crash | X11 WSI headless backend missing; FEX nodrv attribution still provisional |
| M9F `9acb549` | Native Wayland Vulkan presentation; FROG X11→Wayland surface proof | No Gamescope-side handshake rewrite required |
| M9G | Early single-thread ICD initialization; real FROG game conversion and frames | Consumer timeout+nvgpu channel timeout, STOP; launcher/game-content identity not visually established |
| M9H | Native240s and corrected FROG synth240s tests; strict lifetime/cleanup acceptance | Original t1 command-buffer lifetime defect corrected; t3 is the strict PASS |
| M9I | Minimal x64 D3D11/DXVK/FROG reproduces consumer frame timeout after~96s | Repro is not Unity-specific; no new nvgpu fault that run |
| M9J | LaunchOptions loss from malformed VDF editing proven; restore native-game recipes; verified archive cleanup | No GPU-stack regression required to explain SMB/FEZ missing options |
| M9K / M9K2 | Unturned preliminary test; HL2DM Windows x64 D3D9Ex reaches menu | Poor cadence and upside-down/mirrored visual output; bypass flag not orientation lever |
| M9L | Consumer fence timeout following HL2DM stop; session recovered | FEX ladder not started in this block; teardown race shared-boundary evidence |

## FEX qualification ladder and consumer hardening: 9–12 October

| Stage | Result | Remaining limit |
|---|---|---|
| M9L2 | ARM Wine and x64 console; in-process AppID SteamAPI PASS | Initial inherited-SteamAppId diagnosis later narrowed/corrected |
| M9L3 | Full Steam GUI target+SteamAPI PASS; SteamGameId manual shim path understood | Console headless conhost differs; manual incomplete env is not normal Steam failure |
| M9L4/R | Genuine FEX tool identity; GUI target exits but shim/helper chain remains300s; reviewed offscreen package | Natural lifecycle is genuinely unresolved |
| M9L5 | Offscreen D3D11 blocked in Wine desktop WM_NULL readiness | Native target maps were initially captured from wrong pv-wrap; later corrected |
| M9L6 | Desktop owner/message-pump helper fixes readiness | Headless null driver sufficient for handshake, not all graphics WSI |
| M9L7 | Readiness PASS; D3D11 instance creation fails | First precise Vulkan extension boundary next |
| M9L8 | Native NVIDIA works; Wine headless omits Win32 surface; Sarek requests it | `-7` later corrected to EXTENSION_NOT_PRESENT |
| M9L9 | Private offscreen-only Sarek; D3D11 hardware draw+4096-pixel scan+clean target exit PASS | No picture expected, no WSI; full chain forced cleanup remains |
| M9L10 | Visible client refuses: x64 window absent | “Thin winex11 PE is a nonfunctional stub” retracted next stage |
| M9L11 | Desktop display GUID/GraphicsDriver registration fix; real x64 winex11 window and FROG conversion | Wayland caps return−3 inside ARM runtime |
| M9L12 | Delayed bare-name NVIDIA producer dlopen gap fixed; host/runtime/Wine surface queries match | Swapchain/mapping not yet tested there |
| M9L13 | Create-only PASS;20s visible x64/FEX Vulkan human PASS | Consumer BAD_STATE at end;240s not run |
| M9L14 | Latent queue UAR/UAF and teardown UAF reproduced on PC; fixes/logging | Not proven cause of original EGL BAD_STATE |
| M9L15/R | Lifetime/retirement/owner pins; independent review; narrow release with bit-exact accepted control | First linked candidate wrong build base, explicitly NOT FOR DEPLOY; release corrected |
| M9L16 |20s fixed candidate visible; client exits cleanly | Late new stream connect0x321c; fix did not eliminate transition failure |
| M9L17 units05/06 | Hidden-before-teardown ACK;20s and240s raw-FEX Vulkan machine+human PASS; generation finalizes | Forced Wine-chain cleanup and planned compositor SIGKILL still separate; no game PASS |
| M9L17 private HL2DM | Genuine private FEX tool action; helper selector refusal; private native ntdll anchor and constructor diagnostics prepared | Later V8 metadata-init candidate not actually qualified; game GPU not established |
| Trace16 / trace17 R3 | Startup/recovery Release0x321c; trace17 rc11 before game action | Shared consumer startup resize is current first hardware blocker |
| M9L18 preparation | PC-only startup capture wiring using frozen lifetime-fixed candidate | No hardware result claimed; existing STOP retained |

The full historical report identity inventory is in `sources/reports.json`.
Short commit IDs above identify the private development record, not commits of
this documentation repository. User-provided M8t and hardware verdicts are
explicit inputs where a dedicated report identity is unavailable.
