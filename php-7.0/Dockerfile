# This Dockerfile is used to build an image containing basic stuff to be used as a Jenkins slave build node.
FROM dkdde/docker-jenkins-slave:base
LABEL maintainer.orig="Ervin Varga <ervin.varga@gmail.com>" \
      maintainer.first="Nicolai Reuschling 'nicolai.reuschling@dkd.de'" \
      maintainer.second="Michael Skrynski 'michael.skrynski@dkd.de'"

# Install PHP 7.0 (latest version)
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - &&\
    add-apt-repository ppa:ondrej/php &&\
    apt-get update -q &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends php7.0 php7.0-bcmath php7.0-bz2 php7.0-curl php7.0-dba php7.0-dom php7.0-mbstring php7.0-SimpleXML php7.0-soap php7.0-wddx php7.0-xml php7.0-xmlreader php7.0-xmlwriter php7.0-zip &&\
    apt-get -q clean -y && rm -rf /var/lib/apt/lists/* && rm -f /var/cache/apt/*.bin
