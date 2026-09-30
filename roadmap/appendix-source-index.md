# Appendix: source index

All sources cited by `01-exec-briefing.md` and `02-technical-roadmap.md`. Read dates are 2026-09-15 unless noted; everything added or updated in the 2026-09-28 revision was read on 2026-09-28, and in the 2026-09-29 revision on 2026-09-29. Repository anchors are `path:line` at the stated commit.

## Repositories read locally

| Repository | Commit / version | Location | Notes |
|---|---|---|---|
| omacom/try-omarchy | `58cbac5` (main, 2026-09-15) for the 2026-09-16 anchors; `b2a78a1` (upstream/main, 2026-09-29, unreleased) for everything marked "main" (the 2026-09-22 revision read `7f3ce66`, the 2026-09-28 revision `898f920`) | local checkout | v0.4.1 tagged 2026-09-15 and still the latest release; 43 PRs merged to main since v0.4.1 by 15 authors, as of 2026-09-29 (one commit, #286, since `898f920`) |
| omacom/omarchy | `e48f8382` (quattro, v4.0.0-358, 2026-09-12); `e332dc97` (quattro, 2026-09-28) for the architecture guard | local checkout | v4.0.4 (2026-09-15) still the latest release; still one `uname -m` guard |

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
| `guest/pinned-packages/README.md` | aquamarine and hyprtoolkit ABI pin recipes, held together with Hyprland (directory deleted on main by PR #286, 2026-09-29) |
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

## try-omarchy on main at b2a78a1 (read 2026-09-22 at 7f3ce66, re-read 2026-09-28 at 898f920 and 2026-09-29)

| Anchor | What it establishes |
|---|---|
| `docs/integration-updates.md` | Integration manager: read-only 9p share at `/mnt/try-omarchy-updates`, hash-verified bundles, backups, resumable install, status port; boundary quote: "does not replace the kernel, upgrade the graphics stack, repair package holds, install 1Password integration, or reproduce every change in a newer factory image" |
| `integrations/DESIGN.md` | Design note; initial payload sudo Touch ID, now "sudo Touch ID support and the Mac battery mirror" (b2a78a1); kernel and graphics package replacement excluded |
| `docs/integration-updates.md` "Features and boundaries" (b2a78a1) | "The bundle contains upstream sudo Touch ID support and the Mac battery mirror" (#252); boundary sentence unchanged |
| `guest/pacman.aarch64.conf:17` | `IgnorePkg = linux-aarch64 linux-aarch64-headers hyprland aquamarine hyprtoolkit hyprland-guiutils` |
| `guest/packages.lock.json:408` | `"linux-aarch64": "7.2.8-1"` since PR #286; line 409 read `"7.2.6-1"` at `898f920` |
| `guest/README.md` (b2a78a1) | "Aquamarine 0.15.1-1, Hyprtoolkit 0.6.0-1, and hyprland-guiutils 0.2.2-4 use signed Arch Linux ARM packages from the transaction lock", held with the patched Hyprland on `IgnorePkg`; the keyring-seeding note from omarchy-pkgs `7e448b9` (pinned before `898f920`) moved here from the deleted `guest/pinned-packages/README.md` |
| `docs/architecture.md` (b2a78a1) | The held set uses signed upstream ARM packages "so updates cannot split the validated graphics stack"; the aquamarine and Hyprtoolkit rebuild passage is gone |
| `guest/native-overlay/usr/local/bin/omarchy-native-mac-share:85` | 9p mount with `cache=mmap` |
| `macos/Sources/OmarchyVMHelper/GuestIntegrationStatus.swift` | `NSStatusItem` "VM integrations" created by the integration bridge process |
| `macos/run-qemu-gpu.sh:1901` | The launcher script starts `--bridge-integrations` as its own process beside QEMU |
| `macos/Info.plist:29` | `LSUIElement` true |
| `macos/qemu-port-forwarding.sh:13` | `QEMU_PORT_FORWARDING_NETDEV='user,id=omarchy-net'`: the NAT netdev has no `restrict` option, so `10.0.2.2` reaches services bound to the Mac's 127.0.0.1 |
| `README.md:427-428` | NAT mode (the default): "From Omarchy, connect to `10.0.2.2:<Mac port>` to reach a service running on the Mac"; no port forward needed for guest-to-Mac connections. The user netdev has no `restrict` option (`macos/qemu-port-forwarding.sh`), so 10.0.2.2 reaches services bound to the Mac's 127.0.0.1; bridged mode has no 10.0.2.2 |
| `README.md` "Passing a USB device to Omarchy (experimental)" | One device, off until chosen; "Unprivileged, which is how this app ships"; `com.apple.vm.device-access` or root needed for driver-claimed devices; "Which door to open is a decision for whoever ships the app" |
| `docs/memory-reclamation.md` | `virtio-balloon-pci,free-page-reporting=on` plus pinned HVF reclaim patch (unmap, demand-zero remap, acknowledge); asynchronous; needs runtime update and VM restart, not a new disk |
| `docs/app-updates.md` | Version display and opt-in release checks (#234); Sparkle 2 recommended for installation; prerequisites listed |
| `docs/architecture.md` (diff vs 58cbac5) | aquamarine 0.15.1-1 / libaquamarine.so=14 against Hyprland 0.56.2 (read at `898f920`; passage removed by PR #286); battery port `dev.tryomarchy.battery`; macOS 15 on EL1, VirGL source-built for 15.0; Traditional Chinese kernel argument; disk capacity |
| `macos/build-qemu-gpu-runtime.sh` | VirGL 1.3.0 from gitlab.freedesktop.org source with startergo tap v1.0.42 patches; ANGLE 1.0.16 and libepoxy 1.0.5 still from bottles |
| `macos/Sources/OmarchyVMHelper/VMApplicationController.swift` (`shutDownForSettings`) | `system_powerdown` over QMP is used by the restart-for-settings path; app quit still forwards SIGTERM (line 465 at b2a78a1) |
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
| try-omarchy #151, #213 | M4 crashes (EXC_GUARD in hv_vm_unmap; M4 Pro) | #151 closed 2026-09-29 as fixed by #277 (unreleased); #213 open, crash report requested 2026-09-29 |
| try-omarchy #133, #191 | Touch ID / Keychain; passkeys and Bluetooth | open |
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
| try-omarchy #189, #68 | Guest build memory on 16 GB Macs; macOS minimum mismatch and from-source build blockers | #189 closed 2026-09-28 by PR #271; #68 open, every part fixed on main per a 2026-09-29 comment, to close after the next release |
| omarchy #8645, #8530, #9576, #8897 | Upstream aarch64 threads: x86-only install menu entries; Voxtype; 1Password pin; man-db/Chromium SIGTRAP | open |
| try-omarchy PR #193 | Discover and update sudo integrations in existing VMs (Fail-Safe): the integration manager | merged 2026-09-21 |
| try-omarchy PR #243 | Return unused guest RAM to macOS (themartiano); 728.3 / 742.3 / 718.3 MiB returned per 768 MiB freed on macOS 27.0, 0 with reporting off; 29 PRs and 31 commits since v0.4.1 from 13 authors as of 2026-09-22 | merged 2026-09-22 |
| try-omarchy PR #152 | Automatic startup, in-guest Try Omarchy Settings, Restart Try Omarchy… (drdator) | merged 2026-09-21 |
| try-omarchy PR #246 | Sparse VM disk capacity with a 64 GiB default (themartiano); closes #104, #132, #244 | merged 2026-09-22 |
| try-omarchy PR #242 | Restore macOS 15 support with updated graphics; VirGL 1.3.0 source-built (themartiano) | merged 2026-09-22 |
| try-omarchy PR #185 | Repair update holds in existing guests (Fail-Safe) | merged 2026-09-21 |
| try-omarchy PR #176, #182, #184, #188, #190, #195, #209, #220, #225, #226, #233, #234, #239, #240 | Alacritty acceleration; 1Password Touch ID; clock recovery; CJK and Traditional Chinese; ISO keyboard; us-acentos; bridged MAC; battery mirror; man-db; grab-tap recovery; 1Password installer; app version and release checks; trackpad scrolling; Ghostty on ARM64 | merged 2026-09-21..22 |
| macOS 27.0 Golden Gate release | https://www.macrumors.com/roundup/macos-27/ | Released 2026-09-14, build 26A428; the build named in #231 and #250 |
| macOS 27.0.1 | https://www.macrumors.com/2026/09/28/apple-releases-macos-27-0-1/ | Released 2026-09-28 with bug fixes, alongside macOS 26.7.1 and 15.8.1 (read 2026-09-29) |
| try-omarchy #231 | macOS 27.0 (26A428): helper self-terminates; nested-virt probe abort non-fatal; macOS 27 sends a Quit AppleEvent | open, 2026-09-18; 2026-09-29 comment (stevederico): does not reproduce on a Mac mini M4 at 27.0.1 with v0.4.1, including under memory pressure |
| try-omarchy #250 | macOS 27.0: v0.4.1 VM stops during startup | closed 2026-09-24 by its reporter: no longer reproduces on 26A428 with v0.4.1 |
| try-omarchy #230 | Chromium-family browsers crash-loop the GPU process on VirGL (M5 Max, macOS 27.2 beta); ES 3.0 context request refused with `EGL_BAD_ATTRIBUTE` | open, 2026-09-18 |
| try-omarchy #232, #238, #222, #212 | App self-update; port-forward teardown blocks restart; text-injection keys; scrolling and space switching | #238 closed by PR #269 and #222 by PR #273, both 2026-09-28; #232 and #212 open |
| try-omarchy #183 | Start menu after shutdown | closed 2026-09-22 (maintainer: likely addressed by #152) |
| try-omarchy PR #168 | Maintainer 2026-09-21: "Works on my M2 Pro. What do we need to make this production-ready?"; build fixes pushed; still CONFLICTING | open |
| try-omarchy PR #197, #236, #248, #252, #254, #268, #269, #271, #272, #273, #277, #278, #282 | VirGL fence polling 1 ms on macOS; experimental one-device USB passthrough (vcanuel); stale Alacritty entry; battery mirror through the integration manager (NimbleAINinja); HDA playback across graphics stalls; hold `hyprland-guiutils` (closes #266); TCP forward restart (closes #238); build jobs bounded by memory (closes #189); guest lock-screen PAM policy (closes #259); injected keyboard text in Cocoa (closes #222); only unmap mapped HVF sections (stevederico); opt-in HVF trace log; bounded test concurrency | merged 2026-09-23..28 (read 2026-09-28) |
| try-omarchy PR #277 / #151 | v0.4.1 runtime 13 maps / 30 unmaps (20 on unmapped ranges); main with #277 13 / 10, Mac mini M4, macOS 26.6.2 (stevederico, #151 comment 2026-09-28) | merged 2026-09-28; #151 closed 2026-09-29 |
| try-omarchy PR #275 | Keep macOS 27 from quitting the VM launcher: Info.plist opt-outs plus `disableAutomaticTermination` / `disableSuddenTermination`; "Not verified on macOS 27" | open; 2026-09-29 author comment: #231 not reproduced on 27.0.1, PR kept as a guard (read 2026-09-29) |
| try-omarchy PR #284 | Take Hyprtoolkit 0.6.0-1 and `hyprland-guiutils` 0.2.2-4 from the mirror; keep the aquamarine rebuild; lock refresh includes `linux-aarch64` 7.2.8-1; fixes #280 | open, CONFLICTING with main after PR #286 (read 2026-09-29) |
| try-omarchy PR #286 | Use current ARM graphics packages and simplify guest builds (themartiano): aquamarine 0.15.1-1, Hyprtoolkit 0.6.0-1, `hyprland-guiutils` 0.2.2-4 as signed Arch Linux ARM packages; deletes `guest/pinned-packages/`, `build-pinned-abi-packages.sh` and `abiPackagePins`; lock refresh to `linux-aarch64` 7.2.8-1; "Desktop interaction was not tested" | merged 2026-09-29 (`b2a78a1`) |
| try-omarchy #285 | Build the guest from the official Omarchy ARM mirror (stevederico): "as discussed with @themartiano", the Omarchy ARM mirror and later the installer team's ARM image, with Try's VM patches on top; prototype branch `exp/omarchy-edge-stack` reached the Hyprland desktop on an M4 Mac mini (macOS 26.6), and an M3 MacBook Air (macOS 15) also built the guest; three options for Hyprland and #5; 12 Install entries with no aarch64 build; "nothing here is decided" | open, 2026-09-29 (findings checked 2026-09-28) |
| try-omarchy #180 | `neovim`: upstream installs `omarchy-nvim`, which is on the edge tier only (2026-09-29 comment) | open |
| try-omarchy PR #270, #274, #276, #283, #279 | Stable ARM repository and guest migration (fixes #261); ARM Install-menu gaps (fixes #264); Spotify web app on ARM64 (refs #263); audio mixer at the Mac output rate (fixes #265); Command chord under full grab (refs #181) | open (read 2026-09-28) |
| try-omarchy #281 | Shared folder `cache=mmap` on `linux-aarch64 7.2.6-1`, inside a netfs/9p regression (7.1-rc5 to 7.3-rc2, writeback cache modes) that can return or write NUL bytes; fix first in 7.2.8 (PiaoyangGuohai1) | open, 2026-09-28 |
| try-omarchy #280, #266 | `make guest` fails: `hyprland-guiutils` 0.2.2-3 gone from Arch Linux ARM, 0.2.2-4 needs `libhyprtoolkit.so=6`; `omarchy update` fails the same way | #280 open (PR #286's author reports `make build` passing on main; not independently confirmed); #266 closed 2026-09-28 by PR #268 |
| try-omarchy #261, #262, #263, #264 | Guest `[omarchy]` repo at `pkgs.omarchy.org/$arch`, whose `omarchy.db` holds only `omarchy-keyring` (read 2026-09-30); apps aarch64 on edge only; Install menu offers apps with no aarch64 build; raw `target not found` (seadogger; #262 as filed 2026-09-25, before stable gained some of those apps) | open |
| try-omarchy #257, #258, #259, #265 | #233's 1Password fix in no release; in-app disk resize; lock screen not working; guest audio ~6% fast (a 2026-09-28 follow-up measured about 4.8% fast at a 48 kHz Mac output, 0.3% at 44.1 kHz) | #259 closed 2026-09-28 by PR #272; others open |
| omarchy-pkgs #199 | Publish an aarch64 tree at pkgs.omarchy.org | open; on 2026-09-29 the tiers' `omarchy.db` files list stable 35, rc 37 and edge 160 packages (2026-09-28: 34, 36, 159; 2026-09-22: 21, 21, 127). Stable and rc carry `hyprland-0.56.2-4`; rc carries `linux-aurora-7.1.12.aurora2-9`; edge carries `hyprland-0.56.2-4`, `aquamarine-0.15.1-1.1`, `hyprtoolkit-0.5.4-5.1`, `hyprland-guiutils-0.2.2-3`, `linux-aurora-7.1.12.aurora2-11`, `omarchy-mac`, `omarchy-mac-boot`, `m1n1-aurora`. Stable also carries `cursor-bin`, `visual-studio-code-bin`, `claude-desktop` |
| omarchy-pkgs PR #240 | Add native AArch64 package build support (riverscn) | closed 2026-09-18, not merged: "Closing in favour of smaller PRs: 11 of these 21 packages already have aarch64 on master" |
| omarchy-pkgs PR #223 | Publish a reusable aarch64 repository artifact | closed 2026-09-16; the Snapdragon ISO consumes the published ARM repositories directly |
| omarchy-mac PR #345 | Use official ARM edge for the Hyprland stack: edge `[omarchy]` with `Usage = Sync` ("refreshed but excluded from automatic package selection and upgrades"), explicit targets `omarchy/hyprland`, `omarchy/hyprtoolkit`, `omarchy/hyprland-guiutils`; aquamarine and the rest resolve from the regular repositories (`docs/arm-package-sources.md`) | merged 2026-09-05 (re-read 2026-09-30) |
| omarchy-mac PR #354 | Recover Apple Silicon installs stranded before the ARM package sources policy | merged 2026-09-06 |
| omarchy-iso PR #129 | Boot and install Omarchy on Snapdragon X ARM64 systems | merged 2026-08-27 |

## Web sources

| Source | URL | What it establishes |
|---|---|---|
| Introducing Omarchy M (2026-09-11) | https://omarchy.org/news/2026/09/introducing-omarchy-m/ | Team, roles (Eduardo: Try Omarchy; Shun Li: aarch64 packages and generic ARM64 VM images plus guest tools; Joshua Warren: "mlx-omarchy, which brings Apple's MLX machine learning framework to Linux on Apple GPUs through Vulkan"; first-release target M1/M2, "Newer machines are being worked on in parallel", with named people on M4 and M5), "finish what Asahi started", apple@omarchy.org |
| DHH on Omarchy M | https://x.com/dhh/status/2098507502979539361 | Incorporation announcement |
| Try Omarchy for Windows README, CHANGELOG.md, and docs/V1-READINESS.md | https://github.com/omacom/try-omarchy-windows | Process template: v1 scope, release gates, evidence records, compatibility revisions. V1-READINESS.md removed 2026-09-20 ("Remove the public v1 roadmap"). v1.0.0 release notes drafted 2026-09-22 (`8824eb9`) were withdrawn on 2026-09-23 (`be615ae`) and re-issued as v0.1.0 notes the same day (`ec22609`); app releases v0.1.0 through v0.6.2 (latest, 2026-09-29; v0.6.2 limits SSH login for the quick-start account to keys); no v1.0.0 app release (a runtime release, `runtime-v1-r18`, is titled for v1.0.0) (read 2026-09-29) |
| try-omarchy-windows `docs/RELEASE-READINESS.md` | https://github.com/omacom/try-omarchy-windows/blob/master/docs/RELEASE-READINESS.md | Created 2026-09-23 (`be615ae`, "Document public preview and non-preview readiness"); a per-release review with signed-candidate and acceptance records under `docs/evidence/`, updated through v0.6.2, with signed-candidate records through v0.6.1 (read 2026-09-29) |
| Introducing Omarchy Dragon (2026-09-18) | https://omarchy.org/news/2026/09/introducing-omarchy-dragon/ | Snapdragon team; Matt Gilg on aarch64 packages; Miguel Cruz shared with Omarchy M |
| Apple support: background app termination in macOS 27 | https://support.apple.com/en-us/125671 | Rules exempt only apps with "a visible presence on screen, such as an icon in the upper right part of the menu bar, or a visible window" |
| Squirrel.Mac #336 (2026-09-19) | https://github.com/Squirrel/Squirrel.Mac/issues/336 | Updater helper reported failing on macOS 27; installs completed 30 of 30 on 26.7, 12 of 16 on 27.0 |
| utmapp/UTM #6929 | https://github.com/utmapp/UTM/issues/6929 | "QEMU seems to only support 1 GPU-supported display at a time" |
| rust-vmm/vhost #110 | https://github.com/rust-vmm/vhost/issues/110 | virtiofsd's vhost dependency does not build on macOS; open since 2022 |
| omarchy-pkgs PR #591 | https://github.com/omacom/omarchy-pkgs/pull/591 | `linux-aurora`, "the Apple Silicon kernel Omarchy Macs boot today", Aurora Silicon's fork of the Asahi kernel, for edge and rc; merged 2026-09-23; since 2026-09-25 (`0a16d43`) the recipe publishes to edge only |
| omarchy-pkgs `pkgbuilds/linux-aurora/config` | https://github.com/omacom/omarchy-pkgs | `CONFIG_ARM64_16K_PAGES=y`; `# CONFIG_SND_HDA_INTEL is not set`; `CONFIG_VIRTIO_PCI`, `CONFIG_DRM_VIRTIO_GPU`, `CONFIG_9P_FS`, `CONFIG_VIRTIO_BALLOON` as modules (read 2026-09-28) |
| omarchy-pkgs `pkgbuilds/hyprtoolkit/PKGBUILD` | https://github.com/omacom/omarchy-pkgs | `pkgver=0.6.0` since `6ce22c36` (2026-09-10); the published edge aarch64 build is still 0.5.4-5.1, built 2026-09-05 (read 2026-09-28) |
| omarchy-pkgs PR #628 | https://github.com/omacom/omarchy-pkgs/pull/628 | aquamarine 0.15.1-1.1 on edge, aarch64 only: Arch's 0.15.1-1 plus an Apple DCP CRTC-rescan patch; names a follow-up Hyprland rebuild; merged 2026-09-25 |
| omarchy-pkgs PR #470 | https://github.com/omacom/omarchy-pkgs/pull/470 | `omarchy-steam-fex` for Apple silicon, merged 2026-09-20 |
| omarchy-pkgs `pkgbuilds/hyprland/.omarchy/package.json` | https://github.com/omacom/omarchy-pkgs | Hyprland aarch64 `rebuilt_against` aquamarine 0.15.0-2 (unchanged 2026-09-28); `pkgbuilds/aquamarine` now exists (PR #628) |
| omarchy-pkgs `pkgbuilds/hyprland/PKGBUILD` and `README.md` | https://github.com/omacom/omarchy-pkgs | `arch=(aarch64)`; carries hyprwm/Hyprland#16343 as `software-renderer.patch` with `sync: false`, to be dropped "once the selected upstream release includes the #16343 behavior"; `release_ring: fast` builds edge, rc and stable (read 2026-09-30) |
| hyprwm/Hyprland PR #16343 | https://github.com/hyprwm/Hyprland/pull/16343 | Detect software rendering from the GL renderer; merged 2026-09-27, in no release (latest v0.56.2, 2026-08-05) |
| try-omarchy `guest/patches/hyprland/rounded-border-coverage.patch` (b2a78a1) | local checkout | 291 lines across 11 files; rewrites the border annulus mask in `border.glsl` for every rounded border and changes client-surface coverage for opaque, decorated main surfaces with a border of at least 2 px; no renderer or VirGL check; no hyprwm PR or issue proposes it (searched 2026-09-30) |
| pkgs.omarchy.org tier databases and pacman | https://pkgs.omarchy.org | Each tier serves its database only as `omarchy.db`; pacman builds the sync database filename from the section name (`lib/libalpm/be_sync.c`) and requires unique repository names, so a guest syncs one tier. Edge ahead of Arch Linux ARM wins by name for 8 packages as of 2026-09-30: aquamarine, hyprland, hyprpm, hyprland-guiutils, hyprtoolkit, nvidia-open-dkms, nvidia-utils, xdg-terminal-exec (edge 0.14.3-1, ALARM 0.14.3-2) |
| try-omarchy #285 prototype `stevederico/try-omarchy:exp/omarchy-edge-stack` | https://github.com/stevederico/try-omarchy | Based on `898f920`; `[omarchy]` edge with `SigLevel = Required DatabaseOptional` ahead of `[core]`; `stockHyprland`; its lock records hyprtoolkit 0.5.4-5.1 and guiutils 0.2.2-3 (read 2026-09-30) |
| omarchy `migrations/1789325478.sh` (2026-09-14) | https://github.com/omacom/omarchy | First architecture guard upstream: exits on non-x86_64 |
| omarchy `manual/49-omarchy-on.md` | https://github.com/omacom/omarchy | Lists a Parallels guide under "Apple Virtual Machine"; no mention of Try Omarchy |
| omarchy `config/chromium-flags.conf` | https://github.com/omacom/omarchy | Wayland, password-store, and extension flags only; no GPU flags |
| qemu-devel: HVF migration fix series (April 2026) | https://ratatoskr.run/qemu-arm/2026/04/14443671/t | Snapshot-load assertion and dirty-logging crash on HVF; merge status unconfirmed |
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
| Ollama: Anthropic API compatibility | https://docs.ollama.com/api/anthropic-compatibility, https://github.com/ollama/ollama/releases/tag/v0.14.0 | `/v1/messages` since v0.14.0 (2026-01-10), so Claude Code and other Anthropic-API clients can use local models (read 2026-09-29) |
| Ollama: MLX on Apple silicon | https://ollama.com/blog/mlx, https://github.com/ollama/ollama/releases/tag/v0.30.0 | v0.19.0 (2026-03-27): "Ollama is now powered by MLX on Apple Silicon in preview", for selected model architectures, on Macs with more than 32 GB; v0.30.0 (2026-05-13) adds llama.cpp support that "augments the MLX engine on Apple Silicon" (read 2026-09-29) |
| LM Studio | https://lmstudio.ai/changelog/lmstudio/lmstudio-v0.4.1, https://lmstudio.ai/docs/developer/openai-compat, https://lmstudio.ai/blog/lmstudio-v0.3.4 | 0.4.1 (2026-01-30): "New: Anthropic API compatibility endpoint: `/v1/messages`"; OpenAI-compatible server at `localhost:1234`; MLX engine beside llama.cpp since 0.3.4 (2024-10-08) (read 2026-09-29) |
| Apple ML research: LLMs with MLX and the M5 Neural Accelerators | https://machinelearning.apple.com/research/exploring-llms-mlx-m5 | Base M5 vs base M4 MacBook Pro (2025-11-19): time to first token up to 4x faster, generation 19-27% faster, bandwidth 153 vs 120 GB/s; decode stays memory-bandwidth bound (read 2026-09-29) |
| llama.cpp #10982 | https://github.com/ggml-org/llama.cpp/issues/10982 | M2 Max, Mistral 7B Q4_K_M, December 2024: Asahi Vulkan pp512 92.16 / tg128 21.93 t/s against macOS Metal 580.26 / 61.18 t/s; the gap persists in 2026 in the same issue: M1 Ultra, Llama2-7B Q4_0, Metal 925.95 / 103.31 against Asahi 206.10 / 32.58 (2026-06-06) (read 2026-09-29) |
| joshuaswarren/mlx-omarchy README, "Cross-OS parity state (2026-09-25)" | https://github.com/joshuaswarren/mlx-omarchy | Linux GPU Qwen decode against macOS on the same Mac: M1 37.39 / 47.05 tok/s (0.79x), M1 Max 77.33-77.48 / 179.47 tok/s (0.43x); "No GPU or Parakeet cell meets the bar yet" (read 2026-09-29) |
| Apple: MacBook Pro and Mac Studio specifications | https://www.apple.com/macbook-pro/specs/, https://www.apple.com/mac-studio/specs/ | 128 GB unified memory only with M5 Max, 40-core GPU, 614 GB/s; M5 Pro up to 64 GB at 307 GB/s; M5 Ultra 96-512 GB at 1.2 TB/s (read 2026-09-29) |
| M5 Max (128 GB) local LLM benchmarks | https://www.hardware-corner.net/m5-max-local-llm-benchmarks-20261233/, https://omlx.ai/benchmarks/fax1o712 | Third-party MLX runs: gpt-oss-120B (64 GB) 87.9 to 64.5 t/s generation from 4K to 32K context; Qwen3.5-122B-A10B 4-bit (70-76 GB peak) 65.9 to 54.9 t/s; Qwen3-Coder-Next 8-bit (81-94 GB peak) 75.6 to 65.3 t/s; Qwen3.5-27B dense 6-bit 23.6 to 14.9 t/s (read 2026-09-29) |
| macOS GPU wired-memory limit | https://github.com/ivanopcode/devnote-override-macos-metal-vram-cap | Default GPU wired limit about 75% of RAM; `iogpu.wired_limit_mb` raises it (secondary; read 2026-09-29) |
| Parallels Desktop 27 graphics driver | https://www.mactrast.com/2026/08/parallels-desktop-27-update-now-apple-silicon-only-offers-faster-graphics-and-ai-acceleration-thanks-to-new-metal-based-graphics-driver/ | Context for the Windows VM beside Try |
