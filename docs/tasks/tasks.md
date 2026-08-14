# Tasks

Actionable checklist derived from [requirements](../requirements/requirements.md).

## 1. Provision a remote Linux server

- [ ] Choose a cloud provider (e.g. DigitalOcean, AWS, Linode, Hetzner)
- [ ] Create an account and verify billing / free credits if applicable
- [ ] Launch a Linux instance (e.g. Ubuntu droplet / EC2 instance)
- [ ] Note the server public IP address and default login user (often `root` or `ubuntu`)
- [ ] Confirm the instance is running and reachable (e.g. ping or provider dashboard status)

## 2. Create two SSH key pairs locally

- [ ] Generate the first key pair: `ssh-keygen -t ed25519 -f ~/.ssh/<key1-name> -C "<comment>"`
- [ ] Generate the second key pair: `ssh-keygen -t ed25519 -f ~/.ssh/<key2-name> -C "<comment>"`
- [ ] Confirm both private keys (`.ssh/<key1-name>`, `.ssh/<key2-name>`) and public keys (`.pub`) exist locally
- [ ] Set appropriate permissions on private keys: `chmod 600 ~/.ssh/<key-name>`

## 3. Add both public keys to the server

- [ ] Connect to the server once (provider console, initial root password, or provider-injected key)
- [ ] Add the first public key to `~/.ssh/authorized_keys` on the server (or use provider UI / `ssh-copy-id`)
- [ ] Add the second public key to the same `~/.ssh/authorized_keys` file
- [ ] Ensure server permissions are correct:
  - [ ] `chmod 700 ~/.ssh`
  - [ ] `chmod 600 ~/.ssh/authorized_keys`

## 4. Verify SSH access with both keys

- [ ] Connect using the first key:
  ```bash
  ssh -i <path-to-private-key-1> user@server-ip
  ```
- [ ] Disconnect and connect using the second key:
  ```bash
  ssh -i <path-to-private-key-2> user@server-ip
  ```
- [ ] Confirm both sessions land on the server without password prompts (key auth only)

## 5. Configure `~/.ssh/config` for alias-based access

- [ ] Create or edit `~/.ssh/config` on your local machine
- [ ] Add a `Host` entry for the first key (alias, hostname, user, identity file)
- [ ] Add a `Host` entry for the second key (separate alias, same hostname/user, different identity file)
- [ ] Set config file permissions: `chmod 600 ~/.ssh/config`
- [ ] Connect using each alias:
  ```bash
  ssh <alias-1>
  ssh <alias-2>
  ```

## 6. Document the solution

- [ ] Write steps taken in `solution.md` at the repo root (provider used, key names, config snippets — no secrets)
- [ ] Confirm no private keys or secrets are committed to the repository
- [ ] Verify `.gitignore` excludes private key files if any keys live inside the project directory

## Stretch goal: Harden with fail2ban

- [ ] Install fail2ban on the server (e.g. `apt install fail2ban` on Debian/Ubuntu)
- [ ] Enable and start the fail2ban service
- [ ] Configure an SSH jail (default or custom) to ban repeated failed login attempts
- [ ] Test that fail2ban is active: `sudo fail2ban-client status sshd` (or equivalent jail name)
