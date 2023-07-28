# DevOps
#https://www.jenkins.io/doc/book/installing/docker/

docker rm -f $(docker ps -qa | grep "ubuntu_devops" | awk '{print $3}') 
docker rmi $(docker images | grep "ubuntu_devops" | awk '{print $3}') 

docker network create jenkins

docker run \
  --name jenkins-docker \
  --rm \
  --detach \
  --privileged \
  --network jenkins \
  --network-alias docker \
  --env DOCKER_TLS_CERTDIR=/certs \
  --volume jenkins-docker-certs:/certs/client \
  --volume jenkins-data:/var/jenkins_home \
  --publish 2376:2376 \
  docker:dind \
  --storage-driver overlay2

docker build -t ubuntu_devops .


docker run \
  --name ubuntu_devops \
  --restart=on-failure \
  --detach \
  --network jenkins \
  --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client \
  --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 \
  --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  ubuntu_devops

  
docker exec -it -u 0 ubuntu_devops /bin/bash

