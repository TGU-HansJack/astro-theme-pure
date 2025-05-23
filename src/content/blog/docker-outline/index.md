---
heroImage: {"src": "./outline.png", "color": "#B4C6DA"}
title: Docker部署Outline 使用Github OAuth和MinIO
description: Docker快速部署Outline知识库，并且通过Github OAuth作为OAuth服务，使用MinIO作为存储
publishDate: 2025-04-04 13:31
tags:
  - Docker
  - Outline
  - Cloudflare
  - R2
  - Github
draft: false
language: 简体中文
comment: false
---


# 一、准备资源

- 云服务器、Docker和Docker Compose
- 域名
- Github账号

# 二、部署环境

- 1Panel （自带Docker&Docker Compose）
- Redis
- Minio
- PostgreSQL


# 三、部署过程

|Service|Compatible|
|---|---|
|Amazon S3|✅|
|Minio|✅|
|DigitalOcean Object Storage|✅|
|Alibaba Cloud / Aliyun OSS|✅ ([discussion](https://github.com/outline/outline/discussions/4955))|
|Scaleway|✅ ([discussion](https://github.com/outline/outline/discussions/2666#discussioncomment-6781502))|
|Cloudflare R2|❌ ([discussion](https://github.com/outline/outline/discussions/4480#discussioncomment-5449856))|
|Backblaze|❌ ([discussion](https://github.com/outline/outline/discussions/2666#discussioncomment-3421528))|
|OVH Object Storage|❌|
上面表格可知，Cloudflare R2目前并不支持，我们使用MinIO。

## 1、MinIO配置

MinIO直接在1panel里面安装应用商店就可以了，其中MinIO端口情况：
- 9000代理minio.example.com 
- 9001代理api-minio.example.com

部署完成后，请访问`https://minio.example.com`，使用设定的用户名和密码登录。

登录后，在设置中将`Region`设置为`cn-1`，并且在`Bucket`中增加一个`bucket`，命名为`outline`用于存储`outline`的数据，并把访问权限设置为`私有`。

**可选操作**：
在`Bucket`->`outline`->`Access`->`User`中增加一个用户，用于 outline 服务访问 minio 服务。

## 2、Outline部署

下面创建一个文件夹 `outline` 里面创建 `docker-compose.yml`  和  `.env` 

下面是 佬Mikuの鬆 的Github OAuth 配置：

```yml title="docker-compose.yml"

version: 3.8 # [!code --]
services:
  outline:
    image: ${DOCKER_OUTLINE_IMAGE_NAME}
    env_file: ./.env
    ports:
      - 9303:3000
    container_name: outline
    restart: always
    networks:
      - outline
    extra_hosts:
      - ${DOCKER_OUTLINE_HOSTNAME}:0.0.0.0
    depends_on:
      - postgres
      - redis

  redis:
    image: ${DOCKER_REDIS_IMAGE_NAME}
    env_file: ./.env
    volumes:
      - ./redis/redis.conf:/redis.conf # 配置文件持久化
    container_name: ${DOCKER_REDIS_HOST}
    restart: always
    networks:
      - outline
    command: [redis-server, /redis.conf] # 启动命令

  postgres:
    image: ${DOCKER_POSTGRES_IMAGE_NAME}
    env_file: ./.env
    environment:
      POSTGRES_DB: ${DOCKER_POSTGRES_DB}
      POSTGRES_USER: ${DOCKER_POSTGRES_USER}
      POSTGRES_PASSWORD: ${DOCKER_POSTGRES_PASSWORD}
    volumes:
      - ./database-data:/var/lib/postgresql/data # 数据持久化
    container_name: ${DOCKER_POSTGRES_HOST}
    restart: always
    networks:
      - outline

networks:
  outline:
```

```shell title=".env"

# 镜像设置
DOCKER_OUTLINE_IMAGE_NAME=docker.getoutline.com/outlinewiki/outline:latest
DOCKER_POSTGRES_IMAGE_NAME=postgres
DOCKER_REDIS_IMAGE_NAME=redis
DOCKER_SSO_IMAGE_NAME=soulteary/sso-server:1.1.5

# 容器名称
DOCKER_REDIS_HOST=outline_redis
DOCKER_POSTGRES_HOST=outline_postgres
DOCKER_SSO_HOST=outline_sso-server

# postgres设置
DOCKER_POSTGRES_USER=outline
DOCKER_POSTGRES_PASSWORD=outline
DOCKER_POSTGRES_DB=outline


# GitHub OAuth设置，填写前面记下的信息

GITHUB_CLIENT_ID=
GITHUB_CLIENT_SECRET=


# outline设置
DOCKER_OUTLINE_HOSTNAME=outline.example.com # outline域名（需要自己使用Nginx反向代理）

# –––––––––––––––– outline必需 –––––––––––––––-

NODE_ENV=production

# 生成一个十六进制编码的32字节随机密钥。你应该在终端使用 `openssl rand -hex 32`
# 来生成一个随机值。
SECRET_KEY=d8e45ea2e5d298d976464888dea86c92b72dfa73a48cb8903454205c02c732b3

# 生成一个唯一的随机密钥。格式不重要，但你仍然可以使用
# `openssl rand -hex 32` 在你的终端来产生这个。
UTILS_SECRET=cf561a25ab8bd24c58e6d74edd726f60de11fd5c3fb88289c725a48ab3b7b759

# 对于生产环境，请将这些指向你的数据库，在开发中默认
# 应该是开箱即用的。
DATABASE_URL=postgres://${DOCKER_POSTGRES_USER}:${DOCKER_POSTGRES_PASSWORD}@${DOCKER_POSTGRES_HOST}:5432/${DOCKER_POSTGRES_DB}
DATABASE_CONNECTION_POOL_MIN=
DATABASE_CONNECTION_POOL_MAX=
# 取消注释此项以禁用连接到Postgres的SSL
PGSSLMODE=disable

# 对于redis，你可以指定一个与ioredis兼容的url，像这样
REDIS_URL=redis://${DOCKER_REDIS_HOST}:6379
# 或者，如果你想提供额外的连接选项，
# 使用一个base64编码的JSON连接选项对象。参考ioredis文档
# 了解可用选项的列表。
# 示例：使用Redis Sentinel来实现高可用性
# {"sentinels":[{"host":"sentinel-0","port":26379},{"host":"sentinel-1","port":26379}],"name":"mymaster"}
# REDIS_URL=ioredis://eyJzZW50aW5lbHMiOlt7Imhvc3QiOiJzZW50aW5lbC0wIiwicG9ydCI6MjYzNzl9LHsiaG9zdCI6InNlbnRpbmVsLTEiLCJwb3J0IjoyNjM3OX1dLCJuYW1lIjoibXltYXN0ZXIifQ==

# URL应该指向完全合格的、公开可访问的URL。如果使用代理，
# URL中的端口和PORT可能不同。
URL=https://${DOCKER_OUTLINE_HOSTNAME}
PORT=3000

# 查看在运行一个独立的协作
# 服务器的[文档](docs/SERVICES.md)，在正常操作中不需要设置这个。
# COLLABORATION_URL=

# 为了支持上传头像和文档附件的图片，必须提供
# 一个兼容s3的存储。推荐使用AWS S3来实现冗余
# 但如果你想保持所有文件存储在本地，可以使用
# 例如minio (https://github.com/minio/minio) 的替代方案。

# 设置S3的更详细指南可以在这里找到：
# => https://wiki.generaloutline.com/share/125de1cc-9ff6-424b-8415-0d58c809a40f
# 这里使用在minio部署时使用的用户名和密码，或者使用在上一节中可选操作中创建的用户
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_REGION=cn-1
# AWS_S3_ACCELERATE_URL=
AWS_S3_UPLOAD_BUCKET_URL=https://api-minio.example.com
AWS_S3_UPLOAD_BUCKET_NAME=outline
AWS_S3_FORCE_PATH_STYLE=true
AWS_S3_ACL=private

# 指定要使用的存储系统。可能的值是"s3"或"local"之一。
# 对于"local"，头像图片和文档附件将被保存在本地磁盘上。
FILE_STORAGE=s3

# 如果上面为FILE_STORAGE配置了"local"，则这设置了所有附件/图片的父目录
# 确保该进程有权限创建
# 这个路径，并且也有权限向其写入文件。
FILE_STORAGE_LOCAL_ROOT_DIR=/var/lib/outline/data

# 上传附件允许的最大大小。
FILE_STORAGE_UPLOAD_MAX_SIZE=262144000

# 覆盖文档导入的最大大小，通常这应该比文档附件的最大大小要低。
# FILE_STORAGE_IMPORT_MAX_SIZE=

# 覆盖工作区导入的最大大小，这些可能特别大
# 并且文件是临时的，会在一段时间后自动删除。
# FILE_STORAGE_WORKSPACE_IMPORT_MAX_SIZE=

# –––––––––––––– 认证 ––––––––––––––

# 第三方登录凭证，至少需要配置Google、Slack、
# 或Microsoft中的一个才能进行工作安装，否则你将没有登录
# 选项。

# 要配置Slack认证，你需要在
# => https://api.slack.com/apps
# 创建一个应用程序
#
# 在配置Client ID时，添加一个重定向URL到"OAuth & Permissions"：
# https://<URL>/auth/slack.callback
# SLACK_CLIENT_ID=从slack获取一个密钥
# SLACK_CLIENT_SECRET=获取上面密钥的密钥

# 要配置Google认证，你需要在
# => https://console.cloud.google.com/apis/credentials
# 创建一个OAuth客户端ID
#
# 在配置客户端ID时，添加一个已授权的重定向URI：
# https://<URL>/auth/google.callback
# GOOGLE_CLIENT_ID=
# GOOGLE_CLIENT_SECRET=

# 要配置Microsoft/Azure认证，你需要创建一个OAuth客户端。见
# 指南了解如何设置你的Azure应用：
# => https://wiki.generaloutline.com/share/dfa77e56-d4d2-4b51-8ff8-84ea6608faa4
# AZURE_CLIENT_ID=
# AZURE_CLIENT_SECRET=
# AZURE_RESOURCE_APP_ID=

# GitHub认证

# 填写 GitHub OAuth application 的 Client ID 和 Client secret
OIDC_CLIENT_ID=${GITHUB_CLIENT_ID}
OIDC_CLIENT_SECRET=${GITHUB_CLIENT_SECRET}

# 填写 GitHub 的 OAuth endpoint，参考 https://docs.github.com/en/apps/oauth-apps/building-oauth-apps/authorizing-oauth-apps#web-application-flow
OIDC_AUTH_URI=https://github.com/login/oauth/authorize
OIDC_TOKEN_URI=https://github.com/login/oauth/access_token

# OAuth 授权的权限范围
OIDC_SCOPES=read:user user:email

# 通过 GitHub API 获取用户基本信息
OIDC_USERINFO_URI=https://api.github.com/user
OIDC_USERNAME_CLAIM=name

# 让登录界面显示为“使用 GitHub 继续”
OIDC_DISPLAY_NAME=GitHub


# –––––––––––––––– 可选 ––––––––––––––––

# 为HTTPS终止提供Base64编码的私钥和证书。仅当你不使用外部反向代理时才需要此项。见文档：
# https://wiki.generaloutline.com/share/1c922644-40d8-41fe-98f9-df2b67239d45
# SSL_KEY=
# SSL_CERT=

# 如果使用Cloudfront/Cloudflare分发或类似的服务，可以在下面设置。
# 这将导致javascript、样式表和图像的路径被更新为
# 在CDN_URL中定义的主机名。在CDN配置中，原始服务器
# 应设置为与URL相同。
# CDN_URL=

# 在生产环境中自动重定向到https。默认值为true，但如果你能确保
# SSL在外部负载均衡器处终止，可以设置为false。
FORCE_HTTPS=false

# 让安装通过发送匿名统计数据到
# 维护者来检查更新
ENABLE_UPDATES=false

# 应该启动多少个进程。作为一个合理的规则，将你服务器
# 可用内存除以512来大致估算
WEB_CONCURRENCY=1

# 如果你的反向代理已经记录了传入的http
# 请求，而这变得重复，你可以删除这一行
# DEBUG=http

# 配置服务器日志的最低严重性级别。应该是
# error, warn, info, http, verbose, debug 和 silly 之一
LOG_LEVEL=info

# 为了完整的Slack集成，包括搜索和发布到频道，还需要
# 以下配置，更多细节
# => https://wiki.generaloutline.com/share/be25efd1-b3ef-4450-b8e5-c4a4fc11e02a
#
# SLACK_VERIFICATION_TOKEN=你的令牌
# SLACK_APP_ID=A0XXXXXXX
# SLACK_MESSAGE_ACTIONS=true

# 可选地启用Sentry (sentry.io) 来跟踪错误和性能，
# 并可选地在UI中添加一个Sentry代理隧道以绕过广告拦截器：
# https://docs.sentry.io/platforms/javascript/troubleshooting/#using-the-tunnel-option)
# SENTRY_DSN=
# SENTRY_TUNNEL=

# 为了支持发送出站的事务性电子邮件，如"文档已更新"或
# "你被邀请了"，你需要提供SMTP服务器的认证
SMTP_HOST=smtp.qq.com
SMTP_PORT=465
SMTP_USERNAME=@qq.com
SMTP_PASSWORD=
SMTP_FROM_EMAIL=@qq.com
SMTP_REPLY_EMAIL=@qq.com
SMTP_TLS_CIPHERS=
SMTP_SECURE=true

# 默认界面语言。见translate.getoutline.com查看
# 可用的语言代码及其大致翻译的百分比。
DEFAULT_LANGUAGE=zh_CN

# 可选地启用应用程序Web服务器的速率限制器
RATE_LIMITER_ENABLED=true

# 为速率限制器配置默认的限流参数
RATE_LIMITER_REQUESTS=1000
RATE_LIMITER_DURATION_WINDOW=60

# Iframely API配置
# IFRAMELY_URL=
# IFRAMELY_API_KEY=

```


