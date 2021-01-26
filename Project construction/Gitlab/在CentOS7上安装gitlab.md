GitLab 版本不同，命令会有所不同（网上说的而基本都是gitlab-rails console production ），推荐大家直接上 GitLab 官网去找对应版本的命令
我测是使用gitlab-rails console production是进不去GitLab 控制台的
gitlab-rails dbconcole
sudo gitlab-rails console
sudo -u git -H bundle exec rails console -e production

user.password = '39upyoung'
user.password_confirmation = '39upyoung'