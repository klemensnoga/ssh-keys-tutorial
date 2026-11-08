# 04 The SSH config file: basics

[Agenda](README.md) | Previous: [03 Copy public key](03-copy-public-key.md) | Next: [05 SSH agent](05-ssh-agent.md)

**Time:** 0:13-0:18 | **Exercises 2 and 3**

## Why a config file

It stores per-host settings, so `ssh ares` replaces a long command with user name, host name and key file. Location: `~/.ssh/config` (Windows: `C:\Users\<name>\.ssh\config`).

```
touch ~/.ssh/config
chmod 600 ~/.ssh/config
```

## Basic rules

- The file is a list of `Host` blocks. The `Host` word is your alias.
- **The first value found for each option wins.** Put specific hosts first and the general `Host *` block last.
- Indentation is optional but makes the file readable.

## Exercise 2: config for ares.cyfronet.pl

```
Host ares
    HostName ares.cyfronet.pl
    User plguser
    IdentityFile ~/.ssh/id_ed25519_ares
    IdentitiesOnly yes
```

Replace `plguser` with your PLGrid login.

| Option | Purpose |
|---|---|
| `HostName` | real server name |
| `User` | login, so you do not type `plguser@` |
| `IdentityFile` | which private key to use |
| `IdentitiesOnly yes` | offer only the listed key, which prevents "Too many authentication failures" when the agent holds many keys |

## Exercise 3: add keep-alive

Keep-alive sends a small probe so idle connections are not dropped by firewalls or NAT.

```
Host *
    ServerAliveInterval 60
    ServerAliveCountMax 3
```

A probe goes out every 60 seconds. After 3 unanswered probes the session is closed, so a dead connection is noticed within about 3 minutes instead of hanging.

## Multiple keys

One block per destination, each with its own `IdentityFile` and `IdentitiesOnly yes` (see [06 Second key](06-second-key-athena.md)).

## Check the result

```
ssh -G ares            # print the final settings SSH will use
ssh ares               # connect with the alias
```

A complete ready-to-paste file is in [config.example](ares/config.example).

---
[Agenda](README.md) | Previous: [03 Copy public key](03-copy-public-key.md) | Next: [05 SSH agent](05-ssh-agent.md)
