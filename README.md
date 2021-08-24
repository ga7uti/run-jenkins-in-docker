# Run Jenkins in Docker
 Go to [Jenkins Documentation](https://www.jenkins.io/doc/tutorials/build-a-multibranch-pipeline-project/#run-jenkins-in-docker) for full explaination
 
1. Create a network
```bash
docker network create jenkins
```
2. Run the following command 
```bash
docker run --name jenkins-docker --rm --detach \
  --privileged --network jenkins --network-alias docker \
  --env DOCKER_TLS_CERTDIR=/certs \
  --volume jenkins-docker-certs:/certs/client \
  --volume jenkins-data:/var/jenkins_home \
  --publish 2376:2376 docker:dind --storage-driver overlay2
```
3. Build image using the Dockerfile
```bash
docker build -t myjenkins .
```
4. Run the image as container
```bash
docker run --name jenkins-blueocean --rm --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  --volume "$HOME":/home \
  myjenkins
```
5. Run http://your_ip:8080

6. Get initialPassword for jenkins
```bash
docker logs jenkins-blueocean
```
