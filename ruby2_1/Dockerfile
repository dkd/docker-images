FROM ubuntu:14.04
LABEL maintainer.first="Nicolai Reuschling 'nicolai.reuschling@dkd.de'" \
      maintainer.second="Michael Skrynski 'michael.skrynski@dkd.de'"

# Add locales after locale-gen as needed
# Upgrade packages on image
# Preparations for sshd
# RUN locale-gen de_DE.UTF-8 &&\
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q update &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q upgrade -y -o Dpkg::Options::="--force-confnew" --no-install-recommends &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
      curl \
      git-core \
      libxslt-dev \
      libxml2-dev \
      libmysqlclient-dev \
      build-essential \
      sqlite3 \
      libsqlite3-dev \
      libcurl4-openssl-dev \
      imagemagick \
      libmagickwand-dev \
      # language-pack-de-base \
      openssh-server &&\
    mkdir -p /var/run/sshd

# Setting language
# ENV LANG de_DE.UTF-8
# ENV LANGUAGE de_DE:de
# ENV LC_ALL de_DE.UTF-8

# Install JDK 7 (latest edition)
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
      openjdk-7-jre-headless


# Install Ruby 2.1 (latest version)
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
      apt-transport-https \
      ca-certificates \
      software-properties-common &&\
    add-apt-repository ppa:brightbox/ruby-ng &&\
    apt-get update -q &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
      ruby2.1 \
      ruby2.1-dev \
      nodejs \
      npm

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

RUN mkdir /home/jenkins/.ssh && mkdir /home/jenkins/composer-cache && mkdir /home/jenkins/bundler-cache
RUN chown -R jenkins:jenkins /home/jenkins/.ssh &&\
    chmod -R 0700 /home/jenkins/.ssh &&\
    chown -R jenkins:jenkins /home/jenkins/composer-cache &&\
    chmod -R 0700 /home/jenkins/composer-cache &&\
    chown -R jenkins:jenkins /home/jenkins/bundler-cache &&\
    chmod -R 0700 /home/jenkins/bundler-cache

# Mount volumes
VOLUME ["/home/jenkins/.ssh", "/home/jenkins/composer-cache", "/home/jenkins/bundler-cache"]

# Default command
CMD ["/usr/sbin/sshd", "-D"]
