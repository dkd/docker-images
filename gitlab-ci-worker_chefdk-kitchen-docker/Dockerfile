FROM chef/chefdk
LABEL maintainer="Ivan Golman <ivan.golman@dkd.de>, dkd Internet Service GmbH."

ENV CHEF_LICENSE="accept-silent"

RUN chef gem install kitchen-docker rubocop overcommit 

RUN apt-get update && \
    apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common \
    xz-utils

ARG scversion="stable"
RUN wget -qO- "https://github.com/koalaman/shellcheck/releases/download/${scversion?}/shellcheck-${scversion?}.linux.x86_64.tar.xz" | tar -xJv && cp "shellcheck-${scversion}/shellcheck" /usr/bin/
 
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
RUN add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) \
    stable"

RUN apt-get update && \
    apt-get install -y docker-ce-cli && \
    apt-get clean && apt-get autoclean && apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/log/*log /var/log/apt/* /var/lib/dpkg/*-old /var/cache/debconf/*-old
