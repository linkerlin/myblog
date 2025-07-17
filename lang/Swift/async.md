---
title: Swift 6、TypeScript 和 Python 的 async/await 机制异同


---


#### 引言
Async/await 是一种编程模式，用于处理异步操作（如网络请求、文件读写或延时任务），让代码看起来像同步代码一样线性易读，同时避免阻塞程序的执行流。它最早在 C# 中流行，后来被多种语言采用，包括 Swift、TypeScript（基于 JavaScript）和 Python。

这份报告基于最新信息（截至 2025 年 7 月）调研 Swift 6、TypeScript 和 Python 的 async/await 机制，重点比较它们的异同。特别关注：
- **阻塞**：是否会暂停整个程序或线程，导致其他任务无法进行。
- **性能**：开销、效率，尤其是相对于同步代码的差异。
- **并发执行位置**：异步任务在哪里运行（如单线程事件循环、多线程或 actor 模型）。

Swift 6 的 async/await 构建在 Swift 5.5 引入的基础上，主要增强了严格并发检查和数据安全。TypeScript 的 async/await 运行在 JavaScript 引擎（如 Node.js）的事件循环上。Python 的 async/await 基于 asyncio 库，也使用事件循环。

报告将先分别介绍每个语言的机制，然后用表格比较异同，最后给出 Demo 代码示例。解释力求通俗易懂，像在和朋友聊天一样，避免专业术语或用简单例子说明。

#### Swift 6 的 async/await 机制
Swift 6 的 async/await 是 Apple 生态（iOS、macOS 等）中处理并发的现代方式。它从 Swift 5.5 开始引入，Swift 6 加强了“严格并发”模式，确保代码在多线程环境中不会出现数据竞争（比如两个任务同时改同一个变量导致混乱）。

- **如何工作**：用 `async` 标记函数表示它可能需要等待（比如网络调用），用 `await` 调用它。等待时，函数“暂停”但不阻塞线程——系统会切换到其他任务。完成后自动 resuming。
- **阻塞**：非阻塞。await 只暂停当前函数的执行路径，但线程可以继续跑其他东西。适合 UI 应用，避免卡顿主线程。
- **性能**：比同步代码有轻微开销（async 调用比 sync 贵，因为涉及任务调度），但简化了代码，减少手动错误处理。总体上提升效率，尤其在 I/O 操作中。Swift 6 的严格检查可能在编译时增加时间，但运行时更安全、更快（避免运行时崩溃）。
- **并发执行位置**：多线程模型。任务默认在后台线程运行，UI 更新用 `@MainActor` 强制主线程。使用 “actor” 来隔离状态，确保线程安全。Swift 的并发是“结构化”的：任务像树状嵌套，自动处理取消和错误传播。

简单比喻：像厨师做饭——await 等菜煮熟时，去洗碗而不是傻站着。

#### TypeScript 的 async/await 机制
TypeScript 是 JavaScript 的超集，它的 async/await 直接基于 JS（EcmaScript 2017 引入）。常用于 Node.js 或浏览器环境，处理如 HTTP 请求的异步操作。

- **如何工作**：`async` 函数返回 Promise，用 `await` 等待 Promise 解析。底层是回调，但语法更干净。
- **阻塞**：非阻塞。await 暂停当前函数，但事件循环继续处理其他事件。适合 I/O 重任务（如 web 服务器），但 CPU 重任务（如复杂计算）会阻塞整个循环，因为是单线程。
- **性能**：低开销，事件循环高效处理大量并发 I/O（Node.js 能轻松handle 成千上万连接）。比线程模型轻量，但 CPU 密集时性能差（需用 worker threads 补救）。总体比回调地狱快，因为代码更简洁，少 bug。
- **并发执行位置**：单线程事件循环。所有任务在主线程排队，I/O 操作（如网络）交给操作系统异步处理。不是真并行（parallel），而是并发（concurrent）——像单人多任务切换。

比喻：像服务员点单——await 等菜上时，去服务其他桌，而不是干等。

#### Python 的 async/await 机制
Python 的 async/await 从 3.5 引入，基于 asyncio 库。常用于 web（如 FastAPI）或脚本中处理异步 I/O。

- **如何工作**：`async def` 定义协程，用 `await` 等待。需要事件循环（如 `asyncio.run()`）驱动。
- **阻塞**：非阻塞。await 让出控制权，循环继续其他协程。但 CPU 重任务会阻塞循环（单线程）。
- **性能**：协程轻量（比线程便宜），适合 I/O（用 uvloop 可比 Node.js 快 2 倍）。开销小，但事件循环管理不当会影响整体性能。比同步代码慢点（调度 overhead），但并发时总时间短。
- **并发执行位置**：单线程事件循环。协程像轻量任务，在主线程切换。I/O 操作异步委托给 OS。不是多线程（可用 multiprocessing 结合），焦点在并发而非并行。

比喻：像调度员安排公交——await 等车到时，去安排下一辆。

#### 异同比较
以下表格总结三者的异同。重点突出阻塞、性能和并发位置。

