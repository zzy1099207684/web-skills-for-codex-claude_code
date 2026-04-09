[English Version](./README.md)

# Web Ralplan

`web-ralplan` 是一个面向 Web 规划任务的 Codex skill。它基于 `ralplan` 的共识规划流程，再额外加入浏览器侧测试规划，避免 Web 任务只围绕代码检查来做计划。

它本质上结合了两部分：

- `ralplan` 的共识规划工作流
- 面向 Web 的浏览器验证规划，默认以 `playwright-cli` 作为浏览器验证工具

说白了就是：如果一个任务会影响用户在浏览器里看到或操作到的东西，那么在真正开始开发前，计划里就必须先把“页面怎么测”写清楚。

## 它是干什么的

这个 skill 用在 Web 任务的规划阶段。

它会强制最终计划和测试文档不能只写：

- 单元测试
- lint
- build
- 类型检查

还必须把下面这些内容写进去：

- 影响的是哪个页面或路由
- 要验证哪个用户流程
- 页面上出现什么结果才算成功
- 哪些浏览器侧失败会直接阻塞完成

## 为什么需要它

很多任务在计划阶段看起来挺完整，但对 Web 项目来说还是太“代码导向”了。

常见坏结果是：

- 计划里只写 repo 侧检查
- 浏览器验证留到执行时临场发挥
- 页面 bug、console 报错、请求失败、移动端问题在很后面才暴露

`web-ralplan` 的作用，就是把这些浏览器相关要求提前写进 plan 和 test-spec。

## 核心规则

只要任务和浏览器可见行为有关，最终计划里就必须明确写出：

- 目标页面或路由
- 关键浏览器流程
- 页面可见成功结果
- repo 侧检查项
- 浏览器侧检查项
- 浏览器侧失败条件

失败条件至少应该明确包括：

- 相关 API 请求失败
- 相关本地代理请求失败
- 相关浏览器 console 报错
- 相关运行时错误

## 典型衔接方式

最自然的链路就是：

```text
web-ralplan -> web-ralph
```

- `web-ralplan` 先把 Web 计划和测试规格写清楚
- `web-ralph` 后面再真正执行，并用浏览器验证闭环

## 依赖前提

- 这是一个带浏览器验收要求的 Web 任务
- 当前任务或仓库里有足够信息，能识别页面、路由和关键用户流程
- 如果需要明确默认浏览器工具，环境里最好有 `playwright-cli`

## 目录结构

```text
web-ralplan/
  SKILL.md
  README.md
  README.zh-CN.md
  agents/
    openai.yaml
  references/
    browser-test-plan.md
```

## 文件说明

- `SKILL.md`：技能本体
- `agents/openai.yaml`：UI 展示元数据
- `references/browser-test-plan.md`：浏览器测试规格参考模板

## 示例提示词

```text
Use $web-ralplan to create a Web implementation plan and test spec for this auth-flow change, including browser validation and failure conditions.
```

## 说明

- 这个 skill 是故意做成通用的，不绑定 React、Vue、Next.js、Vite，也不假设固定的 repo 结构。
- 它只负责把计划和测试规格写清楚，不直接执行代码。
