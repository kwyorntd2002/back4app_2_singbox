# Back4App Sing-box VLESS 部署模板

这是一个用于在 [Back4App](https://www.back4app.com/) 免费容器云平台上零成本部署 **Sing-box VLESS** 节点的配置模板。

相比于 GitHub Codespaces，Back4App 作为正式的 PaaS 容器托管平台，拥有 **7x24 小时在线** 的优势，且提供每月 100GB 的免费流量，非常适合作为个人备用节点。

---

## 📂 仓库包含文件

* **`Dockerfile`**: 容器构建脚本。基于轻量级 Alpine Linux，自动下载并运行官方 Sing-box 核心。
* **`config.json`**: Sing-box 的核心配置文件。默认配置为 VLESS + WS 协议，监听内部 `8080` 端口。

---

## 🚀 部署教程 (纯手动挡极速版)

### 第一步：修改核心配置（务必执行）
在部署之前，请先修改本仓库中的 `config.json` 文件：
1.  **修改 UUID**: 将 `"你的UUID"` 替换为你自己生成的真实 UUID（如 `93f6e6d0-...`）。
2.  6cb0b504-95bc-40e5-9bbd-4ead29aef40d
3.  **修改 Path**: 将 `"/chat"` 修改为你想要的伪装路径（以 `/` 开头）。

### 第二步：在 Back4App 创建应用
1.  登录 [Back4App Containers](https://www.back4app.com/containers) 面板。
2.  点击 **"Create New App"**，选择 **"Container"** 模式。
3.  绑定你的 GitHub 账号，并选择本仓库（`hezhanleiok/singbox`）。
4.  在部署设置中填写以下参数：
    * **App Name**: 随意起一个英文名（例如 `my-web-api`）。
    * **Branch**: `main`
    * **Root Directory**: `./`
    * **Port (端口)**: **`8080`** (极其重要，需与 config.json 中一致)
5.  点击底部的 **"Create App"** / **"Deploy"** 开始自动构建。

### 第三步：获取域名并测试
1.  部署完成后，在 Back4App 面板左上方或右侧找到你的专属域名（格式如 `你的App名.b4a.run`）。
2.  观察部署日志（Logs），只要看到 `sing-box started` 或 `DEPLOYMENT READY` 即代表服务端运行成功。

---

## 📱 客户端配置参数 (以 v2rayN 为例)

Back4App 会在最外层自动套用 TLS 加密，因此我们在客户端配置时，端口必须填 `443`。

* **地址 (Address)**: `你的App名.b4a.run` (去掉开头的 https://)
* bakc4app2-3b64tqp1.b4a.run
* **端口 (Port)**: `443`
* **用户 ID (UUID)**: 填入你在 config.json 中修改的 UUID
* **传输协议 (Network)**: `ws`
* **伪装类型 (Header type)**: `none`
* **路径 (Path)**: 填入你在 config.json 中修改的路径 (如 `/chat`)
* **底层传输安全 (TLS)**: `tls`
* **SNI**: `你的App名.b4a.run` (与地址相同)

### 🔗 一键导入链接生成公式
你可以将自己的参数替换到下方的模板中，直接复制整段字符串导入到 v2rayN、Shadowrocket 等客户端：

```text
vless://填写你的UUID@填写你的域名:443?encryption=none&security=tls&type=ws&host=填写你的域名&path=%2f填写你的路径&sni=填写你的域名#Back4App节点
