# dkdde/gitlab-ci-worker:node-12_php-7.4_ruby-2.7
FROM ubuntu:bionic-20200526

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
      build-essential \
      sqlite3 \
      libsqlite3-dev \
      libcurl4-openssl-dev \
      imagemagick \
      libmagickwand-dev \
      net-tools \
      libssl-dev \
      libgtk2.0-0 \
      libgtk-3-0 \
      libnotify-dev \
      libgconf-2-4 \
      libnss3 \
      libxss1 \
      libasound2 \
      libxtst6 \
      xauth \
      xvfb \
      libmariadb-dev

# Install Git
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
      apt-transport-https \
      ca-certificates \
      software-properties-common &&\
    add-apt-repository ppa:git-core/ppa &&\
    apt-get update -q &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends git

RUN DEBIAN_FRONTEND="noninteractive" add-apt-repository ppa:ondrej/php &&\
    DEBIAN_FRONTEND="noninteractive" apt-get update -q

# Install PHP (php7.4-cli)
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends php7.4-cli \
    php7.4-bcmath php7.4-bz2 php7.4-curl php7.4-dba php7.4-dom php7.4-gd php7.4-intl php7.4-mbstring php7.4-mysql php7.4-SimpleXML php7.4-soap php7.4-xml php7.4-xmlreader php7.4-xmlwriter php7.4-zip
RUN curl -SLO https://getcomposer.org/download/1.10.8/composer.phar &&\
    mv composer.phar /usr/local/bin/composer &&\
    chmod +x /usr/local/bin/composer

# Install Ruby (v2.7)
RUN add-apt-repository ppa:brightbox/ruby-ng &&\
    apt-get update -q &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
      ruby2.7 \
      ruby2.7-dev

# Install Rake, update RubyGems and install Bundler
RUN gem install rake --version=13.0.1 --no-doc --no-post-install-message &&\
    gem update --system 3.1.4 --no-doc --no-post-install-message &&\
    gem install bundler --version=2.1.4 --no-doc --no-post-install-message &&\
    gem install bundler --version=1.17.3 --no-doc --no-post-install-message &&\
    gem install bundler --version=2.0.2 --no-doc --no-post-install-message &&\
    gem list

# Install NodeJS (v12.18.1)
RUN curl -SLO "https://nodejs.org/dist/v12.18.1/node-v12.18.1-linux-x64.tar.xz" &&\
    tar -xJf "node-v12.18.1-linux-x64.tar.xz" -C /usr/local --strip-components=1 &&\
    ln -s /usr/local/bin/node /usr/local/bin/nodejs &&\
    rm /node-v12.18.1-linux-x64.tar.xz

# Install npm (v6.14.5)
RUN npm install -g npm@6.14.5

# Cleanup
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q autoremove &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q clean -y &&\
    rm -rf /var/lib/apt/lists/* &&\
    rm -f /var/cache/apt/*.bin