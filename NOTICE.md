# Credits, origin and publication scope

This port builds on substantial existing work. The project does not claim to
have invented the Switch Linux hardware support, Steam ARM64 translation stack,
Vulkan compositor or Tegra block-linear display mechanism.

- **Valve and Gamescope contributors** — upstream compositor, renderer,
  Xwayland integration, swapchain/content-override protocol and SteamOS/Steam.
  Gamescope base: `6867f509874f9bc52e12d6f4c4596cdf0d5be6b4`.
  [ValveSoftware/gamescope](https://github.com/ValveSoftware/gamescope).
- **theofficialgman** — direct Tegra display/GOB/FLIP4 reference, with the
  existing author notices of **gavin_darkglider** and other contributors
  retained in the source publications. Reference:
  `ecfb76aadbdc93cd7008e77bec944724cd70c364`.
  [vk_layer_tegra_dc_present](https://github.com/theofficialgman/vk_layer_tegra_dc_present).
- **SildurFX / Switchdeck**, **Ivy**, Switchdeck-Extras and contributors —
  Cortex-A57 Steam client/runtime recipes, FEX/Proton packaging, Box64 and
  DXVK-Sarek/VKD3D recipes. Ivy is credited by Switchdeck for the original
  steam-arm64 download work. Historical reference:
  `be0578fbf00c5a14a988a830f914f2c34cdaa1a1`.
  [Switchdeck](https://github.com/SildurFX/Switchdeck) and
  [Switchdeck-Extras](https://github.com/SildurFX/Switchdeck-Extras).
  Switchdeck's own GPL licensing is preserved in any source package containing
  derived material; this repository does not copy or relicense its scripts.
- **Switchroot and its contributors**, including **CTCaer**,
  **theofficialgman**, **Gavin_Darkglider** and the wider Linux/Hekate community —
  L4T kernel/BSP, boot support, hardware integration, drivers and sleep hooks
  that made the tested platform possible.
  [Switchroot Linux documentation](https://wiki.switchroot.org/wiki/linux) and
  [Hekate](https://github.com/CTCaer/hekate).
  The exact source commit of the tested kernel build remains unknown; crediting
  Switchroot is not a claim of exact reconstructed kernel provenance.
- **NVIDIA** — L4T graphics stack, Tegra display/nvhost UAPI and MIT-licensed
  EGLStream Wayland protocols. The source releases preserve actual file
  notices; proprietary driver payloads are outside this documentation license.
- **Mesa, LLVM, Wine, Proton, FEX, Box64, DXVK-Sarek, VKD3D, Wayland, Xwayland,
  wlroots and their contributors** — the renderer, compatibility, translation,
  protocol and window-system building blocks discussed here.
- **Google / Android staging sync contributors** and **OE4T/Linux Tegra
  maintainers** — legacy fence UAPI/reference source used in retirement
  analysis. This project adapted those definitions and did not author them.
- **SteamOS ARM community references**, including SteamOS-ARM-Handhelds and
  SteamOS-ARM-SM8550 — early bring-up comparisons. Their layouts and hardware
  assumptions were not taken as proof for Switch.

Original engineering work here includes the direct Gamescope integration,
private OPAQUE_FD buffer protocol, EGLStream consumer/bridge, geometry and
compatibility adaptations, source/build qualification, synthetic tests,
readiness/ownership/recovery instrumentation and this curated research record.
The source repositories identify derivative origins at file/component level.

The record is written from reports and direct user checkpoints through
12 October 2026. Published dates are historical experiment dates, not a claim
that this repository re-ran the tests. Source-only preparation and publication
did not run additional hardware tests or lift the development STOP.
