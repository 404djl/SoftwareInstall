Docker 的基本使用可以分为以下几个方面：**镜像管理、容器操作、数据挂载、网络设置、Dockerfile 构建镜像** 等。以下是最常用的基本命令和使用方式，适合初学者入门。

------

## 🧱 一、镜像管理（Images）

### ✅ 1. 搜索镜像

```bash
docker search ubuntu
```

### ✅ 2. 拉取镜像

```bash
docker pull ubuntu
```

### ✅ 3. 查看本地镜像

```bash
docker images
```

### ✅ 4. 删除镜像

```bash
docker rmi ubuntu
```

------

## 📦 二、容器操作（Containers）

### ✅ 1. 运行容器

```bash
docker run -it ubuntu bash
```

解释：

- `-it`：交互式终端
- `ubuntu`：镜像名
- `bash`：容器中执行的命令

### ✅ 2. 后台运行容器

```bash
docker run -d --name mynginx nginx
```

解释：

- `-d`：后台运行
- `--name`：指定容器名称

### ✅ 3. 查看运行中的容器

```bash
docker ps
```

### ✅ 4. 查看所有容器（包括停止的）

```bash
docker ps -a
```

### ✅ 5. 停止/启动/重启容器

```bash
docker stop mynginx
docker start mynginx
docker restart mynginx
```

### ✅ 6. 删除容器

```bash
docker rm mynginx
```

------

## 📁 三、数据挂载（Volumes）

### ✅ 1. 映射主机目录到容器

```bash
docker run -v /host/path:/container/path -it ubuntu
```

### ✅ 2. 使用命名卷（volume）

```bash
docker volume create mydata
docker run -v mydata:/data -it ubuntu
```

------

## 🌐 四、网络设置（Networks）

### ✅ 1. 查看 Docker 网络

```bash
docker network ls
```

### ✅ 2. 运行容器并绑定端口

```bash
docker run -d -p 8080:80 nginx
```

解释：

- 主机 8080 端口映射到容器 80 端口

------

## 🔨 五、构建自定义镜像（使用 Dockerfile）

### ✅ 1. 创建一个 Dockerfile 示例：

```dockerfile
# Dockerfile
FROM ubuntu
RUN apt update && apt install -y nginx
CMD ["nginx", "-g", "daemon off;"]
```

### ✅ 2. 构建镜像

```bash
docker build -t mynginx .
```

### ✅ 3. 运行自定义镜像

```bash
docker run -p 8080:80 mynginx
```

------

## 🔍 六、进入容器内部

### ✅ 使用 exec（推荐）

```bash
docker exec -it mynginx bash
```

### ✅ 使用 attach（不推荐）

```bash
docker attach mynginx
```

------

## 🧹 七、清理命令

```bash
docker container prune     # 删除所有已停止容器
docker image prune         # 删除无用镜像
docker system prune -a     # 全部清理
```

------

## 📄 八、查看日志和状态

```bash
docker logs mynginx              # 查看容器日志
docker inspect mynginx           # 查看容器详细信息
docker stats                     # 实时监控容器资源
```

------

