# Try Omarchy as the macOS-resident Omarchy M lane: five decisions and a v1

As of 2026-09-22. Written against upstream main at commit 7f3ce66 (unreleased; the last tagged release is v0.4.1 from 2026-09-15) and measured on a Mac mini M4 Pro (24 GB, macOS 26.6.2, build 25G83) running the installed 0.4.0 app. Every file anchor, issue number, and URL is listed in the source index that accompanies this briefing. A longer technical roadmap sits behind this document; stop here unless you want the problem graph.

Status: first draft, for discussion. It exists to start the conversation that leads to a more refined roadmap; nothing in it is settled. A first version was written on 2026-09-16 against 0.4.1; this revision absorbs the twenty-nine pull requests merged since.

## TL;DR

Try Omarchy runs the Omarchy Linux desktop inside a virtual machine on an Apple silicon Mac. This briefing argues that it should become a daily-use machine for people who keep macOS as their home operating system. A month ago that claim was blocked by four problems. In the week after 2026-09-15 the maintainer and contributors closed or half-closed two of them: an update channel for existing VMs now exists for host integrations, and freed guest memory now returns to macOS. The app also gained automatic startup and a clean restart path. What remains is narrower, and the more useful thing to write down now is not a feature list but the decisions that keep the next thirty pull requests coherent. This briefing proposes five, for the maintainer to accept, amend, or reject.

- **Product contract.** Try Omarchy is the "macOS stays home base" lane of Omarchy M, the official Apple silicon effort. It complements the native dual-boot path; it does not compete with it or lead to it.
- **Update contract.** The new integration manager is the right shape for delivering Try Omarchy's own fixes to an existing VM without a factory reset. Extend it to the payloads it still excludes: the kernel pair, the graphics runtime, package holds, and backports.
- **Packaging ownership.** Of the three compositor packages Try Omarchy rebuilds itself, Hyprland and hyprtoolkit are now published on the shared aarch64 package channel at the library version Try pins; aquamarine is not yet. Consume the channel; keep the rebuild recipe for what it lacks.
- **VMM bet.** Stay on QEMU over Apple's Hypervisor.framework with virgl and ANGLE for graphics. The memory-return work that just shipped used exactly the freedom Hypervisor.framework gives and Virtualization.framework does not.
- **v1 definition.** Publish a release-readiness document with explicit gates, the way the Windows edition of Try Omarchy did on its way to declaring v1.0.0.

Measured on the author's M4 Pro Mac mini with 0.4.0 on macOS 26.6: the launch-crash reports did not reproduce (three of the four are on macOS 27.0, released 2026-09-14), the VM idles at under a tenth of a CPU core, and it shuts down cleanly on request. Everything not in the four asks at the end is marked as the maintainer's call.

## Terms used

