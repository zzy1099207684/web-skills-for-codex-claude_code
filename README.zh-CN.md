
# web-skills-for-codex-claude_code

[English Version](./README.md)

![skills](https://img.shields.io/badge/skills-2-blue)
![workflow](https://img.shields.io/badge/workflow-deep--interview%20%E2%86%92%20web--ralplan%20%E2%86%92%20web--ralph-6f42c1)
![oh-my-codex](https://img.shields.io/badge/oh--my--codex-compatible-111827)
![playwright-cli](https://img.shields.io/badge/browser_validation-playwright--cli-2ea44f)
![license](https://img.shields.io/badge/license-MIT-yellow)

技能说明：

- `web-ralplan`：[英文](./web-ralplan/README.md) | [中文](./web-ralplan/README.zh-CN.md)
- `web-ralph`：[英文](./web-ralph/README.md) | [中文](./web-ralph/README.zh-CN.md)

这是一个帮助你做 Web 开发的 skills 集合。

它解决两个实际问题：

- 写代码前，先把网页相关的计划和测试方式写清楚
- 写代码后，不只看测试和构建，还要真的打开浏览器把页面流程跑通

这个仓库包含两个技能：

- `web-ralplan`：负责规划阶段，把页面流程怎么测、什么情况算失败写进计划和测试文档
- `web-ralph`：负责执行阶段，改完代码后持续验证，直到浏览器里的真实页面流程通过为止

另外有一条总规则：

- 在 `oh-my-codex` 的工作流里，`deep-interview` 应该放在第一步

推荐链路：

```text
deep-interview -> web-ralplan -> web-ralph
```

## 安装前你要有的东西

先确认这几个前置条件：

1. 已经安装 Codex CLI
2. 已经装好 `oh-my-codex`
3. 本机有 `node` 和 `npm`

如果你的 `oh-my-codex` 已经可用，下面这个命令应该能跑：

```bash
omx setup --force --verbose
```

如果 `omx` 命令不存在，说明你还没把 `oh-my-codex` 装好，这一步要先完成。

## 一次性安装命令

进入这个仓库目录后，直接复制下面这段：

```bash
omx setup --force --verbose && \
npm install -g @playwright/cli@latest && \
mkdir -p "${HOME}/.codex/skills" && \
rm -rf "${HOME}/.codex/skills/web-ralplan" "${HOME}/.codex/skills/web-ralph" && \
cp -R ./web-ralplan "${HOME}/.codex/skills/web-ralplan" && \
cp -R ./web-ralph "${HOME}/.codex/skills/web-ralph"
```

这段命令会做三件事：

1. 运行 `oh-my-codex` 的 setup
2. 安装 `playwright-cli`
3. 把这两个 skill 复制到你的 `~/.codex/skills` 目录里

## 安装完成后检查

继续复制这段：

```bash
omx doctor && \
playwright-cli --version && \
ls "${HOME}/.codex/skills/web-ralplan" "${HOME}/.codex/skills/web-ralph"
```

如果都正常，说明安装完成。

## 开始用

### 第一步：先做需求澄清

```text
$deep-interview "clarify this web task before planning and execution"
```

先把需求、范围、非目标、边界条件问清楚。

### 第二步：再做 Web 规划

```text
$web-ralplan "plan this web task with browser validation"
```

它会先把这些东西写进计划和测试文档：

- 要改哪些页面
- 要测哪些用户流程
- 页面上什么结果算成功
- 什么浏览器错误算失败

### 第三步：最后做 Web 执行

```text
$web-ralph "finish this web task and verify it in the browser"
```

它会要求：

- 代码侧检查通过
- 浏览器里真实流程也通过
- console / runtime / API / proxy 相关错误不能存在

## 目录说明

```text
codex-web-skills/
  README.md
  README.zh-CN.md
  web-ralplan/
    SKILL.md
    README.md
    README.zh-CN.md
    agents/
      openai.yaml
    references/
      browser-test-plan.md
  web-ralph/
    SKILL.md
    README.md
    README.zh-CN.md
    agents/
      openai.yaml
    references/
      browser-checklist.md
```

## 最后一句

如果你只想无脑装完就开始用，流程就是：

1. 进入这个目录
2. 复制“一次性安装命令”
3. 复制“安装完成后检查”
4. 先用 `$deep-interview`
5. 再用 `$web-ralplan`
6. 最后用 `$web-ralph`
