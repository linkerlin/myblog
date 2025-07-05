# Vitest 入门教程：从零开始的通俗易懂指南

欢迎学习 **Vitest**，一个基于 Vite 的超快单元测试框架！无论你是前端新手还是老手，这个教程将带你一步步掌握 Vitest 的核心功能，用简单易懂的方式帮你快速上手。本教程基于 Vitest 官方文档（截至 2025 年 7 月）和其他可靠资源编写，涵盖安装、配置、编写测试用例、测试组件等内容。

---

## 什么是 Vitest？

Vitest 是一个专为现代 JavaScript 项目设计的测试框架，尤其适合使用 Vite 构建的项目。它由 Vite 和 Vue 团队成员开发，具有以下特点：
- **超快**：利用 Vite 的快速编译和热更新机制，测试速度飞快。
- **与 Vite 高度集成**：复用 Vite 的配置和插件，减少重复工作。
- **Jest 兼容**：支持 Jest 的 API，方便从 Jest 迁移。
- **支持多种框架**：适用于 Vue、React、Svelte 等前端框架。
- **功能丰富**：支持快照测试、代码覆盖率、Mock、并行测试等。

如果你用过 Jest，Vitest 会让你感到非常熟悉；如果你是新手，Vitest 的简单配置和直观 API 也能让你快速上手！

---

## 第一步：安装和设置 Vitest

### 1. 创建一个 Vite 项目
如果你还没有项目，可以用 Vite 创建一个：
```bash
npm create vite@latest my-vitest-project
```
- 选择框架（如 Vue、React 或 Vanilla JavaScript）。
- 选择 TypeScript 或 JavaScript（Vitest 都支持）。
- 进入项目目录并安装依赖：
```bash
cd my-vitest-project
npm install
```

### 2. 安装 Vitest
在项目中安装 Vitest 及相关依赖：
```bash
npm install --save-dev vitest
```
如果需要测试 Vue 或 React 组件，可能还需要安装额外的工具：
- **Vue**：`@vue/test-utils` 和 `happy-dom` 或 `jsdom`。
- **React**：`@testing-library/react` 和 `jsdom`。
示例（以 Vue 为例）：
```bash
npm install --save-dev @vue/test-utils happy-dom
```

### 3. 配置 Vitest
Vitest 可以复用 Vite 的配置文件 `vite.config.ts`，只需添加 `test` 配置项。

创建一个简单的 `vite.config.ts`：
```javascript
/// <reference types="vitest" />
import { defineConfig } from 'vite'

export default defineConfig({
  test: {
    environment: 'happy-dom', // 模拟浏览器环境
    globals: true, // 启用全局 API（如 test、expect），无需手动导入
  },
})
```
- `environment`：选择 `happy-dom`（轻量）或 `jsdom`（更完整但稍慢）来模拟浏览器 DOM。
- `globals: true`：让 `test`、`expect` 等 API 全局可用，类似 Jest。
- 如果用 TypeScript，顶部添加 `/// <reference types="vitest" />` 以启用类型支持。

