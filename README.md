# Docker 项目集合

本项目是 Docker 服务的部署教程和示例集合，包含常用服务的 Docker Compose 配置和运行指令，帮助用户快速搭建和运行相关服务。

## 目录

- [music_tag 部署](#music_tag-部署)
- [portainer 部署](#portainer-部署)
- [mt-photos 部署](#mt-photos-部署)
- [项目贡献](#项目贡献)
- [许可证](#许可证)

## music_tag 部署

`music_tag` 是一个音乐标签管理的 Web 服务。

- 官网：[music_tag 官网](https://github.com/xhongc/music_tag_web)
- GitHub：[music_tag GitHub](https://github.com/xhongc/music_tag_web)

以下是快速部署的命令：
```bash
docker run -d \
    -p 8001:8001 \
    -v <音乐库地址>:/app/media \
    -v <musictag数据地址>:/app/data \
    --restart=always \
    xhongc/music_tag_web:latest
```

请将 `<音乐库地址>` 和 `<musictag数据地址>` 替换为实际路径。

## portainer 部署

Portainer 是一个用于管理 Docker 环境的轻量级服务。

- 官网：[Portainer 官网](https://www.portainer.io/)
- GitHub：[Portainer GitHub](https://github.com/portainer/portainer)

使用以下命令进行部署：

```bash
docker run -d \
    -p 8000:8000 \
    -p 9443:9443 \
    --name=portainer \
    --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v <数据存放位置>:/data \
    portainer/portainer-ce:latest
```

注意：`/var/run/docker.sock` 是容器内部目录，**不要修改**。

## mt-photos 部署
mt-photos` 是一个照片管理服务。

- GitHub：[mt-photos GitHub](https://github.com/mtphotos/mt-photos)

以下是 Docker Compose 配置文件示例：

```yaml
version: "3"

services:
  mtphotos:
    image: mtphotos/mt-photos:arm-latest
    container_name: mtphotos
    restart: always
    ports:
      - 8063:8063
    volumes:
      - <config存放位置>:/app/config
      - <upload存放位置>:/app/upload
      - <照片位置>:/photos
    environment:
      - TZ=Asia/Shanghai
      - LANG=C.UTF-8
      - PUID=1000
      - PGID=100
      - POSTGRES_HOST=mtphotos_db
      - POSTGRES_PASSWORD=<数据库密码>
    depends_on:
      - mtphotos_db

  mtphotos_db:
    image: mtphotos/mt-photos-pg:arm-latest
    container_name: mtphotos_pg
    restart: always
```

请将 `<config存放位置>`、`<upload存放位置>`、`<照片位置>` 和 `<数据库密码>` 替换为实际的配置。

## 项目贡献

欢迎提交问题、功能请求或贡献代码！请阅读 [贡献指南](CONTRIBUTING.md) 了解更多信息。

## 许可证

本项目采用 [MIT License](LICENSE) 进行许可。
