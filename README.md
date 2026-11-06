# SSH Keys for Cyfronet Users: Tutorial Agenda

Hands-on tutorial for new PLGrid/Cyfronet users: log in to **ares.cyfronet.pl** and **athena.cyfronet.pl** with SSH keys, keep sessions alive, and manage keys with an agent.

- **Part 1: Tutorial** (30 min), live demo with attendees following on their own laptops
- **Part 2: Q&A and backup topics** (30 min), hands-on help plus advanced questions

## Before the session

Attendees need:

- a PLGrid account with access to Ares and Athena (login of the form `plgXXXX`)
- a terminal: macOS Terminal, Linux terminal, or Windows PowerShell / WSL / PuTTY
- OpenSSH client available (`ssh -V` prints a version)

Quick-start materials: [one-page handout](handout.md) and [example SSH config](config.example).

## Part 1: Tutorial (30 min)

| Time | Topic | Exercise | Guide |
|---|---|---|---|
| 0:00-0:05 | First login and the `known_hosts` file | Observe the prompt, find the new entry | [01 First login](01-first-login-known-hosts.md) |
| 0:05-0:10 | Generating keys and which algorithm | **Ex. 1:** generate an Ed25519 key pair for Ares | [02 Generate keys](02-generate-keys.md) |
| 0:10-0:16 | Config file basics: multiple keys, keep-alive | **Ex. 2-3:** config for ares.cyfronet.pl, add keep-alive | [03 Config file](03-config-file.md) |
| 0:16-0:23 | SSH agent and making it persistent (macOS, Linux, Windows) | **Ex. 4:** start the agent, make it persistent | [04 SSH agent](04-ssh-agent.md) |
| 0:23-0:26 | A different key for athena.cyfronet.pl | **Ex. 5:** second key and config block | [05 Second key](05-second-key-athena.md) |
| 0:26-0:30 | Wrap-up: register the public key at portal.plgrid.pl, best practices, troubleshooting | **Ex. 6:** add public keys, test login | [06 Register key](06-register-key-portal.md), [07 Best practices](07-best-practices-troubleshooting.md) |

## Part 2: Q&A and backup (30 min)

| Time | Activity |
|---|---|
| 0:30-0:45 | Hands-on help: finish exercises, fix individual problems (Windows agent, portal registration, permissions) |
| 0:45-1:00 | Advanced questions, see [08 Backup topics](08-qa-backup-topics.md) |

## Exercise overview

1. Generate a key pair with an elliptic-curve algorithm (Ed25519)
2. Prepare a config file for ares.cyfronet.pl using the generated key
3. Add keep-alive
4. Start the agent and make it persistent
5. Use a different key for athena.cyfronet.pl
6. Add the public keys to your account at portal.plgrid.pl and test

## Facilitator notes

- Time is tight: share the [example config](config.example) so attendees can paste it and edit the username.
- Ask Windows attendees at the start which option they use (PowerShell, WSL, PuTTY) and split help during Q&A.
- Keep a spare Ed25519 test key pair for anyone blocked at the portal step.
- Before the session, verify against current Cyfronet/PLGrid documentation: the published host fingerprints, the exact portal menu for SSH keys, and any key-type requirements. These were not checked when this material was written.
- Send the [handout](handout.md) afterward.

## Contents

- [01 First login and known_hosts](01-first-login-known-hosts.md)
- [02 Generating keys](02-generate-keys.md)
- [03 Config file](03-config-file.md)
- [04 SSH agent](04-ssh-agent.md)
- [05 Second key for Athena](05-second-key-athena.md)
- [06 Register key at portal.plgrid.pl](06-register-key-portal.md)
- [07 Best practices and troubleshooting](07-best-practices-troubleshooting.md)
- [08 Q&A backup topics](08-qa-backup-topics.md)
- [Handout (one page)](handout.md)
- [Example SSH config](config.example)
