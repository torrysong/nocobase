# NocoBase Monorepo 结构与部署说明

## 一、Monorepo 架构确认

**是的，NocoBase 使用了 Monorepo 结构。**

### 1.1 Monorepo 工具配置

**Lerna + Yarn Workspaces**

- **Lerna**: 用于管理多包版本和发布
  - 配置文件：`lerna.json`
  - 版本管理：统一版本号 `1.9.32`
  - 发布工具：`lerna publish`

- **Yarn Workspaces**: 用于包依赖管理
  - 配置在根目录 `package.json` 中
  - Workspace 模式：`packages/*/*` 和 `packages/*/*/*`
  - 支持包之间的内部依赖

### 1.2 目录结构

```
nocobase/
├── packages/
│   ├── core/              # 核心模块
│   │   ├── acl/          # 权限控制
│   │   ├── actions/      # 动作系统
│   │   ├── app/          # 应用核心
│   │   ├── auth/         # 认证系统
│   │   ├── build/        # 构建工具
│   │   ├── cache/        # 缓存系统
│   │   ├── cli/          # 命令行工具
│   │   ├── client/       # 前端核心
│   │   ├── create-nocobase-app/  # 应用创建工具
│   │   ├── data-source-manager/  # 数据源管理器
│   │   ├── database/     # 数据库核心
│   │   ├── devtools/     # 开发工具
│   │   ├── evaluators/   # 表达式求值器
│   │   ├── lock-manager/ # 锁管理器
│   │   ├── logger/       # 日志系统
│   │   ├── resourcer/    # 资源路由
│   │   ├── sdk/          # SDK
│   │   ├── server/       # 服务端核心
│   │   ├── snowflake-id/ # 雪花ID生成
│   │   ├── telemetry/    # 遥测
│   │   ├── test/         # 测试工具
│   │   └── utils/        # 工具函数
│   │
│   ├── plugins/          # 插件目录
│   │   └── @nocobase/    # 官方插件
│   │       ├── plugin-acl/
│   │       ├── plugin-client/
│   │       ├── plugin-data-source-main/
│   │       ├── plugin-ui-schema-storage/
│   │       ├── plugin-workflow/
│   │       └── ... (80+ 个插件)
│   │
│   └── presets/          # 预设配置
│       └── nocobase/     # NocoBase 预设（包含所有内置插件）
│
├── lerna.json            # Lerna 配置
├── package.json          # 根 package.json
├── tsconfig.json         # TypeScript 配置
├── Dockerfile            # Docker 构建文件
├── docker-compose.yml    # Docker Compose 配置
└── ...
```

## 二、核心包功能说明

### 2.1 Core 核心包

#### **@nocobase/app** - 应用核心
- **功能**: 应用入口，整合 Server 和 Preset
- **依赖**: `@nocobase/server`, `@nocobase/preset-nocobase`
- **作用**: 提供应用启动和初始化逻辑

#### **@nocobase/server** - 服务端核心
- **功能**: Koa 服务器、插件系统、中间件管理
- **依赖**: 
  - `koa` - Web 框架
  - `@nocobase/database` - 数据库
  - `@nocobase/resourcer` - 资源路由
  - `@nocobase/acl` - 权限控制
- **作用**: 提供 HTTP 服务、API 路由、插件加载

#### **@nocobase/client** - 前端核心
- **功能**: React 前端框架、Schema 组件系统
- **依赖**:
  - `react`, `react-dom`
  - `@formily/react`, `@formily/antd-v5`
  - `antd`
  - `react-router-dom`
- **作用**: 提供前端 UI 组件、Schema 渲染、状态管理

#### **@nocobase/database** - 数据库核心
- **功能**: Sequelize ORM 封装、Collection 管理、Field 系统
- **依赖**: `sequelize`, `joi`
- **作用**: 提供数据模型定义、数据库操作、迁移管理

