# 06 A different key for athena.cyfronet.pl

[Agenda](README.md) | Previous: [05 SSH agent](05-ssh-agent.md) | Next: [07 Best practices](07-best-practices-troubleshooting.md)

**Time:** 0:24-0:26 | **Exercise 5**

## Why a separate key

- If one key is lost or compromised, you revoke only that one.
- You can tell by the key comment which machine or service uses it.
- It shows how `IdentityFile` plus `IdentitiesOnly` select the right key per host.

## Exercise 5

1. Generate the second key:

   ```
   ssh-keygen -t ed25519 -a 100 -f ~/.ssh/id_ed25519_athena -C "you@laptop-athena"
   ```

2. Add a block **above** `Host *` in `~/.ssh/config`:

   ```
   Host athena
       HostName athena.cyfronet.pl
       User plguser
       IdentityFile ~/.ssh/id_ed25519_athena
       IdentitiesOnly yes
   ```

3. Load it into the agent: `ssh-add ~/.ssh/id_ed25519_athena`
4. Check the settings: `ssh -G athena | grep -i identityfile`
5. Register the public key `~/.ssh/id_ed25519_athena.pub` in the PLGrid portal with a name such as `laptop-athena`, as in [03 Register public key, Method 1](03-copy-public-key.md#method-1-plgrid-portal-recommended-on-plgrid).
6. After a few minutes, test: `ssh athena`

---
[Agenda](README.md) | Previous: [05 SSH agent](05-ssh-agent.md) | Next: [07 Best practices](07-best-practices-troubleshooting.md)
