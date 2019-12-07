# Concourse kubernetes setup and POC

This repository contains configuration and instructions for setting up concourse ci on a kubernetes cluster.
Please follow the instructions below to setup the POC on your cluster.

# Prerequisites
- Running Kubernetes cluster with kubectl already logged in
- helm installation running on the Kubernetes cluster
- fly cli (can be downloaded directly from the installed concourse UI)

# Setup

```
DEPLOYMENT_NAME="concoursedev"

# add repo
helm repo add concourse https://concourse-charts.storage.googleapis.com/

# inspect helm values
helm inspect values concourse/concourse > values.yaml

# edit config
vim values.yaml

# TODO: generate certificates

# install helm chart
helm install concourse/concourse --name $DEPLOYMENT_NAME --values values.yaml

# get status
helm status $DEPLOYMENT_NAME


# configure and install certificate for ssl termination
cp certificate.yaml.example certificate.yaml
nano certificate.yaml
kubectl apply -f certificate.yaml


# update new settings when changing or update to new release
helm upgrade $DEPLOYMENT_NAME concourse/concourse --values values.yaml
```


# Use fly cly for configuration
```
export ENDPOINT="https://concourse.yourdomain.com/"

# login
fly -t concoursedev login -c $ENDPOINT

# test task
cd example_pipelines
fly -t concoursedev e -c hello_world.yml

# test pipeline
fly -t concoursedev sp -c basic_pipeline.yml -p basic-pipeline
```


# Links
https://medium.com/concourse-ci/installing-concourse-5-0-on-pivotal-container-service-using-helm-9f20e4e1b8bf
