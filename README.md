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

Then connect to a server through the bastion:
```
ssh -A -o ProxyCommand='ssh -W %h:%p -p 2200 bastion@localhost' user@backendserver
```

In practice localhost would be the hostname of a machine running this container and
user@backendserver would be the user and hostname of the machine you wanted to connect.

## Updating SSH user config
To avoid using the long form ssh command above, you can add this option to be applied
by default in your ssh client user config. It's even possible to use wildcards for host to use the bastion as a proxy for an entire domain.

`~/.ssh/config` snippet:
```
Host foo
HostName foo
User me
ProxyCommand ssh -W %h:%p -p 2200 bastion@docker
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
