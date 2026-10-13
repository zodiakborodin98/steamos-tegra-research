# Architecture and the boundaries between components

**Current update 13 October 2026:** [Steam UI now works on Zink/NVIDIA](ZINK-STEAM-UI.md),
with a persistent 480p profile and CPU rollback. Optional diagnostics are off by
request; post-change verdict pending. The following record through 12 October is
historical and does not override the new route-specific UI evidence. Game/consumer
stability, zero-copy, fresh image and boot qualification remain separate.

## Tested platform and boot

The early installed baseline combined an official ARM64 SteamOS/Frame userland
with a Switchroot/L4T hardware layer. The observed console used Tegra X1 rev B,
four Cortex-A57 cores (ARMv8.0), NVIDIA L4T 32.7.4 and Linux 4.9.140-l4t build
149. Its physical framebuffer was portrait 720×1280, 32-bit, pitch2880.

The boot chain was Hekate L4T-loader → ATF/U-Boot → kernel/initramfs/device tree →
read-only SquashFS lower + writable upper → systemd. Later storage/staging work
expanded development capacity. A hash of the immutable lower does not prove
that the mutable upper remained unchanged. Results belong to the tested console,
not all Switch Lite/V2/OLED panels, docked modes, HDR, VRR or multiple outputs.

The BSP/kernel was reused. A candidate source tree and a matching embedded
configuration establish lineage and configuration identity, but not the exact
source revision of build149. Switchroot is an essential upstream, not a kernel
reimplementation by this project.

## Direct display output

```text
Gamescope Vulkan composition on NVIDIA
  → GPU compute conversion: linear RGBA to Tegra GOB/block-linear
  → persistent exported scanout buffers
  → completed GPU fence and explicit external ownership
  → legacy Tegra DC FLIP4
  → LCD
```

This avoids a DRM Atomic backend and host Xorg for scanout. `FBIOBLANK` is used
to control head state; it is not a CPU framebuffer pixel-copy presentation path.
Direct-display layout and ioctl work are based on gman's reference.

Three persistent scanout slots are reused only after validated own-post-fence
retirement. Legacy staging sync-fence info is required on this kernel: modern
sync-file ioctl returns ENOTTY. Success cannot be inferred from two vblanks,
POLLERR/POLLHUP or a sleep. The final displayed buffer may retire only after a
successor or owned DC close; waiting for the front buffer itself can deadlock.

Accepted geometry uses o180sx1: the portrait panel and producer/texture transform
require the recorded rotation and source-X compensation. It is not a universal
orientation rule for every client. FROG/direct override Windows paths initially
showed different geometry; correct raw-FEX fixture orientation was separately
observed, while Box64 game geometry was not universally fixed.

## Client-frame transport

Old NVIDIA L4T does not provide the modern DMA_BUF_EXT/GBM/DRI3 contract that
ordinary Gamescope expects. Exporting a scanout buffer and importing an arbitrary
client DMA-BUF are different operations.

```text
X11 client → internal EGLStream-capable Xwayland ─┐
                                                ├→ NVIDIA EGLStream producer
Native Wayland EGL/Vulkan client ────────────────┘
   → GLES consumer acquire / capture / GPU copy
   → private Vulkan OPAQUE_FD buffer + ready/returned semaphores
   → Gamescope-owned Vulkan texture
   → composition / GOB / FLIP4
```

A separate custom Wayland buffer protocol carries fixed metadata and slot
identity. Accepted OPAQUE_FD transport is same-NVIDIA-stack interoperability, not
a claim that every exported opaque FD can be CPU-mapped, passed to any compositor
or used as an arbitrary VkImage. A 4KiB roundtrip is a primitive proof; later
client/window/texture tests add distinct coverage.

The implementation intentionally includes copies. Calling it zero-copy would be
false. Memory ownership, imported FD consumption, generations, ready/returned
events, slot retirement, backpressure and lifetime must remain coherent.

## Steam UI history

The working CPU-compatible path was:

```text
Steam / CEF windowless OSR
 → CPU BGRA OnPaint
 → BrowserComposerGL / private A57 Mesa / llvmpipe
 → SDL X11 / internal Xwayland
 → EGLStream / NVIDIA Gamescope / LCD
```

CEF itself was subsequently demonstrated using real NVIDIA EGL/ANGLE. This did
not remove CPU OSR capture: Steam's main UI used windowless CEF, shared textures
were disabled, and the active consumer was `OnPaint`, not an implemented
`OnAcceleratedPaint` path. A successful surfaceless CEF renderer does not imply
a GPU-native UI transport.

