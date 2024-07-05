# README

## Introduction
This project tries to automate image scanning and checking and also helm package linting using Tekton Pipelines.

## Pre-requisites
1. An Openshift cluster with ACS installed.
2. A Quay.io account to store the container images.

## Pipeline resources:
* pipeline 
* secrets
* tasks
* pvc

## Pipeline
The pipeline is defined in the file `pipeline.yaml`. It contains the following tasks:

![tekton-pipeline](images/pipeline.png)

## Install the pipeline
```shell
helm install ssc-pipeline .
```  

Trigger the pipeline
```bash
$ tkn pipeline start ssc-pipeline \
    -p git-url=https://github.com/anmiralles/quarkus-fruits-api.git \
    -p image=quay.io/rh-ee-amiralle/quarkus-fruits-api \
    -w name=shared-workspace,claimName=java-pipeline-pvc \
    -w name=dockerconfig,secret=quay-secret \
    --use-param-defaults
```

Log progress as shown. If there are two or more running, the shell is interactive to allow you to pick correct log
```shell
$ tkn pipeline logs -f
? Select pipelinerun:  [Use arrows to move, type to filter]
> build-and-deploy-run-xy7rw started 36 seconds ago
  build-and-deploy-run-z2rz8 started 40 seconds ago
```

Re-run last pipeline
```shell
$ tkn pipeline start build-and-deploy --last
```
## Related information

https://tekton.dev/docs/

