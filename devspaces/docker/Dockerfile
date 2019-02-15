FROM buildpack-deps:stretch-scm

LABEL description="Devspaces implementation for Jenkins"

#
# Install deps and configure UTF-8 by default
#
RUN apt-get -qq update \
    && DEBIAN_FRONTEND=noninteractive apt-get -y --allow-downgrades --allow-remove-essential --allow-change-held-packages install \
    apt-transport-https \
    curl \
    htop \
    mc \
    socat \
    wget \
    vim \
    && apt-get -qqy --allow-unauthenticated install gnupg2 locales locales-all \
    && locale-gen en_US.UTF-8 && dpkg-reconfigure locales \
    && rm -rf /var/lib/apt/lists/*

# UTF-8 by default
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

#
# Pull Zulu OpenJDK binaries from official repository:
# Version: 8-latest
#
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9 \
    && echo "deb http://repos.azulsystems.com/ubuntu stable main" >> /etc/apt/sources.list.d/zulu.list \
    && apt-get -qq update \
    && apt-get -qqy install zulu-8 \
    && rm -rf /var/lib/apt/lists/*

#
# Maven
#
ENV MAVEN_VERSION=3.6.0
ENV USER_HOME_DIR="/root"
ENV BASE_URL=https://apache.osuosl.org/maven/maven-3/${MAVEN_VERSION}/binaries

RUN mkdir -p /usr/share/maven /usr/share/maven/ref \
  && curl -fsSL -o /tmp/apache-maven.tar.gz ${BASE_URL}/apache-maven-${MAVEN_VERSION}-bin.tar.gz \
  && tar -xzf /tmp/apache-maven.tar.gz -C /usr/share/maven --strip-components=1 \
  && rm -f /tmp/apache-maven.tar.gz \
  && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn

ENV MAVEN_HOME /usr/share/maven
ENV MAVEN_CONFIG "$USER_HOME_DIR/.m2"

#
# Jenkins
#
ENV LOGDIR /var/log/jenkins
ENV LOGFILE $LOGDIR/jenkins.log
RUN mkdir /var/log/jenkins

WORKDIR /data/

COPY assets/configs/settings-docker.xml /usr/share/maven/ref/
COPY assets/devspaces-entrypoint.sh /usr/local/bin/devspaces-entrypoint.sh
RUN chmod +x /usr/local/bin/devspaces-entrypoint.sh

ENTRYPOINT ["/usr/local/bin/devspaces-entrypoint.sh"]

# for main web interface:
EXPOSE 8080

# will be used by attached slave agents:
EXPOSE 50000
