# 🚀 Docker Image Mirror to Any ACR

使用 GitHub Actions 自动同步公共 Docker 镜像到任意容器镜像服务（如Azure ACR, 阿里云 ACR），提升访问速度，解决拉取缓慢或失败的问题。

---

📘 This README is also available in [English](./README.md)

---

## ✨ 功能特点

- 📥 支持自动拉取并转存国外镜像（DockerHub、GHCR、Quay 等）
- 🔁 支持定时同步、手动触发、或每次修改镜像列表后自动执行
- 🔐 使用 GitHub Secrets 安全管理登录凭据
- 🧩 兼容任意 OCI 兼容镜像源

---

## 📁 项目结构

```
 ├── .github/workflows/
 │   └── sync.yml            # GitHub Actions 同步逻辑
 ├── images-list.txt         # 你要同步的镜像清单（只写源镜像名）
 ├── .gitignore
 └── LICENSE
```



## 🧾 使用方法

### 1️⃣ 编辑镜像列表

修改根目录下的 `images-list.txt`，添加你希望同步的镜像：

```
nginx
docker.io/library/busybox:1.36
ghcr.io/openfaas/faas:latest
```

- 可省略 tag（默认为 `latest`）
- 支持带 registry 地址的镜像
- 每行一个镜像，支持注释行（以 `#` 开头）

---

### 2️⃣ 配置 GitHub Secrets

前往你的仓库：`Settings → Secrets and variables → Actions → Secrets`，添加：

| 名称            | 示例值                            | 用途                           |
| --------------- | --------------------------------- | ------------------------------ |
| `ACR_USERNAME`  | 用户名                            | 阿里云镜像仓库用户名（主账号） |
| `ACR_PASSWORD`  | 密码                              | 登录凭证（控制台创建）         |
| `ACR_REGISTRY`  | registry.cn-hangzhou.aliyuncs.com | 镜像仓库地址                   |
| `ACR_NAMESPACE` | `myrepo`                          | ACR 命名空间                   |

---


### 3️⃣ 启动同步任务

支持三种触发方式：

- ✅ **手动触发**：在 GitHub Actions 页面点击 "Run workflow"
- 🕒 **定时触发**：每天凌晨自动执行（可修改）
- 📝 **更新镜像列表时触发**：每次修改 `images-list.txt` 自动同步

---

## 📦 推送后的镜像命名规则

假设 `ACR_NAMESPACE` 是 `myrepo`，以下是示例：

| `images-list.txt` 里的值       | 推送后的镜像地址                                        |
| ------------------------------ | ------------------------------------------------------- |
| `nginx`                        | `registry.cn-hangzhou.aliyuncs.com/myrepo/nginx:latest` |
| `docker.io/library/nginx:1.25` | `registry.cn-hangzhou.aliyuncs.com/myrepo/nginx:1.25`   |
| `ghcr.io/xxx/abc:dev`          | `registry.cn-hangzhou.aliyuncs.com/myrepo/abc:dev`      |

> 🔁 仅保留最终镜像名（不包含源 registry、组织路径）

---

## 📄 License

[MIT](./LICENSE)

---

## 🙌 鸣谢

本项目使用 GitHub Actions 自动完成任务，如你觉得有帮助，欢迎 Star 🌟！