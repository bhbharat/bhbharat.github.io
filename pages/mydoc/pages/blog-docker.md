---
title: Docker and Openshift
tags: 
keywords: rouge, pygments, prettify, color coding,
last_updated: Sept 5, 2022
summary: ""
sidebar: mydoc_sidebar
permalink: docker.html
folder: mydoc
---
[Edit page here](https://github.com/bhbharat/bhbharat.github.io/edit/main/pages/mydoc/pages/blog-docker.md)

## Docker commands

```
docker login registry.web.boeing.com -u 3477482 -p git_token
docker images
docker ps -a
docker build -t registry.web.boeing.com/bharat.bhushan/767_ncr/streamlit .
docker search
docker inspect image
docker run -dp 0.0.0.0:7474:7474 registry.web.boeing.com/bharat.bhushan/767_ncr/neo4j_ncr
docker run -dp 0.0.0.0:7474:7474 -p=0.0.0.0:7687:7687 registry.web.boeing.com/bharat.bhushan/767_ncr/neo4rat.bhushan/767_ncr/neo4j_ncr
docker tag flask_app registry.web.boeing.com/bharat.bhushan/767_ncr/test
docker push registry.web.boeing.com/bharat.bhushan/767_ncr/test
docker system prune
docker exec -it <container_name_or_id> /bin/sh


```


## openshift commands

```
oc login --token=oc_token --server=https://api.kcs-pre-clt.k8s.boeing.com:6443
oc create secret docker-registry gitlab-registry --docker-server=registry.web.boeing.com --docker-username=3477482 --docker-password=git_token  --docker-email=bharat.bhushan@boeing.com
oc secrets link default gitlab-registry --for=pull

oc get projects
oc get secrets
oc status
oc project 767ncr
oc get all
oc new-app registry.web.boeing.com/bharat.bhushan/767_ncr/streamlit --name streamlit1
Oc get pods
oc expose service streamlit1
oc rsh pod_name
Oc delete route.route.openshift.io/streamlit1
oc get deployment.apps/streamlit -o yaml
oc get pvc
oc set volume deployment.apps/streamlit --add -t pvc --claim-name=myclaim --mount-path=/
oc delete imagestream,deployment,dc,svc,route,pvc -l app=streamlit

Add SSL - 
oc edit route.route.openshift.io/streamlit
Add tls:termination: edge below host - 
spec:
  host: time-cpuser1-test.apps.kcstd-clt.cloud.boeing.com
  port:
    targetPort: 8080-tcp
  tls:
    termination: edge

```
