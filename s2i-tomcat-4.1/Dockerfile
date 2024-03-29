FROM openshift/base-centos7

EXPOSE 8080

ENV TOMCAT_VERSION=8.5.45 \
    MAVEN_VERSION=3.5.4 \
    STI_SCRIPTS_PATH=/usr/libexec/s2i/

LABEL io.k8s.description="Platform for building and running JEE applications on Tomcat" \
      io.k8s.display-name="Tomcat Builder" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,tomcat" \
      io.openshift.s2i.destination="/opt/s2i/destination"

COPY apache-maven-3.5.4-bin.tar.gz /
COPY apache-tomcat-8.5.45.tar.gz /

# Install Maven, Tomcat 8.5.24
RUN INSTALL_PKGS="tar java-1.8.0-openjdk java-1.8.0-openjdk-devel" && \
    yum install -y --enablerepo=centosplus $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum clean all -y && \
    tar -xvf /apache-maven-3.5.4-bin.tar.gz    -C /usr/local && \
    ln -sf /usr/local/apache-maven-$MAVEN_VERSION/bin/mvn /usr/local/bin/mvn && \
    mkdir -p $HOME/.m2 && \
    mkdir -p /tomcat && \
    tar -xvf /apache-tomcat-8.5.45.tar.gz --strip-components=1 -C /tomcat && \
    rm -rf /tomcat/webapps/* && \
    mkdir -p /opt/s2i/destination && \
    mkdir /tmp/src && \
    mkdir -p /opt/maven/repository/ && \
    chmod 777 /opt/maven/repository

# Add s2i customizations
ADD ./settings.xml $HOME/.m2/
COPY ROOT.war /tomcat/webapps/
# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
#COPY ./s2i/bin/ $STI_SCRIPTS_PATH
COPY ./s2i/bin/ /usr/libexec/s2i

RUN chmod -R 777 /tomcat && \
    chmod -R a+rw $HOME && \
    chmod -R +x $STI_SCRIPTS_PATH && \
    chmod -R g+rw /opt/s2i/destination

USER 185

CMD $STI_SCRIPTS_PATH/usage
