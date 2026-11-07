<p align="center">
  <a href="https://romlin-emu.github.io"><img src="https://raw.githubusercontent.com/romlin-emu/.github/main/profile/banner.svg" alt="Romlin" width="820"></a>
</p>

<p align="center">
  <a href="https://github.com/romlin-emu/romlin/releases/tag/nightly"><img src="https://img.shields.io/github/v/release/romlin-emu/romlin?include_prereleases&label=release&color=56309b&labelColor=28252e" alt="Latest release, including prereleases"></a>
  <img src="https://img.shields.io/badge/platforms-macOS%20%7C%20Windows%20%7C%20Linux-56309b?labelColor=28252e" alt="Platforms: macOS, Windows and Linux">
  <a href="https://romm.app"><img src="https://img.shields.io/badge/RomM-5.0%2B-56309b?labelColor=28252e" alt="Requires RomM 5.0 or newer"></a>
  <a href="https://github.com/romlin-emu/romlin/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-GPL--3.0--or--later-56309b?labelColor=28252e" alt="License: GPL-3.0-or-later"></a>
</p>

<p align="center"><strong>The RomM desktop client.</strong><br>Download games, launch the right emulator, sync saves and play offline. Free and open source for macOS, Windows and Linux.</p>

<p align="center">
  <a href="https://romlin-emu.github.io">Website</a> ·
  <a href="https://romlin-emu.github.io/install/">Install</a> ·
  <a href="https://romlin-emu.github.io/systems/">Systems</a> ·
  <a href="https://romlin-emu.github.io/faq/">FAQ</a> ·
  <a href="https://github.com/romlin-emu/romlin/releases">Releases</a>
</p>

<p align="center">
  <img src="https://romlin-emu.github.io/img/library-1600.webp" alt="Upstream RomP library, showing recently played games from many consoles" width="820">
  <br><em>Screenshot shows the upstream RomP interface from which Romlin is derived.</em>
</p>

## What is Romlin?

