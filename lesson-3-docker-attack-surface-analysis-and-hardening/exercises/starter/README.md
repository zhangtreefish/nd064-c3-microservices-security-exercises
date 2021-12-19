# Purpose of this Folder

This folder should contain the starter code and instructions for the exercise.
## Notes
### use the SUSE vm to test docker-bench
1. sudo zypper install docker python3-docker-compose 
and the other commands
per https://en.opensuse.org/Docker
<<<<<<< HEAD
verify:vagrant@localhost:~> cat /etc/docker/daemon.json
=======

>>>>>>> 012709f (feature: run docker-bench in SUSE vm)
2. get suse
vagrant@localhost:~> docker pull opensuse/leap:latest 
//permission denied
fix: sudo chmod 666 /var/run/docker.sock
per https://stackoverflow.com/questions/48957195/how-to-fix-docker-got-permission-denied-issue

3. install go
sudo zypper addrepo https://download.opensuse.org/repositories/devel:languages:go/openSUSE_Leap_15.2/devel:languages:go.repo
sudo zypper refresh
sudo zypper install go1.15
per https://www.phillipsj.net/posts/installing-go-on-opensuse/

4. git: 
sudo zypper install git
per http://git-scm.com/download/linux

5. //error cannot find package "github.com/hashicorp/hcl/hcl/printer" in any of:
vagrant@localhost:~> mkdir go_projects
export GOPATH=$HOME/go_projects
export GOBIN=$GOPATH/bin

<<<<<<< HEAD
finally at starter/exercise/:
=======
finally:
>>>>>>> 012709f (feature: run docker-bench in SUSE vm)
go get github.com/aquasecurity/docker-bench //same error
git clone github.com/aquasecurity/docker-bench 

6. get project 
git clone git@github.com:zhangtreefish/nd064-c3-microservices-security-exercises.git
git@github.com: Permission denied (publickey).
https://github.com/zhangtreefish/nd064-c3-microservices-security-exercises.git instead.

<<<<<<< HEAD
7. at vagrant@localhost:

cd /home/vagrant/nd064-c3-microservices-security-exercises/lesson-3-docker-attack-surface-analysis-and-hardening/exercises/starter

cd /home/vagrant/go_projects/src/docker-bench

build:
docker build -t opensuse/hardened-v1.0 . --no-cache=true
8. at /home/vagrant/go_projects/src/docker-bench folder inside suse vm:
go build -o docker-bench 
./docker-bench --help 

./docker-bench --include-test-output >docker-bench3.txt 

./docker-bench

cat docker-bench3.txt | grep FAIL 

//39 checks fail

./docker-bench --version 18.09

cat /etc/audit/audit.rules

9. docker ps --quiet --all | xargs docker inspect --format '{{ .Id }}: Privileged={{ .HostConfig.Privileged }}'
docker build . -t opensuse/leap:latest -m 256mb --no-cache=true
docker build . -t mystuff:latest -m 256mb --no-cache=true

docker build . -t opensuse/leap:v1.0.0 -m 256mb

docker tag opensuse/leap:latest treefishdocker/udacity-microservices-security:hardened-v1.0

from starter/: do docker push: //408; refresh browser and retry: 
docker push treefishdocker/udacitysecurity:hardened-v1.0 //Successfully signed docker.io/treefishdocker/udacitysecurity:hardened-v1.0 but no tag at dockerhub : wrong 

docker images --digests | grep treefishdocker/udacitysecurity:hardened-v1.0 

docker trust inspect --pretty treefishdocker/udacitysecurity:hardened-v1.0
docker trust key generate treefish

export DOCKER_CONTENT_TRUST_SERVER=docker.io/opensuse/leap

docker trust signer add --key treefish.pub treefish docker.io/opensuse/leap
 
vagrant@localhost:~/nd064-c3-microservices-security-exercises/lesson-3-docker-attack-surface-analysis-and-hardening/exercises/starter> docker build . -t mystuff
docker tag mystuff:latest treefishdocker/udacity-microservices-security:latest
vagrant@localhost:~/nd064-c3-microservices-security-exercises/lesson-3-docker-attack-surface-analysis-and-hardening/exercises/starter> docker run --interactive --tty --memory 256mb opensuse/leap /bin/bash // with -u not work
634c53ae1eb6:/ # 

