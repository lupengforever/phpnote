## GIT

### linux

一般情况下 linux 自带 git
先查看当前版本

```text
    git --version
    git version 1.7.1
    yum info git//查看yum仓库git  信息
    yum remove git//卸载低版本git
    yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel
    yum install  gcc perl-ExtUtils-MakeMaker
    wget https://github.com/git/git/archive/v2.9.2.tar.gz//下载最新版
    tar zxf git-2.9.2.tar.gz //解压文件
    cd git-2.9.2//进入文件目录
    ./configure --prefix=/usr/local
    make && make install
    ln -s 当前目录/git /usr/bin
    git --version//查看版本
```

配置连接 例如：github
全局配置用户名密码

```text
    git config --global user.name "xxx"
    git config --global user.email "xxx@gmail.com"
```

生成 key 并配置 github

```text
    ssh-keygen -t rsa -C "qiubing.it@gmail.com" //生成 key 文件目录在~/.ssh下面的id_rsa.pub
    cat ~/.ssh/id_rsa.pub//可以查看此文件内容并复制到github
    ssh -T git@github.com
    // 然后就可以尽情使用git了 就是如此简单
    // 当在linux下clone项目后 之后git操作 每次都要输入账号密码，会让你觉得有一点点烦，不必惊慌请在项目下执行下面命令
    git config --global credential.helper store
    // 然后在执行一次操作以后就不用再次输入账号密码
```
