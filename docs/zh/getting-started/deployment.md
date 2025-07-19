# 部署

本指南将引导您完成在您自己的服务器上部署 Hyac 的过程。

## 先决条件

- 一台安装了 Docker 和 Docker Compose 的服务器。
- 一个域名（可选，但建议在生产环境中使用）。

## 1. 克隆仓库

```bash
git clone https://github.com/Pidbid/Hyac.git
cd Hyac
```

## 2. 配置环境变量

将 `.env.example` 文件复制为 `.env` 并填写所需的值。

```bash
cp .env.example .env
```

## 3. 启动服务

```bash
docker-compose up -d
```

这将在后台启动所有必需的服务。

## 4. 启动前端

```bash
cd web
pnpm install
pnpm dev
```

## 5. 访问系统

- 前端: `http://localhost:5173`
- 服务端 API 文档: `http://localhost:8000/docs`
