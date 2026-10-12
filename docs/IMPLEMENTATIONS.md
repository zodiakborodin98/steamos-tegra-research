# Implementations and acceptance boundaries

## Display and renderer

| Component | What was implemented | Evidence / limitation |
|---|---|---|
| A57 cross environment | Curated target sysroot, explicit ARMv8-A flags, target pkg-config, native scanner/shader tools separation | ELF/provider/ISA qualification; QEMU not target acceptance |
| Direct presenter | Vulkan buffer storage, GPU GOB transform, Tegra DC window/FLIP4, head enable/restore, triple-buffer reuse | First-frame physical PASS; validated legacy retirement; bounded120s commission |
| Retirement | Modern ioctl failure falls back only to checked legacy fence-info; no ERR/HUP success or timing-only reuse | Target120/600/1800 and120s tests; not universal kernel proof |
| Gamescope Tegra backend | Standalone output backend and first-frame/live renderer integration | NVIDIA compositor/LCD proven; no DRM Atomic substitute |
| Renderer feature compatibility | RGB-only route, supported feature chain, real dummy descriptors, LUT specialization key, limited pipeline creation | Avoids pretending unavailable robustness/dynamic rendering/DMA-BUF features exist |
| GOB shader/orientation | Exact preferred GLSL and generated SPIRV; accepted o180sx1 presenter/first-frame path | Rotation/mirror baseline physically accepted |
| Private OPAQUE transport | Slots, exact dimensions/stride/format/device identity, exported memory/ready/returned semaphores, copy+release | Real interprocess GPU readback and client-window tests; no arbitrary DMA-BUF import |
| Native Wayland clients | Private protocol and EGL/Vulkan EGLStream production | Actual LCD delivery; primitive and full-window qualifications distinct |
| EGLStream consumer | GLES external texture acquire/capture, GPU-copy bridge into Vulkan-owned image, per-record lifecycle | Stream state/resize/teardown failures remain open |
| Xwayland | Pinned EGLStream-capable build with correct XKB runtime paths, private GBM/provider wiring | Actual internal Xwayland NVIDIA EGLStream proven |
| Content override | Existing Gamescope swapchain protocol reused; XID/server/window lifecycle tied to alternate surface | Preserves X11 input/focus while submitting Wayland content; not itself VkSwapchain |

The accepted deployment ELF is a frozen historical build, not the current dirty
upstream checkout. It required accepted TU snapshots and the retained orientation
objects/GLSL. Source publication reconstructs these inputs; a fresh successful
build is compilation evidence and must not be mislabeled historical hardware
binary identity.

## Steam user session and device integration

| Component | Implementation | Acceptance boundary |
|---|---|---|
| Steam seed/runtime | Preserved A57-compatible ARM client/runtime and full actual library tail | Genuine executable/provenance needed; cwd seed differs from active content root |
| Private Mesa | ARMv8.0 software GLX with explicit provider closure and zero-LSE payload checks | Softpipe then llvmpipe functional; not system-wide replacement |
| LLVM native init | Generated header X86→AArch64 correction while retaining target LLVM | Real llvmpipe context/draw/readback PASS |
| Mesa depthfix2 | XImage actual drawable depth rather than selected GLX visual depth | MIT-SHM BadMatch resolved without alpha/visual spoof |
| Automatic UI service | Additive systemd system unit, child-specific software GL env, exact parent Xwayland env, display on/off prep, rollback | Activation and reboot/user UI PASS; no restart loop claim |
| Alpha replacement | SDDM/autologin path disabled for Game Mode, exclusive owned-scope handling | Internal Xwayland stays; host Xorg/Openbox/Zenity not needed for Game Mode |
| System Composer | Real parent selection and rollback; default promoted | Human functional PASS, less CPU; active GL composer removed, libraries remain mapped |
| CEF EGL | Resolver/interposition + coherent GLVND dispatch enabling real NVIDIA ANGLE contexts | CPU OnPaint OSR output remains; UI not thereby zero-copy |
| UI presets |720p/480p/360p/240p variants and measured OSR scaling | Lower pixel count improves responsiveness; presents are not visible FPS |
| UI/game geometry | Xwayland426×240 UI override with logical game1280×720 output | User accepted native games with selectable resolution in windowed mode |
| Audio | One user PipeWire/WirePlumber/Pulse stack; UCM retained; access-rule linking/export corrected | Actual playback and reported human sound; no competing per-session audio stack |
| Volume | evdev volume keys adjust actual Pulse default sink | Command75→80→75; general user button confirmation; per-boot IDs need discovery |
| Touch | evdev contact/position→XTest pointer/button with current startup size/180° transform | Single-pointer helper; not full multitouch or independently complete drag/corner acceptance |
| Gamepad/gyro | Existing hardware/Steam input used without broad replacement | User confirmation; native game focus must be exclusive |
| Native game focus | XDG windows in Gamescope arbitration with Steam-visible PID/AppID metadata, experimental M8g patch | M8g interrupted: natural game AppID seen, complete exclusive focus/input transition not qualified; later general game acceptance does not close that precise gap |
| Power | logind suspend owner, last-wins drop-in, narrow login1 polkit actions, existing Switchroot hooks | Later user sleep/resume confirmation; client restart lifecycle separate |

## Compatibility tools, games and synthetic proofs

