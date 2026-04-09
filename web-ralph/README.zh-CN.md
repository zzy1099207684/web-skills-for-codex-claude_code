[English Version](./README.md)

# Web Ralph

`web-ralph` 是一个面向 Web 开发任务的 Codex skill，基于 `oh-my-codex` 的 `ralph` 工作流并结合 `playwright-cli` 浏览器验证能力，要求浏览器可见的改动不仅通过代码侧校验，也必须通过真实浏览器流程验证后，任务才算完成。

它本质上是把两部分结合起来：

- `oh-my-codex` 里的 `ralph` 完成闭环
- `playwright-cli` 提供的浏览器侧验证能力

说白了就是：`test`、`build`、`lint` 过了还不够。只要这个任务会影响用户在浏览器里看到或操作到的东西，就必须把真实页面流程也验证通过，任务才算完成。

## 它是干什么的

这个 skill 本质上是 `ralph` 工作流的 Web 版本增强。

它保留了 `ralph` 那种“没做完就继续干”的闭环逻辑，但额外加了一条硬规则：

只要是 Web 场景，就必须跑浏览器验证。

它适合做这些事情：

- 修 UI bug
- 验证表单和页面跳转
- 检查浏览器里可见的状态变化
- 验证响应式布局
- 抓出那些代码检查发现不了、但浏览器里会炸的问题

## 核心规则

只要是浏览器相关的任务，下面这些条件必须同时成立，任务才算完成：

- 代码侧检查通过
- 目标浏览器流程 fresh pass
- 页面上能看到预期结果
- 浏览器 console 没有相关错误
- 没有相关 API 请求失败
- 没有相关代理请求失败

只要这里面有一个没过，闭环就不能结束，必须继续修。

## 为什么要有这个东西

很多 Web 任务在代码层面看起来像“做完了”，但浏览器里其实还是坏的。

常见情况比如：

- 按钮渲染出来了，但请求失败
- 路由能打开，但 console 一直报错
- 表单看起来提交了，但后端调用是坏的
- 桌面端正常，手机端直接崩

`web-ralph` 的作用，就是把这些浏览器层面的失败也纳入“是否完成”的判断标准。

## 依赖前提

- 环境里能用 `playwright-cli`
- 应用能在本地跑起来，或者有一个可访问的目标 URL
- 要验证的浏览器流程是明确且可复现的

## 目录结构

```text
web-ralph/
  SKILL.md
  README.md
  README.zh-CN.md
  agents/
    openai.yaml
  references/
    browser-checklist.md
```

## 文件说明

- `SKILL.md`：技能本体
- `agents/openai.yaml`：UI 展示元数据
- `references/browser-checklist.md`：浏览器验证补充清单

## 示例提示词

```text
Use $web-ralph to fix this login flow and keep going until the repo checks and browser flow both pass.
```

## 说明

- 这个 skill 是故意做成通用的，不绑定 React、Vue、Vite、Next.js，也不假设固定的 repo 结构。
- 真正运行时，应该先从当前项目里识别启动命令、目标 URL 和关键用户流程，再进入浏览器闭环。
