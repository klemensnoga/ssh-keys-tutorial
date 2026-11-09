# 03 Registering the public key and first key login

[Agenda](README.md) | Previous: [02 Generate keys](02-generate-keys.md) | Next: [04 Config file](04-config-file.md)

**Time:** 0:08-0:13 | **Exercise 1b**

## How key login works

The server keeps the public keys allowed to log in as you in one file on the remote host:

```
~/.ssh/authorized_keys      # one public key per line
```

Registering your public key means adding **its single line** to the keys the server accepts for you. Only the `.pub` file is ever copied. The private key stays on your machine.

On PLGrid clusters (Ares, Athena, Helios) the supported way is the PLGrid portal (Method 1). Methods 2 and 3 copy the key into `authorized_keys` yourself; they need a way to log in once without the new key (for example your account password or another method your site allows), because the key is not installed yet.

## Method 1: PLGrid portal (recommended on PLGrid)

[Portal PLGrid](https://portal.plgrid.pl/) has  a central service for managing all your SSH keys at [PLGrid Infrastructure](https://plgrid.pl/). You upload them on the [Profile & affiliations](https://portal.plgrid.pl/users/affiliations) tab, in the [SSH keys](https://portal.plgrid.pl/users/ssh_keys) section. For detailed instructions see the [PLGrid documentation](https://guide.plgrid.pl/en/portal-for-science/profile/manage#ssh-keys).

1. Print the public key and copy the whole line (it starts with `ssh-ed25519`):

   ```
   cat ~/.ssh/id_ed25519_ares.pub
   ```

   On Windows PowerShell use `type $env:USERPROFILE\.ssh\id_ed25519_ares.pub`.

2. Log in to **https://portal.plgrid.pl** with your PLGrid account.
3. Open "Profile & affiliations" and find the [SSH keys](https://portal.plgrid.pl/users/ssh_keys) section.
4. Add the **public** key (`.pub`) with a recognizable name, for example `laptop-ares`. Never paste the private key.
5. Wait a few minutes if needed for the key to propagate to the clusters.

## Method 2: `ssh-copy-id`

```
ssh-copy-id -i ~/.ssh/id_ed25519_ares.pub plguser@ares.cyfronet.pl
```

It logs in with your existing method, creates `~/.ssh` with correct permissions if needed, appends the key to `authorized_keys` without duplicating it, and fixes permissions. Afterwards it prints a hint to try logging in.

- `-i` selects the public key. Without it, it uses your default keys only.
- Available by default on Linux and in WSL. On macOS it is included with recent versions; if `ssh-copy-id` is missing, use Method 3. It does **not** exist in Windows PowerShell, so use Method 3 there or WSL.

## Method 3: manual copy with `scp`

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

1. Register the key in the PLGrid portal (Method 1), or copy it with Method 2 or 3.
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
| `Permission denied (publickey)` | The key is not accepted: wait a few minutes after adding it in the portal, check that the whole `.pub` line was pasted (or appended), permissions, and `User` |
| `ssh-copy-id: command not found` | Use Method 3 |
| Works with `-i`, not without | Expected until the [config file](04-config-file.md) or the [agent](05-ssh-agent.md) is set up |

More diagnostics in [07 Best practices and troubleshooting](07-best-practices-troubleshooting.md).

---
[Agenda](README.md) | Previous: [02 Generate keys](02-generate-keys.md) | Next: [04 Config file](04-config-file.md)
