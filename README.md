# OpenShift 4 Jenkins Helm Chart

Based on [OpenShift Jenkins Container Image](https://github.com/openshift/jenkins)

https://blog.openshift.com/stateful-workloads-and-the-two-data-center-conundrum/
https://blog.openshift.com/deploying-openshift-applications-multiple-datacenters/
https://searchservervirtualization.techtarget.com/feature/The-difference-between-disaster-avoidance-and-recovery

## Install prerequisites

### Install OpenShift CLI

### Install Helm >=v3.0.0 CLI

Download the latest release of the helm cli

```shell 
# Set helm version
helm_version=v3.0.0 

# Download tar
wget -o helm.tar.gz https://get.helm.sh/helm-${helm_version}-linux-amd64.tar.gz

# Unarchive to folder on PATH
tar -c ${home}/bin -xvf helm.tar.gz
```

## Prep OpenShift clusters

### Create projects

### Grant permissions

### Deploy Sample applications

## Deploy Jenkins

### Modify chart values

```
cp values.yaml runtime-config.yaml
```

#### Add OpenShift credentials configuration 

#### Add OpenShift cluster configuration

### Deploy Chart

```
# Pull in dependant charts
helm dep update

# Deploy jenkins
helm install jenkins-test -f runtime-config.yaml ./
```

## Misc

### Helm Chart Dependency Information:
https://helm.sh/docs/topics/charts/#chart-dependencies
https://helm.sh/docs/topics/chart_template_guide/subcharts_and_globals/

### OpenShift helm bug:
https://bugzilla.redhat.com/show_bug.cgi?id=1773682