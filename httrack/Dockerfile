FROM ubuntu:trusty-20170817
LABEL maintainer.first="Nicolai Reuschling 'nicolai.reuschling@dkd.de'" \
      maintainer.second="Michael Skrynski 'michael.skrynski@dkd.de'"

RUN DEBIAN_FRONTEND="noninteractive" apt-get -q update &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q upgrade -y -o Dpkg::Options::="--force-confnew" --no-install-recommends &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends locales
RUN locale-gen en_US.UTF-8 de_DE.UTF-8 && localedef -i en_US -f UTF-8 en_US.UTF-8

# Setting language
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

RUN DEBIAN_FRONTEND="noninteractive" apt-get -q update &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q upgrade -y -o Dpkg::Options::="--force-confnew" --no-install-recommends &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
      openjdk-8-jre-headless \
      httrack \
      git-core \
      openssh-server \
      lftp &&\
    mkdir -p /var/run/sshd

# Add user jenkins to the image
RUN useradd -m -d /home/jenkins -s /bin/sh jenkins

# Cleanup
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q autoremove &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q clean -y &&\
    rm -rf /var/lib/apt/lists/* &&\
    rm -f /var/cache/apt/*.bin

# Modify SSHD config
ADD sshd/sshd_config /etc/ssh/sshd_config

# Standard SSH port
EXPOSE 22

RUN mkdir /home/jenkins/.ssh
RUN chown -R jenkins:jenkins /home/jenkins/.ssh && chmod -R 0700 /home/jenkins/.ssh

# Mount volumes
VOLUME ["/home/jenkins/.ssh"]

# Default command
CMD ["/usr/sbin/sshd", "-D"]
