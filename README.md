# ALIST-FLY

是 alist 服务的 fly.io 部署形式

## alist

一个支持多种存储的文件列表程序，使用 Gin 和 Solidjs。

我部署的是使用 aria2 离线下载的版本，支持aria2 离线下载。

## fly.io 

- 支持自定义域名
- 全球性支持，部署在hkg(中国香港)可以正常访问，也可自行套ECDN。
- 需要信用卡
- 全天候免费不停机的服务
- Fly.io 会根据单个用户或者组织计费，如果想要更多免费的应用，那么你可以创建多个组织。
    - VM: shared-cpu	每个月 2340 小时	全天候运行 3 个 256 MB 内存的共享 CPU 的 VM
    - Volumes	3GB	提供 3GB 永久存储
    - Bandwidth	160GB / 月	亚洲和印度免费流量是 30G，美国和欧洲是 100G
- 本次部署是采用数据持久层来存档 Alist 的 Sqlite3 数据库。用PostgreSQL 多用一个 VM 资源，并且免费用户一旦创建了 PostgreSQL 就消耗掉了免费的 1G 空间，如果用 Sqlite3 则只消耗免费的 1G 空间。

## 部署

1.准备工作

```bash
# 安装 FlyCTL

# macOS
curl -L https://fly.io/install.sh | sh

# Linux
curl -L https://fly.io/install.sh | sh

# Windows
powershell -Command "iwr https://fly.io/install.ps1 -useb | iex"

# 注册
flyctl auth signupc

# 登陆
flyctl auth login

# 拉取本项目文件
git clone https://github.com/lushi78778/alist-fly.git

# 到项目文件目录
cd alist-fly
```

2.开始部署

```bash
# 确认在项目目录下，已经安装好FlyCTL，并登录

# 创建应用 直接应用我的 [(fly.toml](fly.toml) 模板，注意看一下文字，不难我不想截图了。如需要调整vm规格及时调整，注意免费额度限制就好。
# 这个命令会直接尝试部署，但[(fly.toml](fly.toml)定义了一个硬盘空间，需要手动创建，所以第一次会失败，这是正常的。
flyctl launch

# 创建名为data的硬盘空间 1GB，创建成功会返回相关信息，包括卷id
flyctl volumes create data --size 

# 部署应用，这是就成功部署了。
flyctl deploy
```

3.参考命令

```bash
# 删除卷的参考命令
fly vol destroy <卷id>

# 连接虚拟机
fly ssh console
```







