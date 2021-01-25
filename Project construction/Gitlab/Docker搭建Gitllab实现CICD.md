### Pull the gitlab image(拉取Gitlab的Docker镜像)
docker pull gitlab/gitlab-ce

### Prepare gitlab local foldders(准备Gitlab的本地文件夹)
```shell
sudo mkdir /home/gitlab
sudo chmod -Rf 777 /home/gitlab
sudo cd /home/gitlab 
sudo mkdir etcgitlab
sudo mkdir loggitlab
sudo mkdir optgitlab
```

### Create a docker  network（创建一个docker网络）
```shell
sudo docker network create --attachable --driver overlay my-net-git 
```
- Output (控制台输出)
```shell
 sudo docker network create --attachable --driver overlay my-net-git 
zlqzca6hzs4rrebakcfw4bqqd
```
> --attachable ,can be manually attached (可被手动附加)

> --overlay   , can support Docker Swarm network (可支持Docker Swarm集群网络)

### Run the gitlab server (运行gitlab server镜像)
```shell
sudo docker run -td -p 3443:443 -p 3980:80 -p 3222:22 --name gitlab --restart always \
--network my-net-git -v /usr/local/gitlab/etcGitlab:/etc/gitlab -v /usr/local/gitlab/logGitlab:/var/log/gitlab -v /usr/local/gitlab/optGitlab:/var/opt/gitlab gitlab/gitlab-ce
```
> --network my-net-git let the gitlab container join the network my-net-git(让容器加入到my-net-git网络)

> -p ,host port to container port mapping (主机到容器的端口映射)

### 