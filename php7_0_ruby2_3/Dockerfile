FROM ubuntu:16.04
LABEL maintainer.first="Nicolai Reuschling 'nicolai.reuschling@dkd.de'" \
      maintainer.second="Michael Skrynski 'michael.skrynski@dkd.de'"

# Add locales after locale-gen as needed
# Upgrade packages on image
# Preparations for sshd

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
      curl \
      locales \
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

# Install JDK 8 (latest edition)
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
      openjdk-8-jre-headless

# Install PHP 7.0 (latest version)
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends php7.0                        php7.0-bcmath                  php7.0-bz2                  php7.0-curl                  php7.0-dba                  php7.0-dom                  php7.0-mbstring                  php7.0-SimpleXML                  php7.0-soap                  php7.0-wddx                  php7.0-xml                  php7.0-xmlreader                  php7.0-xmlwriter                  php7.0-zip              
# Install Ruby 2.3 (latest version)
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
      ruby2.3 \
      ruby2.3-dev \
      nodejs \
      npm

# Update RubyGems and install Bundler
RUN gem update --system && gem install bundler --version=1.15.1 --no-doc

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