Romlin is a desktop client for your self-hosted [RomM](https://romm.app) server, version 5.0 or newer. It downloads each game and the right emulator, plays downloaded games offline, and syncs saves and save states with RomM.

- **One-click play:** installs the emulator and fetches required BIOS files from your RomM server's firmware.
- **Save sync:** syncs before and after playing. If both copies changed, you choose which to keep; the other is backed up. Xbox and PS3 saves do not sync yet.
- **Sandboxed games:** each game runs in its own process with restricted file access. xemu, RPCS3 and Eden run outside the sandbox.
- **RetroAchievements:** achievements, popups and leaderboards. Hardcore mode awaits RetroAchievements approval.
- **50+ systems:** Atari 2600 through PS3 and original Xbox, plus arcade games and home computers.
- **Controllers and multiplayer:** automatic player assignment, remappable controls, and support for light guns, mice and keyboards.
- **Per-console looks:** CRT TV, CRT monitor and other screen styles suited to each system.
- **Free forever:** no paid features, subscriptions, ads, tracking or accounts of its own. See the [privacy policy](https://github.com/romlin-emu/romlin/blob/main/PRIVACY.md).

## Getting started

### 1. Install

Download the [nightly build](https://github.com/romlin-emu/romlin/releases/tag/nightly) and follow the [installation guide](https://romlin-emu.github.io/install/). There is no published stable release yet; use the nightly channel until the [first stable release](https://github.com/romlin-emu/romlin/releases/latest) is available.

| Platform | Download and first launch |
| --- | --- |
| macOS 11+, Apple silicon | ZIP. For unsigned builds, right-click Romlin and choose **Open**. |
| Windows, x86-64 | ZIP. For unsigned builds, choose **More info → Run anyway**. |
| Linux, x86-64 | AppImage, .deb, .rpm or .tar.gz. |
| Steam Deck / SteamOS | Use the [standalone installer](https://github.com/romlin-emu/installer); it also adds a Steam shortcut and artwork. |

The standalone installer supports all three desktop platforms. Exit Steam completely first; on Steam Deck, switch to Desktop Mode. Run as your normal user, without sudo. SteamOS/Linux and macOS need Bash, Python 3.9+, curl and tar; Windows needs PowerShell 5.1+. All need internet access. These commands download and run the installer from its `main` branch; [review the scripts and requirements](https://github.com/romlin-emu/installer) before running them.

<details>
<summary>SteamOS / Linux</summary>

On SteamOS:

```sh
(
  set -eu
  temp_dir=$(mktemp -d)
  trap 'rm -rf "$temp_dir"' EXIT
  curl -fsSL https://github.com/romlin-emu/installer/archive/refs/heads/main.tar.gz -o "$temp_dir/installer.tar.gz"
  tar -xzf "$temp_dir/installer.tar.gz" -C "$temp_dir"
  bash "$temp_dir/installer-main/install-steamos.sh" --nightly
)
```

On Linux:

```sh
(
  set -eu
  temp_dir=$(mktemp -d)
  trap 'rm -rf "$temp_dir"' EXIT
  curl -fsSL https://github.com/romlin-emu/installer/archive/refs/heads/main.tar.gz -o "$temp_dir/installer.tar.gz"
  tar -xzf "$temp_dir/installer.tar.gz" -C "$temp_dir"
  bash "$temp_dir/installer-main/install-linux.sh" --nightly
)
```

</details>

<details>
<summary>macOS</summary>

```sh
(
  set -eu
  temp_dir=$(mktemp -d)
  trap 'rm -rf "$temp_dir"' EXIT
  curl -fsSL https://github.com/romlin-emu/installer/archive/refs/heads/main.tar.gz -o "$temp_dir/installer.tar.gz"
  tar -xzf "$temp_dir/installer.tar.gz" -C "$temp_dir"
  bash "$temp_dir/installer-main/install-macos.sh" --nightly
)
```

</details>

<details>
<summary>Windows (PowerShell)</summary>

```powershell
& {
    $ErrorActionPreference = 'Stop'
    $tempDir = Join-Path ([IO.Path]::GetTempPath()) ('romlin-installer-' + [guid]::NewGuid().ToString('N'))
    New-Item -ItemType Directory -Path $tempDir | Out-Null
    try {
        [Net.ServicePointManager]::SecurityProtocol = [Net.ServicePointManager]::SecurityProtocol -bor [Net.SecurityProtocolType]::Tls12
        $archive = Join-Path $tempDir 'installer.zip'
        Invoke-WebRequest -UseBasicParsing -Uri 'https://github.com/romlin-emu/installer/archive/refs/heads/main.zip' -OutFile $archive
        Expand-Archive -LiteralPath $archive -DestinationPath $tempDir
        $script = Join-Path $tempDir 'installer-main\install-windows.ps1'
        Unblock-File -LiteralPath $script
        powershell -NoProfile -ExecutionPolicy RemoteSigned -File $script -Nightly
        if ($LASTEXITCODE -ne 0) { throw 'Romlin installation failed.' }
    } finally {
        Remove-Item -LiteralPath $tempDir -Recurse -Force
    }
}
```

This unblocks the downloaded script and sets the execution policy only for the child PowerShell process. Follow your organization’s script policy if it prevents the command.

</details>

Restart Steam after installation to see **Romlin** in your Library. Steam Deck users can return to Gaming Mode. Once a stable release is published, omit `--nightly` or `-Nightly` to install it.

### 2. Connect

Enter your RomM server address. Approve Romlin in RomM by scanning the code or opening the link. Romlin never stores your password.

### 3. Play

Open a game and press **Play**. Romlin downloads the game and sets up its emulator. Downloaded games remain playable when your server is offline.

## Repositories

| Repository | Purpose |
| --- | --- |
| [romlin](https://github.com/romlin-emu/romlin) | The native desktop app, written in Rust. |
| [installer](https://github.com/romlin-emu/installer) | Standalone installers with Steam shortcuts and artwork. |
| [ARMSX2](https://github.com/romlin-emu/ARMSX2) | PS2 libretro core for macOS ARM64. |
| [parallel-ps2](https://github.com/romlin-emu/parallel-ps2) | PS2 libretro core for Windows and Linux x86-64. |
| [fake-08](https://github.com/romlin-emu/fake-08) | PICO-8 libretro core. |
| [homebrew-tap](https://github.com/romlin-emu/homebrew-tap) | Homebrew cask; disabled until the first verified release. |
| [romlin-emu.github.io](https://github.com/romlin-emu/romlin-emu.github.io) | Website and user guides. |

## Help & contributing

- [Report a bug, request a feature or report a game issue](https://github.com/romlin-emu/romlin/issues/new/choose).
- [Read the FAQ](https://romlin-emu.github.io/faq/) or [build Romlin from source](https://github.com/romlin-emu/romlin/blob/main/README.md#running).
- Report server-side problems to [RomM](https://github.com/rommapp/romm/issues).
- [Report security vulnerabilities privately](https://github.com/romlin-emu/romlin/security/advisories/new).

---

Romlin includes no games, BIOS, firmware or keys. Use your own, including Switch `prod.keys` and firmware dumped from your console, and PS3 system software (`PS3UPDAT.PUP`), supplied through RomM's firmware library. Romlin is not affiliated with RomM.

Romlin is a fork of [RomP](https://github.com/RompEmu/RomP) by [Beshr Kayali Reinholdsson](https://beshr.com). Upstream authors, copyrights and licenses are preserved. The PS2 and PICO-8 cores derive from the PCSX2, ARMSX2, parallel-ps2 and FAKE-08 projects under their own licenses.
