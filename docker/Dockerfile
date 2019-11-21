FROM registry.access.redhat.com/ubi8/ubi:latest

MAINTAINER Nick Curry <code@nickcurry.io>

# TODO: Get these args working in below RUN commands
ARG jenkins_version=2.205
ARG jenkins_checksum=41a91e67b174a1c7290d53f4b92a93c78fdc6e1cda160485a3318bc4dfbeeefa
ARG tini_version=v0.18.0

ENV SUMMARY="Jenkins s2i builder image" \
    DESCRIPTION="S2I builder image suitable for generating preconfigured Jenkins instances" \
    HOME=/var/lib/jenkins \
    JENKINS_HOME=/var/lib/jenkins \
    JENKINS_UC=https://updates.jenkins.io \
    JENKINS_UC_EXPERIMENTAL=https://updates.jenkins.io/experimental \
    JENKINS_INCREMENTALS_REPO_MIRROR=https://repo.jenkins-ci.org/incrementals \
    JENKINS_VERSION=${jenkins_version} \
    TINI_VERSION=${tini_version}

LABEL io.k8s.description="$DESCRIPTION" \
      io.k8s.display-name="Jenkins" \
      io.openshift.tags="jenkins,s2i,ci" \
      io.openshift.expose-services="8080:http" \
      io.openshift.s2i.scripts-url=image:///usr/libexec/s2i \
      maintainer="Nick Curry <code@nickcurry.io>" \
      com.redhat.dev-mode="DEV_MODE:false" \
      com.redhat.dev-mode.port="DEBUG_PORT:5858" \
      summary="$SUMMARY" \
      description="$DESCRIPTION" \
      version="0.1.0" \
      name="jenkins" \
      help="For more information visit https://github.com/nccurry/openshift-4-jenkins-helm"

RUN useradd -d $JENKINS_HOME -u 1001 -g 0 -m -s /bin/bash jenkins \
  && mkdir -p /usr/share/jenkins/ref/init.groovy.d \
  && chown -R 1001:0 /usr/share/jenkins \
  && yum -y install java-1.8.0-openjdk.x86_64

# Use tini as subreaper in container to adopt zombie processes
COPY tini_pub.gpg $JENKINS_HOME/tini_pub.gpg
RUN curl -fsSL https://github.com/krallin/tini/releases/download/v0.18.0/tini-static-amd64 -o /sbin/tini \
  && curl -fsSL https://github.com/krallin/tini/releases/download/v0.18.0/tini-static-amd64.asc -o /sbin/tini.asc \
  && gpg --no-tty --import $JENKINS_HOME/tini_pub.gpg \
  && gpg --verify /sbin/tini.asc \
  && rm -rf /sbin/tini.asc /root/.gnupg \
  && chmod +x /sbin/tini

RUN curl -fsSL https://repo.jenkins-ci.org/public/org/jenkins-ci/main/jenkins-war/2.205/jenkins-war-2.205.war -o /usr/share/jenkins/jenkins.war \
  && echo "41a91e67b174a1c7290d53f4b92a93c78fdc6e1cda160485a3318bc4dfbeeefa /usr/share/jenkins/jenkins.war" | sha256sum --check

USER ${jenkins}

VOLUME ["/var/lib/jenkins"]

EXPOSE 8080 50000