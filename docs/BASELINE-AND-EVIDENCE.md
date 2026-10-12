# Baseline identities, reproducibility and limits

## Historical accepted inputs

| Input | Identity / historical role |
|---|---|
| Gamescope upstream base | `6867f509874f9bc52e12d6f4c4596cdf0d5be6b4` |
| Core port source state | `d0a5dbfde9210adf1ec8b42055a9388a10a5c7be`, with accepted TU overrides and orientation inputs |
| Accepted Gamescope ELF | SHA256 `b9f5847fe6378204126ba4beaa1b3c75b2b42831a4e1b91ebb3370d23723d3d8` |
| Lifetime-fixed M9L15R candidate | SHA256 `4b93ac3d60cc754cdec4e0aaf457fd21cb3176b0fbab820e48ae68abb047d43e` |
| Installed original immutable lower | SHA256 `f7ce8399088f7e8b409250f5de7e5022ddaa8a40f3a8289df8db97139a0adff4` |
| Kernel gzip payload identity | SHA256 `d51bd5ad07878b31d71987b8280a7bbc1e8f88d2e70ee34692bb5dd3d08cd405` |
| Embedded/running kernel configuration | SHA256 `9ff883a5a9a720b6a974b890d94df391f2ab60027d93b87d1455668497779c31` |
| gman direct-present reference | `ecfb76aadbdc93cd7008e77bec944724cd70c364` |
| Switchdeck historical reference | `be0578fbf00c5a14a988a830f914f2c34cdaa1a1` |
| Candidate kernel source | `7d95822acda1f6dab3f3d1d099b43ff9e0d0e626`; not exact build149 proof |
| Legacy sync reference | OE4T/Linux-Tegra4.9 `78abfa9d9f4c9bc47c91b64a4f1a02952b67db0e` |
| A57 Mesa software source | Mesa26.0.3; tar SHA256 `ddb7443d328e89aa45b4b6b80f077bf937f099daeca8ba48cabe32aab769e134` |
| Zink historical source | Mesa24.2.8, separate from the26.0.3 llvmpipe source |
| FEX tool tested | Proton11.0-1 ARMv8.0; native ARM Wine11 + WoW64/ARM64EC-FEX |
| DXVK game/offscreen reference | DXVK-Sarek1.13.0; private M9L9 opt-in offscreen variant separately identified |
| Driver/device | L4T32.7.4; Tegra X1 revB; physical Vulkan1.2.131 |

These are evidence identities, not redistributed binary download instructions.
No ELF/kernel/rootfs payload is in this research repository. When an input is
not independently obtainable with its original rights/notices, its hash does
not make it an open-source artifact.

## The accepted source is not one live checkout

Historical accepted binaries were assembled with pinned snapshots and retained
objects. Later upstream-checkout HEAD included unaccepted M8g flat/focus changes.
M9L15's first candidate accidentally linked that later build base; it was marked
NOT FOR DEPLOY. M9L15R restored the accepted objects and reproduced the accepted
control byte-for-byte, then linked only the new consumer and log-hook objects.

For a source release, preferred shader GLSL and protocol XML must accompany
generated SPIRV/C/headers. Build scripts and target/native toolchain separation
are part of the recipe. The source export includes those materials; an old
object hash and link line alone are insufficient corresponding source.

Fresh source build success proves compilation and linkage of that exported
recipe. It does not automatically reproduce the exact ELF bytes or inherit the
historical physical-screen acceptance. Runtime closure, deployment, controller,
hardware and user results must be identified separately.

## What “baseline preserved” means

Development tests retained immutable accepted payload/manifest/autostart and
separate rollback anchors; runtime helpers/overlays were additive. It does not
mean the entire original user partition stayed bitwise pristine, or that every
future boot was tested. Accepted lower hash, current mutable drop-ins, Steam
content/root, runtime library choices and active tools are separate identities.

The baseline was recovered to a visible active Steam session after recorded
faults. That is recovery availability, not a blanket stability PASS. Repeated
planned stop-timeouts/SIGKILL and startup Release failures still exist.

