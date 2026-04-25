# Ansible 一键部署 WordPress 博客（Docker + Docker Compose）

## 项目简介
本项目使用 Ansible 自动化部署一个基于 Docker 和 Docker Compose 的 WordPress 个人博客，包含 MySQL 8.0 数据库。只需一条命令，即可在任意 CentOS 服务器上完成 Docker 安装、镜像加速配置、服务启动等全流程。

## 技术栈
- Ansible（自动化编排）
- Docker & Docker Compose（容器化）
- WordPress + MySQL 8.0

## 目录结构
ansible-wordpress-deploy/
├── README.md
├── inventory # 目标主机清单
├── site.yml # 主 playbook
├── docker-compose.yml # WordPress + MySQL Compose 文件
├── vars/
│ └── main.yml # 可配置变量
└── files/
└── (如有额外文件可放此)

## 快速开始

### 1. 准备控制机
- 安装 Ansible（版本 ≥ 2.9）
- 确保控制机可以 SSH 登录目标服务器（推荐使用密钥认证）

### 2. 修改配置
- 编辑 `inventory` 文件，替换为你的服务器 IP 和 SSH 用户
- 编辑 `vars/main.yml`，可以修改端口、数据库密码等（可选）

### 3. 执行部署
```bash
ansible-playbook -i inventory site.yml
