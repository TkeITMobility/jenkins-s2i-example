# The FROM will be replaced when building in OpenShift
FROM rhel7/rhel

MAINTAINER Siamak Sadeghianfar <ssadeghi@redhat.com>

# Install headless Java
USER root
RUN yum repolist --disablerepo=* && \
    yum clean all && \
    yum-config-manager --disable rhel* 1>/dev/null && \
    yum-config-manager --enable rhel-7-server-rpms && \
    yum-config-manager --enable rhel-7-server-ose-3.2-rpms && \
    export INSTALL_PKGS="nss_wrapper java-1.8.0-openjdk \
        java-1.8.0-openjdk-devel nss_wrapper gettext tar git which \
        perl-XML-XPath" && \
    yum clean all && \
    yum install -y --setopt=tsflags=nodocs install $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum clean all && \
    curl -o ~/oc.tar.gz https://s3.amazonaws.com/oso-preview-docker-registry/client-tools/3.2/oc-3.2.1.3-1-linux.tar.gz && \
    cd /usr/bin && tar xvzf ~/oc.tar.gz && \
    mkdir -p /var/lib/jenkins && \
    chown -R 1001:0 /var/lib/jenkins && \
    chmod -R g+w /var/lib/jenkins

# Copy the entrypoint
COPY configuration/* /var/lib/jenkins/
USER 1001

# Run the JNLP client by default
# To use swarm client, specify "/var/lib/jenkins/run-swarm-client" as Command
ENTRYPOINT ["/var/lib/jenkins/run-jnlp-client"]