#### **@nocobase/cli** - 命令行工具
- **功能**: 提供 `nocobase` 命令
- **命令**:
  - `nocobase dev` - 开发模式
  - `nocobase start` - 启动服务
  - `nocobase build` - 构建
  - `nocobase pm` - 插件管理
- **作用**: 项目管理和开发工具

#### **@nocobase/resourcer** - 资源路由
- **功能**: RESTful API 路由系统
- **作用**: 将 Collection 映射为 REST API

#### **@nocobase/acl** - 权限控制
- **功能**: 基于角色的访问控制（RBAC）
- **作用**: Collection、Field、Action 级别的权限管理

#### **@nocobase/actions** - 动作系统
- **功能**: 定义和执行数据操作动作
- **作用**: CRUD 操作、自定义动作

#### **@nocobase/data-source-manager** - 数据源管理器
- **功能**: 管理多个数据源（主数据库、外部数据库、API）
- **作用**: 统一数据源接口

#### **@nocobase/utils** - 工具函数
- **功能**: 通用工具函数
- **作用**: 提供共享的工具方法

### 2.2 Presets 预设包

#### **@nocobase/preset-nocobase** - NocoBase 预设
- **功能**: 包含所有内置插件的预设配置
- **包含插件**: 80+ 个官方插件
- **作用**: 提供开箱即用的完整功能集

### 2.3 主要插件包（部分）

#### **数据相关**
- `plugin-data-source-main` - 主数据源插件
- `plugin-data-source-manager` - 数据源管理
- `plugin-collection-sql` - SQL 集合
- `plugin-collection-tree` - 树形集合

#### **UI 相关**
- `plugin-client` - 客户端插件
- `plugin-ui-schema-storage` - UI Schema 存储
- `plugin-theme-editor` - 主题编辑器
- `plugin-mobile` - 移动端支持

#### **工作流**
- `plugin-workflow` - 工作流核心
- `plugin-workflow-manual` - 人工节点
- `plugin-workflow-delay` - 延迟节点
- `plugin-workflow-sql` - SQL 节点

#### **功能插件**
- `plugin-users` - 用户管理
- `plugin-auth` - 认证
- `plugin-file-manager` - 文件管理
- `plugin-notification-manager` - 通知管理

## 三、部署方式

### 3.1 部署方式概览

NocoBase 支持三种部署方式：

1. **Docker 部署**（推荐，适合无代码场景）
2. **create-nocobase-app 部署**（适合低代码开发）
3. **Git 源码部署**（适合开发调试）

### 3.2 Docker 部署（推荐）

#### 3.2.1 Dockerfile 构建流程

**多阶段构建：**

**阶段1: Builder（构建阶段）**
```dockerfile
FROM node:20-bookworm as builder
# 1. 安装依赖
RUN yarn install
# 2. 构建所有包
RUN yarn build --no-dts
# 3. 版本管理
RUN yarn lerna version ${NEWVERSION} -y
# 4. 发布到私有仓库（Verdaccio）
RUN yarn release:force --registry $VERDACCIO_URL
# 5. 创建应用
RUN yarn create nocobase-app my-nocobase-app
# 6. 安装生产依赖
RUN yarn install --production
# 7. 打包
RUN tar -zcf ./nocobase.tar.gz
```

**阶段2: Runtime（运行阶段）**
```dockerfile
FROM node:20-bookworm-slim
# 1. 安装系统依赖（PostgreSQL客户端、Nginx等）
# 2. 复制构建产物
COPY --from=builder /app/nocobase.tar.gz
# 3. 解压应用
RUN tar -zxf nocobase.tar.gz
# 4. 启动脚本
CMD ["/app/docker-entrypoint.sh"]
```

#### 3.2.2 Docker Compose 部署

**docker-compose.yml 包含：**

