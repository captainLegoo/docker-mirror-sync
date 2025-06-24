# 🚀 Docker Image Mirror to Any ACR

Use GitHub Actions to automatically mirror public Docker images to any container registry (e.g., Azure Container Registry, Alibaba Cloud ACR), improving access speed and reliability, especially for users in regions with limited connectivity to upstream registries.

---

📖 本文档也提供中文版：[点击查看中文说明](./README.zh.md)

---

## ✨ Features

- 📥 Automatically pull and push images from registries like DockerHub, GHCR, Quay, etc.
- 🔁 Supports scheduled sync, manual trigger, or automatic sync when `images-list.txt` is updated
- 🔐 Manage registry credentials securely using GitHub Secrets
- 🧩 Compatible with any OCI-compliant image source

---

## 📁 Project Structure

```
├── .github/workflows/
│ └── sync.yml # GitHub Actions workflow definition
├── images-list.txt # List of images to sync (source images only)
├── .gitignore
└── LICENSE
```

## 🧾 How to Use

### 1️⃣ Edit the Image List

Modify the `images-list.txt` file in the project root:

```
nginx
docker.io/library/busybox:1.36
ghcr.io/openfaas/faas:latest
```

- Tag is optional (defaults to `latest`)
- Registry URLs are supported
- One image per line
- Comments are allowed (lines starting with `#`)

---

### 2️⃣ Configure GitHub Secrets

Go to your repository: **Settings → Secrets and variables → Actions → Secrets**, and add the following secrets:

| Name            | Example                           | Description                        |
| --------------- | --------------------------------- | ---------------------------------- |
| `ACR_USERNAME`  | your-username                     | ACR username (main account or RAM) |
| `ACR_PASSWORD`  | your-password                     | ACR access credential              |
| `ACR_REGISTRY`  | registry.cn-hangzhou.aliyuncs.com | ACR registry address               |
| `ACR_NAMESPACE` | myrepo                            | Target namespace in your ACR       |

---

### 3️⃣ Trigger the Sync

There are three ways to trigger synchronization:

- ✅ **Manual trigger**: Click “Run workflow” in GitHub Actions
- 🕒 **Scheduled sync**: Automatically runs daily (customizable)
- 📝 **File update trigger**: Runs when `images-list.txt` is modified

---

## 📦 Target Image Naming Rules

Assuming your `ACR_NAMESPACE` is `myrepo`, images will be pushed with the following naming logic:

| Entry in `images-list.txt`     | Resulting ACR Image URL                                 |
| ------------------------------ | ------------------------------------------------------- |
| `nginx`                        | `registry.cn-hangzhou.aliyuncs.com/myrepo/nginx:latest` |
| `docker.io/library/nginx:1.25` | `registry.cn-hangzhou.aliyuncs.com/myrepo/nginx:1.25`   |
| `ghcr.io/username/abc:dev`     | `registry.cn-hangzhou.aliyuncs.com/myrepo/abc:dev`      |

> 🔁 Only the final image name is retained. Registry and organization path are removed.

---

## 📌 Notes

- Ensure that the target namespace (`myrepo`) is already created in ACR
- ACR web console may show pushed images with slight delay due to caching
- Private image support is possible but requires additional login configuration

---

## 📄 License

This project is licensed under the [MIT License](./LICENSE).

---

## 🙌 Acknowledgments

This project is powered by GitHub Actions. If you find it useful, feel free to ⭐ Star it or contribute via PR!