| Implementation | Proven result | Not proven |
|---|---|---|
| FEZ launch helper | Selects x64 executable inside original Steam runtime chain, no game-file edit |32-bit FEZ/box32 compatibility |
| SMB native Wayland | Real Box64 Linux path, visible gameplay; later resolution accepted | All fullscreen/resize cadence regressions eliminated |
| FEX tool inventory | Native ARM-Wine + WoW64/ARM64EC-FEX, real x64 PE proof | A downloaded x86-64 Proton app is an ARM-FEX update |
| Box64 Sarek recipe | D3D11 Unity device and D3D9Ex HL2DM menu reached on NVIDIA | General Windows game compatibility, steady cadence, correct all-game orientation |
| Launch gate response | ShowInterstitials analyzed; genuine Steam UI response passed gate | Durable state from live regex localconfig edits; unconditional virtual-pad effectiveness |
| Early ICD init | Headless real ICD load+initial instance/enumeration at qualified constructor boundary; DISPLAY restored | An arbitrary multithreaded setenv/unsetenv wrapper is safe |
| FROG layer wiring | Actual process gate, socket, layer insertion, exact XID conversion before caps, downstream Wayland surfaces | FORCE_BYPASS alone proves gate/layer activation or fixes orientation |
| FEX desktop owner | Helper in the same Wine session owns desktop and pumps WM_NULL; fresh readiness checks | Full natural shim chain exits after target |
| FEX desk registration | DEVICEMAP→GUID→GraphicsDriver and desktop atom/property enable real thin-PE→Unix winex11 path | Thin PE size is evidence of missing driver implementation |
| Producer closure | Exact-name target-scoped NVIDIA producer lookup/retry | Any device-init success guarantees delayed WSI closure |
| Private offscreen Sarek | Surface extension opt-in filter after true enumeration, WSI refusal; hardware draw/event/readback4096 clean | Same variant can present games; API-counter fully measured |
| FEX Vulkan visible client | Hidden window; gate; real queries/device/swapchain; mapping last; dynamic orientation markers | Shader pattern delivery equals game readiness |
| Controlled teardown | Hide window, independent fresh X-server ACK, then Vulkan cleanup | X ACK is a Wayland/GPU retirement fence; closed games obey that order automatically |
| Private FEX game closure | Separate tool identity, copied native mutable/runtime anchor files, pinned immutable shared edges | Selector/bootstrap actual acceptance automatically follows PC tests |

## Consumer lifetime hardening

Confirmed latent defects were repaired in a separate candidate rather than
rewriting the accepted payload:

- Queue owns jobs with shared lifetime. Timeout cannot leave a pointer to a
  returned stack object or falsely complete a subsequent job occupying it.
- Retirement and submission share a lock/barrier; no new FRAME/CONSUME is
  accepted after generation teardown begins.
- Owner pins protect submission, completed copies and release through owner-side
  fetch/finalization; worker performs only GL/EGL cleanup.
- wlserver/owner-side finalization runs after pins are gone; record and transport
  slot are not freed while queued/active teardown still uses them.
- Late frames are quarantined honestly. Started GPU work cannot be canceled by
  declaring a timeout. Acquire bounds and refused APIs fail closed.
- Exact generation, surface/client/buffer, stream state and primary EGL errors,
  slow jobs and teardown/finalize events are logged with bounded output.

The merged real source TU had52 plain tests, fail-closed subprocess controls and
clean ASan in M9L15. Independent build-provenance review uncovered a wrong
M8g-linked base; the corrected M9L15R control linked byte-for-byte to the accepted
ELF, and the release differed only in the consumer and log-hook objects.

PC ownership PASS is not a cure for all0x321c. M9L16 still failed on a late stream
connect. Controlled unmap improved unit05/06 and finalized the fixture stream,
but startup resize and natural compositor shutdown remain open.

## Evidence, cleanup and safety tooling

These are engineering implementations, not guarantees against a kernel hang:

- Exclusive units/scopes, PID+starttime actor ownership, finite deadlines and
  fresh run namespaces; owned TERM/grace/KILL without broad process killing.
- Independent journal/kernel watchers covering setup, launch, teardown,
  rollback and post-grace; sticky first-fault STOP.
- Separate app/adapter/wrapper/watch/restore/archive/transport statuses; partial
  relay bytes kept and remote state UNKNOWN after transport loss.
- Current Steam→CEF-link→viewable UI identity rather than stale PID or an exact
  English title; UTF-8/Russian UI title qualification and fresh log cursors.
- Fail-closed build artifact acceptance: new temp output, compiler rc, markers
  and hash; stale old ELF cannot pass a failed build.
- Final result requires interval+counts+target exit+cleanup; a SUMMARY before
  failed cleanup or a timeout is not a PASS.
- Archive-to-PC with full hashes and symlink verification before narrow cleanup;
  active payloads, games, saves, current prefixes and rollback anchors protected.
- Stale tmpfs segments removed only after ownership/reference/inode checks;
  disk reserves retained; no automatic capacity-pressure deletion in test runners.
- Source/build provenance control and preferred shader/protocol inputs retained;
  dependency pins and native/target tooling separated for publication.

Known tooling failures were disclosed: stale STOP flags, false generic FATAL
matches, wrong pv-wrap target PID, inherited files, wrong report/log source,
timeout rc suppressed by a runner, compiler marker mismatch, read-only preflight
that was not initially an actual refusal gate, and incomplete kernel ring
interpretation. Fixing these is essential to trustworthy results; it does not
reclassify old unsuccessful attempts as PASS.
