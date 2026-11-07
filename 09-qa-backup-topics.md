# 09 Q&A and backup topics for advanced questions

[Agenda](README.md) | Previous: [08 Best practices](08-best-practices-troubleshooting.md)

**Time:** 0:30-1:00. The first 15 minutes are hands-on help; the rest is for advanced questions.

## Prepared topics

### ProxyJump (reaching nodes behind a login host)

```
Host node-*
    HostName %h.example.internal
    User plguser
    ProxyJump ares
```

No agent forwarding is needed, and your key stays on your laptop.

### SSH tunnels (Jupyter, TensorBoard)

```
ssh -N -o ExitOnForwardFailure=yes -L 8888:node042:8888 ares
```

Then open `http://localhost:8888`. Run the service on a compute node through the scheduler, not on the login node. Use `-R` and `-D` only where site policy allows.

### Connection reuse (ControlMaster)

```
Host ares
    ControlMaster auto
    ControlPath ~/.ssh/cm-%r@%h:%p
    ControlPersist 10m
```

Repeated logins and transfers reuse one connection. Not supported by the Windows built-in OpenSSH client; use WSL.

### Multi-factor authentication (MFA) with TOTP

Some HPC sites add a second factor on top of the SSH key: a time-based one-time password (TOTP). After the server accepts your public key, it asks for a 6-digit code from an authenticator app on your phone or laptop. The app and the site share a secret, which you set up in the site's user portal (at JSC this is JuDoor).

Example from the JSC JUPITER login hosts:

```
$ ssh plguser@login.jupiter.fz-juelich.de
(plguser@login.jupiter.fz-juelich.de) JSC TOTP Verification code:
```

The key passphrase (or the agent) comes first, then the code.

- Each code is valid for 30 seconds and works only once.
- If a code is rejected, check for a typo, wait for the next code if you reused one, and check that the clock on your phone or laptop is correct. Also check that the app holds the right secret for that site.

**Fewer code prompts:** use [connection reuse (ControlMaster)](#connection-reuse-controlmaster). JSC suggests a long persistence time:

```
Host *
    ControlMaster auto
    ControlPath ~/.ssh/control-%h-%p-%r
    ControlPersist 8h
```

Until the time runs out, later `ssh`, `scp`, `rsync` and git connections to the same host need neither the key nor a code, even after you close the first terminal. Caveats: all these sessions go to the same login node, a reboot of your laptop means logging in again, and the Windows built-in OpenSSH client does not support this (use WSL).

**Exemption at JSC:** keys stored on FIDO hardware tokens (see [FIDO2 hardware keys](#fido2-hardware-keys)) and SSH certificates skip the TOTP step. Other sites may handle this differently.

> Verify before the session: whether PLGrid/Cyfronet requires or offers TOTP for Ares and Athena, and in which portal it is set up. This was not checked when this material was written.

Source: [JSC JUPITER access documentation, MFA with TOTP](https://apps.fz-juelich.de/jsc/hps/jupiter/access.html#mfa-with-totp)

### Agent forwarding and its risks

`ssh -A` or `ForwardAgent yes` lets the remote host use your local agent. Root on the remote host can use it while you are connected. Prefer `ProxyJump`, or a dedicated key for Git on the cluster.

### File transfer and remote editing

`scp`, `rsync -av --partial`, `sftp`, VS Code Remote-SSH using the aliases from your config.

### Persistent sessions

`tmux` or `screen` keeps shells alive across disconnects.

### FIDO2 hardware keys

`ssh-keygen -t ed25519-sk` ties the key to a hardware token. Requires a recent OpenSSH on both sides and server support; check site policy first.

### RSA vs Ed25519 compatibility

Ed25519 is supported by all current OpenSSH versions; very old clients or servers may need RSA (3072+ bits).

### Reloading keys on macOS after reboot

`ssh-add --apple-load-keychain` reloads all Keychain keys into the agent.

---
[Agenda](README.md) | Previous: [08 Best practices](08-best-practices-troubleshooting.md)
