# 03 Copying the public key to the remote host and first key login

[Agenda](README.md) | Previous: [02 Generate keys](02-generate-keys.md) | Next: [04 Config file](04-config-file.md)

**Time:** 0:08-0:13 | **Exercise 1b**

## How key login works

The server keeps the public keys allowed to log in as you in one file on the remote host:

```
~/.ssh/authorized_keys      # one public key per line
```

Copying your public key means **appending its single line** to that file on the remote host. Only the `.pub` file is ever copied. The private key stays on your machine.

You need a way to log in once without the new key (for example your account password or another method your site allows), because the key is not installed yet.

## Method 1: `ssh-copy-id` (recommended)

```
ssh-copy-id -i ~/.ssh/id_ed25519_ares.pub plguser@ares.cyfronet.pl
```

It logs in with your existing method, creates `~/.ssh` with correct permissions if needed, appends the key to `authorized_keys` without duplicating it, and fixes permissions. Afterwards it prints a hint to try logging in.

- `-i` selects the public key. Without it, it uses your default keys only.
- Available by default on Linux and in WSL. On macOS it is included with recent versions; if `ssh-copy-id` is missing, use Method 2. It does **not** exist in Windows PowerShell, so use Method 2 there or WSL.

## Method 2: manual copy with `scp`

1. Copy the public key to a temporary file in your home directory on the remote host:

   ```
   scp ~/.ssh/id_ed25519_ares.pub plguser@ares.cyfronet.pl:~/id_ed25519_ares.pub
   ```

2. Log in and append it to `authorized_keys`, then fix permissions and remove the temporary file:

   ```
   ssh plguser@ares.cyfronet.pl
   mkdir -p ~/.ssh
   chmod 700 ~/.ssh
   cat ~/id_ed25519_ares.pub >> ~/.ssh/authorized_keys
   chmod 600 ~/.ssh/authorized_keys
   rm ~/id_ed25519_ares.pub
   exit
   ```

   Use `>>` (append). A single `>` would **overwrite** the file and delete every key already there, including ones that let you in.

Do not `scp` straight onto `~/.ssh/authorized_keys` for the same reason: it replaces the whole file. That is only safe if the file does not exist yet.

**One-liner alternative** (also works from Windows PowerShell where `ssh-copy-id` is missing):

```
type $env:USERPROFILE\.ssh\id_ed25519_ares.pub | ssh plguser@ares.cyfronet.pl "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
```

On macOS/Linux replace `type ...` with `cat ~/.ssh/id_ed25519_ares.pub`.

Why permissions matter: if `~`, `~/.ssh` or `authorized_keys` is writable by others, the server's SSH daemon ignores the file and silently falls back to other login methods.

## Exercise 1b: log in to Ares with the new key

1. Copy the key with Method 1 or Method 2.
2. Log in, telling SSH exactly which key to use. The config file is not written yet (that is the next step):

   ```
   ssh -i ~/.ssh/id_ed25519_ares -o IdentitiesOnly=yes plguser@ares.cyfronet.pl
   ```

3. Enter the **key passphrase** when asked (for example `Enter passphrase for key '...id_ed25519_ares'`). Asking for the key passphrase, not the account password, means key authentication works.
4. See which key was used, with verbose output:

   ```
   ssh -v -i ~/.ssh/id_ed25519_ares -o IdentitiesOnly=yes plguser@ares.cyfronet.pl 2>&1 | grep -iE "offering|accepted|authenticated"
   ```

   Look for `Offering public key: ...id_ed25519_ares` and `Authenticated to ares.cyfronet.pl ... using "publickey"`.

## If it fails

| Symptom | Likely cause |
|---|---|
| Asked for the account password again | Key not installed, wrong file permissions on the remote side, or the server does not accept this key yet |
| `Permission denied (publickey)` | Password login is disabled and the key is not accepted: check the key line was appended, permissions, `User`, or register it via the [portal](07-register-key-portal.md) |
| `ssh-copy-id: command not found` | Use Method 2 |
| Works with `-i`, not without | Expected until the [config file](04-config-file.md) or the [agent](05-ssh-agent.md) is set up |

More diagnostics in [08 Best practices and troubleshooting](08-best-practices-troubleshooting.md).

---
[Agenda](README.md) | Previous: [02 Generate keys](02-generate-keys.md) | Next: [04 Config file](04-config-file.md)
