# IoT-vulhub

受 [Vulhub](https://github.com/vulhub/vulhub) 项目的启发，希望做一个 IoT 版的固件漏洞复现环境。

## 安装

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

**在构建 qemu-system 前务必下载对应的 qemu 镜像！**

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

# 解包固件
docker run -v $PWD/firmware:/root/firmware binwalk -Mer /root/firmware/firmware.bin

# 自动化编译环境（目前通常有三种模拟方式）
docker-compose -f docker-compose-user.yml build     # QEMU 用户模式模拟
docker-compose -f docker-compose-system.yml build   # QEMU 系统模式模拟
docker-compose -f docker-compose-firmadyne.yml build   # firmadyne 模拟

# 启动整个环境
docker-compose -f docker-compose-xxxx.yml up

# 每个环境目录下都有相应的说明文件，请阅读该文件，进行漏洞/环境测试。

# 测试完成后，删除整个环境：
docker-compose -f docker-compose-xxxx.yml down -v
```

## 漏洞环境列表

请查看[漏洞环境列表](./vuln_list.md)。

## 贡献指南

在研究漏洞的同时，也请给我们提交一份复现环境吧！[贡献指南](./CONTRIBUTION.md)。

## 开源协议

IoT-vulhub is licensed under the MIT License. See LICENSE for the full license text.
