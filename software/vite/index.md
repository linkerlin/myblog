---
title: Vite 是什么？
---
Vite 是一个现代化的前端构建工具和开发服务器，专为快速开发和优化 JavaScript 项目设计。它由 Vue.js 作者尤雨溪创建，具有以下特点：
- **极速开发体验**：利用浏览器原生 ES 模块（ESM），无需打包即可启动开发服务器，热更新（HMR）极快。
- **高效生产构建**：生产环境使用 Rollup 打包，生成优化的静态资源。
- **支持多种框架**：兼容 Vue、React、Svelte、Vanilla JS 等。
- **简单配置**：开箱即用，配置灵活，适合小型到大型项目。
- **插件生态**：复用 Rollup 和 Vite 插件，扩展性强。

相比 Webpack，Vite 的开发启动速度更快，配置更简单，非常适合现代前端开发。

---

## Vite 简明教程

这个教程将带你快速创建一个 Vite 项目，运行开发服务器，并构建生产代码。总耗时约 5 分钟！

### 1. 安装 Node.js
确保你已安装 Node.js（版本 >= 18.0.0）。检查版本：
```bash
node -v
```
如果版本过低，下载最新 LTS 版本（https://nodejs.org/）或使用 `nvm` 安装：
```bash
nvm install 18
nvm use 18
```

### 2. 创建 Vite 项目
运行以下命令创建项目：
```bash
npm create vite@latest my-vite-project
```
- 按提示选择框架（如 Vue、React、Vanilla）：
  - 键入 `Vanilla`（纯 JavaScript）或 `Vue` 等。
- 选择语言：`JavaScript` 或 `TypeScript`。
- 完成后进入项目目录并安装依赖：
```bash
cd my-vite-project
npm install
```

### 3. 启动开发服务器
运行开发服务器：
```bash
npm run dev
```
- Vite 会在 `http://localhost:5173` 启动一个开发服务器。
- 打开浏览器访问，你会看到一个简单的页面（例如 Vite 的欢迎页面）。
- 修改 `src/main.js`（或 `src/main.ts`），保存后页面会自动刷新，体验热更新（HMR）。

### 4. 项目结构
Vite 项目结构非常简单，以下是典型目录：
```
my-vite-project/
├── node_modules/        # 依赖
├── public/             # 静态资源（如图片）
├── src/               # 源代码
│   ├── main.js       # 入口文件
│   ├── App.vue      # 主组件（如果使用 Vue/React）
│   └── assets/      # 样式、图片等
├── index.html        # 入口 HTML
├── package.json      # 项目配置
└── vite.config.js    # Vite 配置文件
```

### 5. 编写简单代码
以 Vanilla JS 为例，编辑 `src/main.js`：
```javascript
import './style.css'

document.querySelector('#app').innerHTML = `
  <h1>Hello, Vite!</h1>
  <button id="click-me">Click me</button>
`

document.querySelector('#click-me').addEventListener('click', () => {
  alert('You clicked me!')
})
```
- 保存后，浏览器会自动更新，显示一个标题和可点击的按钮。
- Vite 会自动处理 CSS 导入和模块化。

### 6. 配置 Vite（可选）
Vite 的默认配置已足够，但你可以通过 `vite.config.js` 自定义。例如：
```javascript
import { defineConfig } from 'vite'

export default defineConfig({
  server: {
    port: 3000, // 自定义端口
  },
  build: {
    outDir: 'dist', // 构建输出目录
  },
})
```
- `server.port`：更改开发服务器端口。
- `build.outDir`：指定生产构建输出目录。

### 7. 构建生产代码
生成生产环境的优化代码：
```bash
npm run build
```
- Vite 使用 Rollup 打包，输出到 `dist` 目录。
- 预览构建结果：
```bash
npm run preview
```
- 这会在本地启动一个服务器，模拟生产环境（默认 `http://localhost:4173`）。

### 8. 部署（可选）
将 `dist` 目录中的文件上传到静态托管服务（如 Netlify、Vercel、GitHub Pages）即可部署。确保 `index.html` 在根目录。

---

## 常见问题
- **启动失败（如 `vite: command not found`）**：
  - 确保运行 `npm install` 安装依赖。
  - 使用 `npx vite` 或检查 `package.json` 的 `scripts` 是否包含 `"dev": "vite"`。
  - 确认 Node.js 版本 >= 18。
- **热更新不生效**：
  - 检查文件是否在 `src` 目录下。
  - 确保没有语法错误（查看浏览器或终端错误信息）。
- **需要更多功能**：
  - 添加插件（如 `@vitejs/plugin-vue` 用于 Vue）。
  - 参考官方文档：https://vitejs.dev/

---

## 总结
你已经学会：
- 使用 Vite 创建并运行一个前端项目。
- 编写简单代码并体验热更新。
- 配置 Vite 和构建生产代码。

Vite 的速度和简单性让前端开发更高效！继续探索官方文档（https://vitejs.dev/）或尝试集成框架（如 Vue、React）以扩展项目。

