FROM openshift/base-centos7

EXPOSE 8080

ENV STI_SCRIPTS_PATH=/usr/libexec/s2i/

LABEL io.k8s.description="Platform for building and running JEE applications on Tomcat" \
      io.k8s.display-name="Tomcat Builder" \
      io.openshift.tags="builder,tomcat" \
      io.openshift.s2i.destination="/opt/s2i/destination"


# Install Maven, Tomcat 8.5.24
RUN INSTALL_PKGS="tar java-1.8.0-openjdk java-1.8.0-openjdk-devel" && \
    yum install -y --enablerepo=centosplus $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum clean all -y && \
    rm -rf /spring/boot/* && \
    mkdir -p /opt/s2i/destination && \
    mkdir /tmp/src && \
    mkdir  /springboot/ && \
    mkdir /jar/ && \
    mkdir /logs	

# Add s2i customizations
# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
#COPY ./s2i/bin/ $STI_SCRIPTS_PATH
COPY ./s2i/bin/ /usr/libexec/s2i

RUN chmod -R a+rw /logs && \
    chmod -R a+rw /springboot && \
    chmod -R a+rw /jar && \
    chmod -R a+rw $HOME && \
    chmod -R +x $STI_SCRIPTS_PATH && \
    chmod -R g+rw /opt/s2i/destination

USER 1001

CMD $STI_SCRIPTS_PATH/usage