```yaml
services:
  # NPM 私有仓库（构建时使用）
  verdaccio:
    image: verdaccio/verdaccio
    ports: ["${VERDACCIO_PORT}:${VERDACCIO_PORT}"]
  
  # 数据库服务
  postgres:
    image: postgres:latest
    environment:
      POSTGRES_USER: ${DB_USER}
      POSTGRES_DB: ${DB_DATABASE}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    ports: ["${DB_POSTGRES_PORT}:5432"]
  
  mysql:
    image: mysql:8
    environment:
      MYSQL_DATABASE: ${DB_DATABASE}
      MYSQL_USER: ${DB_USER}
      MYSQL_PASSWORD: ${DB_PASSWORD}
    ports: ["${DB_MYSQL_PORT}:3306"]
  
  # NocoBase 应用
  nocobase:
    image: node:20-bookworm-slim
    command: ["yarn", "start"]
    env_file: ./.env
    volumes:
      - ./:/app
    ports: ["${APP_PORT}:${APP_PORT}"]
```

#### 3.2.3 Docker 启动流程

**docker-entrypoint.sh 执行步骤：**

1. **解压应用包**
   ```bash
   tar -zxf /app/nocobase.tar.gz -C /app/nocobase
   ```

2. **生成 Nginx 配置**
   ```bash
   yarn nocobase create-nginx-conf
   ```

3. **生成实例ID**
   ```bash
   yarn nocobase generate-instance-id
   ```

4. **启动 Nginx**
   ```bash
   nginx
   ```

5. **启动应用**
   ```bash
   yarn start --quickstart
   ```

### 3.3 create-nocobase-app 部署

#### 3.3.1 创建应用

```bash
# 全局安装
npm install -g create-nocobase-app

# 创建应用
create-nocobase-app my-app

# 或使用 yarn
yarn create nocobase-app my-app
```

#### 3.3.2 应用结构

创建的应用包含：
```
my-app/
├── packages/
│   └── app/              # 应用代码
│       ├── client/      # 前端代码
│       └── server/      # 服务端代码
├── .env                  # 环境变量
├── package.json
└── ...
```

#### 3.3.3 启动应用

```bash
cd my-app

# 安装依赖
yarn install

# 开发模式
yarn dev

# 生产模式
yarn build
yarn start
```

### 3.4 Git 源码部署

#### 3.4.1 克隆源码

```bash
git clone https://github.com/nocobase/nocobase.git
cd nocobase
```

#### 3.4.2 安装依赖

```bash
# 安装所有包的依赖
yarn install
```

#### 3.4.3 构建

```bash
# 构建所有包
yarn build

# 或单独构建
yarn build:client  # 构建前端
yarn build:server  # 构建服务端
```

#### 3.4.4 启动

```bash
# 开发模式
yarn dev

# 生产模式
yarn start
```

### 3.5 环境变量配置

**必需的环境变量：**

```env
# 数据库配置
DB_DIALECT=postgres          # 数据库类型: postgres, mysql, mariadb
DB_HOST=localhost
DB_PORT=5432
DB_DATABASE=nocobase
DB_USER=nocobase
DB_PASSWORD=nocobase

# 应用配置
APP_PORT=13000
APP_KEY=your-secret-key

# 存储配置
STORAGE_TYPE=local
STORAGE_BASE_URL=http://localhost:13000/storage/uploads
```

### 3.6 生产环境部署建议

#### 3.6.1 使用 Docker（推荐）

**优势：**
- 环境一致性好
- 部署简单
- 升级方便（只需拉取新镜像）

**步骤：**
```bash
# 1. 拉取镜像
docker pull nocobase/nocobase:latest

# 2. 运行容器
docker run -d \
  --name nocobase \
  -p 13000:13000 \
  -e DB_DIALECT=postgres \
  -e DB_HOST=postgres \
  -e DB_DATABASE=nocobase \
  -e DB_USER=nocobase \
  -e DB_PASSWORD=nocobase \
  -v ./storage:/app/nocobase/storage \
  nocobase/nocobase:latest
```

#### 3.6.2 使用 PM2（Node.js 进程管理）

