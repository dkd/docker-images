# dkdde/gitlab-ci-worker:node-10_php-7.0_ruby-2.3
FROM ubuntu:xenial-20191212

LABEL maintainer.first="Nicolai Reuschling 'nicolai.reuschling@dkd.de'"

# Setup environment
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

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


# Install PHP (php7.0-cli)
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends php7.0-cli \
    php7.0-bcmath php7.0-bz2 php7.0-curl php7.0-dba php7.0-dom php7.0-gd php7.0-intl php7.0-mbstring php7.0-SimpleXML php7.0-soap php7.0-wddx php7.0-xml php7.0-xmlreader php7.0-xmlwriter php7.0-zip

# Install Ruby (v2.3)
RUN add-apt-repository ppa:brightbox/ruby-ng &&\
    apt-get update -q &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
      ruby2.3 \
      ruby2.3-dev

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
