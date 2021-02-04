# 写在前面
gitlab的内存占用非常高，最起码8g
### Pull the gitlab image(拉取Gitlab的Docker镜像)
docker pull gitlab/gitlab-ce

### Prepare gitlab local foldders(准备Gitlab的本地文件夹)
```shell
sudo mkdir /usr/local/gitlab
sudo chmod -Rf 777 /usr/local/gitlab
sudo cd /usr/local/gitlab 
sudo mkdir etcgitlab
sudo mkdir loggitlab
sudo mkdir optgitlab
```

### Create a docker  network（创建一个docker网络）
```shell
# 初始化
docker swarm init
# 创建
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
sudo docker run -td -p 13443:443 -p 13980:80 -p 13222:22 --name gitlab --restart always \
--network my-net-git -v /usr/local/gitlab/etcgitlab:/etc/gitlab -v /usr/local/gitlab/loggitlab:/var/log/gitlab -v /usr/local/gitlab/optgitlab:/var/opt/gitlab gitlab/gitlab-ce
```
> --network my-net-git let the gitlab container join the network my-net-git(让容器加入到my-net-git网络)

> -p ,host port to container port mapping (主机到容器的端口映射)

### Prepare gitlab-runner Dockerfile(准备Dockerfile文件)
```shell
sudo mkdir docker
sudo chmod 777 docker
sudo vi docker/Dockerfile
```
Copy below content to the file(复制以下内容到文件中)
```shell
FROM centos:6.10

MAINTAINER Liping

RUN bash -c "curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh | bash"

RUN bash -c  "yum install -y expect.x86_64 expect-devel.x86_64"

RUN bash -c "yum install -y gitlab-runner"

ADD ./docker/my-init.sh /home/sh/my-init.sh

RUN chmod 777 /home/sh/my-init.sh

CMD ["/home/sh/my-init.sh"]
```

### 测试你的gitlab
1. 开放端口
```shell
firewall-cmd --zone=public --add-port=13980/tcp --permanent # 开放5672端口
firewall-cmd --reload   # 配置立即生效
```
2. 输入链接测试
Open your gitlab server url ,the first access will ask you to set a new password.

Navigate to http://localhost:13980/admin/runners

Find the Server URL and to Token for Runner Registration.

打开gitlab server url,首次访问时会要求设置一个新密码。导航到http://localhost:9080/admin/runners

查找图所示 的url 和token 用于Runner 注册.
