FROM ubuntu

LABEL vendor="Karaf Container" \
  maintainer="Don Corley <don@tourgeek.com>" \
  license="Apache License, Version 2.0" \
  name="Karaf Container"

ARG KARAF_VERSION=3.0.4
ARG JAVA_VERSION=8u231
ARG JDK_VERSION=1.8.0_231
ARG MAVEN_VERSION=3.6.2

ENV JAVA_HOME=/opt/oracle/jdk
ENV KARAF_HOME=/opt/apache/karaf
ENV SONATYPE_WORK=/sonatype-work
ENV USER_NAME=karaf
ENV USER_HOME=/home/${USER_NAME}
# ENV KARAF_DATA=karaf-data

RUN useradd -r -u 200 -m -c "karaf role account" -d ${USER_HOME} -s /bin/false ${USER_NAME}
WORKDIR ${USER_HOME}

# VOLUME ${KARAF_DATA}

# Set up environment
RUN apt update;apt upgrade -y
RUN apt install -y git curl wget nano

#FROM openjdk:8-jre
#ENV KARAF_INSTALL_PATH /opt
#ENV KARAF_HOME $KARAF_INSTALL_PATH/apache-karaf
#ENV PATH $PATH:$KARAF_HOME/bin
##COPY $KARAF_HOME

EXPOSE 8101 1099 44444 8181 5005

# Install karaf
RUN wget http://archive.apache.org/dist/karaf/$KARAF_VERSION/apache-karaf-$KARAF_VERSION.tar.gz \
&& tar zxvf apache-karaf-$KARAF_VERSION.tar.gz \
&& rm -f apache-karaf-$KARAF_VERSION.tar.gz \
&& mkdir -p /opt/apache \
&& mv apache-karaf-$KARAF_VERSION /opt/apache/
RUN update-alternatives --install "/usr/local/bin/karaf" "karaf" "/opt/apache/apache-karaf-$KARAF_VERSION/bin/karaf" 10 \
&& chmod a+x /usr/local/bin/karaf \
&& update-alternatives --set "karaf" "/opt/apache/apache-karaf-$KARAF_VERSION/bin/karaf" \
&& ln -s apache-karaf-$KARAF_VERSION $KARAF_HOME
RUN chgrp -R karaf /opt/apache/apache-karaf-$KARAF_VERSION \
&& chmod -R g+rw /opt/apache/apache-karaf-$KARAF_VERSION

# Install jdk
# RUN apt install -y openjdk-8-jdk icedtea-netx
# RUN apt install -y openjdk-11-jdk icedtea-netx
# COPY bin/jdk-6u45-linux-x64.bin .
# RUN ./jdk-6u45-linux-x64.bin
# RUN mkdir -p /opt/oracle;mv jdk1.6.0_45 /opt/oracle;ln -s /opt/oracle/jdk1.6.0_45 /opt/oracle/java;ln -s /opt/oracle/java/bin/java /usr/local/bin/java

COPY bin/jdk-$JAVA_VERSION-linux-x64.tar.gz .
RUN tar zxvf jdk-$JAVA_VERSION-linux-x64.tar.gz \
&& rm -f jdk-$JAVA_VERSION-linux-x64.tar.gz \
&& mkdir -p /opt/oracle \
&& mv jdk$JDK_VERSION /opt/oracle/
RUN update-alternatives --install "/usr/bin/java" "java" "/opt/oracle/jdk$JDK_VERSION/bin/java" 10 \
&& update-alternatives --install "/usr/bin/javac" "javac" "/opt/oracle/jdk$JDK_VERSION/bin/javac" 10 \
&& update-alternatives --install "/usr/bin/javaws" "javaws" "/opt/oracle/jdk$JDK_VERSION/bin/javaws" 10 \
&& chmod a+x /usr/bin/java \
&& chmod a+x /usr/bin/javac \
&& chmod a+x /usr/bin/javaws \
&& update-alternatives --set "java" "/opt/oracle/jdk$JDK_VERSION/bin/java" \
&& update-alternatives --set "javac" "/opt/oracle/jdk$JDK_VERSION/bin/javac" \
&& update-alternatives --set "javaws" "/opt/oracle/jdk$JDK_VERSION/bin/javaws" \
&& ln -s jdk$JDK_VERSION $JAVA_HOME
# ln -s /usr/lib/jvm/jdk1.8.0/jre/lib/amd64/libnpjp2.so ~/.mozilla/plugins/

# Install Maven (older version)
# apt install -y maven
RUN wget http://apache.cs.utah.edu/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz \
&& tar zxvf apache-maven-$MAVEN_VERSION-bin.tar.gz \
&& rm -f apache-maven-$MAVEN_VERSION-bin.tar.gz \
&& mkdir -p /opt/apache \
&& mv apache-maven-$MAVEN_VERSION /opt/apache/
ENV MAVEN_HOME=/opt/apache/maven
RUN update-alternatives --install "/usr/local/bin/mvn" "mvn" "/opt/apache/apache-maven-$MAVEN_VERSION/bin/mvn" 10 \
&& chmod a+x /usr/local/bin/mvn \
&& update-alternatives --set "mvn" "/opt/apache/apache-maven-$MAVEN_VERSION/bin/mvn" \
&& ln -s apache-maven-$MAVEN_VERSION $MAVEN_HOME

USER ${USER_NAME}

CMD ["karaf"]
