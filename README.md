# This fork: Atom 1.60.0 + my setup, in one place

Atom was archived by GitHub on 2022-12-15 and its package registry (atom.io,
`apm`) is gone. I still use it. This fork of `atom/atom` keeps everything
needed to put my exact editor back on a fresh machine:

| What | Where |
|---|---|
| The program | Release [`v1.60.0-victus`](../../releases/tag/v1.60.0-victus) → `AtomSetup-x64.exe` (the official 1.60.0 installer, re-hosted here because forks do not inherit upstream releases) |
| My `~/.atom` | [`setup/dot-atom/`](setup/dot-atom) — config, keymap, snippets, styles, init script, and the installed packages with their source |
| Upstream source | Everything else in this repo, unchanged, from `atom/atom` `master` at `1c3bd35` |

Branch `custom` is the default. `master` tracks upstream and will never move
again.

## Packages (vendored, no registry needed)

| Package | Version | Origin | Notes |
|---|---|---|---|
| `language-ziran` | 0.6.3 | [ziranlang/ziran-vscode](https://github.com/ziranlang/ziran-vscode) | Ziran grammar ported from the VS Code extension. The one that matters. |
| `language-lua` | 0.9.11 | [FireZenk/language-lua](https://github.com/FireZenk/language-lua) | |
| `language-powershell` | 5.0.0 | [jugglingnutcase/language-powershell](https://github.com/jugglingnutcase/language-powershell) | |
| `language-batchfile` | 0.7.7 | [mmims/language-batchfile](https://github.com/mmims/language-batchfile) | Folder is named `language-batchfile-master` (installed from a zip) |

All four are pure grammar packages: no `node_modules`, no native code, so
they are committed whole and work on any Atom 1.x.

## Config

`config.cson` is small on purpose: `whitespace` package disabled, telemetry
off, font size 17, welcome screen off. `keymap.cson`, `snippets.cson`,
`styles.less` and `init.coffee` are still Atom's blank templates — kept so
the folder is a complete snapshot, not a partial one.

## Restore on a fresh Windows machine

1. Download `AtomSetup-x64.exe` from the
   [`v1.60.0-victus` release](../../releases/tag/v1.60.0-victus) and run it.
   It installs to `%LOCALAPPDATA%\atom` and puts `atom` on the user PATH.
2. Start Atom once so it creates `%USERPROFILE%\.atom`, then close it.
3. Copy the contents of `setup/dot-atom/` over `%USERPROFILE%\.atom\`:
   ```powershell
   robocopy .\setup\dot-atom "$env:USERPROFILE\.atom" /E
   ```
4. Start Atom. Grammars load from `.atom/packages`; nothing is fetched.

Do **not** run `apm install` — the registry it talks to no longer exists.
Atom's self-updater is dead for the same reason, so this install will never
change under you.

## Keeping the snapshot current

After changing config or adding a package, re-copy and commit:

```powershell
robocopy "$env:USERPROFILE\.atom" .\setup\dot-atom /E /XD node_modules .git blob-store compile-cache storage recovery .apm .node-gyp .npm
git add setup && git commit -m "setup: update .atom snapshot" && git push
```

Caches (`compile-cache`, `storage`, `blob-store`, `.apm`) are deliberately
excluded; Atom rebuilds them.

## If 1.60.0 ever stops running

[Pulsar](https://github.com/pulsar-edit/pulsar) is the community
continuation of Atom with a modernised build. It reads the same `.atom`
layout, so `setup/dot-atom/` restores into it unchanged. Building Atom 1.60
itself from this source on a current machine is not realistic (Electron 9,
Node 12, Python 2 era toolchain); this fork exists to preserve the binary and
the setup, not to rebuild it.

---

# Original README

# Atom

[![Build status](https://dev.azure.com/github/Atom/_apis/build/status/Atom%20Production%20Branches?branchName=master)](https://dev.azure.com/github/Atom/_build/latest?definitionId=32&branchName=master)

> Atom and all repositories under Atom will be archived on December 15, 2022. Learn more in our [official announcement](https://github.blog/2022-06-08-sunsetting-atom/)

Atom is a hackable text editor for the 21st century, built on [Electron](https://github.com/electron/electron), and based on everything we love about our favorite editors. We designed it to be deeply customizable, but still approachable using the default configuration.

![Atom](https://user-images.githubusercontent.com/378023/49132477-f4b77680-f31f-11e8-8357-ac6491761c6c.png)

![Atom Screenshot](https://user-images.githubusercontent.com/378023/49132478-f4b77680-f31f-11e8-9e10-e8454d8d9b7e.png)

Visit [atom.io](https://atom.io) to learn more or visit the [Atom forum](https://github.com/atom/atom/discussions).

Follow [@AtomEditor](https://twitter.com/atomeditor) on Twitter for important
announcements.

This project adheres to the Contributor Covenant [code of conduct](CODE_OF_CONDUCT.md).
By participating, you are expected to uphold this code. Please report unacceptable behavior to atom@github.com.

## Documentation

If you want to read about using Atom or developing packages in Atom, the [Atom Flight Manual](https://flight-manual.atom.io) is free and available online. You can find the source to the manual in [atom/flight-manual.atom.io](https://github.com/atom/flight-manual.atom.io).

The [API reference](https://atom.io/docs/api) for developing packages is also documented on Atom.io.

## Installing

### Prerequisites
- [Git](https://git-scm.com)

### macOS

Download the latest [Atom release](https://github.com/atom/atom/releases/latest).

Atom will automatically update when a new release is available.

### Windows

Download the latest [Atom installer](https://github.com/atom/atom/releases/latest). `AtomSetup.exe` is 32-bit. For 64-bit systems, download `AtomSetup-x64.exe`.

Atom will automatically update when a new release is available.

You can also download `atom-windows.zip` (32-bit) or `atom-x64-windows.zip` (64-bit) from the [releases page](https://github.com/atom/atom/releases/latest).
The `.zip` version will not automatically update.

Using [Chocolatey](https://chocolatey.org)? Run `cinst Atom` to install the latest version of Atom.

### Linux

Atom is only available for 64-bit Linux systems.

Configure your distribution's package manager to install and update Atom by following the [Linux installation instructions](https://flight-manual.atom.io/getting-started/sections/installing-atom/#platform-linux) in the Flight Manual.  You will also find instructions on how to install Atom's official Linux packages without using a package repository, though you will not get automatic updates after installing Atom this way.

#### Archive extraction

An archive is available for people who don't want to install `atom` as root.

This version enables you to install multiple Atom versions in parallel. It has been built on Ubuntu 64-bit,
but should be compatible with other Linux distributions.

1. Install dependencies (on Ubuntu):
```sh
sudo apt install git libasound2 libcurl4 libgbm1 libgcrypt20 libgtk-3-0 libnotify4 libnss3 libglib2.0-bin xdg-utils libx11-xcb1 libxcb-dri3-0 libxss1 libxtst6 libxkbfile1
```
2. Download `atom-amd64.tar.gz` from the [Atom releases page](https://github.com/atom/atom/releases/latest).
3. Run `tar xf atom-amd64.tar.gz` in the directory where you want to extract the Atom folder.
4. Launch Atom using the installed `atom` command from the newly extracted directory.

The Linux version does not currently automatically update so you will need to
repeat these steps to upgrade to future releases.

## Building

* [Linux](https://flight-manual.atom.io/hacking-atom/sections/hacking-on-atom-core/#platform-linux)
* [macOS](https://flight-manual.atom.io/hacking-atom/sections/hacking-on-atom-core/#platform-mac)
* [Windows](https://flight-manual.atom.io/hacking-atom/sections/hacking-on-atom-core/#platform-windows)

## Discussion

* Discuss Atom on [GitHub Discussions](https://github.com/atom/atom/discussions)

## License

[MIT](https://github.com/atom/atom/blob/master/LICENSE.md)

When using the Atom or other GitHub logos, be sure to follow the [GitHub logo guidelines](https://github.com/logos).
