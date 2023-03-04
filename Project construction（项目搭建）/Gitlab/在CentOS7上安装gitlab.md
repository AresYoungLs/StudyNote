# 在CentOS7上安装gitlab

GitLab 版本不同，命令会有所不同（网上说的而基本都是gitlab-rails console production ），推荐大家直接上 GitLab 官网去找对应版本的命令
我测是使用gitlab-rails console production是进不去GitLab 控制台的
gitlab-rails dbconcole
sudo gitlab-rails console
sudo -u git -H bundle exec rails console -e production

user.password = '39upyoung'
user.password_confirmation = '39upyoung'

## Gitlab给分支设定权限

 发表于 2018-06-06 |  分类于 工作与技术 |  热度: ℃
 字数统计: 321 |  阅读时长 ≈ 1
这几天边吃瓜边关注崔永元和冯小刚、刘震云的爱恨情仇
给gitlab的各位开发设置权限是很重要的，不然他们就可能会偷偷的把执行分支合并甚至git pull来破坏线上环境。

首先先确定在project下各位人员的身份，在设置（setting）–成员(members)里面，可以看到projects现有的用户和用户组，如图：
akb48

由于我这个gitlab已经是汉化版的了，这里做一个简单的中英对比：Master是“主程序员”、Developer是“开发人员”、Reporter是“报告者”，这个身份只有读权限可以创建代码片段，一般来说都给测试人员，而Guest就是“访客”了，它只能提交问题和评论。

然后再到版本库（Repository）里选择保护分支（Protected Branches），如图：
akb48

Allowed to merge就是分支合并权限，Allowed to push就是推送权限，这两个可以根据不同人的身份进行控制。如果受保护，除了master权限的人员，其余人都不可以push、delete等操作。默认情况下master分支是处于被保护状态下的，developer角色的人是无法提交到master分支的。

如果是docker的话，那么gitlab权限问题修复会用到如下命令：
docker exec -it gitlab update-permissions
docker restart gitlab容器的ID
