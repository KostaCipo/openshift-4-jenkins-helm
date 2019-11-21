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

## Modify values

```
cp values.yaml runtime-config.yaml
```

## Deploy Chart

```
# Pull in dependant charts
helm dep update

# Deploy jenkins
helm install jenkins-test -f runtime-config.yaml ./
```




## Helm Chart Dependency Information:
https://helm.sh/docs/topics/charts/#chart-dependencies
https://helm.sh/docs/topics/chart_template_guide/subcharts_and_globals/


## OpenShift helm bug:
https://bugzilla.redhat.com/show_bug.cgi?id=1773682