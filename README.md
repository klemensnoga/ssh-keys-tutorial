# SSH Keys for Cyfronet Users: Tutorial Agenda

Hands-on tutorial for new PLGrid/Cyfronet users: log in to **ares.cyfronet.pl** and **athena.cyfronet.pl** with SSH keys, keep sessions alive, and manage keys with an agent.

- **Part 1: Tutorial** (30 min), live demo with attendees following on their own laptops
- **Part 2: Q&A and advanced topics** (30 min), hands-on help plus advanced questions

## Before the session

Attendees need:

- a PLGrid account with access to Ares and Athena (login of the form `plgXXXX`)
- a terminal: macOS Terminal, Linux terminal, or Windows PowerShell or WSL (see the note on PuTTY below)
- OpenSSH client available (`ssh -V` prints a version)

Quick-start materials: [one-page handout](ares/handout.md) and [example SSH config](ares/config.example).

> **Disclaimer: PuTTY.** We discourage using PuTTY for this tutorial and for Cyfronet access. It is not covered here: it uses its own key format and tools, so the commands and configuration in these materials do not apply to it, and we do not support it. Windows users should use the built-in OpenSSH client in PowerShell, or WSL.

## Part 1: Tutorial (30 min)

| Time | Topic | Exercise | Guide |
|---|---|---|---|
| 0:00-0:04 | First login and the `known_hosts` file | Observe the prompt, find the new entry | [01 First login](01-first-login-known-hosts.md) |
| 0:04-0:08 | Generating keys and which algorithm | **Ex. 1:** generate an Ed25519 key pair for Ares | [02 Generate keys](02-generate-keys.md) |
| 0:08-0:13 | Register the public key (PLGrid portal, `ssh-copy-id` or manual `scp`), first key login | **Ex. 1b:** register the key for Ares and log in with it | [03 Register public key](03-copy-public-key.md) |
| 0:13-0:18 | Config file basics: multiple keys, keep-alive | **Ex. 2-3:** config for ares.cyfronet.pl, add keep-alive | [04 Config file](04-config-file.md) |
| 0:18-0:24 | SSH agent and making it persistent (macOS, Linux, Windows) | **Ex. 4:** start the agent, make it persistent | [05 SSH agent](05-ssh-agent.md) |
| 0:24-0:26 | A different key for athena.cyfronet.pl | **Ex. 5:** second key and config block | [06 Second key](06-second-key-athena.md) |
| 0:26-0:30 | Wrap-up: best practices, troubleshooting | Test `ssh ares` and `ssh athena` | [07 Best practices](07-best-practices-troubleshooting.md) |

## Part 2: Q&A and advanced topics (30 min)

| Time | Activity |
|---|---|
| 0:30-0:45 | Hands-on help: finish exercises, fix individual problems (Windows agent, portal registration, permissions) |
| 0:45-1:00 | Advanced questions, see [08 Q&A and advanced topics](08-qa-advanced-topics.md) |

## Contents

- [01 First login and known_hosts](01-first-login-known-hosts.md)
- [02 Generating keys](02-generate-keys.md)
- [03 Register public key (PLGrid portal) and first key login](03-copy-public-key.md)
- [04 Config file](04-config-file.md)
- [05 SSH agent](05-ssh-agent.md)
- [06 Second key for Athena](06-second-key-athena.md)
- [07 Best practices and troubleshooting](07-best-practices-troubleshooting.md)
- [08 Q&A and advanced topics](08-qa-advanced-topics.md)
- [Handout (one page)](ares/handout.md), also as [PDF](ares/handout.pdf)
- [Example SSH config](ares/config.example)
- [README for Ares](ares/README.Ares.md): where to find these materials and the example config
- [Trainer notes](trainer-notes.md)
