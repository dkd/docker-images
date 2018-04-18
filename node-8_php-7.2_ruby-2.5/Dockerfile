FROM ubuntu:bionic-20180410
LABEL maintainer.first="Nicolai Reuschling 'nicolai.reuschling@dkd.de'"

# Add user jenkins to the image
RUN useradd -m -d /home/jenkins -s /bin/sh jenkins

# Add locales after locale-gen as needed
# Upgrade packages on image
# Preparations for sshd
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q update &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q upgrade -y -o Dpkg::Options::="--force-confnew" --no-install-recommends &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends locales &&\
    locale-gen en_US.UTF-8 de_DE.UTF-8 && localedef -i en_US -f UTF-8 en_US.UTF-8

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
      net-tools \
      # language-pack-de-base \
      openssh-server &&\
    mkdir -p /var/run/sshd

# Modify SSHD config
ADD sshd/sshd_config /etc/ssh/sshd_config

# Standard SSH port
EXPOSE 22

# local cache directory
RUN mkdir /home/jenkins/.ssh &&\
    chown -R jenkins:jenkins /home/jenkins/.ssh &&\
    chmod -R 0700 /home/jenkins/.ssh &&\
    mkdir /home/jenkins/composer-cache &&\
    chown -R jenkins:jenkins /home/jenkins/composer-cache &&\
    chmod -R 0700 /home/jenkins/composer-cache &&\
    mkdir /home/jenkins/.npm &&\
    chown -R jenkins:jenkins /home/jenkins/.npm &&\
    chmod -R 0700 /home/jenkins/.npm &&\
    mkdir /home/jenkins/bundler-cache &&\
    chown -R jenkins:jenkins /home/jenkins/bundler-cache &&\
    chmod -R 0700 /home/jenkins/bundler-cache

VOLUME ["/home/jenkins/.ssh", "/home/jenkins/composer-cache", "/home/jenkins/bundler-cache"]


# Install JDK (v8)
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
      openjdk-8-jre-headless

# Install PHP (php7.2-cli)
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends php7.2-cli            php7.2-bcmath          php7.2-bz2          php7.2-curl          php7.2-dba          php7.2-dom          php7.2-mbstring          php7.2-SimpleXML          php7.2-soap          php7.2-wddx          php7.2-xml          php7.2-xmlreader          php7.2-xmlwriter          php7.2-zip      
# Install Ruby (v2.5)
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
      apt-transport-https \
      ca-certificates \
      software-properties-common &&\
    add-apt-repository ppa:brightbox/ruby-ng &&\
    apt-get update -q

RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
      ruby2.5 \
      ruby2.5-dev

# Update RubyGems and install Bundler
RUN gem update --system  --no-doc --no-post-install-message && gem install bundler --version= --no-doc --no-post-install-message

# Install nodejs (v8.10.0)
RUN DEBIAN_FRONTEND="noninteractive" curl -SLO "https://nodejs.org/dist/v8.10.0/node-v8.10.0-linux-x64.tar.xz" &&\
    tar -xJf "node-v8.10.0-linux-x64.tar.xz" -C /usr/local --strip-components=1 &&\
    ln -s /usr/local/bin/node /usr/local/bin/nodejs &&\
    rm /node-v8.10.0-linux-x64.tar.xz


# Cleanup
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q autoremove &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q clean -y &&\
    rm -rf /var/lib/apt/lists/* &&\
    rm -f /var/cache/apt/*.bin

# Default command
CMD ["/usr/sbin/sshd", "-D"]
