---
title: 使用Docker Compose安装Lobe数据库版本
author: 囧囧JOJO
top: false
cover: false
mathjax: false
tags: [docker, GPT, LLM]
date: 2024-12-13 14:39:58 +0800
img:
coverImg:
password:
summary:
---

## 准备篇

本次安装采用方案为：Github 登陆认证 + Cloudflare R2 存储，本机只需要安装PostgreSQL数据库、Nginx和LobeChat即可。

如需要其他方案，请自行修改。

安装之前，首先需要准备以下内容：

- [Docker Compose](https://www.runoob.com/docker/docker-compose.html)
- [Github账号](https://github.com/signup)
- [Cloudflare账号](https://dash.cloudflare.com/sign-up)
- [手](https://www.baidu.com/s?wd=%E6%89%8B%E5%9C%A8%E8%BA%AB%E4%BD%93%E7%9A%84%E5%93%AA%E4%B8%AA%E9%83%A8%E4%BD%8D)

除此之外，还需要准备以下文件文件：

- `docker-compose.yml`
- `.env`
- `nginx.conf`
- `ssl/NEW_DOMAIN.pem`
- `ssl/NEW_DOMAIN.key`
- `env.txt`
- `run.sh`

最终目录结果大致如下：
```
├── docker-compose.yml
├── nginx.conf
├── .env
└── ssl
    ├── lobe.jojo.host.key
    └── lobe.jojo.host.pem
```

这里除了 `env.txt` 和 `ssl` 文件，其他的文件内容不需要做修改，先直接保存即可。

### docker-compose.yml文件

```yml
name: lobe-chat-database
services:
  postgresql:
    image: pgvector/pgvector:pg16
    container_name: lobe-postgres
    ports:
      - '5432:5432'
    volumes:
      - './data:/var/lib/postgresql/data'
    environment:
      - 'POSTGRES_DB=lobe'
      - 'POSTGRES_PASSWORD=NEW_POSTGRES_PASSWORD' # 需要替换为你的 PostgreSQL 数据库密码
    healthcheck:
      test: ['CMD-SHELL', 'pg_isready -U postgres']
      interval: 5s
      timeout: 5s
      retries: 5
    restart: always

  lobe:
    image: lobehub/lobe-chat-database
    container_name: lobe-database
    ports:
      - '3210:3210'
    depends_on:
      - postgresql
    env_file:
      - .env
    restart: always

  nginx:
    image: nginx:latest
    container_name: lobe-nginx
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
      - ./ssl:/etc/nginx/ssl:ro
    depends_on:
      - lobe
    restart: always

volumes:
  data:
    driver: local
```
    
### .env文件

```
# 必填，LobeChat 域名，用于 tRPC 调用
# 请保证此域名在你的 NextAuth 鉴权服务提供商、S3 服务商的 CORS 白名单中
APP_URL=https://NEW_DOMAIN/

# Postgres 相关，也即 DB 必需的环境变量
# 必填，用于加密敏感信息的密钥，可以使用 openssl rand -base64 32 生成
KEY_VAULTS_SECRET=NEW_KEY_VAULTS_SECRET # 需要替换为你生成的 KEY_VAULTS_SECRET 密钥
# 必填，Postgres 数据库连接字符串，用于连接到数据库
# 格式：postgresql://username:password@host:port/dbname，如果你的 pg 实例为 Docker 容器且位于同一 docker-compose 文件中，亦可使用容器名作为 host
DATABASE_URL=postgresql://postgres:NEW_POSTGRES_PASSWORD@postgresql:5432/lobe  # 需要替换为你实际的 PostgreSQL 数据库密码

# NEXT_AUTH 相关，也即鉴权服务必需的环境变量
# 可以使用 auth0、Azure AD、GitHub、Authentik、Zitadel、Logto 等，如有其他接入诉求欢迎提 PR
# 目前支持的鉴权服务提供商请参考：https://lobehub.com/zh/docs/self-hosting/advanced/auth#next-auth
# 如果你有 ACCESS_CODE，请务必清空，我们以 NEXT_AUTH 作为唯一鉴权来源
# 必填，用于 NextAuth 的密钥，可以使用 openssl rand -base64 32 生成
NEXT_AUTH_SECRET=NEW_NEXT_AUTH_SECRET # 需要替换为你生成的 NEXT_AUTH_SECRET 密钥
# 必填，指定鉴权服务提供商，这里以 GitHub 为例
NEXT_AUTH_SSO_PROVIDERS=github
# 必填，NextAuth 的 URL，用于 NextAuth 的回调
NEXTAUTH_URL=https://NEW_DOMAIN

# NextAuth 鉴权服务提供商部分，以 GitHub 为例
# 其他鉴权服务提供商所需的环境变量，请参考：https://lobehub.com/zh/docs/self-hosting/environment-variables/auth
AUTH_GITHUB_ID=NEW_AUTH_GITHUB_ID # 需要替换为你 GitHub OAuth 应用的 Client ID
AUTH_GITHUB_SECRET=NEW_AUTH_GITHUB_SECRET # 需要替换为你 GitHub OAuth 应用的 Client Secret

# 代理相关，如果你需要的话（比如你使用 GitHub 作为鉴权服务提供商）
# HTTP_PROXY=http://localhost:7890
# HTTPS_PROXY=http://localhost:7890

# S3 相关，也即非结构化数据（文件、图片等）存储必需的环境变量
# 这里以 Cloudflare R2 为例
# 必填，S3 的 Access Key ID
S3_ACCESS_KEY_ID=NEW_S3_ACCESS_KEY_ID # 需要替换为你 Cloudflare R2 的 Access Key ID
# 必填，S3 的 Secret Access Key
S3_SECRET_ACCESS_KEY=NEW_S3_SECRET_ACCESS_KEY # 需要替换为你 Cloudflare R2 的 Secret Access Key
# 必填，S3 的 Endpoint，用于服务端/客户端连接到 S3 API
S3_ENDPOINT=NEW_S3_ENDPOINT # 需要替换为你 Cloudflare R2 的 S3 Endpoint
# 必填，S3 的 Bucket
S3_BUCKET=NEW_S3_BUCKET # 需要替换为你 Cloudflare R2 的 Bucket 名称
# 必填，S3 的 Public Domain，用于客户端通过公开连接访问非结构化数据
S3_PUBLIC_DOMAIN=NEW_S3_PUBLIC_DOMAIN # 需要替换为你 Cloudflare R2 的 Public Domain
# 选填，S3 的 Enable Path Style。 对于 Cloudflare R2, 设置为 0
S3_ENABLE_PATH_STYLE=0
```

### nginx.conf文件

```conf

events {
    worker_connections 1024;
}

http {
    upstream lobe {
        server lobe:3210;
    }

    server {
        listen 80;
        server_name NEW_DOMAIN;
        return 301 https://$server_name$request_uri;
    }

    server {
        listen 443 ssl;
        server_name NEW_DOMAIN;

        ssl_certificate /etc/nginx/ssl/NEW_DOMAIN.pem;
        ssl_certificate_key /etc/nginx/ssl/NEW_DOMAIN.key;

        location / {
            proxy_pass http://lobe;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}
```

### run.sh文件

```bash
#!/bin/bash

# 定义要修改的文件列表
files=("docker-compose.yml" ".env" "nginx.conf")

# 检查 env.txt 文件是否存在
if [ ! -f "env.txt" ]; then
  echo "错误: env.txt 文件不存在"
  exit 1
fi

# 循环读取 env.txt 文件中的每一行
while IFS= read -r line; do
  # 忽略空行和注释行
  if [[ -z "$line" || "$line" == \#* ]]; then
    continue
  fi

  # 使用等号分割 key 和 value
  IFS="=" read -r key value <<< "$line"

  # 去除 key 前后的空格
  key=$(echo "$key" | sed 's/^[[:space:]]*//;s/[[:space:]]*$//')

   # 对 value 中的特殊字符进行转义
  escaped_value=$(echo "$value" | sed 's/[\/&]/\\&/g;s/\+/\\+/g;s/=/\=/g;s/\./\\./g')

  # 如果 key 不为空，则执行替换
  if [[ -n "$key" ]]; then
    # 循环遍历文件列表
    for file in "${files[@]}"; do
      # 检查文件是否存在
      if [ -f "$file" ]; then
        # 使用 sed 命令进行替换，如果不存在则不替换
        sed -i "s/${key}/${escaped_value}/g" "$file"
      fi
    done
  fi
done < "env.txt"

echo "文件替换完成"

# 执行 docker compose up -d
docker compose up -d
echo "docker compose up -d 执行完成"
```

### env.txt文件
这个文件内容主要是用于统一整理所需要的参数，后面方便替换。 暂时可以先复制内容，根据后文获取到对应的数据后，再进行修改：

```
# 域名地址
NEW_DOMAIN=xxxxxx

# S3
NEW_S3_ACCESS_KEY_ID=xxxxxxxxx
NEW_S3_SECRET_ACCESS_KEY=xxxxxxxxx
NEW_S3_ENDPOINT=xxxxxxxxx
NEW_S3_BUCKET=xxxxxxxxx
NEW_S3_PUBLIC_DOMAIN=xxxxxxxxx

# GitHub OAuth
NEW_AUTH_GITHUB_ID=xxxxxxxxx
NEW_AUTH_GITHUB_SECRET=xxxxxxxxx

# POSTGRES
NEW_POSTGRES_PASSWORD=xxxxxxxxx

# SECRET
NEW_KEY_VAULTS_SECRET=xxxxxxxxx
NEW_NEXT_AUTH_SECRET=xxxxxxxxx
```

### ssl文件

这边因为用到了Cloudfare R2，所以，我们的ssl文件也就直接从Cloudfare中获取吧。这里默认已经将域名解析到Cloudfare了。

登陆Cloudfare后，打开[账户主页](https://dash.cloudflare.com/)，接着选择你的域名。
![PixPin_2024-12-13_15-31-57.png](../assets/images/2024-12-13-docker_compose_install_lobe_database_version.md/PixPin_2024-12-13_15-31-57.png)

接着选择`SSL/TLS`，然后选择`源服务器`，点击`创建证书`。

![PixPin_2024-12-13_15-33-57.png](../assets/images/2024-12-13-docker_compose_install_lobe_database_version.md/PixPin_2024-12-13_15-33-57.png)

在`主机名`一栏，删掉原有的内容，填写新的域名的前缀（比如你要搭建的域名为lobe.yourdormain.com，则输入lobe即可），其他参数默认即可，然后点击创建。
![PixPin_2024-12-13_15-37-12.png](../assets/images/2024-12-13-docker_compose_install_lobe_database_version.md/PixPin_2024-12-13_15-37-12.png)

创建完成后会在新的页面内显示你的`源证书`和`私钥`。
其中，新建一个文件，名为`NEW_DOMAIN.pem`，将`源证书`的内容复制到里面。
新建一个文件，名为`NEW_DOMAIN.key`，将`私钥`的内容复制到里面。

注意，这里的`NEW_DOMAIN`需要替换为你的域名（例如：`lobe.yourdormain.com.pem`和`lobe.yourdormain.com.key`）。
![PixPin_2024-12-13_15-41-07.png](../assets/images/2024-12-13-docker_compose_install_lobe_database_version.md/PixPin_2024-12-13_15-41-07.png)

### Cloudflare R2参数获取
参照[官方文档](https://lobehub.com/zh/docs/self-hosting/advanced/s3/cloudflare-r2)获取对应的参数。

参数一览：
```shell
# S3 秘钥
NEW_S3_ACCESS_KEY_ID=9998d6757e276cf9f1edbd325b7083a6
NEW_S3_SECRET_ACCESS_KEY=55af75d8eb6b99f189f6a35f855336ea62cd9c4751a5cf4337c53c1d3f497ac2

# 存储桶的名称
NEW_S3_BUCKET=lobechat
# 存储桶的请求端点
NEW_S3_ENDPOINT=https://0b33a03b5c993fd2f453379dc36558e5.r2.cloudflarestorage.com
# 存储桶对外的访问域名
NEW_S3_PUBLIC_DOMAIN=https://0b33a03b5c993fd2f453379dc36558e5.r2.cloudflarestorage.com
```
注意：这里的`NEW_S3_ENDPOINT`和`NEW_S3_PUBLIC_DOMAIN`保持一致即可。

### Github OAuth参数获取
参照[官方文档](https://lobehub.com/zh/docs/self-hosting/advanced/auth/next-auth/github)获取对应的参数。

或者你可以参照我的方法进行创建，与官方文档创建方案不一致，但也可以使用。

1. 登陆Github，并访问[OAuth APP 创建](https://github.com/settings/applications/new)
2. 填写应用名称（可以随意填写）、主页地址（你的域名，例如：https://lobe.jojo.host/ ）、认证回调地址（你域名下的回调地址，例如：https://lobe.jojo.host/api/auth/callback/github ），点击创建即可。
3. 点击创建客户端密钥
![PixPin_2024-12-13_16-03-19.png](../assets/images/2024-12-13-docker_compose_install_lobe_database_version.md/PixPin_2024-12-13_16-03-19.png)
创建完成后就能看到显示的`Client ID`和`Client Secret`了，注意不要刷新网页，刷新后`Client Secret`就看不到了。

参数一览：
```shell
# GitHub OAuth
NEW_AUTH_GITHUB_ID=Ov23li77pl9JHKGps6ZF
NEW_AUTH_GITHUB_SECRET=32bec1548326955fdd3dfc6b88bfa5c89cf1a37a
```


至此，我们已经准备好了大部分的参数，目前还剩下`NEW_POSTGRES_PASSWORD`、`NEW_KEY_VAULTS_SECRET`、`NEW_NEXT_AUTH_SECRET`三个参数需要自己生成。
- NEW_POSTGRES_PASSWORD: PostgreSQL 数据库密码，可以自由设置，注意一定要复杂一些。
- NEW_KEY_VAULTS_SECRET 和 NEW_NEXT_AUTH_SECRET: 用于加密敏感信息的密钥，可以在服务器上使用 `openssl rand -base64 32` 生成，两个密钥尽量保持不一致。

如果完成了以上内容，则我们已经得到了一个完整的`env.txt`文件。

## 直接运行

在终端中执行以下命令：
```bash
bash run.sh
```
现在，不出意外的话，LobeChat数据库版本已经成功安装了，打开域名应该就能够访问使用了。