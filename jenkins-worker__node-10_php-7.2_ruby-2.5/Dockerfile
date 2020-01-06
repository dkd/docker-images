# dkdde/jenkins-worker:node-10_php-7.2_ruby-2.5
FROM ubuntu:bionic-20191202

LABEL maintainer.first="Nicolai Reuschling 'nicolai.reuschling@dkd.de'"

# Setup environment
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# Add user jenkins
RUN useradd -m -d /home/jenkins -s /bin/sh jenkins

# Setup locale
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q update &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q upgrade -y -o Dpkg::Options::="--force-confnew" --no-install-recommends &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends locales &&\
    locale-gen en_US.UTF-8 de_DE.UTF-8 && localedef -i en_US -f UTF-8 en_US.UTF-8

# for testing purposes: DEBIAN_FRONTEND="noninteractive" apt-get -q upgrade -y -o Dpkg::Options::="--force-confnew" --no-install-recommends &&\
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
      openssh-client \
      wget \
      curl \
      unzip \
      locales \
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
      libssl-dev

# Install Git
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
      apt-transport-https \
      ca-certificates \
      software-properties-common &&\
    add-apt-repository ppa:git-core/ppa &&\
    apt-get update -q &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends git

# Create local cache directories
RUN mkdir -p {/home/jenkins/.ssh,/home/jenkins/composer-cache,/home/jenkins/.npm,/home/jenkins/bundler-cache} &&\
    chown -R jenkins:jenkins {/home/jenkins/.ssh,/home/jenkins/composer-cache,/home/jenkins/.npm,/home/jenkins/bundler-cache} &&\
    chmod -R 0700 {/home/jenkins/.ssh,/home/jenkins/composer-cache,/home/jenkins/.npm,/home/jenkins/bundler-cache}


# Install JDK (v8)
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
      openjdk-8-jre-headless


# Install PHP (php7.2-cli)
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends php7.2-cli \
    php7.2-bcmath php7.2-bz2 php7.2-curl php7.2-dba php7.2-dom php7.2-gd php7.2-intl php7.2-mbstring php7.2-mysql php7.2-SimpleXML php7.2-soap php7.2-wddx php7.2-xml php7.2-xmlreader php7.2-xmlwriter php7.2-zip

# Install Ruby (v2.5)
RUN add-apt-repository ppa:brightbox/ruby-ng &&\
    apt-get update -q &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
      ruby2.5 \
      ruby2.5-dev

# Install Rake, update RubyGems and install Bundler
RUN gem install rake --version=13.0.1 --no-doc --no-post-install-message &&\
    gem update --system 3.1.2 --no-doc --no-post-install-message &&\
    gem install bundler --version=2.1.4 --no-doc --no-post-install-message &&\
    gem install bundler --version=2.0.2 --no-doc --no-post-install-message &&\
    gem install bundler --version=1.17.3 --no-doc --no-post-install-message &&\
    gem list

# Install NodeJS (v10.18.0)
RUN curl -SLO "https://nodejs.org/dist/v10.18.0/node-v10.18.0-linux-x64.tar.xz" &&\
    tar -xJf "node-v10.18.0-linux-x64.tar.xz" -C /usr/local --strip-components=1 &&\
    ln -s /usr/local/bin/node /usr/local/bin/nodejs &&\
    rm /node-v10.18.0-linux-x64.tar.xz

# Install npm (v6.13.4)
RUN npm install -g npm@6.13.4

# Cleanup
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q autoremove &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q clean -y &&\
    rm -rf /var/lib/apt/lists/* &&\
    rm -f /var/cache/apt/*.bin
