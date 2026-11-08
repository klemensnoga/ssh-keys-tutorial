# Trainer notes

[Agenda](README.md)

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
- Time is tight: share the [example config](ares/config.example) so attendees can paste it and edit the username.
- Ask Windows attendees at the start which option they use (PowerShell or WSL) and split help during Q&A.
- Keep a spare Ed25519 test key pair for anyone blocked at the portal step.
- Before the session, verify against current Cyfronet/PLGrid documentation: the published host fingerprints, the exact portal menu for SSH keys, and any key-type requirements. These were not checked when this material was written.
- Send the [handout](ares/handout.md) afterward.

---
[Agenda](README.md)
