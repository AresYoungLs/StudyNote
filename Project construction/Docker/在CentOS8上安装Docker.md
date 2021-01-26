### 更新所有相关软件
```shell
 yum update
 ```
---

 ### 卸载旧版本
 ```shell
 yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```
---

### 安装yum-utils软件包（提供yum-config-manager 实用程序）并设置稳定的存储库。
```shell
yum install -y yum-utils

yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
---

### 如果是在国内，有需要的话需要设置阿里云yum源
```shell
yum-config-manager --add-repo  http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo 
```
---

### 查看Docker版本
```shell
yum list docker-ce --showduplicates | sort -r
```
---

### 安装Docker
```shell
yum install docker-ce
```
>这个时候需要安装containerd.io，我们可以到这个网站复制文件名称粘贴到这个连接后面
    <https://download.docker.com/linux/centos/7/x86_64/stable/Packages/>
> 如果连接超时错误
解决方法：换成国内的源
```shell
dnf install https://mirrors.aliyun.com/docker-ce/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.6-3.3.el7.x86_64.rpm
```
> 继续安装
```shell
yum install docker-ce docker-ce-cli
```
---

### 启动Docker,并设置为开机自启
```shell
systemctl start docker
systemctl enable docker
```
---

### 启动docker服务远程允许访问
1. 编辑服务器上的docker.service文件
```shell
vi /usr/lib/systemd/system/docker.service
```
> 大约 在 14行 原来的值
```shell
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
```
修改为
```shell
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0.0:2375 -H unix://var/run/docker.sock
```
如果报错，尝试
```shell
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock -H tcp://0.0.0.0:2375
```
2. 保存修改退出，重启docker
```shell
systemctl daemon-reload

service docker restart
```
3. 测试远程连接是否正常 输出下面的内容 如果出现以下内容则能正常连接：
```shell
curl http://localhost:2375/version

{"Platform":{"Name":"Docker Engine - Community"},"Components":[{"Name":"Engine","Version":"19.03.9","Details":{"ApiVersion":"1.40","Arch":"amd64","BuildTime":"2020-05-15T00:24:05.000000000+00:00","Experimental":"false","GitCommit":"9d988398e7","GoVersion":"go1.13.10","KernelVersion":"4.18.0-147.el8.x86_64","MinAPIVersion":"1.12","Os":"linux"}},{"Name":"containerd","Version":"1.2.6","Details":{"GitCommit":"894b81a4b802e4eb2a91d1ce216b8817763c29fb"}},{"Name":"runc","Version":"1.0.0-rc8","Details":{"GitCommit":"425e105d5a03fabd737a126ad93d62a9eeede87f"}},{"Name":"docker-init","Version":"0.18.0","Details":{"GitCommit":"fec3683"}}],"Version":"19.03.9","ApiVersion":"1.40","MinAPIVersion":"1.12","GitCommit":"9d988398e7","GoVersion":"go1.13.10","Os":"linux","Arch":"amd64","KernelVersion":"4.18.0-147.el8.x86_64","BuildTime":"2020-05-15T00:24:05.000000000+00:00"}
```
4. 开放端口

> 需要将2375端口进行开放才能被远程连接，如果是阿里云主机的话，可以直接登录阿里云去进行开放 (如果是虚拟机的话，可以用以下命令进行开放)：
```shell
firewall-cmd --zone=public --add-port=2375/tcp --permanent
```
> 如果是虚拟机 也可以暂时 关闭 防火墙
查看防火墙运行状态
```shell
firewall-cmd --state
running
```
> running 表示正在 运行
关闭防火墙命令

```shell
systemctl stop firewalld
```
>本机 浏览器上输入 http://ip:2375/version
>有返回结果，则可以正常使用了,在idea中 正常连接了
---

### 防火墙开启 有可能阻止容器间访问 使用下面的设置修改 使防火墙运行状态下可以使容器间正常通讯
```shell
#配置docker0服务到受信任连接
[root@localhost ~]# nmcli connection modify docker0 connection.zone trusted

#停止NetworkManager（检测网络、自动连接网络的程序）服务
[root@localhost ~]# systemctl stop NetworkManager.service

#修改docker网络接口为内部区域（永久）
[root@localhost ~]# firewall-cmd --permanent --zone=trusted --change-interface=docker0
success

#重启docker服务
[root@localhost ~]# systemctl restart docker.service
```

### 