FROM phusion/baseimage:0.9.20

ARG GO_VERSION="17.2.0-4587"
ARG DOWNLOAD_URL="https://download.gocd.io/binaries"

# Add proxy settings for apt
ADD https://download.gocd.io/binaries/17.2.0-4587/deb/go-agent_17.2.0-4587_all.deb /tmp/go-agent.deb

WORKDIR /tmp
RUN update-ca-certificates && mkdir /etc/service/go-agent
ADD go-agent-start.sh /etc/service/go-agent/run
RUN chmod +x /etc/service/go-agent/run

# Install docker, git, jdk7, jdk8, maven, python tools, sudo, unzip
RUN add-apt-repository -y ppa:openjdk-r/ppa && \
    apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D && \
    echo "deb https://apt.dockerproject.org/repo ubuntu-xenial main" | tee /etc/apt/sources.list.d/docker.list && \
    apt-get update && \
    apt-get install -y -q docker-engine=1.12.6-0~ubuntu-xenial    \
                          git                                     \
                          maven                                   \
                          openjdk-8-jdk-headless                  \
                          openjdk-7-jdk                           \
                          python                                  \
                          python-pip                              \
                          python3-pip                             \
                          python-yaml                             \
                          sudo                                    \
                          unzip &&                                \
    pip install awscli          \
                beautifulsoup4  \
                boto            \
                dateparser      \
                click &&        \
    pip3 install awscli         \
                 beautifulsoup4 \
                 boto           \
                 dateparser     \
                 click &&       \
    dpkg -i /tmp/go-agent.deb &&                              \
    sed -i -e 's/DAEMON=Y/DAEMON=N/' /etc/default/go-agent && \
    echo "go ALL=NOPASSWD: ALL" >> /etc/sudoers            

# Install rsc, rancher-compose, jq, pup, packer, terraform, ruby tools
RUN curl -k https://binaries.rightscale.com/rsbin/rsc/v6/rsc-linux-amd64.tgz | tar -zxf - -O rsc/rsc > /usr/local/bin/rsc &&                                     \
	  chmod +755 /usr/local/bin/rsc &&                                                                                                                             \
                                                                                                                                                                 \
    curl -LkSs https://github.com/rancher/rancher-compose/releases/download/v0.12.1/rancher-compose-linux-amd64-v0.12.1.tar.gz > /root/rancher-compose.tar.gz && \
    tar -zxvf /root/rancher-compose.tar.gz && cp ./rancher-compose-v*/rancher-compose /usr/local/bin &&                                                          \
                                                                                                                                                                 \
    curl -LkSs https://github.com/stedolan/jq/releases/download/jq-1.5/jq-linux64 > /usr/local/bin/jq &&                                                         \
	  chmod +x /usr/local/bin/jq &&                                                                                                                                \
                                                                                                                                                                 \
    curl -LSs https://github.com/ericchiang/pup/releases/download/v0.4.0/pup_v0.4.0_linux_amd64.zip > /tmp/pup.zip &&                                            \
    unzip -d /usr/local/bin /tmp/pup.zip && chmod +x /usr/local/bin/pup && rm -Rf /tmp/pup.zip &&                                                                \
                                                                                                                                                                 \
    curl -sSL https://releases.hashicorp.com/packer/1.0.0/packer_1.0.0_linux_amd64.zip > /tmp/packer.zip &&                                                      \
    unzip -d /usr/local/bin /tmp/packer.zip && chmod +x /usr/local/bin/packer && rm -Rf /tmp/packer.zip &&                                                       \
                                                                                                                                                                 \
    curl -sSL https://releases.hashicorp.com/terraform/0.9.3/terraform_0.9.3_linux_amd64.zip > /tmp/terraform.zip &&                                             \
    unzip -d /usr/local/bin /tmp/terraform.zip && chmod +x /usr/local/bin/terraform && rm -Rf /tmp/terraform.zip &&                                              \
                                                                                                                                                                 \
    curl -sSL https://get.rvm.io | bash -s stable --ruby && /bin/bash -c "source /usr/local/rvm/scripts/rvm &&                                                   \
    gem install bigdecimal bson bson_ext json mime-types minitest netrc rake rdoc rest-client rubygame" &&                                                       \
    echo "source /usr/local/rvm/scripts/rvm" >> /var/go/.bashrc
 l5
## Need this to fix issues with mktemp ## and clean apt cache
RUN chmod -R 777 /tmp && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Change to user go
USER go

# Install node tools
RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash && /bin/bash -i -c "nvm install node"

USER root

CMD ["/sbin/my_init"]