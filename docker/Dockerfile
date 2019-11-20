FROM registry.access.redhat.com/ubi8/ubi:latest

MAINTAINER Nick Curry <code@nickcurry.io>

ARG USER=jenkins
ARG GROUP=root
ARG UID=1001
ARG GID=0
ARG HTTP_PORT=8080
ARG AGENT_PORT=50000
ARG JENKINS_HOME=/var/lib/jenkins
ARG REF=/usr/share/jenkins/ref
ARG JENKINS_VERSION=2.205
ARG JENKINS_URL=https://repo.jenkins-ci.org/public/org/jenkins-ci/main/jenkins-war/${JENKINS_VERSION}/jenkins-war-${JENKINS_VERSION}.war

ENV SUMMARY="Jenkins s2i builder image" \
    DESCRIPTION="S2I builder image suitable for generating preconfigured Jenkins instances"

LABEL io.k8s.description="$DESCRIPTION" \
      io.k8s.display-name="Jenkins" \
      io.openshift.tags="jenkins,s2i,ci" \
      io.openshift.expose-services="${HTTP_PORT}:http" \
      io.openshift.s2i.scripts-url=image:///usr/libexec/s2i
      maintainer="Nick Curry <code@nickcurry.io>" \
      com.redhat.dev-mode="DEV_MODE:false" \
      com.redhat.dev-mode.port="DEBUG_PORT:5858" \
      summary="$SUMMARY" \
      description="$DESCRIPTION" \
      version="0.1.0" \
      name="jenkins" \
      help="For more information visit https://github.com/nccurry/openshift-4-jenkins-helm" \

ENV HOME=$JENKINS_HOME \
    JENKINS_HOME=$JENKINS_HOME \
    JENKINS_UC=https://updates.jenkins.io \
    JENKINS_UC_EXPERIMENTAL=https://updates.jenkins.io/experimental \
    JENKINS_INCREMENTALS_REPO_MIRROR=https://repo.jenkins-ci.org/incrementals \
    JENKINS_VERSION=$JENKINS_VERSION \
    LANG=en_US.UTF-8 \
    LC_ALL=en_US.UTF-8

RUN mkdir -p $JENKINS_HOME \
  && chown ${uid}:${gid} $JENKINS_HOME \
  && useradd -d "$JENKINS_HOME" -u ${uid} -g ${gid} -m -s /bin/bash ${user} \
  && mkdir -p ${REF}/init.groovy.d

# Use tini as subreaper in Docker container to adopt zombie processes
ARG TINI_VERSION=v0.18.0
COPY tini_pub.gpg ${JENKINS_HOME}/tini_pub.gpg
RUN curl -fsSL https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini-static-$(dpkg --print-architecture) -o /sbin/tini \
  && curl -fsSL https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini-static-$(dpkg --print-architecture).asc -o /sbin/tini.asc \
  && gpg --no-tty --import ${JENKINS_HOME}/tini_pub.gpg \
  && gpg --verify /sbin/tini.asc \
  && rm -rf /sbin/tini.asc /root/.gnupg \
  && chmod +x /sbin/tini

# could use ADD but this one does not check Last-Modified header neither does it allow to control checksum
# see https://github.com/docker/docker/issues/8331
RUN curl -fsSL ${JENKINS_URL} -o /usr/share/jenkins/jenkins.war \
  && echo "${JENKINS_SHA}  /usr/share/jenkins/jenkins.war" | sha256sum -c -

USER ${jenkins}

VOLUME ["/var/lib/jenkins"]

EXPOSE ${HTTP_PORT} ${AGENT_PORT}