docker inspect --format='{{.Config.Memory}}' 08c832dd1163s
4.5 
export DOCKER_CONTENT_TRUST=1
5.10
docker ps --quiet --all | xargs docker inspect --format '{{ .Id }}: Memory={{ .HostConfig.Memory }}'
docker ps --quiet --all | xargs docker inspect --format 'Memory={{ .HostConfig.Memory }}'
docker inspect --format='{{.Config.Memory}}' 0ecb6cb98b60

https://github.com/aquasecurity/docker-bench/issues/101

sudo docker ps --quiet --all | xargs sudo docker inspect --format '{{ .Id }}: RestartPolicyName={{ .HostConfig.RestartPolicy.Name }} MaximumRetryCount={{.HostConfig.RestartPolicy.MaximumRetryCount }}'
//which deletes all of the (stopped) containers. (And also its sibling,
docker ps -a -q | xargs docker rm -v
//which deletes images with no label and no running container.)
docker images -f dangling=true -q | xargs docker rmi

docker build . -t opensuse/leap:v1.0.0 -m 256mb
//check if any containers:
vagrant@localhost:~/go_projects/src/docker-bench> docker ps --quiet --all
//start one:
vagrant@localhost:~/go_projects/src/docker-bench> docker run -d opensuse/leap:v1.0.0
then run 
vagrant@localhost:~/go_projects/src/docker-bench> ./docker-bench //39 including 5.10
Open or create the Docker daemon configuration file /etc/docker/daemon.json and add:        "data-root": "/test" per https://searchitoperations.techtarget.com/tip/Get-started-with-Docker-Bench-for-Security

docker info | grep -I Root

./docker-bench --benchmark cis-1.2
getent group docker //docker:x:478:vagrantls /etc/

vagrant@localhost:~/go_projects/src/docker-bench>
sudo zypper install audit //Created symlink /etc/systemd/system/multi-user.target.wants/auditd.service -> /usr/lib/systemd/system/auditd.service.
https://documentation.suse.com/sles/15-SP1/html/SLES-all/cha-sw-cl.html#sec-zypper-softman-sources

vagrant@localhost:~/go_projects/src/docker-bench> systemctl status auditd
sudo systemctl enable auditd

./docker-bench --version 18.09 

//42 FAIL
per https://doc.opensuse.org/documentation/leap/security/html/book-security/cha-audit-setup.html
sudo systemctl enable audit

sudo vi /etc/audit/audit.rules
vagrant@localhost:~> sudo cat /etc/audit/audit.rules //## This file is automatically generated from /etc/audit/rules.d
vagrant@localhost:~> sudo cat /etc/audit/rules.d/docker.rules //# Audit rules based on CIS Docker 1.6 Benchmark v1.0.0
//https://benchmarks.cisecurity.org/tools2/docker/CIS_Docker_1.6_Benchmark_v1.0.0.pdf
//# Not all of these apply to SUSE.
from inside the SUSE vm, the /etc/audit/rules.d/ is:

//1.2.3: `-w /usr/bin/dockerd -k docker`
service auditd restart

sudo systemctl status docker
sudo -i //for auditctl
sudo systemctl start docker && sudo systemctl enable docker && sudo systemctl status docker
//39 FAIL!
   
   RUN useradd -d /home/ztf -m -s /bin/bash ztf

docker run -d opensuse/leap:v1.0.0

docker run -u 1001 --interactive --tty --memory 256mb  treefishdocker/udacity-microservices-security:hardened-v1.1 /bin/bash 
//<1001 is the id of the new user>
//get "I have no name!@a1cdc6d77381:/> "

docker inspect --format='{{.Config.Memory}}' 1bb6e73bec2c

sudo useradd --help
sudo useradd -D tf
docker: Error response from daemon: unable to find user tf: no matching entries in passwd file.

docker ps -a | awk '{ print $1,$2 }' | grep myfourpone | awk '{print $1 }' | xargs -I {} docker rm {}
=======
7. build:
docker build -t opensuse/hardened-v1.0 . --no-cache=true
8. at docker-bench folder inside suse vm:
go build -o docker-bench 
./docker-bench --help 
./docker-bench --include-test-output >docker-bench.txt 

./docker-bench --version 18.09
>>>>>>> 012709f (feature: run docker-bench in SUSE vm)
