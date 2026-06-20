# Ansible 一键部署 WordPress 博客

[![Ansible](https://img.shields.io/badge/Ansible-2.9+-EE0000?logo=ansible&logoColor=white)](https://www.ansible.com/)
[![Docker](https://img.shields.io/badge/Docker-20.10+-2496ED?logo=docker&logoColor=white)](https://www.docker.com/)
[![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)
[![Platform](https://img.shields.io/badge/platform-CentOS%207%2B-brightgreen)]()

使用 Ansible + Docker Compose 在 CentOS 服务器上一键部署 WordPress 个人博客，包含 MySQL 8.0 数据库。全程自动化，无需手动安装 Docker 环境。

## 架构概览

```
┌─────────────────────────────────────┐
│           CentOS Server             │
│  ┌──────────────┐ ┌──────────────┐ │
│  │  WordPress   │ │   MySQL 8.0  │ │
│  │  Container   │ │  Container   │ │
│  │   (80端口)    │ │  (3306端口)  │ │
│  └──────────────┘ └──────────────┘ │
│         ▲ Docker Network ▲          │
│  ┌──────────────────────────────┐   │
│  │       Docker + Compose       │   │
│  │     (Ansible 自动安装)        │   │
│  └──────────────────────────────┘   │
│              ▲                      │
│  ┌──────────────────────────────┐   │
│  │     Ansible 控制端 (本机)     │   │
│  └──────────────────────────────┘   │
└─────────────────────────────────────┘
```

## 技术栈

| 技术 | 用途 |
|------|------|
| Ansible | 自动化编排，安装 Docker、部署服务 |
| Docker | 容器化运行 WordPress 和 MySQL |
| Docker Compose | 多容器编排 |
| WordPress | 博客应用 |
| MySQL 8.0 | 数据库 |

## 目录结构

```
wordpress-docker-ansible-deploy/
├── README.md
├── inventory              # 目标主机清单
├── site.yml               # 主 playbook
├── docker-compose.yml     # WordPress + MySQL 编排文件
└── vars/
    └── main.yml           # 可配置变量
```

## 快速开始

### 1. 环境要求

- **控制端：** 任意 Linux/macOS，安装 Ansible ≥ 2.9
- **被控端：** CentOS 7+，SSH 密钥认证已配置

### 2. 克隆项目

```bash
git clone https://github.com/psy-psy329/wordpress-docker-ansible-deploy.git
cd wordpress-docker-ansible-deploy
```

### 3. 修改配置

编辑 `inventory` 文件，替换为你的服务器 IP：

```ini
[wordpress]
你的服务器IP ansible_user=root
```

编辑 `vars/main.yml`（可选）：

```yaml
docker_compose_version: "2.24.0"
compose_project_dir: "/opt/myblog"
```

### 4. 执行部署

```bash
ansible-playbook -i inventory site.yml
```

Playbook 会自动完成：
1. 安装 Docker 和 Docker Compose
2. 配置镜像加速器
3. 创建项目目录
4. 启动 WordPress + MySQL 容器

### 5. 访问博客

部署完成后，浏览器访问 `http://<你的服务器IP>` 进入 WordPress 安装向导。

## 常用命令

```bash
# 重启服务
ansible all -i inventory -m shell -a "docker compose -f /opt/myblog/docker-compose.yml restart"

# 查看容器状态
ansible all -i inventory -m shell -a "docker ps"

# 查看日志
ansible all -i inventory -m shell -a "docker compose -f /opt/myblog/docker-compose.yml logs --tail=50"

# 停止服务
ansible all -i inventory -m shell -a "docker compose -f /opt/myblog/docker-compose.yml down"
```

## 排错指南

| 问题 | 可能原因 | 解决方法 |
|------|----------|----------|
| Docker 安装失败 | yum 仓库不可用 | 检查网络，确认 `/etc/yum.repos.d/docker-ce.repo` |
| 镜像拉取慢 | 未配置镜像加速 | 检查 `/etc/docker/daemon.json` |
| 端口被占用 | 80 端口冲突 | 修改 `docker-compose.yml` 中的端口映射 |
| 数据库连接失败 | MySQL 未就绪 | 等待 30 秒后重试，MySQL 首次启动较慢 |

## License

MIT
