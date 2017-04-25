# Docker Images Project

Create multiple Docker images for Jenkins.

## Adding images

- modify `template.yml` to your needs
- run `rake`

You can compile images for your local development with `rake 'default[true]'`

## Mounting images

- `docker run -p 2222:22 -v PATH_TO_SSH_KEYS:/home/jenkins/.ssh CONTAINER_ID`

## Gotchas

You will need to generate a SSH key-pair and replace the `./sshd/jenkins_worker.pub` file
