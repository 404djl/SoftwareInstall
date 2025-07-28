## ubuntu20安装docker

### 1. 更新系统包

首先，确保系统包时最新的：

```bash
sudo apt remove docker docker-engine docker.io containerd runc   # 卸载旧版本（如已安装）
sudo apt update
sudo apt upgrade -y
```

### 2. 安装依赖包

安装Docker所需的依赖包

```bash
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
```

### 3. 添加Docker官方GPG密钥

添加 Docker 的官方 GPG 密钥以确保下载的软件包是安全的：

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

### 4. 添加Docker仓库

将 Docker 的稳定版仓库添加到 APT 源列表中：

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 5. 更新并安装Docker

安装 Docker CE（社区版）、Docker CLI 和 Containerd：

```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
```

### 6. 启动并启用Docker服务

启动Docker服务并设置为开机自启

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

### 7. 配置加速器（使用 `https://docker.xuanyuan.me`）

修改或创建配置文件：

```bash
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://docker.xuanyuan.me"]
}
EOF
```
- 免费加速器
  ```bash
  {
  "registry-mirrors": [
    "https://2a6bf1988cb6428c877f723ec7530dbc.mirror.swr.myhuaweicloud.com",
    "https://dockerpull.pw",
    "https://hub.mirrorify.net",
    "https://image.cloudlayer.icu",
    "https://dockerproxy.net",
    "https://hub.fast360.xyz",
    "https://docker.1ms.run",
    "https://hub1.nat.tf"
  ]
}
```

重启Docker服务:

```bash
sudo systemctl daemon-reexec
sudo systemctl restart docker
```

### 8. 可选：设置当前用户权限（避免每次都用 sudo）

```bash
sudo usermod -aG docker $USER
# 然后重新登录终端或重启系统使用户组生效
```

----

如需同时配置多个镜像源（比如阿里云、七牛等），你可以在 `daemon.json` 中写成：

```bash
{
  "registry-mirrors": [
    "https://docker.xuanyuan.me",
    "https://<其他镜像源>"
  ]
}
```



> **使用DockerHub 代理，以下以 [docker.xuanyuan.me](https://link.zhihu.com/?target=https%3A//docker.xuanyuan.me/) 为例：可以根据列表自行替换**

``` bash
docker pull docker.xuanyuan.me/library/mysql:5.7
```

说明：library是一个特殊的命名空间，它代表的是官方镜像。如果是某个用户的镜像就把library替换为镜像的用户名。
