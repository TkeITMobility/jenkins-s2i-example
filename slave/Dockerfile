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
    yum-config-manager --enable rhel-7-server-extras-rpms && \
    export INSTALL_PKGS="nss_wrapper java-1.8.0-openjdk \
        java-1.8.0-openjdk-devel dejavu-sans-fonts nss_wrapper gettext tar git which \
        perl-XML-XPath skopeo" && \
    yum clean all && \
    yum install -y --setopt=tsflags=nodocs install $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum clean all && \
    curl -o ~/oc.tar.gz https://mirror.openshift.com/pub/openshift-v3/clients/3.4.1.18/linux/oc.tar.gz && \
    cd /usr/bin && tar xvzf ~/oc.tar.gz && \
    mkdir -p /var/lib/jenkins && \
    chown -R 1001:0 /var/lib/jenkins && \
    chmod -R g+w /var/lib/jenkins
    
RUN curl https://letsencrypt.org/certs/isrgrootx1.pem -o /etc/pki/ca-trust/source/anchors/isrgrootx1.pem && update-ca-trust

# Copy the entrypoint
COPY configuration/* /var/lib/jenkins/
USER 1001

# Run the JNLP client by default
# To use swarm client, specify "/var/lib/jenkins/run-swarm-client" as Command
ENTRYPOINT ["/var/lib/jenkins/run-jnlp-client"]
