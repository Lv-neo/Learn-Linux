# 手动设置yum repo

以docker-engine为例，官网例子如下，但天朝防火墙让大家都下不了

```
sudo tee /etc/yum.repos.d/docker.repo <<-'EOF'
[dockerrepo]
name=Docker Repository
baseurl=https://yum.dockerproject.org/repo/main/centos/7/
enabled=1
gpgcheck=1
gpgkey=https://yum.dockerproject.org/gpg
EOF
```

我准备用阿里云的yum源替换
先参考[替换yum源为阿里云](aliyun.md)

修改配置
```
[dockerrepo]
name=Docker Repository
#baseurl=https://yum.dockerproject.org/repo/main/centos/7/
baseurl=http://mirrors.aliyun.com/docker-engine/yum/repo/main/centos/7/
enabled=1
gpgcheck=0
```