> **注意**：Vitest 3.0+ 默认使用 `vitest/config`，未来可能移除 `<reference types="vitest" />`。请参考最新文档。[](https://vitest.dev/guide/)

### 4. 添加测试脚本
在 `package.json` 中添加测试命令：
```json
"scripts": {
  "test": "vitest",
  "test:run": "vitest run"
}
```
- `vitest`：启动测试并进入**监听模式**（开发时自动重新运行测试）。
- `vitest run`：运行一次测试，适合 CI 环境。

---

## 第二步：编写你的第一个测试

### 1. 创建测试文件
Vitest 默认识别文件名包含 `.test.` 或 `.spec.` 的文件（如 `sum.test.js`）。在项目根目录或 `src` 下创建一个测试文件，例如 `src/sum.test.js`：
```javascript
import { test, expect } from 'vitest'

function sum(a, b) {
  return a + b
}

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3)
})
```
- `test`：定义一个测试用例，第一个参数是测试名称，第二个是测试函数。
- `expect`：断言工具，用于验证结果是否符合预期。
- `toBe`：检查值是否严格相等。

### 2. 运行测试
执行以下命令：
```bash
npm run test
```
你会看到类似输出：
```
✓ src/sum.test.js (1)
  ✓ adds 1 + 2 to equal 3
Test Files 1 passed (1)
Tests 1 passed (1)
Duration 311ms
```
Vitest 默认开启**监听模式**，修改代码后会自动重新运行测试。按 `q` 退出，或用 `npm run test:run` 运行一次。

---

## 第三步：测试 Vue 组件

让我们通过一个简单的 Vue 组件来学习如何测试组件逻辑和渲染。

### 1. 创建一个 Vue 组件
在 `src/components` 下创建 `Counter.vue`：
```vue
<template>
  <div>
    <p>Count: {{ count }}</p>
    <button @click="increment">Increment</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      count: 0,
    }
  },
  methods: {
    increment() {
      this.count++
    },
  },
}
</script>
```

### 2. 编写组件测试
在 `src/components/__tests__/Counter.spec.js` 中创建测试：
```javascript
import { mount } from '@vue/test-utils'
import { test, expect } from 'vitest'
import Counter from '../Counter.vue'

test('Counter increments count when button is clicked', async () => {
  const wrapper = mount(Counter)
  
  // 检查初始渲染
  expect(wrapper.text()).toContain('Count: 0')
  
  // 模拟点击按钮
  await wrapper.find('button').trigger('click')
  
  // 检查点击后的状态
  expect(wrapper.text()).toContain('Count: 1')
})
```
- `mount`：渲染 Vue 组件，模拟真实 DOM。
- `wrapper.text()`：获取组件的文本内容。
- `trigger('click')`：模拟用户点击事件。

### 3. 运行测试
再次运行 `npm run test`，你会看到组件测试通过！Vitest 会自动识别新的测试文件。

---

## 第四步：高级功能

### 1. 快照测试
快照测试用于验证组件的渲染输出是否与之前一致。在测试中添加：
```javascript
test('Counter renders correctly', () => {
  const wrapper = mount(Counter)
  expect(wrapper.html()).toMatchSnapshot()
})
```
第一次运行时，Vitest 会生成一个快照文件（通常在 `__snapshots__` 文件夹中）。后续运行会比较当前输出与快照是否一致。如果组件有意更改，运行 `vitest -u` 更新快照。

### 2. 代码覆盖率
Vitest 内置代码覆盖率支持。安装覆盖率工具：
```bash
npm install --save-dev @vitest/coverage-v8
```
在 `vite.config.ts` 中启用覆盖率：
```javascript
test: {
  environment: 'happy-dom',
  globals: true,
  coverage: {
    provider: 'v8', // 使用 V8 引擎
    reporter: ['text', 'html'], // 输出文本和 HTML 报告
  },
}
```
运行带覆盖率的测试：
```bash
npm run test -- --coverage
```
测试完成后，查看 `coverage` 目录中的 HTML 报告，了解哪些代码未被测试。

### 3. Mock 和假定时器
Vitest 提供 `vi` 工具用于 Mock 函数或模拟时间。例如，测试一个延迟函数：
```javascript
import { test, expect, vi } from 'vitest'

function delayedCallback(callback) {
  setTimeout(callback, 2000)
}

test('delayedCallback calls callback after 2 seconds', () => {
  vi.useFakeTimers()
  const callback = vi.fn()
  delayedCallback(callback)
  vi.advanceTimersByTime(2000)
  expect(callback).toHaveBeenCalled()
  vi.useRealTimers()
})
```
- `vi.fn()`：创建一个 Mock 函数。
- `useFakeTimers`：模拟 `setTimeout` 等计时器。
- `advanceTimersByTime`：快进时间，避免真实等待。

### 4. 并发测试
Vitest 支持并行运行测试，提升速度。在测试套件或用例中使用 `.concurrent`：
```javascript
import { describe, it } from 'vitest'

describe.concurrent('concurrent suite', () => {
  it('test 1', async () => {
    await new Promise(resolve => setTimeout(resolve, 100))
    expect(true).toBe(true)
  })
  it('test 2', async () => {
    await new Promise(resolve => setTimeout(resolve, 100))
    expect(true).toBe(true)
  })
})
```
所有标记为 `concurrent` 的测试将并行运行，缩短总测试时间。

---

## 第五步：最佳实践和注意事项

1. **文件组织**：
   - 测试文件通常放在 `__tests__` 文件夹或与源代码同级，文件名以 `.test.` 或 `.spec.` 结尾。
   - 例如：`src/components/Counter.vue` 对应 `src/components/__tests__/Counter.spec.js`。

2. **环境隔离**：
   - Vitest 默认隔离每个测试文件的环境，防止状态污染。如果需要更高性能，可以用 `--no-isolate` 禁用（但谨慎使用）。

3. **调试测试**：
   - 在 IDE（如 VS Code）中，使用 Vitest 的官方扩展，获得更好的调试体验。
   - 运行 `vitest --ui` 启动 Vitest UI，查看测试结果的图形化界面。

4. **与 CI 集成**：
   - 在 CI 环境中，使用 `vitest run` 运行测试，确保一次性执行。
   - 配置覆盖率报告，监控测试质量。

5. **迁移提示**：
   - 如果从 Jest 迁移，Vitest 支持大部分 Jest API，只需调整配置和导入方式。
   - 参考官方迁移指南：https://vitest.dev/guide/migration.html[](https://vitest.dev/)

---

## 总结

通过这个教程，你已经学会了：
- 安装和配置 Vitest。
- 编写简单的函数测试和 Vue 组件测试。
- 使用快照测试、代码覆盖率、Mock 和并发测试等高级功能。
- 遵循最佳实践组织和运行测试。

Vitest 的速度和简单性让测试变得更轻松！继续探索官方文档（https://vitest.dev）和其他资源，深入学习更多高级功能，如内联测试、浏览器模式测试等。

**接下来做什么？**
- 尝试为你的项目添加更多测试用例。
- 学习如何测试异步逻辑或网络请求（结合 Mock Service Worker）。
- 探索 Vitest 的插件生态，扩展测试能力。

快乐测试！🎉


