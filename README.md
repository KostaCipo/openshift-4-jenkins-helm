# OpenShift 4 Jenkins Helm Chart

[OpenShift Jenkins Container Image](https://github.com/openshift/jenkins)
[Container as code Plugin](https://github.com/jenkinsci/configuration-as-code-plugin)

## Install Helm >= v3.0.0

Download the latest release of the helm cli

```shell 
HELM_VERSION=v3.0.0
wget -O helm.tar.gz https://get.helm.sh/helm-${HELM_VERSION}-linux-amd64.tar.gz
tar -C ${HOME}/bin -xvf helm.tar.gz
```

# Prepare OpenShift Clusters



```
# Set environment specific parameters
APP_PROJECT_A=app-a
APP_PROJECT_B=app-b
JENKINS_PROJECT=jenkins



# Configure OpenShift
oc new-project ${APP_PROJECT_A}
oc new-project ${APP_PROJECT_B}
oc new-project ${JENKINS_PROJECT}
oc new-app jenkins -n ${JENKINS_PROJECT}
oc policy add-role-to-user edit system:serviceaccount:${JENKINS_PROJECT}:jenkins -n ${APP_PROJECT_A}
oc policy add-role-to-user edit system:serviceaccount:${JENKINS_PROJECT}:jenkins -n ${APP_PROJECT_A}
```

## Configure Jenkins

### Install Plugins

### Configure Credentials

### Configure OpenShift Clusters