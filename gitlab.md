# gitlab tips and tricks 




## gitlab-ci

gitlab have a nice ci system and here is one example how to use it to 
build and publish containers to your gitlab account.

in your repo first create a Dockerfile you can also set docker file commands in the gitlab-ci.yml file
if you want. 




Example .gitlab-ci.yml

```
# docker version to build docker in docker.
image: docker:20.10
# docker:bind is for docker in docker.
services:
  - docker:dind

stages:
  - build-docker
  
# variabels you can set and remmember that gitlab-ci also have inbuilt variables.
variables:
  API_REPOSITORY: $CI_REGISTRY/habbfarm/debian-bullseye-slim
  TAG: debian-bullseye-slim

# command to run before 
before_script:
  - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY

# build options.
build-docker:
  stage: build-docker
  script:
    - docker build -f Dockerfile -t $API_REPOSITORY:$TAG .
    - docker push $API_REPOSITORY:$TAG
  only:
    - master

```
