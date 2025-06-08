docker-compose -f /root/repo/artifactory/docker-compose.yml up -d

openssl rand -hex 32 | tr -d "\n" > /data/artifactory/master.key
chown -R 1030:1030 /data/artifactory/master.key

docker-compose down -v
docker-compose up -d
docker-compose stats
docker volume rm artifactory_data
docker volume ls
docker volume inspect artifactory_data

chown -R 1030:1030 etc;chmod -R 775 etc

ansible-playbook -i inventory/hosts site.yml

curl -I http://localhost:8081/artifactory/

docker exec -it postgresql /bin/bash
docker exec -it artifactory pwd

docker logs --tail 100 -f artifactory


NOTE : Do not create this file db.properties. 
Artifactory container will create by itself from docker-compose.yml /var/opt/jfrog/artifactory/etc/db.properties
