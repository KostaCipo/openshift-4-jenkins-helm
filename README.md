# OpenShift 4 Jenkins Helm Chart

Based on [OpenShift Jenkins Container Image](https://github.com/openshift/jenkins)

## Install Helm >= v3.0.0

Download the latest release of the helm cli

```shell 
# Set helm version
helm_version=v3.0.0 

# Download tar
wget -o helm.tar.gz https://get.helm.sh/helm-${helm_version}-linux-amd64.tar.gz

# Unarchive to folder on PATH
tar -c ${home}/bin -xvf helm.tar.gz
```

## Deploy Chart

```
helm install jenkins-test ./
```