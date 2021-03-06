FROM openshift/base-centos7

MAINTAINER OpenShift Development <dev@lists.openshift.redhat.com>

EXPOSE 5601

ENV AOP_KIBANA_PLUGIN_REPO=https://github.com/openshift/origin-kibana \
    AOP_KIBANA_PLUGIN_VER=v4.5.1-2 \
    ES_HOST=localhost \
    ES_PORT=9200 \
    HOME=/opt/app-root/src \
    KIBANA_HOME=/opt/kibana \
    KIBANA_CONF_DIR=/opt/kibana/config \
    KIBANA_VER=4.6.4 \
    NODE_BIN=/opt/kibana/node/bin/node \
    RELEASE_STREAM=origin

LABEL io.k8s.description="Kibana container for querying Elasticsearch for aggregated logs" \
      io.k8s.display-name="Kibana" \
      io.openshift.expose-services="5601:http" \
      io.openshift.tags="logging,elk,kibana"

ADD kibana.repo /etc/yum.repos.d/kibana.repo
# install the RPMs in a separate step so it can be cached
RUN rpm --import https://packages.elastic.co/GPG-KEY-elasticsearch && \
    INSTALLED_PKGS="kibana-${KIBANA_VER}" && \
    yum install -y --setopt=tsflags=nodocs  ${INSTALLED_PKGS} && \
    yum clean all

ADD probe/ /usr/share/kibana/probe/
ADD kibana.yml ${KIBANA_CONF_DIR}/
ADD run.sh install.sh prep-install.${RELEASE_STREAM} ${HOME}/
RUN ${HOME}/install.sh

WORKDIR ${HOME}
CMD ["./run.sh"]
