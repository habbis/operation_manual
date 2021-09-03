# gitlab tips and tricks 




## gitlab-ci

gitlab have a nice ci system and here is one example how to use it to 
build and publish containers to your gitlab account.

in your repo first create a Dockerfile you can also set docker file commands in the gitlab-ci.yml file
if you want. 

You need to have build user see doc for more options i am going to use deploy tokens created under groups so
it can only access project under that group.

To create a deploy token for gitlab-ci 

```

groups --> yourgroup --> settings --> repository --> Deploy Tokens



```

Then to let gitlab default variables see the deploy token create user called `gitlab-deploy-token`
```
$CI_REGISTRY_USER = gitlab-deploy-token

$CI_REGISTRY_PASSWORD = auto generated token for the user
```



Example dockerfile.
```
FROM debian:bullseye-slim
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update -y && apt-get upgrade -y

```


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
  API_REPOSITORY: $CI_REGISTRY/yourgroup/debian-bullseye-slim
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

links: 

https://snorre.io/blog/2018-02-14-building-docker-images-with-gitlab-ci/

https://blog.callr.tech/building-docker-images-with-gitlab-ci-best-practices/

https://www.freecodecamp.org/news/how-to-setup-ci-on-gitlab-using-docker-66e1e04dcdc2/


terraform related links:

https://docs.gitlab.com/ee/user/infrastructure/iac/terraform_state.html#get-started-using-local-development

https://docs.gitlab.com/ee/user/infrastructure/iac/terraform_state.html
