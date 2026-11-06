# 05 A different key for athena.cyfronet.pl

[Agenda](README.md) | Previous: [04 SSH agent](04-ssh-agent.md) | Next: [06 Register key](06-register-key-portal.md)

**Time:** 0:23-0:26 | **Exercise 5**

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

You can test the connection after the public keys are registered in [06 Register key](06-register-key-portal.md).

---
[Agenda](README.md) | Previous: [04 SSH agent](04-ssh-agent.md) | Next: [06 Register key](06-register-key-portal.md)
