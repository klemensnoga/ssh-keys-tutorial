# SSH keys tutorial: materials

The full tutorial "SSH Keys for Cyfronet Users" (logging in to Ares, Athena and Helios with SSH keys, the config file, the SSH agent, troubleshooting) is on GitHub:

**https://github.com/klemensnoga/ssh-keys-tutorial**

Start with the agenda in [README.md](https://github.com/klemensnoga/ssh-keys-tutorial/blob/main/README.md) or the one-page [handout](https://github.com/klemensnoga/ssh-keys-tutorial/blob/main/handout.md).

## Example SSH config

A ready-to-use `~/.ssh/config` for Ares and Athena: [config.example](https://github.com/klemensnoga/ssh-keys-tutorial/blob/main/config.example)

The config belongs on **your own computer**, not on Ares. To download it there (macOS, Linux, WSL):

```
curl -o ~/.ssh/config.example https://raw.githubusercontent.com/klemensnoga/ssh-keys-tutorial/main/config.example
```

Then copy the parts you need into `~/.ssh/config`, replace `plguser` with your PLGrid login, and run `chmod 600 ~/.ssh/config`.
