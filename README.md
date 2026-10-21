# Zircon Mir3 Server for LazyCat

将 [Zircon Mir3 Server](https://github.com/raphaelcheung/zircon-legend-server)（皓石传奇三）打包为 LazyCat LPK v2。

## ⚠️ 运行数据必须自备

上游把**运行必需的地图 / 运营数据（约 800 MB）放在百度网盘和 QQ 群文件里**，不在 GitHub Release，因此镜像与应用都不含这些数据。启动前需自行下载解压，把 `Map` 目录与数据文件放入本应用的数据目录。应用入口页与 `usage` 都写明了这一前置条件，并提供了一份可直接使用的 `Server.ini`。

内置测试账号：`zrf@zrf.zrf` 与 `raphael@gm.gm`（管理员），密码均为 `123456`。

## 端口

| 用途 | 协议 | 端口 |
| --- | --- | ---: |
| 游戏连接 | TCP | `7000` |
| Web 管理后台 | TCP | `7080` |
| 在线人数 | TCP | `3000` |

## 镜像来源

上游在 release 中发布预编译服务端 zip，镜像由 [dockers-x/zircon](https://github.com/dockers-x/zircon) 构建：

- 运行镜像：`ghcr.1ms.run/dockers-x/zircon:v1.14.0`
- 基于 `mcr.microsoft.com/dotnet/aspnet:8.0`，按平台下载 `Server-<tag>-linux-{x64,arm64}.zip` 解压到 `/zircon`
- 每天检查上游最新 release，有新版本则构建并推送 `v{版本}` 与 `latest`

本仓库使用 GHCR 默认加速地址，并在发布前校验 `linux/amd64` 源镜像与加速地址 digest 一致。

## 数据与运行方式

- 持久化：`/lzcapp/var/zircon` → `/zircon/datas`（地图数据、`Server.ini`、`user.db` 等）
- 服务名：`zircon`
- 未添加健康检查：上游 .NET 服务端没有 HTTP 健康检查端点

## 本地构建

```bash
lzc-cli project build -o dist/application.lpk
lzc-cli lpk info dist/application.lpk
```

## 自动发布

`.github/workflows/lazycat.yml` 使用 `ca-x/lazycat-github-action` 跟踪镜像的不可变 `v*` tag，创建版本化 GitHub Release Asset 并只发布到喵喵私有商店。需要为仓库配置 `APPSTORE_URL`、`APPSTORE_TOKEN`，以及可选的 `APP_ID`、`PRIVATE_STORE_GROUP_CODES`。

## 许可与声明

AGPL-3.0（上游）。上游声明：本项目仅供学习游戏技术，禁止商用及非法用途。