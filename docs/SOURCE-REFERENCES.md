# Source reference inventory

**Current update 13 October 2026:** [Steam UI now works on Zink/NVIDIA](ZINK-STEAM-UI.md),
with a persistent 480p profile and CPU rollback. Optional diagnostics are off by
request; post-change verdict pending. The following record through 12 October is
historical and does not override the new route-specific UI evidence. Game/consumer
stability, zero-copy, fresh image and boot qualification remain separate.

This public repository is an original engineering synthesis, not a dump of the
private development workspace. `sources/reports.json` records a curated set of
documents that informed it, including exact text-byte SHA256 and the last known
historical Git change where available. `sources/inputs.json` records the full
handoff and accumulating baseline/status documents as reference identities.

Logical paths such as `reports/M9L12-wayland-surface-queries-cause-and-fix.md`
are names within the original development record. They are intentionally not
absolute local paths and not links to unpublished raw logs. Historical commit
identifiers refer to that record and do not resolve in this public repository.
A hash identifies a retained document; it does not expose its private content or
turn a local source revision into a publicly fetchable dependency.

## Authority and corrections

Priority for this synthesis:

1. Later direct user observations and explicit correction checkpoints.
2. Latest actual-run reports with independently bound artifacts/results.
3. Specific correction/source analysis over an earlier broad report conclusion.
4. Accumulating CURRENT-STATUS/BASELINE/DECISIONS as maps, with their dated
   entries read in order; old headers and standing tasks are not execution.
5. Plans, build-only packages and proposals as intentions, never as runtime PASS.

The handoff is dated7October and includes earlier superseded states. The newer
M9A–M9L17 reports and user checkpoints correct it. Particularly important are the
M9B incident reclassification, M9D DISPLAY-matched correction, M9L11 thin-driver
correction, M9L14 timeline correction, M9L15R build-base correction and trace17's
startup-only classification.

## Curated source groups

| Source group | Main contribution |
|---|---|
| Alpha/handoff, M0 and BASELINE | Boot/userland/hardware identities and separation from newer untested image |
| M1–M3 | gman-derived presenter, own-fence retirement, real LCD/120 s and unverified30min |
| M4/M5 | Kernel/config provenance and storage explanation with exact limits |
| M6/M7d–f | Capability-correct renderer, native integration and OPAQUE transport |
| M7g | EGLStream/Xwayland/GLX investigation, A57 Mesa/LLVM/depth, Zink tests and helper design |
| CPU autostart/device reports | Actual boot, parent wiring, audio/input/power and lower-resolution UI |
| M7h–M8p | EGL dispatch, real CEF contexts, OSR/composer architecture and performance closures |
| M8q–M8s plus M8t user checkpoint | Native Linux game launch/focus and independent game resolution |
| M9A–M9L | Windows game launch gates, matching host/runtime Vulkan, FROG and consumer faults |
| M9L2–M9L13 | FEX ladder, desktop registration, offscreen D3D11 and visible Vulkan prerequisites |
| M9L14–M9L17 | Consumer lifetime/source fidelity, controlled unmap and private real-game qualification |
| Trace17 disposition/M9L18 preparation | Current first startup-resize boundary and bounded next capture scope |

## User-only evidence explicitly incorporated

- Actual boot reaches Steam UI and later system reload confirmation.
- Buttons/gamepad/gyro; later sleep/resume confirmation.
- FEZ and SMB work with selectable game resolution in windowed mode after M8t.
- Original M9G image was not seen; the later “launcher only” assumption was
  withdrawn and not treated as measured window content.
- Box64 game logos/menu later visible but upside-down/mirrored, slow and unstable.
- M9L13 TOP/LEFT image and orientation visually correct.
- M9L17 unit05 short and unit06 four-minute moving pattern, correct orientation
  and return to Steam.
- Disk warning specifically referred to storage, and disappeared after verified
  cleanup; no inference of consumer causality.
- No artificial OpenCode step/one-run limit; recoverable errors remain inside
  scope, while real safety STOP persists.

This publication omits the conversational transcript, private names, account
identifiers, passwords, network configuration and raw diagnostic payloads.
