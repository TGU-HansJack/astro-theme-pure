---
title: 'Docker部署 SurveyKing'
description: '使用Docker部署卷王问卷系统'
publishDate: '2025-04-04 21:41'
tags: ['Docker', 'SurveyKing']
heroImage: { src: './survey.png', color: '#B4C6DA' }
draft: false
language: '简体中文'
comment: true
---

# 部署过程

## 步骤 1：创建目录并下载必要文件

首先，打开终端并执行以下命令以创建必要的目录结构：

```
mkdir -p ./surveyking/mysqlmkdir ./surveyking/sqlscd ./surveyking
```

## 步骤 2：下载初始化 SQL 文件和 Docker Compose 文件

接下来，我们将下载初始化 MySQL 数据库所需的 SQL 文件和 Docker Compose 配置文件：

```
curl -o sqls/init-mysql.sql https://surveyking.cn/surveyking-docker/sqls/init-mysql.sql
curl -o docker-compose.yml https://surveyking.cn/surveyking-docker/docker-compose.yml.example
```

## 步骤 3：启动 Docker 容器

现在，我们将使用 Docker Compose 启动 SurveyKing 应用程序的容器。确保已经安装了 Docker 和 Docker Compose。执行以下命令：

```
docker-compose up -d
```

这将启动 SurveyKing 应用程序的容器，并且该应用程序应该已经在运行中。

## docker-compose 内容示例

```
services:
  mysql:
    image: mysql:8
    container_name: mysql
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: surveyking
      MYSQL_DATABASE: surveyking
    volumes:
      - ./mysql/conf:/etc/mysql/conf.d
      - ./mysql/data:/var/lib/mysql
      - ./sqls:/docker-entrypoint-initdb.d
    networks:
      app_net:
        ipv4_address: 10.20.52.20
  surveyking:
    image: surveyking/surveyking:latest
    container_name: surveyking
    restart: unless-stopped
    environment:
      PROFILE: mysql
      MYSQL_USER: root
      MYSQL_PASS: surveyking
      DB_URL: jdbc:mysql://mysql:3306/surveyking
    volumes:
      - ./files:/app/files
      - ./logs:/app/logs
    depends_on:
      - mysql
    ports:
      - '1991:1991'
    networks:
      app_net:
        ipv4_address: 10.20.52.10

networks:
  app_net:
    driver: bridge
    enable_ipv6: true
    ipam:
      driver: default
      config:
        - subnet: 10.20.52.0/24
          gateway: 10.20.52.1
        - subnet: FD00:1:1::/64
          gateway: FD00:1:1::1
```

部署教程：[Docker Compose 部署](https://surveyking.cn/open-source/deploy/docker-compose-deploy)