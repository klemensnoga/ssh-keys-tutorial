# SSH Keys for Cyfronet: One-Page Handout

Full guide: [README](README.md) | Example config: [config.example](config.example)

## 1. First login and known_hosts
- `ssh plguser@ares.cyfronet.pl`: check the fingerprint against the official docs, then `yes`.
- The host is saved in `~/.ssh/known_hosts`. Look it up: `ssh-keygen -F ares.cyfronet.pl`.
- "HOST IDENTIFICATION HAS CHANGED": verify first, then `ssh-keygen -R ares.cyfronet.pl`.

## 2. Generate keys (use Ed25519, with a passphrase)
```
ssh-keygen -t ed25519 -a 100 -f ~/.ssh/id_ed25519_ares -C "you@laptop-ares"
ssh-keygen -t ed25519 -a 100 -f ~/.ssh/id_ed25519_athena -C "you@laptop-athena"
chmod 700 ~/.ssh && chmod 600 ~/.ssh/id_ed25519_*
```
RSA (3072+ bits) only as a fallback. Never share the private key.

## 2b. Copy the public key to Ares, then log in with it
```
ssh-copy-id -i ~/.ssh/id_ed25519_ares.pub plguser@ares.cyfronet.pl
# manual alternative (append, never overwrite):
scp ~/.ssh/id_ed25519_ares.pub plguser@ares.cyfronet.pl:~/
ssh plguser@ares.cyfronet.pl 'mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat ~/id_ed25519_ares.pub >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys && rm ~/id_ed25519_ares.pub'
ssh -i ~/.ssh/id_ed25519_ares -o IdentitiesOnly=yes plguser@ares.cyfronet.pl
```
Windows PowerShell has no `ssh-copy-id`: use the manual method. On PLGrid the portal may be the supported way, see section 5.

## 3. Config (`~/.ssh/config`, first match wins, `Host *` last)
```
Host ares
    HostName ares.cyfronet.pl
    User plguser
    IdentityFile ~/.ssh/id_ed25519_ares
    IdentitiesOnly yes
Host athena
    HostName athena.cyfronet.pl
    User plguser
    IdentityFile ~/.ssh/id_ed25519_athena
    IdentitiesOnly yes
Host *
    ServerAliveInterval 60
    ServerAliveCountMax 3
    AddKeysToAgent yes
```
macOS: also add `UseKeychain yes` under `Host *` and `IgnoreUnknown UseKeychain` at the top.

## 4. Agent and persistence
| OS | Setup |
|---|---|
| macOS | Agent runs already. `UseKeychain yes` or `ssh-add --apple-use-keychain KEY` |
| Linux | `eval "$(ssh-agent -s)"`; persist with the desktop keyring, systemd user unit or `keychain` |
| Windows PowerShell | Admin: `Set-Service ssh-agent -StartupType Automatic; Start-Service ssh-agent`, then `ssh-add $env:USERPROFILE\.ssh\id_ed25519_ares` |
| WSL | Own agent: use `keychain`, keep keys in WSL `~/.ssh` |
| PuTTY | Pageant, shortcut with `.ppk` in the Startup folder |

Check: `ssh-add -l`

## 5. Register the public key
Copy `cat ~/.ssh/id_ed25519_ares.pub` (and the athena one) into your account at **portal.plgrid.pl**. Wait a few minutes, then `ssh ares` and `ssh athena`.

## 6. Best practices
Passphrase on every key, one key per service, rotate and remove old keys, `ForwardAgent` only for trusted hosts, keep screen lock and disk encryption on, verify host fingerprints.

## 7. Troubleshooting
| Command | Use |
|---|---|
| `ssh -v ares` | see keys offered and why auth fails |
| `ssh -G ares` | show the effective config |
| `ssh -F /dev/null plguser@ares.cyfronet.pl` | bypass your config |
| `ssh-add -l` | is the key in the agent? |

| Error | Fix |
|---|---|
| `Permission denied (publickey)` | key not registered or propagated, wrong `User` or key |
| `Too many authentication failures` | add `IdentitiesOnly yes` |
| `Bad configuration option: usekeychain` | add `IgnoreUnknown UseKeychain` |
| `Bad owner or permissions` | `chmod 600` on config and keys |
| Idle disconnects | keep-alive settings |
