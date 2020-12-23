# dkdde/jenkins-worker:node-12_ruby-2.7
FROM ubuntu:bionic-20201119

LABEL maintainer.first="Nicolai Reuschling 'nicolai.reuschling@dkd.de'"

# Setup environment
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US:en
ENV LC_ALL=en_US.UTF-8

# Setup locale
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q update &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q upgrade -y -o Dpkg::Options::="--force-confnew" --no-install-recommends &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends locales &&\
    locale-gen en_US.UTF-8 de_DE.UTF-8 && localedef -i en_US -f UTF-8 en_US.UTF-8

# for testing purposes: DEBIAN_FRONTEND="noninteractive" apt-get -q upgrade -y -o Dpkg::Options::="--force-confnew" --no-install-recommends &&\
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
      build-essential \
      curl \
      imagemagick \
      libasound2 \
      libcurl4-openssl-dev \
      libgconf-2-4 \
      libgtk-3-0 \
      libgtk2.0-0 \
      libmagickwand-dev \
      libnotify-dev \
      libnss3 \
      libsqlite3-dev \
      libssl-dev \
      libxml2-dev \
      libxslt-dev \
      libxss1 \
      libxtst6 \
      locales \
      net-tools \
      openssh-client \
      sqlite3 \
      unzip \
      wget \
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



# Install Ruby (v2.7)
RUN add-apt-repository ppa:brightbox/ruby-ng &&\
    apt-get update -q &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
      ruby2.7 \
      ruby2.7-dev

# Update RubyGems and install Bundler
RUN rm -f /usr/lib/ruby/vendor_ruby/rubygems/defaults/operating_system.rb
RUN gem update --system 3.2.3 --no-doc --no-post-install-message &&\
    gem install bundler --version=2.2.2 --no-doc --no-post-install-message &&\
    gem install bundler --version=1.17.3 --no-doc --no-post-install-message &&\
    gem install bundler --version=2.0.2 --no-doc --no-post-install-message &&\
    gem install bundler --version=2.1.4 --no-doc --no-post-install-message &&\
    gem list

# Install NodeJS (v12.20.0)
RUN curl -SLO "https://nodejs.org/dist/v12.20.0/node-v12.20.0-linux-x64.tar.xz" &&\
    tar -xJf "node-v12.20.0-linux-x64.tar.xz" -C /usr/local --strip-components=1 &&\
    ln -s /usr/local/bin/node /usr/local/bin/nodejs &&\
    rm /node-v12.20.0-linux-x64.tar.xz

# Install npm (v6.14.10)
RUN npm install -g npm@6.14.10


# Install JDK (v8)
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
      openjdk-8-jre-headless

# Add user jenkins
RUN useradd -m -d /home/jenkins -s /bin/sh jenkins

# Create local cache directories
RUN mkdir -p {/home/jenkins/.ssh,/home/jenkins/composer-cache,/home/jenkins/.npm,/home/jenkins/bundler-cache,/home/jenkins/.cache} &&\
    chown -R jenkins:jenkins {/home/jenkins/.ssh,/home/jenkins/composer-cache,/home/jenkins/.npm,/home/jenkins/bundler-cache,/home/jenkins/.cache} &&\
    chmod -R 0700 {/home/jenkins/.ssh,/home/jenkins/composer-cache,/home/jenkins/.npm,/home/jenkins/bundler-cache,/home/jenkins/.cache}

# Cleanup
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q autoremove &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q clean -y &&\
    rm -rf /var/lib/apt/lists/* &&\
    rm -f /var/cache/apt/*.bin
