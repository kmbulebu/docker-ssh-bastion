# Sample Usage

Steps:
1. Add your SSH public keys to authorized_keys.

2. Run `docker-compose up` to build a derivative container and start the ssh server on port 2200.

New SSH host keys will be generated as part of the Docker build and your authorized keys will be added.

## docker-compose.yml notes

Additional security hardening was applied through the docker-compose.yml file.
* Unneeded kernel security capabilities were removed.
* The container filesystem is mounted read-only.
* tmpfs filesystem was used to give write permissions for temporary files.
* Resource constraints reduce expose to host exhaustion.
