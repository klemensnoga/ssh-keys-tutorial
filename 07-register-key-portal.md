# 07 Register the public key at portal.plgrid.pl

[Agenda](README.md) | Previous: [06 Second key](06-second-key-athena.md) | Next: [08 Best practices](08-best-practices-troubleshooting.md)

**Time:** 0:26-0:30 | **Exercise 6**

[Portal PLGrid](https://portal.plgrid.pl/) provides centralized service to manage all user SSH keys. User can upload his/her keys  
 in [Profile & affiliations](https://portal.plgrid.pl/users/affiliations) tab, in [SSH keys](https://portal.plgrid.pl/users/ssh_keys) section. For detailed instructions see [PLGrid documentation](https://guide.plgrid.pl/en/portal-for-science/profile/manage#ssh-keys).

## Steps

1. Print each public key and copy the whole line (starts with `ssh-ed25519`):

   ```
   cat ~/.ssh/id_ed25519_ares.pub
   cat ~/.ssh/id_ed25519_athena.pub
   ```

   On Windows PowerShell use `type $env:USERPROFILE\.ssh\id_ed25519_ares.pub`.

2. Log in to **https://portal.plgrid.pl** with your PLGrid account.
3. Open your "Profile & affiliations" and find the section for [SSH keys](https://portal.plgrid.pl/users/ssh_keys).
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
