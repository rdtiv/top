# Appendix: source index

All sources cited by `01-exec-briefing.md` and `02-technical-roadmap.md`. Read dates are 2026-09-15 unless noted. Repository anchors are `path:line` at the stated commit.

## Repositories read locally

| Repository | Commit / version | Location | Notes |
|---|---|---|---|
| omacom/try-omarchy | `58cbac5` (main, 2026-09-15) for the 2026-09-16 anchors; `7f3ce66` (upstream/main, 2026-09-22, unreleased) for everything marked "main" | local checkout | v0.4.1 tagged 2026-09-15; 29 PRs (31 commits) merged to main since v0.4.1 by 13 authors, as of 2026-09-22 |
| omacom/omarchy | `e48f8382` (quattro, v4.0.0-358, 2026-09-12) | local checkout | Tags to v4.0.3 present; v4.0.4 released 2026-09-15 upstream |

## try-omarchy file anchors used

| Anchor | What it establishes |
|---|---|
| `README.md:28` | "Video decoding is CPU-only ... An improved video path is in development." |
| `README.md:41-43` | Render patch source tap unmaintained since 2026-01-14; patch vendored |
| `README.md:129-137` | KosmicKrisp not in the path; unverified items after the QEMU 11.1.1 port |
| `README.md:224-231` | Bridged mode on Wi-Fi enables temporary host-wide DHCP handling that can affect other virtualization apps |
| `README.md:364-374` | Requirements: Apple silicon, macOS 15+; nested virt on M3+ with macOS 26+ |
| `README.md:408-415` | In-guest updater advances ordinary packages only; kernel, runtime, backports pinned; reset is the way to a new factory |
| `docs/architecture.md:39-48` | Nested-virt probe on macOS 26; macOS 15 stays EL1 |
| `docs/architecture.md:56-64` | Host-sleep pause/resume over QMP |
| `docs/architecture.md:84-112` | Touch ID for sudo: Secure Enclave, sudo-only, not login or unlock |
| `docs/architecture.md:114-122` | Shared folder: virtio-9p, `security_model=none`, uid/gid 1000 remap patch, `/mnt/mac` |
| `docs/architecture.md:175-186` | Hyprland rebuilt with rounded-border patch; aquamarine 0.14.0-2 and hyprtoolkit rebuilt and held on IgnorePkg |
| `docs/architecture.md:186-193` | `pre-refresh-pacman` hook restores aarch64 pacman files after a channel refresh writes x86_64 templates |
| `docs/architecture.md:192-225` | Paired boot kits; new app never rebases an existing rootfs; schema-2 recovery boot |
| `docs/architecture.md:263-272` | Two channels: app releases and guest updates; factory reset is the way to opt into a new factory; an in-guest migration channel is explicitly not designed yet |
| `guest/README.md:31-36` | Hyprland is the one source-patched guest package |
| `guest/pinned-packages/README.md` | aquamarine and hyprtoolkit ABI pin recipes, held together with Hyprland |
| `guest/packages.txt:73` | `vulkan-swrast` is the only Vulkan ICD in the guest |
| `guest/spec.json:24-33` | Pinned Omarchy commit `0534987...`, release 4.0.3, channel quattro |
| `guest/spec.json:239-466` | Thirteen reviewed backports, including aarch64-refusal patches for x86-only installers |
| `guest/fragments/pre-refresh-pacman-restore-arm.sh` | The restore hook itself |
| `macos/run-qemu-gpu.sh:21` | `virt,accel=hvf,gic-version=3` |
| `macos/run-qemu-gpu.sh:1471-1500` | Nested-virt capability probe; macOS 15 exclusion referencing issue #211 |
| `macos/run-qemu-gpu.sh:1523-1582` | The QEMU command line: devices, `-cpu host,pmu=off`, direct kernel boot |
| `macos/run-qemu-gpu.sh:1559` | `-device virtio-balloon-pci` (no `free-page-reporting`) |
| `macos/build-qemu-gpu-runtime.sh:61-65, 501-522` | QEMU 11.1.1 commit; HVF-only configure (`--disable-tcg`) |
| `macos/build-qemu-gpu-runtime.sh:107-132` | Pinned bottles from the unmaintained startergo taps |
| `.github/workflows/ci.yml:26-28` | Guest image cannot be built on GitHub-hosted macOS runners |
| `CONTRIBUTING.md` | "one product target: a native Apple Silicon macOS app that runs pinned upstream Omarchy in a project-built ARM64 virtual machine image" |

