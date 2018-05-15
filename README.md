# Docker Images Project

Create multiple Docker images for Jenkins.

## Adding images

- modify `template.yml` to your needs
- run `rake`

## Mounting images

- `docker run -p 2222:22 -v PATH_TO_SSH_KEYS:/home/jenkins/.ssh CONTAINER_ID`