System Composer was then selected through the real parent route
`-system-composer` → webhelper mode1. It reduced CPU cost and removed the active
llvmpipe BrowserComposerGL stage; private Mesa libraries remained mapped for
compatibility. At720p the UI throughput remained low. M8l–M8p identified the
CEF/Viz CPU-facing OSR work as the principal scaling bottleneck, not scanout or
the presence of llvmpipe libraries in maps.

Low-resolution UI is the practical alpha path. M8t allowed a1280×720 logical
game output while Xwayland reported426×240 to the Steam UI. Native games could
use independent larger surfaces; user-tested windowed mode was sufficient.

## Native Linux games

```text
Steam AppID action
 → reaper / SteamLinuxRuntime soldier / scout
 → Box64 x86-64 translation + native-wrapped dependencies
 → game (SDL/FNA/OpenGL)
 → native Wayland / EGLStream
 → Gamescope / LCD
```

Super Meat Boy and FEZ have human-visible/gameplay acceptance in this route.
FEZ's launcher chose its32-bit executable on aarch64; an external helper inside
the genuine Steam launch chain selected the x86-64 executable while preserving
the runtime/AppID/focus context. Launching FEZ manually outside that context
could render textures and play sound without displacing Steam UI.

Native Wayland windows need exclusive input/focus arbitration and Steam-visible
AppID metadata. Duplicating input to both UI and game is a failure, not a fix.

## Windows Box64 and ARM-FEX

Box64 uses x86-64 Wine/Proton with native-wrapped host interfaces. ARM-FEX uses
native AArch64 Wine, WoW64/ARM64EC translation and PE↔Unix thunks. Their native
loader, desktop registration, bridge and runtime closure are not interchangeable.
They can reuse the lower presentation path once they reach it.

```text
Steam action → private tool / pressure-vessel runtime
 → Wine / x64 PE under Box64 or ARM-FEX
 → D3D through DXVK-Sarek or raw Vulkan
 → Wine Win32 surface
 → FROG Gamescope WSI layer converts X11 to downstream Wayland surface
 → NVIDIA Vulkan Wayland/EGLStream
 → accepted consumer / compositor / LCD
```

The FROG gate requires `GAMESCOPE_WAYLAND_DISPLAY` in the actual process and a
usable socket, plus real layer activation. `GAMESCOPE_WSI_FORCE_BYPASS=1` is a
ForceBypass hint in the examined layer, not a universal layer-disable switch.
It does not override all prerequisites, for example explicit `_WINE_ALLOW_FLIP=0`.

L4T's ICD loads an X-conditioned path when DISPLAY points at internal Xwayland
without NV-GLX. Headless initialization permits NVIDIA enumeration, but native
X11 WSI can then call a missing backend and crash. Wayland WSI is a real working
alternative; it is not emulated by lying about surface support.

Early NVIDIA initialization is scoped to qualified processes at constructor
time, with DISPLAY restored and no later application-time mutations. A temporary
global DISPLAY toggle in a26-thread game was not accepted as safe just because
it happened to work. Unix and Win32 environment changes are not the same: a Wine
`RtlSetEnvironmentVariable` does not rewrite native libc getenv state.

ARM runtime also needs a loadable `libnvvulkan-producer.so` for delayed Wayland
WSI setup. Native ICD compute/enumeration can pass without that producer, while
surface-support/caps fail. A target-scoped exact-name retry/pinned path fixed
that particular closure gap.

## Offscreen FEX proof

Headless Wine null-driver mapped Win32 surface support only when an ICD supplied
`VK_EXT_headless_surface`; this L4T ICD does not. DXVK-Sarek unconditionally asked
for Win32 WSI even in a surface-free D3D11 test. A private opt-in DXVK variant
removes exactly that unavailable extension after real enumeration and rejects
all surface/swapchain entry to its presenter. It proves offscreen GPU rendering,
not presentation. It must not be silently used as a game renderer recipe.

## Current fault boundary

The consumer has both confirmed latent software-lifetime defects and observed
EGL state failures. Lifetime fixes use queue-owned jobs, retirement guards,
owner pins and owner-thread finalization. PC UAR/UAF tests passed on the fix.
That does not establish the cause of every `EGL_BAD_STATE_KHR` event.

M9L17 unit05/unit06 qualified controlled unmap-before-teardown on the fixed
candidate for a raw-Vulkan FEX fixture. The genuine-game selector branch still
had separate private-loader/metadata initialization boundaries. Trace17 later
stopped before that branch on Steam startup resize. Fixing the consumer/state
transition is therefore the current shared prerequisite, not another blind FEX
flag combination.
