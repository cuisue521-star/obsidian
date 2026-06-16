---
title: "Hermes Agent 完整使用教程"
source: "https://blog.csdn.net/henrylin9999/article/details/159979426"
author:
  - "[[henrylin9999]]"
published: 2026-04-10
created: 2026-04-17
description: "文章浏览阅读795次，点赞13次，收藏17次。频繁改模型、系统提示、上下文文件、技能加载方式，会提高成本并降低会话稳定性。profile 隔离的是整套 Hermes home，包括配置、技能、会话、memory、gateway 状态等。toolset、MCP、Gateway 授权都遵循同一个原则：只开必须的，不要因为“以后可能用到”而默认暴露。这意味着 Hermes 的效果不仅取决于模型，还取决于工具可用性、上下文质量和你的使用方式。工作、个人、bot、实验环境，最好分 profile，而不是混在一个实例里。推荐你第一次就用它，而不是先上消息平台。_hermes agent使用"
tags:
  - "clippings"
---
### 目录

1. Hermes 是什么
2. 安装与初始化
3. 第一次进入 CLI
4. 配置、模型与 profile
5. 工具、toolset 与执行边界
6. skill、memory 与上下文文件
7. MCP 接入外部系统
8. Gateway 与多平台使用
9. cron、后台任务与自动化
10. 开发扩展
11. 调试、测试与排障
12. 最佳实践

### 1\. Hermes 是什么

Hermes Agent 不是单纯的聊天界面，也不是只能在 IDE 里工作的代码补全器。它更像一个统一的 agent 运行时：

- 在 CLI 里，它可以读写文件、执行命令、管理会话、切换模型、调用工具
- 在 Gateway 里，它可以通过 Telegram、Discord、Slack、WhatsApp、Signal 等平台长期在线
- 通过 skill、memory 和 context file，它能把流程知识、事实信息和项目约束长期保留下来
- 通过 MCP 和 plugins，它能对接外部系统
- 通过 cron 和背景任务，它能定时、异步、持续地工作

你可以把 Hermes 的核心结构概括成下面几层：

- 入口层：CLI、Gateway、ACP、batch runner
- agent 层： `AIAgent` 和 prompt / provider / tool 调度
- 执行层：tools、toolsets、terminal backend、browser backend、MCP
- 持久层：sessions、memory、skills、profiles

这意味着 Hermes 的效果不仅取决于模型，还取决于工具可用性、上下文质量和你的使用方式。

### 2\. 安装与初始化

#### 普通安装

最快的方式是官方安装脚本：

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
source ~/.bashrc
# 或 source ~/.zshrc
hermes
bash1234
```

官方支持 Linux、macOS 和 WSL2。原生 Windows 不在推荐路径里。

#### 仓库开发安装

如果你打算直接修改仓库：

```bash
git clone https://github.com/NousResearch/hermes-agent.git
cd hermes-agent
uv venv venv --python 3.11
source venv/bin/activate
uv pip install -e ".[all,dev]"
bash12345
```

对于这个仓库本身， `AGENTS.md` 明确要求：运行 Python 相关命令前要先 `source venv/bin/activate` 。

#### 初始化配置

首次安装后建议运行：

```bash
hermes setup
bash1
```

它会帮你处理：

- 模型与 provider
- terminal backend
- gateway 平台
- tools
- agent 行为

默认配置目录是 `~/.hermes/` ，你最需要知道的文件通常是：

- `~/.hermes/config.yaml`
- `~/.hermes/.env`
- `~/.hermes/SOUL.md`

`.env` 里通常先配一个可用 provider，例如：

```bash
OPENROUTER_API_KEY=...

