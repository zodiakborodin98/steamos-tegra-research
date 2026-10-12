# Next engineering work

## Current first boundary: startup EGLStream state

Trace17 R3 was a real unit with rc11, a sticky first-fault STOP and intact remote
transport. It stopped after probe-session activation and before SteamAction.
Bootstrap, target refusal, FEX device/graphics and natural game lifecycle were
not measured. The same Release0x321c signature occurred in accepted recovery.

The narrow new task is a startup-only capture around UI1280×800→426×240 using
the existing qualified lifetime-fixed/instrumented candidate. It does not need
another FEX game or driver/backend migration to distinguish this boundary.

The capture must bind generation/stream, dimensions, client/surface/buffer,
window and available ownership events. Preserve the first Release return/error
before QueryStream or later calls. Post-failure query state cannot establish
the state before failure; Xwayland's Wayland client PID may not be the renderer.

Possible outcomes to distinguish:

- Real old-stream producer disconnect/retirement before consumer Release.
- Incorrect acquire/release or lifetime ownership at resize/unmap.
- A different stream's error incorrectly attributed by coarse logs.
- Insufficient observability: exact missing field stated, not guessed.
- No reproduction on the fixed variant: useful bounded outcome, not proof of
  original cause or a baseline repair.

M9L18 preparation contains startup-only wiring, fresh capacity/collision gates,
protected overlay manifests, current watchers and bounded verified restoration.
It is PC preparation at this snapshot. A posted run card or checked build does
not lift the hardware STOP. A newer result must replace this status with its
actual stage, fault/rc and evidence.

## After the state boundary is understood

1. Choose a narrow correction based on the observed contract. Do not start with
   BAD_STATE ignore/retry, fake release, global pacing/geometry workarounds or
   GL finish. Keep initial failure evidence.
2. Exercise startup resize, normal small/large surfaces, unmap/destroy and
   consumer generations through finalization. Use distinct IDs, finite deadlines
   and actual-code tests. Repaired wiring errors can be retested in scope;
   actual consumer/GPU/kernel/link STOP ends hardware work pending decision.
3. Qualify natural compositor shutdown separately from forced systemd SIGKILL.
   Stop-timeout conditioning may affect later startup, but the relationship is
   currently unknown and must not be silently assumed.
4. Return to the private ARM-FEX game tool with actual native-loader anchor,
   helper/target identity and constructor-boundary bootstrap. Trace-only target
   refusal is not a game run; evidence must show the current intended process.
5. Run a real non-Unity Windows x64 game through Steam once these prerequisites
   hold; separate device/draw/window/content/interaction/cadence/clean-exit/user
   outcomes. Preserve the proven raw-Vulkan fixture as a control.
6. Address shim/helper/wineserver natural lifecycle and real game teardown.
   Controlled fixture unmap is not automatically available in a closed game.

## Keep working controls

- Accepted standalone NVIDIA Gamescope/LCD baseline and its source/build pins.
- Low-resolution Steam System Composer UI; parent and child GL environment kept
  separate; active composition must not be inferred from maps alone.
- Linux x64 Wayland SMB and FEZ launch options/runtime/focus. Changes to game
  compatibility mapping must not destroy unrelated VDF entries.
- Offscreen FEX D3D11 exact readback and raw-FEX visible20s/240s controls, with
  their private variant/lifecycle limits.
- Audio, existing input, sleep/resume, autostart and rollback anchors.

## Longer-term product work

Reproducible fresh image creation, source provenance of kernel/BSP, deterministic
dependency acquisition, all-model/panel validation, independent LCD API extraction,
resource stewardship, native input/multitouch, complete power/client lifecycle,
game/performance compatibility matrix and Desktop Mode are product work after the
current consumer boundary. They are not implied complete by publication of code.

UI performance work stays parked unless specifically reprioritized. Its last
measured bottleneck is CPU CEF/Viz OSR cost, and a720p GPU-native capture consumer
would be a new architecture, not a collection of the already-failed flags.
