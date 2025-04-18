---
title: "Docker compose使用、容器迁移"
date:  2025-04-18T13:33:00+08:00
categories: ['Docker']
series: ['Docker Compose']
ShowToc: true
#TocOpen: true
tags: ['Docker', 'Docker Compose', 'container']

---

### Docker Compose 简介

Docker Compose 是 Docker 提供的一个工具，用于定义和运行多容器应用程序。它通过一个 YAML 文件（通常命名为 `docker-compose.yml`）来描述多个容器、服务、网络和卷的配置，简化了多容器应用的部署和管理。Docker Compose 适合开发、测试和生产环境，特别适用于需要协调多个容器的场景。

### Docker Compose 简介

Docker Compose 是 Docker 提供的一个工具，用于定义和运行多容器应用程序。它通过一个 YAML 文件（通常命名为 `docker-compose.yml`）来描述多个容器、服务、网络和卷的配置，简化了多容器应用的部署和管理。Docker Compose 适合开发、测试和生产环境，特别适用于需要协调多个容器的场景。

### Docker Compose模式下的容器迁移

在Docker Compose模式下，迁移涉及整个服务栈（多个容器、配置和数据卷）。以下是推荐方案：

#### 1. 使用Docker Registry迁移镜像并结合Compose文件

**方案概述**：<br />将所有服务镜像推送到Registry，在目标环境使用`docker-compose.yml`文件拉取镜像并启动服务。

**步骤**：

1. 在源环境推送所有服务镜像：

    ```bash
    docker-compose push
    ```
2. 将`docker-compose.yml`文件传输到目标环境。
3. 在目标环境拉取镜像并启动：

    ```bash
    docker-compose pull
    docker-compose up -d
    ```

**优点**：

* **安全性**：Registry提供加密传输和认证。
* **简便性**：Compose文件集中管理配置，一键部署整个服务栈。

**注意事项**：

* 确保Compose文件中的镜像标签与Registry一致。
* 数据卷需单独迁移。

#### 2. 使用`docker save`和Compose文件迁移

**方案概述**：<br />将Compose项目中的所有镜像保存为tar文件，结合`docker-compose.yml`文件迁移，适合离线环境。

**步骤**：

1. 在源环境保存所有服务镜像：

    ```bash
    docker save -o compose-images.tar $(docker-compose images -q | sort -u)
    ```
2. 将tar文件和`docker-compose.yml`传输到目标环境。
3. 在目标环境加载镜像并启动：

    ```bash
    docker load -i compose-images.tar
    docker-compose up -d
    ```

**优点**：

* **安全性**：tar文件可加密传输。
* **简便性**：离线迁移，Compose文件简化配置。

**注意事项**：

* tar文件较大，需注意存储和传输。
* 数据卷需单独处理。

#### 数据卷迁移

与Docker模式类似，Compose模式下的数据卷迁移方式如下：

* **步骤**：

  1. 备份数据卷：通过`docker volume inspect`或备份工具（如`docker-volume-backup`）导出数据。
  2. 在目标环境恢复：将数据复制到目标路径。
* **优点**：确保数据一致性和安全性。
* **注意事项**：确保Compose文件中数据卷配置与源环境一致。

**推荐**：<br />建议使用Docker Registry结合`docker-compose.yml`迁移，因其安全性高且部署简便。若无网络，可用`docker save`方案。数据卷需单独备份。