- **Omarchy**: an opinionated Linux desktop built on Arch Linux and the Hyprland window manager, created by DHH. Omarchy 4 ("Quattro") shipped in August 2026.
- **Try Omarchy**, or **Try**: the macOS app that runs Omarchy in a virtual machine on Apple silicon. Maintained by Eduardo (themartiano) under the Omacom GitHub organization.
- **try-omarchy-windows**: the Windows counterpart, also under Omacom. Same idea, different hypervisor. It declared v1.0.0 in its README and changelog on 2026-09-22 (no release tag yet) and is used here as a process precedent.
- **Omarchy M**: the team announced on 2026-09-11 to bring Omarchy to Apple silicon Macs, both natively (installed beside macOS on M1 and M2 machines through the Asahi Linux project's work) and in a VM (Try Omarchy). **Omarchy Dragon**, announced 2026-09-18, is the equivalent team for Snapdragon laptops; it shares the ARM package work.
- **Asahi Linux**: the community project that reverse-engineered Apple silicon so Linux can run natively on it. It supports M1 through M3; M4 and M5 are still in progress.
- **omarchy-pkgs**: the Omacom repository that builds and signs Omarchy's own packages. Its **aarch64** (64-bit ARM, the architecture of Apple silicon) channel now has **stable**, **rc**, and **edge** tiers; edge is the rolling pre-release tier.
- **Guest** and **host**: the Linux system inside the VM and the Mac running it.
- **VMM** (virtual machine monitor): the program that creates and runs a VM. Here that is **QEMU**, an open-source VMM, using Apple's **Hypervisor.framework** (**HVF**), the low-level API that lets a user-space program run guest CPU instructions on the real processor. Apple's higher-level **Virtualization.framework** (**VZ**) is a different API with its own device models.
- **virgl**, **virglrenderer**, and **ANGLE**: the chain that gives the guest a real GPU. The guest's OpenGL commands travel over a virtual GPU device to virglrenderer on the Mac, which replays them as OpenGL ES; ANGLE translates that into Apple's **Metal** graphics API.
- **Hyprland**: the Wayland compositor (window manager) Omarchy uses. **aquamarine** and **hyprtoolkit** are two libraries it depends on. An **ABI** (application binary interface) mismatch between them and the compositor is why Try Omarchy rebuilds all three.
- **Factory image**: the Linux disk image Try Omarchy builds and ships inside the app. A **factory reset** discards the user's VM and starts from a fresh copy.
- **Boot kit**: the kernel, initramfs, and command line paired with a particular VM disk, so an app update never boots an old disk with a new kernel.
- **Integration manager**: shipped on main in PR #193. The app exposes a read-only share to the guest; a guest command reviews and installs hash-verified bundles of Try Omarchy's own guest-side features, with backups and resumable progress, without a reset.
- **Pipe 1 and pipe 2**: the two update channels. Pipe 1 is Omarchy's own updater advancing ordinary packages, which works today. Pipe 2 is delivery of Try Omarchy's own kernel, runtime, and patches to an existing VM; the integration manager is its first working piece.
- **Free-page reporting**: a virtio balloon feature by which the guest tells the host which memory it has freed, so the host can reclaim it. Shipped on main in PR #243.
- **Horizon 0, 1, 2**: the ordering used in both documents. Horizon 0 unblocks the daily-driver claim; Horizon 1 is what a v1 release gates on; Horizon 2 is after v1.
- **Fail-Safe**: the GitHub handle of Try Omarchy's most active outside contributor and reviewer.
- **Parallels**: the commercial Mac virtualization product the author runs Windows in, beside Try Omarchy.
- **#NNN**: an issue or pull request number in the omacom/try-omarchy repository unless another repository is named.

## Why this exists

You said in our exchange on X that being a daily driver, and much more than "Try", is where this is going, and that you want to structure a public roadmap so everyone can contribute in that direction. Days later Omarchy M was announced with Try Omarchy named as one of its two official entry points, and a week after that Omarchy Dragon was announced for Snapdragon. Try is no longer a demo asking to be taken seriously; it is the lane for people whose home base is macOS, and it now shares its foundations with two hardware teams that own aarch64 packages.

This briefing is the inventory I would have wanted before writing that roadmap. It reads the code as it is on main, reads what Omarchy assumes about the machine it runs on, reads what Apple's virtualization stack does and does not offer, and reduces the result to five decisions and a v1 definition. It is written so you can accept, amend, or reject each decision. Everything not in the four asks at the end is marked as your call.

One disclosure so nothing here reads as more than it is: my involvement with the project is one feature request and its merged pull request, the configurable guest-memory menu (#81, #82), and two unpublished field write-ups from running the guest day to day. I am one demanding user with a specific machine, not the product definition.

## Thesis

Daily-driver Try Omarchy is blocked by a small number of foundational constraints, not by a long feature list, and the list got shorter this month. Since v0.4.1, twenty-nine pull requests landed on main from thirteen people, among them an integration manager for existing VMs (#193), memory return to macOS (#243), automatic startup and a clean restart path (#152), configurable disk capacity (#246), and macOS 15 restored with a source-built renderer (#242). That velocity is itself a finding: the roadmap's job is less "what to build" than "what to say no to, and what to write down so the work stays coherent".

What remains foundational: the integration manager does not yet carry the kernel, the graphics runtime, package holds, or the reviewed backports; Try still rebuilds the compositor stack itself even though the shared channel now ships most of it; the app has not been proven on macOS 27.0, which shipped on 2026-09-14 and is where most of the recent launch-crash reports come from; and the memory-return work has been validated on a headless VM but not on desktop workloads or macOS 26. Everything else that people associate with "daily driver" (hardware video decoding, multiple monitors, snapshots, deeper Apple ecosystem integration) is either in progress, a product choice you can make explicitly, or belongs after v1.

## Who this is for

The Mac-resident operator keeps macOS as the base operating system and runs everything else beside it. Three concrete versions of that person:

1. **The operator who already runs a business on the Mac.** macOS is home base. Windows runs in Parallels for Excel, Power BI, and the rest of the business stack. Omarchy is the environment for agentic coding (working with AI coding agents such as Claude Code or Codex in a terminal), and it has to be a VM on the same machine, because carrying two laptops is not an option and neither is wiping the Mac. This is my case: a Mac mini M4 Pro with 24 GB, moving toward a Mac Studio, with a MacBook Air on the road. The memory math is macOS plus Parallels plus Omarchy, always.
2. **The M4 or M5 owner waiting on Asahi.** Native Linux on M4 is not available today: the Asahi M4 support page lists every M4 device as not installable, with the GPU, display, sleep, and the low-level memory and bus layers underneath them all marked "TBA". Omarchy M has named people on M4 and M5 bring-up, but for this person today the VM is not a compromise, it is the only Omarchy there is on their hardware.
3. **The Mac-resident who wants Omarchy's agentic setup and will never dual-boot.** Omarchy ships thirteen AI coding agents pre-wired as commands, a picker for the default one, a hook that hands application crashes to an agent for diagnosis, and a passwordless-sudo mode for long unattended agent runs. This person wants all of that inside a window they can close. They will not carve a partition and they will not give up iCloud, Continuity, or their Mac apps to get it.

The person this lane is not for is the dual-boot persona: an M1 or M2 owner who wants Omarchy as the computer, with full GPU, external monitors, Touch ID, and disk encryption. That is Omarchy M's native path, owned by the omarchy-mac maintainers and the native installer now being extracted into its own repository. Both lanes share aarch64 packages, guest tools, and a great deal of hard-won knowledge about Apple hardware. They differ in who owns the disk.

## What main already is

It is worth stating plainly how much is already built, because the roadmap is shorter than the feature list suggests. Items marked "main" landed after v0.4.1 and are not yet in a tagged release.

**Runtime.** QEMU 11.1.1 on Hypervisor.framework, HVF-only with QEMU's software CPU emulator compiled out. Since v0.3.0 the guest's interrupt controller (GICv3) is provided by Apple inside the hypervisor rather than emulated, which took idle QEMU from about 65 percent of a core to about 15. Nested virtualization (letting the guest run VMs) is probed live on macOS 26 with M3 or newer, so `/dev/kvm` exists in the guest on that class. The kernel boots directly with no UEFI firmware and no bootloader. Every runtime dependency is pinned and checksummed. On main, the virgl renderer is built from pinned source (VirGL 1.3.0 plus the community patches) instead of a prebuilt bottle, and macOS 15 is supported again (#242).

**Graphics.** Guest OpenGL travels through virgl to virglrenderer on the host, which replays it as OpenGL ES through ANGLE into Metal. On this M4 Pro the guest reports "ANGLE (Apple, ANGLE Metal Renderer: Apple M4 Pro, Version 26.6.2 (Build 25G83))". Resolution and HiDPI scale follow the window. The compositor is real, hardware-accelerated Hyprland. On main, Alacritty is GPU-accelerated on macOS 26 and later (#176).

**Memory.** On main, the balloon device runs with free-page reporting and a pinned Hypervisor.framework patch returns the reported pages to macOS (#243). In the maintainer's headless test on macOS 27.0, a 3 GiB guest that freed 768 MiB returned 718 to 742 MiB each time, versus nothing before. Omarchy still sees its full selected RAM; existing VMs get this on the next launch of an updated app.

**Host integration.** Two-way clipboard for text and PNG. Mac audio routing from inside Omarchy. The FaceTime camera as an on-demand Linux camera device. One shared folder over the 9p network filesystem protocol, with a patch that makes host file ownership agree with the guest account. Loopback-only port forwards with an SSH preset. NAT and bridged networking. Touch ID for sudo, backed by the Secure Enclave. Pause and resume across Mac sleep. New on main: a stable bridged MAC (#209), Touch ID unlock for 1Password (#182), guest clock recovery after sleep (#184), the Mac's battery mirrored into Omarchy's bar (#220), ISO keyboard geometry, CJK fonts and Traditional Chinese, and precise trackpad scrolling (#190, #188, #239).

**Lifecycle.** New on main: opt-in automatic startup; a "Try Omarchy Settings" entry inside Omarchy that reopens the Mac settings window; "Restart Try Omarchy…", which asks Linux to shut down cleanly and relaunches QEMU with the saved settings (#152); a maximum disk size in Resources, 64 GiB by default, applied sparsely on next launch (#246); the integration manager (#193), which delivers reviewed guest-side features to existing VMs from a read-only share with verified hashes and resumable installs, starting with sudo Touch ID; and a repair command for package holds on older guests (#185). Unchanged: each persistent VM keeps a paired boot kit, and the factory image is reproducible from pinned packages and a pinned Omarchy commit, with reviewed backports (patches carried ahead of upstream) recorded by hash.

**Field reliability on the M4 class.** The guest on my Mac mini ran one continuous boot of eight days and twenty-one hours in early September. On 2026-09-16 I launched the installed 0.4.0 VM twice through its own bundled launcher on macOS 26.6.2: it booted and ran both times, the crash reported in #151 and #213 did not reproduce, QEMU idled at 7 to 9 percent of one core with periodic spikes to about 17 percent, and a QMP `system_powerdown` (the virtual power button) shut the guest down cleanly in five seconds. One surprise worth knowing: at about 150 s of idle, matching Omarchy's 150 s screensaver timer, host QEMU load rose to roughly half a core and stayed there. That cause was inferred from the host side, not confirmed in the guest. Two of the 0.4.0 observations are already superseded on main: freed memory did not return to the Mac then and does now, and the app did not autostart then and can now. The measurement notes, with exact commands, are available on request.

That is a foundation, not a prototype. The gaps below are specific.

## The three contracts

The technical roadmap explains each of these; here they are as one line each, because they are the constraints every decision below has to respect.

**Omarchy believes it is the computer.** Its installer wipes the drive, its updater assumes it owns the package transaction and blocks direct use of Arch's package manager (pacman), its snapshots depend on the Snapper tool and the Limine bootloader, its hibernation wants a swap volume the size of RAM, and its hardware scripts choose GPU drivers by scanning the PCI bus for Intel, AMD, or Apple, which never matches a virtual GPU. Nothing upstream has an aarch64 code path; the Mac support page still says M-series is not directly supported. VM hosting is Try's job because upstream has, correctly, never claimed it.

**Try Omarchy is a factory plus a VMM plus host bridges.** Its two update channels are deliberately separate. Ordinary `omarchy update` advances supported packages. Try's own layer reaches an existing disk mainly through the integration manager, plus three ad hoc routes (#152, #185, #243); the manager's documentation states its boundary plainly: it "does not replace the kernel, upgrade the graphics stack, repair package holds, install 1Password integration, or reproduce every change in a newer factory image" (docs/integration-updates.md). Those still need a factory reset, or a manual repair command. Hyprland, aquamarine, and hyprtoolkit are rebuilt in-tree, now at aquamarine 0.15.1 against Hyprland 0.56.2; the shared aarch64 channel publishes Hyprland and hyprtoolkit at those versions but no aquamarine.

**Apple's host is generous in some places and immovable in others.** Virtualization.framework gives Linux guests no 3D acceleration; only Hypervisor.framework with QEMU does. Hypervisor.framework lets the VMM own guest RAM, which is what made #243 possible, and which Virtualization.framework cannot do. Nested virtualization needs M3 or newer; Apple exposes it from macOS 15, but Try enables it only on macOS 26 because the macOS 15 path aborts with an error (#211). Apple silicon uses 16 KiB memory pages rather than the 4 KiB most Linux software assumes, a quiet tax on guest userspace. macOS 27.0 shipped on 2026-09-14 and is where most recent launch-crash reports originate; the launcher has not been proven on it. macOS 27 also adds custom virtual devices, USB passthrough, and layered sparse disk images, none of which change the GPU chain.

## The five decisions

### 1. Product contract: Try Omarchy is the macOS-resident lane

Try stays a macOS-resident VM. It is the complementary Omarchy M lane for people whose home base is macOS. It is not a stepping stone to dual-boot Asahi, and it is not a second Omarchy distribution.

Why it matters now: Omarchy M's announcement frames Try as the way to see the real desktop on a Mac in minutes and the installer as the commitment for M1 and M2 owners. That is the right framing for M1 and M2 owners and the wrong framing for everyone in the three personas above. Saying the lane out loud protects Try from two failure modes: being treated as a demo that ends when the native installer ships, and being pulled toward owning the disk. CONTRIBUTING.md already states the product target in almost these words; the change is to say it in the roadmap and in the Omarchy M conversation.

What it costs: nothing in code. It rules out a few things people will ask for (see non-goals).

Your call: the wording, and whether to say it in the Omarchy M channel or only in the repo.

### 2. Update contract: extend the integration manager until nothing needs a factory reset

This is Horizon 0, and the first piece of it shipped this week. PR #193 built the shape a Try update channel needed: a read-only share, hash-verified bundles, root-private backups, resumable installs, and a status port back to the Mac. Its first payload is sudo Touch ID. By its own design note it excludes the kernel and graphics packages, hold repair, 1Password, and clock recovery, and "does not reproduce every change in a newer factory image". #185, #152, and #243 deliver their pieces by their own routes. So an existing VM can now take some of Try's fixes, three different ways, and still cannot take the ones that matter most for rot: the kernel pair, the graphics runtime, and the reviewed backports.

The evidence is on my machine. The guest I created on 2 September from the v0.3.0 factory still runs an audio bridge script that leaks a set of PipeWire (the Linux audio server) modules on every restart; it restarted 284 times, exhausted the audio server's file descriptors, and retried once a second for five days. An input-method service (fcitx5) that upstream has since fixed restarted 333,398 times on the same guest. Those are exactly the class of fix the integration manager exists to carry.

The decision is to make the integration manager the single pipe-2 route and to widen it deliberately: host-bridge scripts and their services first (the audio bridge is the test case), package holds next, and the kernel pair last through an explicit, validated boot-kit upgrade that reuses the two-pass consent handshake already written for older disks. try-omarchy-windows' "compatibility revision" mechanism remains the in-family precedent for the repository half.

Your call: the order, and whether the kernel pair advances through the same channel or stays a separate, explicitly validated step.

### 3. Packaging ownership: consume the shared aarch64 channel and keep Try's guest tools small

Try should stop being the compositor's distribution. The Hyprland rebuild with a rounded-border patch (needed because the virtual GPU path draws window borders incorrectly) is the documented exception; aquamarine and hyprtoolkit are rebuilt and held alongside it. That is real work that has to be redone on every Hyprland release for as long as the pin exists, and the factory just did it again to move to aquamarine 0.15.1.

The channel to consume now exists and matches. pkgs.omarchy.org serves stable, rc, and edge aarch64 repositories; edge carries 127 packages including hyprland 0.56.2-3, hyprtoolkit, hyprland-guiutils, the omarchy package itself, omarchy-chromium, and 1Password, at the library version Try now pins; aquamarine itself is not on any tier yet, so the rebuild recipe still has one job. omarchy-mac (the native dual-boot project) already pins the three Hyprland packages to that signed edge channel. The large aarch64 build PR (omarchy-pkgs #240) was closed on 2026-09-18 in favour of per-package PRs because half of its packages had already landed. Omarchy Dragon has its own aarch64 package owner, so the channel now has two hardware teams behind it.

Try's part is bounded: consume the official aarch64 packages where they exist, keep the rebuild recipe as a fallback, and keep a small guest-tools package for the host bridges, the boot-export hook, and a VM hardware profile. What Try cannot decide alone is the channel's policy: which tier Try tracks, how the rounded-border patch is upstreamed or carried, and who signs. That is ask 3 below, a conversation with the aarch64 package owners across Omarchy M, Dragon, and omarchy-pkgs.

Your call: whether Try's guest tools become a package in omarchy-pkgs or stay in Try's own local repository, which tier Try tracks, and how the Hyprland patch is retired.

### 4. VMM bet: stay on QEMU, Hypervisor.framework, virgl, and ANGLE

Do not migrate to Virtualization.framework. Three reasons, each one line. Virtualization.framework's Linux graphics device is 2D only; its 3D device exists only for macOS guests, and a community project that tried running Omarchy on it states "there is no GPU acceleration for Linux guests here, and it cannot be turned on". Hypervisor.framework lets the VMM own guest RAM, so freed guest pages can be returned to macOS, which #243 now does; on Virtualization.framework Apple's own container tool documents that freed pages "are not relinquished to the host". Nested virtualization, the native Mac window, and every host bridge are already written against the current stack.

macOS 27's custom virtual devices, USB passthrough through the Accessory Access framework, and DiskImageKit's layered sparse images, plus vmnet port forwarding from macOS 26, are real and worth watching. None of them gives a Linux guest a GPU. They are Horizon 2 candidates for specific devices, not a rewrite trigger.

The caveat is smaller than it was. The virgl renderer is now built from pinned source with the community patches applied in-tree (#242), so the project no longer depends on a prebuilt bottle from a tap that stopped updating in January. ANGLE and libepoxy still come from those bottles. Staying on this stack means naming an owner for the host graphics story or recording it as an accepted risk. That belongs in the v1 gates.

Your call: whether that ownership is you, a named contributor, or an accepted risk written down.

### 5. v1 definition: publish a Mac readiness document with these gates

try-omarchy-windows worked toward v1.0.0 by keeping a readiness document with an intended scope, a shipped baseline with evidence, remaining gates, and the rule that "a code merge, automated test, physical test and public release are distinct evidence". It retired that document on 2026-09-20, two days before its changelog dated v1.0.0. Try for macOS has no equivalent, no milestones, and Discussions turned off, while merging a week of pull requests in two days and growing `docs/` from five files to fourteen. The proposal is to publish the Mac equivalent and let it be the public roadmap you asked for. The technical roadmap contains a draft you can lift.

Proposed gates, all amendable:

- **macOS 27 readiness.** Three of the four open launch-crash reports are on macOS 27.0 (build 26A428, released 2026-09-14): #231 (root-caused to macOS 27 sending a Quit event to the helper seconds after launch; the nested-virtualization probe abort underneath it is non-fatal), #250 (v0.4.1 with an existing VM), and #213. #151 (M4, macOS 26.3, a memory-mapping fault in the hypervisor about three and a half seconds after launch) is the one report that is not 27, and a source triage on it points at an upstream QEMU lead. Neither reproduced on my M4 Pro at 26.6.2. The gate: launch, run, and shut down on macOS 27.0, or a documented "not yet". This is already due, not prospective.
- **The pipe-2 channel** from decision 2, at least through the host-bridge layer.
- **Video decode: landed or explicitly deferred with evidence.** Today video is decoded on the CPU, which is slow at high resolutions. #167 and PR #168 add hardware decoding through the Mac's VideoToolbox; Fail-Safe verified it across HEVC, VP9, and AV1 on an M3 Max with 83 to 88 percent less guest CPU time, and on 2026-09-21 you confirmed it works on an M2 Pro, asked what production-ready needs, and pushed build fixes. v1 tracks that work. This briefing proposes no second video design.
- **Memory return validated where people use it.** #243's own remaining-validation list is the gate: macOS 26, desktop and graphics workloads, and sustained allocation churn. Then document the shipped picker (4, 6, 8, 12 GiB and 4 GiB steps above that, always leaving 4 GiB for macOS) plus a Parallels coexistence envelope as the contract on 24 GB and smaller hosts.
- **App self-update.** #234 shows the app version and offers opt-in release checks; docs/app-updates.md recommends Sparkle for actual installation and lists the signing and versioning prerequisites. A daily driver that has to be reinstalled by hand from a DMG is not one. Gate: Sparkle or equivalent shipped, or the interim checker declared sufficient for v1.
- **A guest CI build path, or an accepted risk.** The continuous-integration configuration records that the guest image cannot be built on GitHub-hosted macOS runners because they lack privileged ARM64 Docker; the runtime now builds in CI on macOS 15 and 26. That may be a runner or cost decision rather than a Try feature; either answer is fine if it is written down.
- **Mac-resident table stakes.** Multiple monitors and coexistence with a Parallels Windows VM (memory, port forwards, the Wi-Fi DHCP side effect that bridged mode has on other virtualization apps, disk, sleep) are what a Mac-resident expects. Either they are in v1, or v1 says explicitly "single display, loopback only".

Explicitly not v1: Vulkan graphics for the guest (Venus and KosmicKrisp), Continuity and Keychain, Bluetooth passkeys, multiple VMs, UEFI boot, USB passthrough, and nested Windows.

Your call: the gates, their order, and whether the document lives in the repo or somewhere more visible.

## Horizons

**Horizon 0 unblocks the claim.** Four problems, two of them narrowed this week: macOS 27 readiness; the rest of pipe 2 (kernel pair, graphics runtime, holds, backports, and the host-bridge scripts as the first payload); host-bridge hygiene (the audio leak class, with the fcitx5 fix as the positive example); and Try's part of packaging convergence. One new candidate: #230 reports Chromium-family browsers crash-looping their GPU process on virgl and freezing the guest on an M5 Max under a macOS 27.2 beta, with the host renderer refusing an OpenGL ES 3.2 context. If that reproduces on released macOS 27.0 or 26, the browser is the daily-driver workload and it belongs here. If "daily driver" ships without these: existing VMs rot, macOS 27 users cannot launch, the bridges eventually make the guest unlivable, and every Omarchy release is a re-port.

**Horizon 1 is v1.** The Horizon 0 problems, plus: memory return validated on desktop workloads; idle cost characterized (the idle screensaver is now the largest remaining cost); the rest of the shutdown story, since quitting the app still kills QEMU while the new restart flow proves the clean path works, and #146's pause indicator; app self-update; APFS copy-on-write snapshots as backup and rollback (#92 has a prototype waiting for a reply), plus a proposal of my own to exclude the VM workspace from Time Machine by default; the video decode track; the shared-folder decision, whether 9p is fast enough for an agentic coding loop over `node_modules` and `git` or virtiofs is needed; the agentic coding loop itself (SSH, VS Code Remote, the CLI agents, secrets, nested Docker for Linux workloads on M3 or newer only); and the Mac-resident table stakes or their explicit exclusion. Shipped on main since the first draft: autostart, in-guest settings, clean restart, disk capacity, keyboard geometry, CJK, stable bridged MAC, battery, 1Password Touch ID, Ghostty, macOS 15.

**Horizon 2 is after v1.** Vulkan for guest applications and video (Venus over the virtual GPU, KosmicKrisp translating to Metal on the host) once the guest graphics driver understands 16 KiB host pages; not for the compositor, which has no Vulkan renderer. UEFI boot only if Omarchy M's generic ARM64 images require it. USB through Accessory Access, a security surface. Keychain, passkeys, login Touch ID, and Bluetooth; the "Omarchy Link" issues that looked like a Calendar, Messages, and Notes design were an accidental bulk publish, never a plan. FEX (an x86 emulator for ARM Linux) for x86-only applications. Multiple VMs. Guest suspend to disk. MLX (Apple's machine-learning framework) on Linux if local models become a persona. DiskImageKit layered images compared against the current APFS clones.

## What is not in this lane

- Try does not become the native installer and does not compete with omarchy-mac. The dual-boot persona is theirs.
- Omarchy's own Windows feature (Windows in a Docker container on nested KVM) is a non-goal here. It exists for people whose computer is Omarchy. The Mac-resident already has a better Windows in Parallels, beside Try, and nested `/dev/kvm` remains an optional convenience for Linux workloads on M3 or newer.
- Try does not own aarch64 packaging. It consumes it.
- Nothing in this briefing volunteers anyone for implementation, including me.

One adjacent project deserves a sentence so it is not mistaken for a competitor: omacosy is a native macOS tiling setup in Omarchy's style with a real Super key and no VM. It is a different answer to a different question, and not a Try work item.

## The asks

1. Accept, amend, or reject the five decisions above.
2. Decide whether a public Mac readiness document, with milestones and a small set of labels, is the next process step, using the one try-omarchy-windows kept on its way to v1.0.0 as the template (retired on 2026-09-20; cite it from history).
3. Settle the aarch64 packaging policy with the channel's owners across Omarchy M, Omarchy Dragon, and omarchy-pkgs: which tier Try tracks, how the Hyprland patch is carried, and where Try's guest tools live, rather than letting Try keep a rebuilt-package island by default.
4. If any of this would benefit from a specific measurement or a specific issue being filed, say which, and it can be noted for whoever picks it up. The technical roadmap notes what was measured on the M4 Pro and what was not.

Everything else here is your call.

## What the technical roadmap adds

Behind this briefing is a problem graph with every item carrying today's state, why it matters for daily-driver, a candidate path, an owner (Try, omarchy-pkgs, Omarchy M, upstream Omarchy, Apple, or QEMU), dependencies, horizon, evidence anchors, and risk; a diagram of the two update pipes; the full capability matrix with what shipped on main since 0.4.1; a liftable draft of the Mac readiness document; and open questions phrased as decisions you make. Read it if you want to file issues from it. Stop here if the five decisions are enough to reply to.
