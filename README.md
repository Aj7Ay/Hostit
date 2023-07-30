# Hostit

# Update your instance 
sudo apt update
# install docker on instance 
sudo apt install docker.io
# start & enable & check status
sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl status docker
# make sure add ubuntu user as docker member in docker group for sudo previlages
sudo usermod -aG docker ubuntu
newgrp docker
# to check images and conatiners
docker images
docker ps 
docker ps -a # to see stopped containers also

# Run Nexus container
1.Create Nexus volume:

docker volume create nexus-data
2. Start Nexus container with volume:

docker run -d --name nexus -p 8081:8081 --mount source=nexus-data,target=/nexus-data sonatype/nexus3

3. Login to Nexus UI at http://your-ip:8081 and create a Docker hosted repo called "docker-private"

4. Stop and remove the Nexus container:

docker stop nexus
docker rm nexus

5. Start Nexus container with port mapping for Docker registry:

docker run -d --name nexus -p 8081:8081 -p 8083:8083 --mount source=nexus-data,target=/nexus-data sonatype/nexus3
6. Login at http://your-ip:8081 and edit the Docker hosted repo to use port 8083 7. Access registry at http://your-ip:8083/v2/_catalog



  vim /etc/docker/daemon.json
  { "insecure-registries":["13.235.23.100:8083"] }
It will look like this { "insecure-registries":["13.235.91.151:8083"] }
•Now restart the docker service using systemctl restart docker.service and check if the insecure registry is added properly.


            docker build -t Docker-publicip:8083/Appname:VERSION .
            docker login -u admin Docker-publicip:8083
            docker push Docker-publicip:8083/Appname:VERSION
            docker rmi Docker-publicip:8083/Appname:VERSION
            docker pull Docker-publicip:8083/Appname:VERSION