```bash
# 安装 PM2
npm install -g pm2

# 启动应用
pm2 start yarn --name nocobase -- start

# 或使用 NocoBase CLI
yarn nocobase pm2 start
```

#### 3.6.3 使用 Nginx 反向代理

**Nginx 配置示例：**

```nginx
server {
    listen 80;
    server_name your-domain.com;

    location / {
        proxy_pass http://localhost:13000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    # 静态文件
    location /storage {
        alias /app/nocobase/storage;
    }
}
```

## 四、Monorepo 开发工作流

### 4.1 本地开发

```bash
# 1. 克隆项目
git clone https://github.com/nocobase/nocobase.git
cd nocobase

# 2. 安装依赖（会自动安装所有 workspace 包的依赖）
yarn install

# 3. 开发模式启动（支持热重载）
yarn dev

# 4. 构建所有包
yarn build

# 5. 运行测试
yarn test
```

### 4.2 包管理

```bash
# 添加新包
cd packages/core
mkdir my-package
cd my-package
yarn init

# 在根目录 package.json 中添加 workspace
# workspaces: ["packages/*/*", "packages/core/my-package"]

# 安装依赖（会自动链接 workspace 内的包）
yarn add @nocobase/client  # 会自动使用 workspace 内的版本
```

### 4.3 版本管理

```bash
# 使用 Lerna 管理版本
yarn lerna version patch    # 补丁版本
yarn lerna version minor    # 次要版本
yarn lerna version major    # 主要版本

# 发布所有包
yarn release
```

### 4.4 插件开发

```bash
# 创建新插件
cd packages/plugins/@nocobase
mkdir plugin-my-plugin
cd plugin-my-plugin

# 插件结构
plugin-my-plugin/
├── src/
│   ├── client/        # 前端代码
│   └── server/        # 服务端代码
├── package.json
└── README.md
```

## 五、部署架构图

```
┌─────────────────────────────────────────┐
│         Docker Container                 │
│  ┌───────────────────────────────────┐  │
│  │         Nginx (反向代理)            │  │
│  │  - 静态文件服务                     │  │
│  │  - API 代理                        │  │
│  └──────────────┬────────────────────┘  │
│                 │                         │
│  ┌──────────────▼────────────────────┐  │
│  │    NocoBase Application            │  │
│  │  ┌──────────────────────────────┐ │  │
│  │  │  @nocobase/app               │ │  │
│  │  │  ├── @nocobase/server        │ │  │
│  │  │  │   ├── Koa Server          │ │  │
│  │  │  │   ├── Plugin System       │ │  │
│  │  │  │   └── API Routes          │ │  │
│  │  │  └── @nocobase/preset-*      │ │  │
│  │  │      └── Plugins (80+)       │ │  │
│  │  └──────────────────────────────┘ │  │
│  └──────────────┬────────────────────┘  │
└─────────────────┼────────────────────────┘
                  │
┌─────────────────▼──────────────────────┐
│         Database (PostgreSQL/MySQL)     │
│  - Collections (数据表)                  │
│  - UI Schemas (页面配置)                │
│  - Users, Roles, Permissions            │
└─────────────────────────────────────────┘
```

## 六、总结

### 6.1 Monorepo 优势

1. **统一管理**: 所有包在一个仓库，版本统一
2. **依赖共享**: 包之间可以直接引用，无需发布
3. **原子提交**: 相关改动可以一起提交
4. **类型安全**: TypeScript 类型可以在包之间共享

### 6.2 部署选择建议

- **生产环境**: 使用 Docker 部署（最简单、最稳定）
- **开发调试**: 使用 Git 源码部署（可以修改源码）
- **定制开发**: 使用 create-nocobase-app（业务代码独立）

### 6.3 关键文件

- `lerna.json` - Monorepo 版本管理
- `package.json` - Workspace 配置
- `Dockerfile` - Docker 构建
- `docker-compose.yml` - 本地开发环境
- `docker-entrypoint.sh` - 容器启动脚本