## try-omarchy on main at 7f3ce66 (read 2026-09-22)

| Anchor | What it establishes |
|---|---|
| `docs/integration-updates.md` | Integration manager: read-only 9p share at `/mnt/try-omarchy-updates`, hash-verified bundles, backups, resumable install, status port; boundary quote: "does not replace the kernel, upgrade the graphics stack, repair package holds, install 1Password integration, or reproduce every change in a newer factory image" |
| `integrations/DESIGN.md` | Design note; initial payload sudo Touch ID; kernel and graphics package replacement excluded |
| `docs/memory-reclamation.md` | `virtio-balloon-pci,free-page-reporting=on` plus pinned HVF reclaim patch (unmap, demand-zero remap, acknowledge); asynchronous; needs runtime update and VM restart, not a new disk |
| `docs/app-updates.md` | Version display and opt-in release checks (#234); Sparkle 2 recommended for installation; prerequisites listed |
| `docs/architecture.md` (diff vs 58cbac5) | aquamarine 0.15.1-1 / libaquamarine.so=14 against Hyprland 0.56.2; battery port `dev.tryomarchy.battery`; macOS 15 on EL1, VirGL source-built for 15.0; Traditional Chinese kernel argument; disk capacity |
| `macos/build-qemu-gpu-runtime.sh` | VirGL 1.3.0 from gitlab.freedesktop.org source with startergo tap v1.0.42 patches; ANGLE 1.0.16 and libepoxy 1.0.5 still from bottles |
| `macos/Sources/OmarchyVMHelper/VMApplicationController.swift` (`shutDownForSettings`) | `system_powerdown` over QMP is used by the restart-for-settings path; app quit still forwards SIGTERM |
| `README.md` (main) | Start automatically; Try Omarchy Settings; Shut down to manage…; Maximum disk size 64 GiB; Ghostty; stable bridged MAC; Touch ID for 1Password; clock recovery; memory reclamation; Traditional Chinese |

## omarchy file anchors used

| Anchor | What it establishes |
|---|---|
| `manual/44-mac-support.md:1-5` | Intel Macs only; M-series not directly supported; Omarchy must be the only OS |
| `manual/47-system-snapshots.md` | Snapshots are Snapper plus Limine; Limine-only |
| `manual/36-system-sleep.md` | Hibernation needs a RAM-sized swap subvolume and Limine |
| `manual/28-windows-vm.md` | Windows is Dockur in Docker on KVM: 64 GB floor, no GPU passthrough, RDP, `~/Windows` share |
| `manual/17-ai.md:5-19` | Thirteen agent CLIs shipped as mise stubs |
| `docs/update-process.md:63-98` | Raw pacman guard; `OMARCHY_UPDATE_PACMAN=1`; `OMARCHY_ALLOW_DIRECT_PACMAN=1` |
| `docs/update-process.md:109-147` | `omarchy update` pipeline; snapper snapshot; `omarchy-migrate` |
| `install/hardware/vulkan.sh` | Vulkan driver chosen by `lspci` vendor string: Intel, AMD, Apple only |
| `install/hardware/all.sh` | Hardware fix-up chain, all x86 or Asahi |
| `plans/backup.md`, `plans/dots.md`, `plans/remote.md`, `plans/server.md` | Upstream plans; none mention VMs or Apple silicon |

## Conversation

| Source | What it establishes |
|---|---|
| X (Twitter) DM exchange between Dan and Eduardo, September 2026 | Eduardo: "Being a daily driver and much more than 'Try' is definitely where we are going, it provides benefits the native version can't match, like the Apple Ecosystem. I want to structure a public roadmap so everyone can contribute in the direction we see Try Omarchy going." |

## Field evidence

| File | What it establishes |
|---|---|
| `the author's field write-up on the audio bridge (unpublished)` (2026-09-11) | Audio bridge leaks PipeWire remap modules across restarts; 284 restarts; pipewire-pulse at 1021/1024 descriptors; 1 Hz retry forever; script owned by no package |
| `the author's field write-up on fcitx5 (unpublished)` | fcitx5 restart loop: 333,398 restarts, 2.0 M journal lines; fix landed upstream as `e48f8382`; Try carried a downstream drop-in (#91, #106) |

## Issues and pull requests

Verbatim maintainer and contributor quotes appear in `02-technical-roadmap.md` rows. Read 2026-09-15/16 with the GitHub CLI.

| Ref | Title | State |
|---|---|---|
| try-omarchy #81 / PR #82 | Feature request and PR: configurable guest memory choice in the start menu (rdtiv) | merged 2026-09-09 |
| try-omarchy #62 | Move to QEMU 11.1.1 and Apple's in-hypervisor GIC (caius72) | merged 2026-09-01, v0.3.0 |
| try-omarchy #167 / PR #168 | Host-backed hardware video decoding / native hardware video decoding, HDR, audio continuity | open |
| try-omarchy #92 | Filesystem-consistent VM snapshots | open |
| try-omarchy #146 | Suspend support / pause | open |
| try-omarchy #145 | Multiple instances | open |
| try-omarchy #151, #213 | M4 crashes (EXC_GUARD in hv_vm_unmap; M4 Pro) | open |
| try-omarchy #133, #191 | Touch ID / Keychain; passkeys and Bluetooth | open |
| try-omarchy #183 | Start menu does not appear after shutdown | open |
| try-omarchy #108, #114, #118, #122, #127, #128 | Omarchy Link series | closed |
| try-omarchy #57, #137, #18 | ARM package availability threads | see roadmap |
| try-omarchy #215, #216 | v0.4.1 fixes (macOS 15 EL1 path; theme and DNS actions) | merged 2026-09-15 |
| try-omarchy PR #101 | Pause the VM across macOS host sleep (removed the manual Cocoa Pause/Resume items) | merged 2026-09-04 |
| try-omarchy PR #143 | Add opt-in Touch ID authentication for guest sudo | merged (v0.4.0) |
| try-omarchy PR #138 | Clarify aarch64 Install failures instead of "target not found" | merged |
| try-omarchy #91 / PR #106 | Downstream fcitx5 restart-loop drop-in | merged 2026-09-04 |
| try-omarchy PR #154 | Safe growth for existing VM disks | merged (v0.4.0) |
| try-omarchy #211 | macOS 15 nested-virt probe abort (HV_BAD_ARGUMENT), referenced by the launcher | see `macos/run-qemu-gpu.sh:1471-1474` |
| try-omarchy PR #141 | Disk size at initial setup | open |
| try-omarchy #189, #68 | Guest build memory on 16 GB Macs; macOS minimum mismatch and from-source build blockers | open |
| omarchy #8645, #8530, #9576, #8897 | Upstream aarch64 threads: x86-only install menu entries; Voxtype; 1Password pin; man-db/Chromium SIGTRAP | open |
| try-omarchy PR #193 | Discover and update sudo integrations in existing VMs (Fail-Safe): the integration manager | merged 2026-09-21 |
| try-omarchy PR #243 | Return unused guest RAM to macOS (themartiano); 728.3 / 742.3 / 718.3 MiB returned per 768 MiB freed on macOS 27.0, 0 with reporting off; 29 PRs and 31 commits since v0.4.1 from 13 authors | merged 2026-09-22 |
| try-omarchy PR #152 | Automatic startup, in-guest Try Omarchy Settings, Restart Try Omarchy… (drdator) | merged 2026-09-21 |
| try-omarchy PR #246 | Sparse VM disk capacity with a 64 GiB default (themartiano); closes #104, #132, #244 | merged 2026-09-22 |
| try-omarchy PR #242 | Restore macOS 15 support with updated graphics; VirGL 1.3.0 source-built (themartiano) | merged 2026-09-22 |
| try-omarchy PR #185 | Repair update holds in existing guests (Fail-Safe) | merged 2026-09-21 |
| try-omarchy PR #176, #182, #184, #188, #190, #195, #209, #220, #225, #226, #233, #234, #239, #240 | Alacritty acceleration; 1Password Touch ID; clock recovery; CJK and Traditional Chinese; ISO keyboard; us-acentos; bridged MAC; battery mirror; man-db; grab-tap recovery; 1Password installer; app version and release checks; trackpad scrolling; Ghostty on ARM64 | merged 2026-09-21..22 |
| macOS 27.0 Golden Gate release | https://www.macrumors.com/roundup/macos-27/ | Released 2026-09-14, build 26A428; the build named in #231 and #250 |
| try-omarchy #231 | macOS 27.0 (26A428): helper self-terminates; nested-virt probe abort non-fatal; macOS 27 sends a Quit AppleEvent | open, 2026-09-18 |
| try-omarchy #250 | macOS 27.0: v0.4.1 VM stops during startup | open, 2026-09-22 |
| try-omarchy #230 | Chromium-family browsers crash-loop the GPU process on VirGL (M5 Max, macOS 27.2 beta); GLES 3.2 context refused | open, 2026-09-18 |
| try-omarchy #232, #238, #222, #212 | App self-update; port-forward teardown blocks restart; text-injection keys; scrolling and space switching | open |
| try-omarchy #183 | Start menu after shutdown | closed 2026-09-22 (maintainer: likely addressed by #152) |
| try-omarchy PR #168 | Maintainer 2026-09-21: "Works on my M2 Pro. What do we need to make this production-ready?"; build fixes pushed; still CONFLICTING | open |
| omarchy-pkgs #199 | Publish an aarch64 tree at pkgs.omarchy.org | open; as of 2026-09-22 stable, rc, and edge aarch64 all return 200 (stable and rc 21 packages, edge 127 including hyprland-0.56.2-3, hyprtoolkit-0.5.4-5.1, hyprland-guiutils-0.2.2-3; no aquamarine on any tier) |
| omarchy-pkgs PR #240 | Add native AArch64 package build support (riverscn) | closed 2026-09-18, not merged: "Closing in favour of smaller PRs: 11 of these 21 packages already have aarch64 on master" |
| omarchy-pkgs PR #223 | Publish a reusable aarch64 repository artifact | closed 2026-09-16; the Snapdragon ISO consumes the published ARM repositories directly |
| omarchy-mac PR #345 | Use official ARM edge for the Hyprland stack | merged 2026-09-05 |
| omarchy-mac PR #354 | Recover Apple Silicon installs stranded before the ARM package sources policy | merged 2026-09-06 |
| omarchy-iso PR #129 | Boot and install Omarchy on Snapdragon X ARM64 systems | merged 2026-08-27 |

## Web sources

| Source | URL | What it establishes |
|---|---|---|
| Introducing Omarchy M (2026-09-11) | https://omarchy.org/news/2026/09/introducing-omarchy-m/ | Team, roles (Eduardo: Try Omarchy; Shun Li: aarch64 packages and generic ARM64 VM images plus guest tools; Joshua Warren: MLX), M1/M2 first, "finish what Asahi started", apple@omarchy.org |
| DHH on Omarchy M | https://x.com/dhh/status/2098507502979539361 | Incorporation announcement |
| Try Omarchy for Windows README, CHANGELOG.md, and docs/V1-READINESS.md | https://github.com/omacom/try-omarchy-windows | Process template: v1 scope, release gates, evidence records, compatibility revisions. V1-READINESS.md removed 2026-09-20 ("Remove the public v1 roadmap"); README and CHANGELOG declare v1.0.0 dated 2026-09-22 but no v1.0.0 release tag exists yet (latest published: v0.0.20-preview) |
| Introducing Omarchy Dragon (2026-09-18) | https://omarchy.org/news/2026/09/introducing-omarchy-dragon/ | Snapdragon team; Matt Gilg on aarch64 packages; Miguel Cruz shared with Omarchy M |
| omacom/omarchy-mac-installer | https://github.com/omacom/omarchy-mac-installer | Native installer extraction, created 2026-09-20; "not an installation release" |
| WWDC26 session 224 | https://developer.apple.com/videos/play/wwdc2026/224/ | macOS 27: custom Virtio devices (Linux guests), USB via Accessory Access, DiskImageKit ASIF layers, vmnet topologies and port forwarding, EFI secure boot |
| Bitrise WWDC26 summary | https://bitrise.io/blog/post/wwdc26-the-virtualization-framework-updates-that-matter-for-large-mac-fleets | Same, secondary |
| Apple: Running GUI Linux in a VM | https://developer.apple.com/documentation/Virtualization/running-gui-linux-in-a-virtual-machine-on-a-mac | VZ Linux graphics device |
| XueshiQiao/omarchy-vm | https://github.com/XueshiQiao/omarchy-vm | VZ has no Linux 3D: "There is no GPU acceleration for Linux guests here, and it cannot be turned on" |
| ArcBox: the macOS VM memory ratchet | https://arcbox.dev/blog/macos-vm-memory-ratchet | VZ cannot return freed guest memory; HVF VMM owns RAM so MADV_FREE_REUSABLE works |
| apple/container technical overview | https://github.com/apple/container/blob/main/docs/technical-overview.md | "memory pages freed to the Linux operating system by processes running in the container's VM are not relinquished to the host" |
| The Register on container machines | https://www.theregister.com/devops/2026/06/11/apple-gives-mac-devs-a-wsl-ish-thing-to-call-their-own/5254153 | Persistent Linux VMs on VZ; memory cannot be released |
| LunarG: KosmicKrisp Vulkan 1.3 conformance | https://www.lunarg.com/lunarg-achieves-vulkan-1-3-conformance-with-kosmickrisp-on-apple-silicon/ | Conformant Vulkan 1.3 on Metal 4, Apple silicon only |
| LunarG: State of Vulkan on Apple, Jan 2026 | https://www.lunarg.com/the-state-of-vulkan-on-apple-jan-2026/ | 1.4 coming; macOS 26+; nothing on Venus |
| milesbuckton/homebrew-qemu-virgl | https://github.com/milesbuckton/homebrew-qemu-virgl | Venus on macOS hosts works but "Apple Silicon Venus requires a patched guest Mesa ICD" for 16 KiB pages |
| startergo/homebrew-qemu-virgl-kosmickrisp | https://github.com/startergo/homebrew-qemu-virgl-kosmickrisp | The tap Try's runtime derives from |
| UTM: Introducing Neptune | https://blog.getutm.app/2026/introducing-neptune-direct3d-virtualization-for-qemu/ | virglrenderer protocol family (vrend, vDRM, Venus, Neptune); macOS host phase later |
| QEMU v11.1.1 `hw/intc/arm_gicv3_hvf.c` | https://gitlab.com/qemu-project/qemu/-/raw/v11.1.1/hw/intc/arm_gicv3_hvf.c | `vmstate_gicv3_hvf` with `hv_gic_state_get_data` / `hv_gic_set_state`; no migration blocker |
| QEMU v11.1.1 `target/arm/hvf/hvf.c` | https://gitlab.com/qemu-project/qemu/-/raw/v11.1.1/target/arm/hvf/hvf.c | No migration blocker; EL2 via `hv_vm_config_set_el2_enabled`; SME gated on macOS 15.2 |
| QEMU HVF series v20 cover letter | https://ratatoskr.run/qemu-devel/2026/03/14435050/t | "Save states are incompatible between kernel-irqchip=on and off on HVF due to opaque vGIC state"; SME not with nested virt |
| Hyprland issue #1396 | https://github.com/hyprwm/Hyprland/issues/1396 | No Vulkan renderer |
| Asahi M4 feature support | https://asahilinux.org/docs/platform/feature-support/m4/ | All M4 devices: installer "no"; features TBA; DART and PCIe TBA |
| Asahi: M3 support (2026-09) | https://asahilinux.org/2026/09/m2-episode-1/ | M3 shipped; M4/M5 in progress |
| omacom/omarchy discussion #7956 | https://github.com/omacom/omarchy/discussions/7956 | Community UTM build: "Omarchy 4 does not refuse to run on ARM64"; 123 of 148 base packages in Arch Linux ARM; no GPU acceleration |
| omacom/omarchy discussion #7960 | https://github.com/omacom/omarchy/discussions/7960 | aarch64 feature request; no maintainer reply |
| omacom/omarchy-mac README | https://github.com/omacom/omarchy-mac | Asahi Alarm plus Omarchy dual-boot installer, M1/M2 |
| paulsp94/omacosy | https://github.com/paulsp94/omacosy | Native macOS tiling in Omarchy style; not a VM |
| FEX-Emu | https://fex-emu.com/ | x86-64 usermode emulation on ARM64 Linux; needs an x86-64 rootfs |
| Parallels Desktop 27 graphics driver | https://www.mactrast.com/2026/08/parallels-desktop-27-update-now-apple-silicon-only-offers-faster-graphics-and-ai-acceleration-thanks-to-new-metal-based-graphics-driver/ | Context for the Windows VM beside Try |