智谱 glm 配置： 
base-url 写这个：
https://open.bigmodel.cn/api/coding/paas/v4
bash12345
```

安装验证建议用：

```bash
hermes version
hermes doctor
hermes status
hermes
bash1234
```

### 3\. 第一次进入 CLI

CLI 是最完整的主入口。推荐你第一次就用它，而不是先上消息平台。

启动：

```bash
hermes
bash1
```

进入后先跑这几条：

```
/help
/model
/usage
/toolsets
text1234
```

然后给一个带动作的任务，而不是“你好”，例如：

```
阅读当前目录结构并总结这个仓库的核心组成。
text1
```

这样你能马上看到 Hermes 是否会用 file 或 terminal 工具。

#### 你很快会常用到的命令

- `/new` / `/reset` ：新会话
- `/title` ：命名会话
- `/resume` ：恢复命名会话
- `/model` ：切换模型
- `/usage` ：看 token、成本、上下文
- `/compress` ：压缩上下文
- `/verbose` ：观察工具输出
- `/background <prompt>` ：后台启动独立任务
- `/skills` ：搜索和安装 skill
- `/tools list` ：看当前工具

CLI 外也别忘了：

```bash
hermes -c
hermes -r "session-title"
bash12
```

复杂任务建议尽早 `/title` ，否则后续恢复会话会很痛苦。

### 4\. 配置、模型与 profile

Hermes 的配置分两 类 ：

- `config.yaml` ：结构化行为配置
- `.env` ：API key、base URL、平台 token、终端环境变量等

最常见的 provider 相关变量有：

- `OPENROUTER_API_KEY`
- `OPENAI_API_KEY`
- `OPENAI_BASE_URL`
- `ANTHROPIC_API_KEY`
- `GLM_API_KEY`

选择模型最直接的方式是：

```bash
hermes model
bash1
```

会话中可以：

```
/model
/model openrouter:anthropic/claude-sonnet-4
/model zai:glm-5
/model custom:qwen-2.5
text1234
```

#### profile 为什么重要

如果你有多个用途，强烈建议用 profile 隔离，而不是把所有配置堆在一个默认实例里。

常见命令：

```bash
hermes profile list
hermes profile create work --clone
hermes profile use work
hermes profile show work
hermes profile use default
bash12345
```

profile 隔离的是整套 Hermes home，包括配置、技能、会话、memory、gateway 状态等。

#### 关于 prompt cache

Hermes 很依赖稳定的系统提示前缀来命中缓存。频繁改模型、系统提示、上下文文件、技能加载方式，会提高成本并降低会话稳定性。不要把“中途乱改配置”当作常态。

### 5\. 工具、toolset 与执行边界

Hermes 的执行力来自工具。每个工具属于一个 toolset，你平时更常控制的是 toolset。

常见 toolset：

- `file`
- `terminal`
- `web`
- `browser`
- `skills`
- `memory`
- `code_execution`
- `delegation`
- `cronjob`

查看当前会话可用能力：

```
/toolsets
/tools list
text12
```

从命令行按会话选择：

```bash
hermes chat --toolsets web,file,terminal
hermes chat --toolsets debugging
hermes chat --toolsets all
bash123
```

#### 怎样选对工具层级

- 读写搜索文件： `file`
- 真正执行命令： `terminal`
- 快速搜索网页或抽取正文： `web`
- 点击按钮、填表单、真实页面交互： `browser`
- 多步机械流程： `execute_code`
- 并行子任务或新上下文分析： `delegate_task`

高频误区是“什么都开、什么都用”，这会增加风险、成本和不可预测性。更好的方式是为任务提供最小且足够的能力集合。

### 6\. skill、memory 与上下文文件

Hermes 的长期稳定性主要来自三类东西：

- skill：如何做
- memory：是什么
- context file：默认背景和规则

#### 使用 skill

查看和安装：

```
/skills
/skills search docker
/skills browse
text123
```

或：

```bash
hermes skills list
hermes skills install official/research/arxiv
bash12
```

skill 安装后通常会直接变成 slash command，例如：

```
/plan 设计一个 REST API
/ascii-art Make a banner
text12
```

#### 使用 memory

适合保存长期事实，例如：

```
记住：我们的 CI 用 GitHub Actions，部署工作流是 deploy.yml。
text1
```

#### 使用 AGENTS.md 和 SOUL.md

- `AGENTS.md` ：项目规则、架构约束、测试要求
- `SOUL.md` ：Hermes 的长期风格和行为倾向

如果你总在重复同一批规则，就应该把它们写进 `AGENTS.md` ，而不是继续每次重说。

#### 一个重要限制

不要把 `AGENTS.md` 写成百科全书。它会进入系统上下文，太长会增加成本并稀释重点。

### 7\. MCP 接入外部系统

MCP 适合把外部系统接成 Hermes 的工具。例如 GitHub、文件系统、内部 API、远程知识库。

当前仓库和代码使用的配置键是：

```yaml
mcp_servers:
  ...
yaml12
```

不是 `mcp.servers` 。

#### 最小 stdio 示例

```yaml
mcp_servers:
  filesystem:
    command: "npx"
    args: ["-y", "@modelcontextprotocol/server-filesystem", "/home/user/projects"]
yaml1234
```

#### 最小 HTTP 示例

```yaml
mcp_servers:
  company_api:
    url: "https://mcp.internal.example.com"
    headers:
      Authorization: "Bearer ***"
yaml12345
```

改完配置后，记得：

```
/reload-mcp
text1
```

#### 先过滤，再暴露

非常推荐一开始就做白名单或黑名单控制：

```yaml
mcp_servers:
  github:
    command: "npx"
    args: ["-y", "@modelcontextprotocol/server-github"]
    env:
      GITHUB_PERSONAL_ACCESS_TOKEN: "***"
    tools:
      include: [list_issues, create_issue, search_code]
      prompts: false
      resources: false
