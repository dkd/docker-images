# This Dockerfile is used to build an image containing basic stuff to be used as a Jenkins slave build node.
FROM dkdde/docker-jenkins-slave:base
LABEL maintainer.orig="Ervin Varga <ervin.varga@gmail.com>" \
      maintainer.first="Nicolai Reuschling 'nicolai.reuschling@dkd.de'" \
      maintainer.second="Michael Skrynski 'michael.skrynski@dkd.de'"

# Install httrack
RUN apt-get update -q &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends httrack &&\
    apt-get -q clean -y && rm -rf /var/lib/apt/lists/* && rm -f /var/cache/apt/*.bin