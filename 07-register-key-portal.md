# 07 Register the public key at portal.plgrid.pl

[Agenda](README.md) | Previous: [06 Second key](06-second-key-athena.md) | Next: [08 Best practices](08-best-practices-troubleshooting.md)

**Time:** 0:26-0:30 | **Exercise 6**

> Verify before the session: the exact menu names in the portal and any key-type requirements were not checked when this material was written. Confirm them in the current PLGrid/Cyfronet documentation.

## Steps

1. Print each public key and copy the whole line (starts with `ssh-ed25519`):

   ```
   cat ~/.ssh/id_ed25519_ares.pub
   cat ~/.ssh/id_ed25519_athena.pub
   ```

   On Windows PowerShell use `type $env:USERPROFILE\.ssh\id_ed25519_ares.pub`.

2. Log in to **https://portal.plgrid.pl** with your PLGrid account.
3. Open your account/profile and find the section for SSH keys.
4. Add each **public** key (`.pub`) with a recognizable name, for example `laptop-ares` and `laptop-athena`. Never paste the private key.
5. Wait a few minutes if needed for the key to propagate to the clusters.
6. Test:

   ```
   ssh ares
   ssh athena
   ```

   With the agent configured, you should not be asked for the passphrase again.

## If it fails

Go to [08 Best practices and troubleshooting](08-best-practices-troubleshooting.md). Typical causes: key not yet propagated, wrong `User`, wrong key file, wrong permissions.

---
[Agenda](README.md) | Previous: [06 Second key](06-second-key-athena.md) | Next: [08 Best practices](08-best-practices-troubleshooting.md)
