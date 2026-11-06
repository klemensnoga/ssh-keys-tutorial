# 08 Q&A and backup topics for advanced questions

[Agenda](README.md) | Previous: [07 Best practices](07-best-practices-troubleshooting.md)

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

### Converting between OpenSSH and PuTTY formats

PuTTYgen: *Conversions* menu, import the OpenSSH key and save as `.ppk`, or export in OpenSSH format.

### Reloading keys on macOS after reboot

`ssh-add --apple-load-keychain` reloads all Keychain keys into the agent.

---
[Agenda](README.md) | Previous: [07 Best practices](07-best-practices-troubleshooting.md)
