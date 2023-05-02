apt -y update
apt install -y apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
apt -y update
apt-cache policy docker-ce
apt  -y install docker-ce

docker volume create --name nexus-data

docker run -d -p 8081:8081 -p 8082:8082 -p 8083:8083 --name nexus -v nexus-data:/nexus-data sonatype/nexus3

fuser -n tcp 8083

git clone https://github.com/mguazzardo/apiflask && cd apiflask

docker build -t flaskapi .

docker login -u admin -p superhacker 127.0.0.1:8083

root@jenkins:/apiflask# docker tag flaskapi 127.0.0.1:8083/flaskapi
root@jenkins:/apiflask# docker push 127.0.0.1:8083/flaskapi

