# Purpose
This container image provides an SSH server such that remote clients can connect
_through_ this server to other SSH servers. This is often necessary in high
security environments where firewalls, routing, and addressing prevent direct
access to the desired servers.

# Sample Usage

This image is not intended to be used directly. You should build upon it to
generate unique SSH host keys and add your SSH public keys to the `bastion` user's
authorized_keys file. See `sample_usage` directory for an example of how to run this
container.

```
cd sample_usage
cat ~/.ssh/id_rsa.pub >> authorized_keys
docker-compose up
```

# Notes

* Only support proxying to other SSH hosts.
* Only support SSH public key authentication.
* Lock down ssh daemon.
  * No shell
  * No TTY
  * No password authentication
  * Limit users
  * Stricter rules
* Single user: `bastion`
* Leverage Docker security features.
