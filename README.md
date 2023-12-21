# IoT-vulhub-2024

该项目源头是Vu1nT0tal/IoT-vulhub的项目，但是我是从xuanxuan那个fork仓库fork过来的。做这个项目是在本地复现的时候遇到一些bug，想重新优化文档让用户可以更快的上手，而且他们也很久没有维护更新了，打算添加一些新的CVE漏洞。

## 基础环境安装

在 ubuntu16.04 下安装 docker/docker-compose：

```sh
# 安装 pip
curl -s https://bootstrap.pypa.io/get-pip.py | python3

# 安装最新版 docker
curl -s https://get.docker.com/ | sh

# 启动 docker 服务
service docker start

# 安装 compose
pip install docker-compose 
```

## 使用说明

```sh
# 下载项目
wget https://github.com/firmianay/IoT-vulhub/archive/master.zip -O iot-vulhub-master.zip
unzip iot-vulhub-master.zip
cd iot-vulhub-master

# （可选）构建 binwalk 容器，方便使用
cd baseImage/binwalk
docker build -t binwalk .           # 本地编译
docker pull firmianay/binwalk       # 或者拉取

# 进入某一个漏洞/环境的目录
cd Vivotek/remote_stack_overflow

# 根据每个漏洞目录的README.md进行对应的安装操作

# 测试完成后，删除整个环境：
docker-compose -f docker-compose-xxxx.yml down -v
```

注意事项：
- 退出 qemu 用 `Ctrl+A`，再输入 `X`
- 容器中使用 systemctl 可能会有问题，使用 `/etc/init.d/xxxx start` 代替
- 如果要从实体机直接访问 Qemu，例如打开固件的 web 界面（实体机 -> Docker -> Qemu）：
  - 首先在启动 docker 时需要将 ssh 端口映射出来，如 `-p 1234:22`
  - 然后在本地开启端口转发，如 `ssh -D 2345 root@127.0.0.1 -p 1234`
  - 最后对浏览器设置 socks5 代理 `127.0.0.1:2345`。Burpsuite/Python利用脚本同理。

## 漏洞环境列表

请查看[漏洞环境列表](./vuln_list.md)。

## 贡献指南

在研究漏洞的同时，也请给我们提交一份复现环境吧！[贡献指南](./CONTRIBUTION.md)。

## 开源协议

IoT-vulhub is licensed under the MIT License. See LICENSE for the full license text.
