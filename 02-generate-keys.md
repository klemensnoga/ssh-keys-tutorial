# 02 Generating SSH keys and choosing the algorithm

[Agenda](README.md) | Previous: [01 First login](01-first-login-known-hosts.md) | Next: [03 Copy public key](03-copy-public-key.md)

**Time:** 0:04-0:08 | **Exercise 1**

## Key pairs in one minute

- A key pair has a **private key** (stays on your machine, never shared) and a **public key** (`.pub`, safe to share, placed on servers).
- The server checks that you hold the private key without it ever being sent.
- Protect the private key with a **passphrase**, so a stolen file is useless on its own.

## Which algorithm

| Algorithm | Recommendation |
|---|---|
| **Ed25519** (elliptic curve) | **Use this.** Short keys, fast, strong, supported by all modern OpenSSH versions |
| RSA | Fallback for legacy systems only; use at least 3072 bits (4096 preferred) |
| ECDSA | Works, but Ed25519 is preferred |
| DSA | Obsolete, do not use |

If your site requires something else, follow the site documentation.

## Exercise 1: generate the key for Ares

```
ssh-keygen -t ed25519 -a 100 -f ~/.ssh/id_ed25519_ares -C "you@laptop-ares"
```

| Option | Meaning |
|---|---|
| `-t ed25519` | algorithm |
| `-a 100` | 100 rounds of key derivation, which makes passphrase brute-forcing slower |
| `-f` | file name (avoids overwriting an existing default key) |
| `-C` | comment, to recognize the key later (for example user, machine, purpose) |

When asked, enter a strong passphrase twice.

Result:

```
~/.ssh/id_ed25519_ares        # private key: never share
~/.ssh/id_ed25519_ares.pub    # public key: goes to the portal
```

Check the permissions (SSH refuses keys that others can read):

```
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519_ares
ls -l ~/.ssh
```

Show the public key and its fingerprint:

```
cat ~/.ssh/id_ed25519_ares.pub
ssh-keygen -lf ~/.ssh/id_ed25519_ares.pub
```

**Windows (PowerShell):** the same command works; the folder is `C:\Users\<name>\.ssh\`. Create it first if needed: `mkdir $env:USERPROFILE\.ssh`.

---
[Agenda](README.md) | Previous: [01 First login](01-first-login-known-hosts.md) | Next: [03 Copy public key](03-copy-public-key.md)