## Evidence levels

1. **Source/static**: code path, symbol/ABI/ISA, extension requirements, metadata
   or artifact construction. It cannot establish actual target execution.
2. **PC test/build**: actual changed code, mocks for external boundaries,
   ASan, shader reproduction, clean build/link. Mocked GPU APIs are not target
   GPU proof; Windows WARP/RTX proof is not NVIDIA Tegra proof.
3. **Hardware primitive**: exact target allocation, compute, export/import,
   readback, real context or single stage. Neighboring stages remain unknown.
4. **Hardware integration**: current run identity, actual app/tool, exact window,
   surface/layer/texture/presentation and lifecycle evidence. API rc0 alone
   still does not prove physical content.
5. **Human physical observation**: user sees moving image, correct orientation,
   content/focus/input and return to UI. Describes the observed run, not all
   runtimes/games/resolutions.
6. **Performance/game acceptance**: actual unique game updates/frame timings and
   acceptable gameplay measured for the chosen scenario. Compositor presents
   cannot stand in for this.

`RESULT PASS` is meaningful only with actual process rc, complete interval,
coherent counts and successful cleanup. Independent Wine self-exit correlation
must bind the actual native PID/starttime to the target wine thread. A helper
summary, a timeout rc124, debugger rc0, launcher rc0, or “unit active” is a
different observation.

## FPS, cadence and stalls

The compositor can repeat a previously accepted texture and render both UI and
game. Its presents/s are not game FPS. Different counters may refer to new
client commits, imported buffers, composed layers, FLIP4 submissions or physical
updates; use their exact names. Historical SMB1:1 game/import/compose/present
correlation is a scenario-specific rate, not a portable universal FPS counter.

Even successful unit05/unit06 contained multi-second completed waits/stalls.
Zero errors does not imply smooth physical cadence. A user sees meaningful
dynamic content, but synthetic marker changes and a few minutes do not establish
playable closed-game performance.

## Process and fault integrity

- New run IDs, fresh cursors, current PID+starttime, actual PE/machine and window
  ownership are required. Historical XIDs/PIDs and inherited prefix logs are not
  current-run provenance.
- Exact child LD closure and runtime tail matter. SONAME, mapped filename or
  broad version-node match alone is insufficient; delayed `dlopen` is separate.
- Actual faults remain STOP. Recoverable build/path/env/instrumentation errors
  can be repaired in the same scoped sprint with new IDs and retained evidence;
  no artificial one-test model limit is imposed.
- Fault patterns must be component/time qualified: a WiFi warning listing the
  nvgpu module is not a GPU fault, and “xkb errors are not fatal” is not FATAL.
- Kernel ring eviction can make whole-ring equality fail; overlapping lines and
  current interval must be analyzed without rewriting an UNKNOWN runtime result.
- SSH loss stops operator target work. No reconnect loop or extra remote cleanup
  is implied; partial bytes and unknown remote state are retained. Network loss,
  GPU fault, OOM, consumer fault and planned sleep are distinct classifications.

## Resource and screenshot limitations

Disk shortage and full SHM occurred repeatedly. They can break UI/startup
independently of GPU. Unreferenced stale segments and archived diagnostic data
were cleaned only after ownership/hash/link verification; active actors and
rollback anchors were preserved. Resource gates do not prove historical absence
of transient pressure, nor prove that every BAD_STATE was caused by pressure.

The tested `gamescopectl screenshot` path crashed Gamescope; it is not part of
safe graphics qualification. XWD/XGetImage may disagree with the actual LCD and
cannot replace the user's final visual verdict.

## Model-wide and image-wide limits

There is no completed universal matrix for all Lite/V2/OLED panels, docking,
HDR/VRR, multiple displays, all refresh rates, Windows32-bit applications,
long unattended soak, full Desktop Mode transitions or every future boot.
Image recipes are source preparation; only an actually installed/booted image
with specific manifests and user acceptance can claim installation PASS.

Changing kernel/global providers/boot/partitions while repairing the consumer
would remove the controlled baseline. The publication itself performed no such
changes or new target tests.
