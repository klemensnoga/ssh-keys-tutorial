# 04 The SSH agent: setup and persistence

[Agenda](README.md) | Previous: [03 Config file](03-config-file.md) | Next: [05 Second key](05-second-key-athena.md)

**Time:** 0:16-0:23 | **Exercise 4**

## What the agent does

`ssh-agent` is a background program that keeps your **unlocked** private keys in memory. You type the passphrase once and every later connection reuses it. Nothing is written to disk in unencrypted form.

Useful commands:

```
ssh-add ~/.ssh/id_ed25519_ares     # load a key (asks for the passphrase)
ssh-add -l                          # list loaded keys
ssh-add -D                          # remove all keys from the agent
```

`AddKeysToAgent yes` in the config loads a key automatically the first time it is used. See [03 Config file](03-config-file.md) and [config.example](config.example).

The agent lives in memory, so after a reboot it is empty unless you set up persistence, described below.

## macOS

The agent is already running (launchd). Make it persistent through the Keychain:

```
Host *
    AddKeysToAgent yes
    UseKeychain yes
```

Or manually: `ssh-add --apple-use-keychain ~/.ssh/id_ed25519_ares`

Older macOS versions used `ssh-add -K`; this was replaced by `--apple-use-keychain` in macOS 12. `UseKeychain` exists only in Apple's OpenSSH, so keep `IgnoreUnknown UseKeychain` at the top of a config shared with other systems. If you installed OpenSSH with Homebrew, `which ssh` should still show `/usr/bin/ssh`.

## Linux

Many desktops start an agent for you (GNOME Keyring, KDE Wallet). Check with `echo $SSH_AUTH_SOCK`. If empty:

```
eval "$(ssh-agent -s)"
```

Persistence options:

- **Desktop keyring** (GNOME Keyring, KDE Wallet): unlocks keys at login.
- **systemd user service**: many current distributions ship `ssh-agent` as a user unit; enable it with `systemctl --user enable --now ssh-agent` if your distribution provides it, and point `SSH_AUTH_SOCK` at its socket.
- **`keychain` tool**: add `eval "$(keychain --eval --quiet id_ed25519_ares)"` to `~/.bashrc` or `~/.zshrc`.
- `AddKeysToAgent yes` in the config in all cases.

## Windows

### PowerShell with built-in OpenSSH (recommended)

Open PowerShell **as Administrator**:

```powershell
Get-Service ssh-agent | Set-Service -StartupType Automatic
Start-Service ssh-agent
```

Then, in a normal PowerShell:

```powershell
ssh-add $env:USERPROFILE\.ssh\id_ed25519_ares
ssh-add -l
```

The Windows agent normally keeps added keys across reboots, and `AddKeysToAgent yes` loads others on first use.

### WSL

WSL has its own separate agent and does not see the Windows one. Options:

- Use `keychain` inside WSL (as in the Linux section) and keep keys in `~/.ssh` inside WSL, **not** under `/mnt/c` (Windows drives break the 600/700 permissions).
- Call Windows `ssh.exe` from WSL to use the Windows agent.
- Bridge to the Windows agent with a helper such as npiperelay or wsl2-ssh-agent; follow that tool's own instructions.

### PuTTY

Run **Pageant**, right-click its tray icon, choose *Add Key*, and select the `.ppk` file. For persistence, put a shortcut to `pageant.exe` with the `.ppk` path as an argument in the Startup folder (`shell:startup`). PuTTY and OpenSSH key formats differ; PuTTYgen's *Conversions* menu converts between them.

## Exercise 4

1. Make sure an agent is running (`ssh-add -l` should not say it cannot connect to an agent).
2. Load the key: `ssh-add ~/.ssh/id_ed25519_ares`.
3. Apply the persistence method for your OS.
4. Confirm with `ssh-add -l`. After logging out and in again (or rebooting later), check that no passphrase prompt appears.

## Security notes

- Anyone who can use your unlocked session can use the loaded keys. Keep a screen lock and disk encryption on.
- Use `ForwardAgent` only for hosts you trust, never under `Host *`.
- `AddKeysToAgent confirm` asks for confirmation on each use if you want extra control.

---
[Agenda](README.md) | Previous: [03 Config file](03-config-file.md) | Next: [05 Second key](05-second-key-athena.md)
