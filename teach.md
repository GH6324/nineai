# **NineAI 开源版 - 项目部署指南**

## **📦 部署方式**
本项目提供 **整合包（YiAiQuickDeploy）** 和 **编译包** 两种部署方式，请根据需求选择。

---

## **🚀 整合包快速部署（推荐）**
### **📌 环境准备**
| 依赖 | 版本要求 |
|------|----------|
| Node.js | ≥ 16 |
| pnpm | ≥ 6 |
| MySQL | ≥ 5.7 |
| Redis | 最新版 |

#### **1. 安装 Node.js**
- 官网下载：[Node.js 官网](https://nodejs.org/)
- 验证安装：
  ```bash
  node -v
  npm -v
  ```

#### **2. 安装 PM2（进程管理）**
```bash
npm install pm2 -g
```

#### **3. 安装 PNPM（包管理）**
```bash
npm install -g pnpm
```

---

### **⚙️ 配置项目**
#### **1. 复制环境变量文件**
```bash
cp .env.example .env
```
- 修改 `.env` 中的数据库、Redis 等配置。

#### **2. 安装依赖**
```bash
pnpm install
# 若安装失败，可切换国内源：
pnpm config set registry https://registry.npmmirror.com
```

---

### **🚀 启动项目**
#### **1. 运行服务**
```bash
pnpm start  # 默认监听 9520 端口
```

#### **2. 访问项目**
- 前端地址：`http://localhost:9520`
- 管理后台：`http://localhost:9520/admin`
  - 普通管理员账号：`admin` / `123456`
  - 超级管理员账号：`super` / `123456`

> ⚠️ **安全提示**：首次登录后请立即修改密码！

---

### **❌ 解决刷新 404 问题**
Nginx 需配置 `try_files` 支持前端路由：
```nginx
location / {
  try_files $uri $uri/ /index.html;
}
```

---

### **🔄 项目升级**
1. 拉取最新代码：
   ```bash
   git pull
   ```
2. 关闭旧进程：
   ```bash
   pm2 delete nineai
   ```
3. 重新安装依赖并启动：
   ```bash
   pnpm install
   pnpm start
   ```

---

## **🔧 编译包部署（手动构建）**
适用于二次开发或定制化需求。

### **1. 后端服务部署**
进入 `service` 目录：
```bash
cd service
cp .env.example .env  # 修改数据库配置
pnpm install
pnpm dev    # 调试模式（自动导入数据库）
pnpm build  # 编译打包
```

### **2. 前端部署**
#### **用户端（Chat）**
进入 `chat` 目录：
```bash
cd chat
pnpm install
pnpm build  # 生成静态文件到 `dist` 目录
```

#### **管理端（Admin）**
进入 `admin` 目录：
```bash
cd admin
pnpm install
pnpm build  # 生成静态文件到 `dist` 目录
```

### **3. 启动服务**
将编译后的文件部署至 Web 服务器（如 Nginx），或使用 PM2 运行后端：
```bash
pm2 start dist/main.js --name nineai
```

---

## **📢 注意事项**
1. **数据库备份**：升级前请备份 MySQL 数据。
2. **端口冲突**：若 9520 端口被占用，修改 `.env` 中的 `APP_PORT`。
3. **HTTPS 配置**：生产环境建议使用 Nginx 配置 SSL 证书。

---

## **💬 获取帮助**
- **问题反馈**：[提交 Issue](https://github.com/your-repo/issues)
- **交流群**：扫码添加微信  
  ![](https://qr.haydenstudio.hk/console/upload/IMG_0122_9053.png)

**🌟 祝您部署顺利！欢迎贡献代码或提出建议！** 🚀
