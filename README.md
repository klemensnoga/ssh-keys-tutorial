# SSH Keys for Cyfronet Users: Tutorial Agenda

Hands-on tutorial for new PLGrid/Cyfronet users: log in to **ares.cyfronet.pl** and **athena.cyfronet.pl** with SSH keys, keep sessions alive, and manage keys with an agent.

- **Part 1: Tutorial** (30 min), live demo with attendees following on their own laptops
- **Part 2: Q&A and backup topics** (30 min), hands-on help plus advanced questions

## Before the session

Attendees need:

- a PLGrid account with access to Ares and Athena (login of the form `plgXXXX`)
- a terminal: macOS Terminal, Linux terminal, or Windows PowerShell or WSL (see the note on PuTTY below)
- OpenSSH client available (`ssh -V` prints a version)

Quick-start materials: [one-page handout](handout.md) and [example SSH config](config.example).

> **Disclaimer: PuTTY.** We discourage using PuTTY for this tutorial and for Cyfronet access. It is not covered here: it uses its own key format and tools, so the commands and configuration in these materials do not apply to it, and we do not support it. Windows users should use the built-in OpenSSH client in PowerShell, or WSL.

## Part 1: Tutorial (30 min)

| Time | Topic | Exercise | Guide |
|---|---|---|---|
| 0:00-0:04 | First login and the `known_hosts` file | Observe the prompt, find the new entry | [01 First login](01-first-login-known-hosts.md) |
| 0:04-0:08 | Generating keys and which algorithm | **Ex. 1:** generate an Ed25519 key pair for Ares | [02 Generate keys](02-generate-keys.md) |
| 0:08-0:13 | Copy the public key to the remote host (`ssh-copy-id` and manual `scp`), first key login | **Ex. 1b:** install the key on Ares and log in with it | [03 Copy public key](03-copy-public-key.md) |
| 0:13-0:18 | Config file basics: multiple keys, keep-alive | **Ex. 2-3:** config for ares.cyfronet.pl, add keep-alive | [04 Config file](04-config-file.md) |
| 0:18-0:24 | SSH agent and making it persistent (macOS, Linux, Windows) | **Ex. 4:** start the agent, make it persistent | [05 SSH agent](05-ssh-agent.md) |
| 0:24-0:26 | A different key for athena.cyfronet.pl | **Ex. 5:** second key and config block | [06 Second key](06-second-key-athena.md) |
| 0:26-0:30 | Wrap-up: register the public key at portal.plgrid.pl, best practices, troubleshooting | **Ex. 6:** add public keys, test login | [07 Register key](07-register-key-portal.md), [08 Best practices](08-best-practices-troubleshooting.md) |

## Part 2: Q&A and backup (30 min)

| Time | Activity |
|---|---|
| 0:30-0:45 | Hands-on help: finish exercises, fix individual problems (Windows agent, portal registration, permissions) |
| 0:45-1:00 | Advanced questions, see [09 Backup topics](09-qa-backup-topics.md) |

## Exercise overview

1. Generate a key pair with an elliptic-curve algorithm (Ed25519)
   - **1b.** Copy the public key to Ares (`ssh-copy-id` or `scp`) and log in with it
2. Prepare a config file for ares.cyfronet.pl using the generated key
3. Add keep-alive
4. Start the agent and make it persistent
5. Use a different key for athena.cyfronet.pl
6. Add the public keys to your account at portal.plgrid.pl and test

## Facilitator notes

- Time is very tight with the copy-key step included (about 5 minutes). Consider shortening the config and agent sections, or moving the manual `scp` method to the Q&A part.
- Time is tight: share the [example config](config.example) so attendees can paste it and edit the username.
- Ask Windows attendees at the start which option they use (PowerShell or WSL) and split help during Q&A.
- Keep a spare Ed25519 test key pair for anyone blocked at the portal step.
- Before the session, verify against current Cyfronet/PLGrid documentation: the published host fingerprints, the exact portal menu for SSH keys, and any key-type requirements. These were not checked when this material was written.
- Send the [handout](handout.md) afterward.

## Contents

- [01 First login and known_hosts](01-first-login-known-hosts.md)
- [02 Generating keys](02-generate-keys.md)
- [03 Copy public key and first key login](03-copy-public-key.md)
- [04 Config file](04-config-file.md)
- [05 SSH agent](05-ssh-agent.md)
- [06 Second key for Athena](06-second-key-athena.md)
- [07 Register key at portal.plgrid.pl](07-register-key-portal.md)
- [08 Best practices and troubleshooting](08-best-practices-troubleshooting.md)
- [09 Q&A backup topics](09-qa-backup-topics.md)
- [Handout (one page)](handout.md)
- [Example SSH config](config.example)