| 方面          | Swift 6                                      | TypeScript (JS)                              | Python (asyncio)                             |
|---------------|----------------------------------------------|----------------------------------------------|----------------------------------------------|
| **引入时间** | Swift 5.5 (2021)，Swift 6 增强严格并发 (2024) | EcmaScript 2017 (TypeScript 2.1+)           | Python 3.5 (2015)                            |
| **核心模型** | 结构化并发 + actor 系统                      | Promise + 事件循环                          | 协程 + 事件循环                             |
| **阻塞行为** | 非阻塞；await 暂停函数但不锁线程，适合 UI   | 非阻塞；事件循环继续，但 CPU 重任务阻塞循环 | 非阻塞；协程切换，但 CPU 重任务阻塞循环     |
| **性能**     | 轻微开销 (任务调度)；Swift 6 优化安全，I/O 高效；比 sync 稍慢但代码简洁 | 低开销；I/O 卓越 (Node.js 规模化)；CPU 重时需 worker | 低开销 (协程廉价)；uvloop 加速 I/O；比 sync 总时间短 |
| **并发执行位置** | 多线程 (后台线程 + 主线程)；actor 隔离状态  | 单线程事件循环；OS 处理 I/O                 | 单线程事件循环；OS 处理 I/O                 |
| **优点**     | 线程安全强 (Sendable 协议)；结构化错误/取消 | 简单、浏览器/Node.js 原生；大规模并发       | 轻量、易集成；web 服务高效                  |
| **缺点**     | 学习曲线 (actor/MainActor)；Swift 6 迁移复杂 | 单线程限 CPU；Promise 链复杂                | 需手动循环管理；调试协程栈难                |
| **适用场景** | App 开发 (iOS)；需多线程安全                | Web 前后端；事件驱动                       | 脚本/web；I/O 重任务                        |
| **与同步差异**| 更安全、非阻塞；但 async 调用贵于 sync     | 非阻塞提升响应；但事件循环可瓶颈            | 并发缩短总时；但需 async 兼容库             |

**异**：
- Swift 是多线程的，真并行可能（CPU 任务可分散线程），而 TypeScript/Python 是单线程并发（适合 I/O，不适合 CPU）。
- Swift 强调结构化和安全（actor 防数据赛），TypeScript/Python 更灵活但易出错。
- 性能：Swift 在多核 CPU 上潜力大；TypeScript/Python 在 I/O 上闪光，但 CPU 时需额外机制。

**同**：
- 都非阻塞，await 让代码线性。
- 都简化异步代码，取代回调/生成器。
- 性能上都比纯同步好在并发场景（总时间短），但有调度开销。
- 都依赖底层运行时（Swift runtime、JS 引擎、asyncio）管理执行。

#### Demo Code 示例
以下给出每个语言的简单 Demo：并发“fetch”两个延时任务（模拟网络）。关注点：
- **阻塞**：代码不会卡住主流程。
- **性能**：总时间接近最长任务（并发），而非相加（同步）。
- **并发位置**：展示执行模型。

**Swift 6 Demo**（需 Xcode/Swift 环境运行；这里是代码，假设在 Playground 或 App 中）：
```swift
import Foundation

func fetchData(delay: UInt64) async -> String {
    try? await Task.sleep(nanoseconds: delay * 1_000_000_000)
    return "Data fetched after \(delay) seconds"
}

@MainActor
func main() async {
    let start = Date()
    async let data1 = fetchData(delay: 2)  // 并发启动
    async let data2 = fetchData(delay: 3)
    let results = await [data1, data2]
    let end = Date()
    print("\(results[0]) and \(results[1])")
    print("Total time: \(end.timeIntervalSince(start)) seconds")
}

Task { await main() }
```
// 预期输出：Data fetched after 2 seconds and Data fetched after 3 seconds\nTotal time: ~3 seconds
// 解释：使用 `async let` 并发在后台线程运行；不阻塞主线程。性能：总时 ~3s（并发）。位置：多线程，Task 调度。

**TypeScript Demo**（Node.js 运行；用 `ts-node` 或编译后）：
```typescript
async function fetchData(delay: number): Promise<string> {
    await new Promise(resolve => setTimeout(resolve, delay * 1000));
    return `Data fetched after ${delay} seconds`;
}

async function main() {
    const start = Date.now();
    const promise1 = fetchData(2);
    const promise2 = fetchData(3);
    const [data1, data2] = await Promise.all([promise1, promise2]);
    const end = Date.now();
    console.log(`${data1} and ${data2}`);
    console.log(`Total time: ${(end - start) / 1000} seconds`);
}

main();
```
// 预期输出：Data fetched after 2 seconds and Data fetched after 3 seconds\nTotal time: ~3 seconds
// 解释：Promise.all 并发；事件循环处理定时器。不阻塞。性能：高效 I/O。位置：单线程循环，setTimeout 由 OS 异步。

**Python Demo**（已用工具运行验证）：
```python
import asyncio
import time

async def fetch_data(delay):
    await asyncio.sleep(delay)
    return f"Data fetched after {delay} seconds"

async def main():
    start = time.time()
    task1 = asyncio.create_task(fetch_data(2))
    task2 = asyncio.create_task(fetch_data(3))
    data1 = await task1
    data2 = await task2
    end = time.time()
    print(f"{data1} and {data2}")
    print(f"Total time: {end - start} seconds")

asyncio.run(main())
```
// 实际输出（工具运行）：Data fetched after 2 seconds and Data fetched after 3 seconds\nTotal time: 3.00168776512146 seconds
// 解释：create_task 并发协程；sleep 非阻塞。性能：总时 ~3s。位置：单线程循环，asyncio.sleep 异步。

这些 Demo 展示：三者都能并发执行，总时间短（非 5s），证明非阻塞和并发益处。但 Swift 可在多核上并行延时（如果 CPU 重），而其他俩在单线程。

#### 结论
Swift 6 的 async/await 更注重安全和多线程，适合复杂 App；TypeScript 和 Python 类似，事件循环驱动， excels 在 web/I/O，但 CPU 时需小心阻塞。选择取决于场景：需多线程选 Swift，需轻量 I/O 选后两者。性能上，三者都提升并发效率，但有小开销——测试你的具体用例！如果需要更深代码或基准测试，欢迎补充。
