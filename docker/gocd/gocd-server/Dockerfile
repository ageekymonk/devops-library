FROM phusion/baseimage:0.9.20

ARG GO_VERSION="17.4.0-4892"
ARG DOWNLOAD_URL="https://download.gocd.io/binaries"

RUN rm -rf /etc/service/sshd /etc/my_init.d/00_regen_ssh_host_keys.sh \
    && apt-get update \
    && apt-get install -y -q unzip openjdk-8-jre-headless git mercurial subversion \
    && mkdir /etc/service/go-server

ADD go-common-scripts.sh /etc/service/go-server/go-common-scripts.sh
ADD go-server-start.sh /etc/service/go-server/run
RUN chmod +x /etc/service/go-server/run /etc/service/go-server/go-common-scripts.sh

ADD $DOWNLOAD_URL/$GO_VERSION/deb/go-server_${GO_VERSION}_all.deb /tmp/go-server.deb

RUN groupadd -r go \
    && useradd -r -c "Go User" -g "go" -d "/var/go" -m -s "/bin/bash" go \
    && mkdir -p /var/lib/go-server/addons /var/log/go-server /etc/go /go-addons /plugins /var/lib/go-server/plugins \
    && ln -sf /plugins /var/lib/go-server/plugins/external \
    && chown -R go:go /var/lib/go-server /var/log/go-server /etc/go /go-addons /var/go /plugins

WORKDIR /tmp
RUN dpkg -i --debug=10 /tmp/go-server.deb \
    && sed -i -e 's/DAEMON=Y/DAEMON=N/' /etc/default/go-server \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

VOLUME ["/var/lib/go-server", "/var/log/go-server", "/etc/go", "/go-addons", "/var/go"]

EXPOSE 8153 8154

CMD ["/sbin/my_init"]
