# 08 Best practices and troubleshooting

[Agenda](README.md) | Previous: [07 Register key](07-register-key-portal.md) | Next: [09 Advanced topics](09-qa-advanced-topics.md)

## Best practices

- Use **Ed25519** keys with a **passphrase**, one key per service.
- Never share or copy the private key. Back it up only in encrypted form.
- Verify host fingerprints on first login. Never blindly delete `known_hosts` entries.
- Rotate keys when a laptop is lost, replaced or retired, and remove old public keys from the portal.
- Use `IdentitiesOnly yes` with an explicit `IdentityFile` per host.
- Enable `ForwardAgent` only for trusted hosts, never under `Host *`.
- Keep disk encryption and a screen lock on.
- Keep `~/.ssh` at 700 and private keys and config at 600.

## Troubleshooting ladder

Work top to bottom:

1. `ssh -v ares` shows which config lines apply, which keys are offered and why authentication fails. Use `-vvv` for more detail.
2. `ssh -G ares` prints the final merged configuration for that host.
3. `ssh -F /dev/null plguser@ares.cyfronet.pl` ignores your config, to see whether the config is the problem.
4. Check permissions:

   ```
   chmod 700 ~/.ssh
   chmod 600 ~/.ssh/config ~/.ssh/id_ed25519_*
   ```

   Exclude the `.pub` files from the second command if you like; they can stay readable.
5. `ssh-add -l` shows whether the agent holds the key. If it cannot connect to an agent, start one (see [05 SSH agent](05-ssh-agent.md)).
6. Check the network: VPN, firewall, correct host name.

## Common problems

| Symptom | Likely cause and fix |
|---|---|
| `Permission denied (publickey)` | Public key not registered or not yet propagated, wrong `User`, or the wrong key is offered. Check with `ssh -v`. |
| `Too many authentication failures` | The agent offers many keys. Add `IdentitiesOnly yes`. |
| `REMOTE HOST IDENTIFICATION HAS CHANGED` | Server key changed. Verify, then `ssh-keygen -R host` (see [01](01-first-login-known-hosts.md)). |
| `Bad configuration option: usekeychain` | macOS-only option on another system. Add `IgnoreUnknown UseKeychain` at the top. |
| `Bad owner or permissions on .../config` | Fix file permissions (600) and ownership. |
| `UNPROTECTED PRIVATE KEY FILE` (Windows or Linux) | Other users can read the key. Restrict permissions. |
| `Connection timed out` | Network or VPN problem, wrong host name. |
| `ssh_exchange_identification: read: Connection reset by peer` (newer OpenSSH: `kex_exchange_identification: ...`) | The server drops the connection before login starts. A common cause is too many failed login attempts from your IP address: a protection mechanism such as fail2ban then blocks that IP for a while. Stop retrying, fix the cause of the failures (wrong `User`, wrong key, a script retrying in a loop), and wait for the block to expire. After checking your setup, try connecting to the server from a client with a different public IP address. If the problem persists, contact the support team and give them your public IP address. |
| `Could not open a connection to your authentication agent` | Agent not running or `SSH_AUTH_SOCK` not set. |
| Session freezes after idle time | Add keep-alive (see [04](04-config-file.md)). |
| Key works in PowerShell but not in WSL (or reverse) | Separate `~/.ssh` folders and agents. Install the matching public key and set up an agent in each. |
| Config seems ignored | First match wins. Check that specific `Host` blocks come before `Host *`, and use `ssh -G`. |

## Debugging the config file

- Typos in directive names cause errors such as `Bad configuration option`. The message names the line.
- Check the effective result with `ssh -G alias`, for example `ssh -G ares | grep -iE 'user|hostname|identityfile|serveralive'`.
- Make a backup before experiments: `cp ~/.ssh/config ~/.ssh/config.bak`.

---
[Agenda](README.md) | Previous: [07 Register key](07-register-key-portal.md) | Next: [09 Advanced topics](09-qa-advanced-topics.md)
