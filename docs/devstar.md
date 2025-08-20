# DevStar Studio

DevStar Studio 是一个Gitea 发行版，在Git代码仓库托管的基础上提供了开发环境DevEnv执行引擎，与VS Code插件或自定义IDE深度融合，形成灵活适配基础软件工具的生态平台，从而为开发者用户提供智能（代码大模型AI+）、安全（完全云原生）、一站式开箱即用的CI/CD全生命周期研发平台。

DevStar Studio是一个通用的一站式软件研发平台，但它最初的目标是服务于汽车软件、消费电子、智能制造等嵌入式软件研发场景中的开发者。

DevStar Studio的愿景：服务全球软件开发者！

如果你想试用在线演示或者使用免费的DevStar服务（有数量限制），请访问 [devstar.cn](https://devstar.cn/)。

如果你想快速本地部署自己的DevStar实例免费试用或者报告问题，请访问 [https://github.com/mengning/DevStar](https://github.com/mengning/DevStar)。

如果你是云服务厂商想为您的客户提供DevStar实例请联系contact@mengning.com.cn

## Quick Start from source code

如果您是在Windows环境下，请在cmd命令行下先运行如下命令：

```
wsl --install -d Ubuntu-20.04 && wsl --setdefault Ubuntu-20.04
```

在Ubuntu-20.04下完成安装：

```bash
# download and install go
wget -c https://go.dev/dl/go1.23.3.linux-amd64.tar.gz 
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.23.3.linux-amd64.tar.gz
export PATH=$PATH:/usr/local/go/bin
go version

# download and install Node.js
wget -c https://nodejs.org/dist/v22.11.0/node-v22.11.0-linux-x64.tar.xz
sudo tar -xf node-v22.11.0-linux-x64.tar.xz -C /usr/local/
echo 'export PATH=/usr/local/node-v22.11.0-linux-x64/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
node -v # should print `v22.11.0`
npm -v # should print `10.9.0`



```

###  提交代码Pull Request

在DevStar Git仓库创建分支
```
git clone https://gitee.com/devstar/devstar.git
git checkout -b YOUR_BRANCH
code devstar

# in VS Code Terminal
TAGS="timetzdata sqlite sqlite_unlock_notify" make watch # for debuging
make test # testing
TAGS="bindata timetzdata sqlite sqlite_unlock_notify" make build # 生成可执行文件
./gitea

# 提交代码
git add FILES
git commit -m "commit log"
git push
```

#### Start from Container Image

```
make docker
public/assets/install.sh start --image=devstar-studio:latest

# 查看日志
public/assets/install.sh logs
# 停止并删除devstar-studio容器
public/assets/install.sh clean
# 删除所有容器
sudo docker stop $(docker ps -aq) && sudo docker rm -f $(docker ps -aq)
```

在DevStar Git仓库发起Pull Request，合并代码后会自动触发CI流水线完成容器镜像的构建并上传到devstar.cn/devstar/devstar-studio:latest 

```
public/assets/install.sh start
```

## 提示

1. **开始贡献代码之前请确保你已经看过了 [贡献者向导（英文）](CONTRIBUTING.md)**.
2. 所有的安全问题，请私下发送邮件给 **contact@mengning.com.cn**。谢谢！

## 文档

关于如何安装请访问我们的 [文档站](https://github.com/mengning/DevStar)，如果没有找到对应的文档，请私下发送邮件给 **contact@mengning.com.cn**和我们交流。

## 贡献流程

Fork -> Patch -> Push -> Pull Request

## 授权许可

本项目的单机发行版授权个人、非盈利性组织、用户数50人以下的商业组织永久免费使用，云服务厂商、大型商业组织或基于Kubernetes云原生环境部署的客户请私下发送邮件给 **contact@mengning.com.cn**获取商业使用授权。谢谢！