yaml12345678910
```

判断标准可以记为：

- 内置工具已经够用：优先内置工具
- 只是流程复杂：优先 skill
- 需要接外部系统：优先 MCP

### 8\. Gateway 与多平台使用

Hermes 可以通过 gateway 暴露到 Telegram、Discord、Slack、WhatsApp、Signal 等平台。

核心命令：

```bash
hermes gateway setup
hermes gateway run
hermes gateway start
hermes gateway stop
hermes gateway status
bash12345
```

常见平台变量包括：

- `TELEGRAM_BOT_TOKEN`
- `DISCORD_BOT_TOKEN`
- `SLACK_BOT_TOKEN`
- `SLACK_APP_TOKEN`

#### 授权控制必须认真做

不要轻易开放所有用户。优先配置：

- `TELEGRAM_ALLOWED_USERS`
- `DISCORD_ALLOWED_USERS`
- `GATEWAY_ALLOWED_USERS`

#### home channel 很重要

进入对应聊天后执行：

```
/sethome
text1
```

cron 和后台结果才知道发去哪里。

消息平台常见命令包括：

- `/new`
- `/status`
- `/model`
- `/personality`
- `/retry`
- `/undo`
- `/sethome`
- `/compress`
- `/title`
- `/resume`
- `/usage`
- `/reload-mcp`
- `/background`

而 `/tools` 、 `/toolsets` 、 `/browser` 之类通常更偏 CLI。

### 9\. cron、后台任务与自动化

Hermes 的 cron 是“在新 session 中运行一个 agent 任务”，而不是单纯执行系统计划任务。所以最关键的规则是：

你的 prompt 必须完全自包含。

#### 创建 job

```
/cron add "0 9 * * 1" 生成每周 AI 摘要并发送到 home channel。
text1
```

或：

```bash
hermes cron create "0 9 * * 1" "生成每周 AI 摘要并发送到 home channel。" --name "weekly-ai"
bash1
```

常见管理：

```
/cron list
/cron run <job_id>
/cron pause <job_id>
/cron resume <job_id>
text1234
```

#### 背景任务

如果只是想把当前任务异步化，而不是按时间重复执行：

```
/background 总结这个仓库的工具系统并写一份建议。
text1
```

#### 自动化设计的一个好模式

- 脚本负责抓数据、做机械步骤、保存状态
- agent 负责解释、筛选、总结

很多监控类任务还应该显式约定：

```
如果没有变化，回复 [SILENT]。
text1
```

这样不会把通知刷屏。

### 10\. 开发扩展

如果你要修改这个仓库，先记住关键结构：

- `run_agent.py` ：AIAgent 核心循环
- `cli.py` ：CLI
- `gateway/run.py` ：Gateway
- `model_tools.py` ：工具发现和 dispatch
- `tools/registry.py` ：工具注册
- `toolsets.py` ：工具分组

#### 新增 tool 的最小路径

一般要动 3 个地方：

1. `tools/your_tool.py`
2. `toolsets.py`
3. `model_tools.py`

其中 handler 应返回 JSON 字符串，错误也应包装成 JSON。

#### 新增 slash command

一般要动：

1. `hermes_cli/commands.py`
2. `cli.py`
3. `gateway/run.py` （如果 Gateway 也支持）

#### profile-safe 规则

不要硬编码 `~/.hermes` 。代码里应使用：

- `get_hermes_home()`

用户可见提示中应使用：

- `display_hermes_home()`

#### 本仓库开发测试

```bash
source venv/bin/activate
python -m pytest tests/ -q
python -m pytest tests/test_model_tools.py -q
python -m pytest tests/gateway/ -q
python -m pytest tests/tools/ -q
bash12345
```

### 11\. 调试、测试与排障

遇到问题时，先按层次判断：

1. 安装层： `hermes` 能否启动
2. provider 层：认证、API key、base URL
3. tool 层：toolset 是否启用，依赖是否满足
4. MCP 层：server 是否加载
5. Gateway 层：token、allowlist、home channel
6. 上下文层：skills、memory、会话状态、prompt cache

通用命令：

```bash
hermes doctor
hermes status
hermes version
hermes profile show <name>
bash1234
```

CLI 内常用辅助：

```
/model
/usage
/toolsets
/tools list
/verbose
text12345
```

MCP 问题优先检查：

- `mcp_servers` 键名是否正确
- 本地依赖命令是否存在
- 是否执行了 `/reload-mcp`

消息平台问题优先检查：

- token 是否正确
- allowlist 是否允许当前用户
- 是否需要 mention
- home channel 是否配置

不要一上来就删整个 `~/.hermes` 。大多数问题都可以局部定位。

### 12\. 最佳实践

最后给你一套更稳的长期使用方式：

#### 1\. 第一条消息尽量把上下文给够

不要只说“帮我修一下”。尽量直接给：

#### 2\. 复杂任务先命名会话

```
/title auth-refactor
text1
```

以后恢复就简单很多。

#### 3\. 重要规则写进 AGENTS.md

不要依赖聊天记录记住所有项目约定。

#### 4\. 长会话定期看 /usage

必要时 `/compress` ，不要等上下文快爆了才处理。

#### 5\. 选对执行方式

- 单一动作：直接工具
- 多步机械流程： `execute_code`
- 并行分析： `delegate_task`
- 可复用流程：skill

#### 6\. 用 profile 管隔离

工作、个人、bot、实验环境，最好分 profile，而不是混在一个实例里。

#### 7\. 对不可信代码使用隔离环境

优先 Docker 、Daytona 等，而不是默认本地 terminal。

#### 8\. 最小暴露原则

toolset、MCP、Gateway 授权都遵循同一个原则：只开必须的，不要因为“以后可能用到”而默认暴露。