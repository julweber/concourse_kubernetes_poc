# Concourse kubernetes setup and POC

This repository contains configuration and instructions for setting up concourse ci on a kubernetes cluster.

# Setup

```
DEPLOYMENT_NAME="concoursedev"

# configure and install certificate for ssl termination
cp certificate.yaml.example certificate.yaml
nano certificate.yaml
kubectl apply -f certificate.yaml

# configure and install concourse with helm and settings from values.yaml
cp values.yaml.example values.yaml
helm install --name $DEPLOYMENT_NAME stable/concourse --values values.yaml

# update new settings when changing or update to new release
helm upgrade $DEPLOYMENT_NAME stable/concourse --values values.yaml
```


# Use fly cly for configuration
```
export ENDPOINT="http://concourse.ag-force.com/"

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
