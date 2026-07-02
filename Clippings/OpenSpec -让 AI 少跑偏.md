---
title: "OpenSpec 是什么？它不是让 AI 更会写代码，而是让 AI 少跑偏"
source: "https://zhuanlan.zhihu.com/p/2027465707961558427"
author:
  - "[[尺呆]]"
published:
created: 2026-07-02
description: "OpenSpec 是什么？它不是让 AI 更会写代码，而是让 AI 少跑偏大家现在聊 AI 编程，已经不太会停留在“它能不能写代码”这个问题上了。 因为这件事，答案已经很明确：能写，而且很多时候写得还挺快。 真正开始让人…"
tags:
  - "clippings"
---
大家现在聊 [AI 编程](https://zhida.zhihu.com/search?content_id=273100929&content_type=Article&match_order=1&q=AI+%E7%BC%96%E7%A8%8B&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODMxMjgxMzIsInEiOiJBSSDnvJbnqIsiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzMxMDA5MjksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.5DXcc37alehQL9dQUeK9e0yi9PdyTsxT2XRYOzXWbds&zhida_source=entity) ，已经不太会停留在“它能不能写代码”这个问题上了。

因为这件事，答案已经很明确：能写，而且很多时候写得还挺快。

真正开始让人头疼的，是另一件事。

**AI 会写，不等于 AI 能稳定交付。**

你让它补一个小功能，它也许十分钟就给你写完了；但你让它改一个真实项目里的登录流程、权限系统、支付逻辑、通知链路，问题马上就出来了。

需求到底理解对了没有？边界是不是它自己脑补的？改动是不是影响了旧行为？这次会话里说清楚了的东西，下次新开一个窗口，它还记不记得？

很多人用 AI Coding 用到后面，都会碰到一个很像的尴尬：

不是它不会写，而是它 **经常在“还没完全搞清楚要做什么”之前，就已经开始往下写了** 。

这也是为什么这段时间，像 OpenSpec 这样的东西开始越来越多人讨论。

它不是一个新的大模型，不是一个替代 Cursor、Claude Code、Copilot 的 IDE，也不是那种“再套一层壳”的 AI 工具。

如果非要用一句话概括它，我会这么说：

> **OpenSpec 是 AI 编程里的 [规范层](https://zhida.zhihu.com/search?content_id=273100929&content_type=Article&match_order=1&q=%E8%A7%84%E8%8C%83%E5%B1%82&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODMxMjgxMzIsInEiOiLop4TojIPlsYIiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzMxMDA5MjksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.-BnABzSariZzQldKaobLPd5atbhDoKl3nDieMu2zV5k&zhida_source=entity) 。它不负责替你执行一切，而是先把“这次到底要改什么”固定下来。**

这篇文章我想讲清楚三件事： **OpenSpec 到底解决什么问题，实际怎么用，以及它为什么会在现在这个阶段变得有价值。**

![](https://pic1.zhimg.com/v2-fb3a4dbbf0a0d5d1ea97f0db9de6e72a_1440w.jpg)

## 为什么 OpenSpec 会出现？先看 AI 编程最常见的失控点

如果你只是让 AI 写个脚本、补个函数、改个页面，那多数时候问题不大。

但只要任务进入真实工程，事情就会迅速变复杂。

复杂并不只是因为代码量大，而是因为 AI 编程在工程环境里有几个特别典型的失控点。

首先， **需求经常只存在于聊天记录里** 。

你今天在对话里解释了一堆背景、边界、例外、历史包袱，AI 当场好像也听懂了。但这些东西很多时候并没有真正沉淀成项目资产。换个会话，换个模型，甚至只是上下文窗口被新内容冲淡，前面那些约束就开始失真。

其次， **AI 特别容易在模糊需求上“顺手发挥”** 。

你说“加一个记住我功能”，它可能顺手改 session、加本地存储、补持久化 cookie、重构认证逻辑，甚至给你引入一套它觉得更优雅的结构。问题不在于它写不出来，而在于它经常把“你想做什么”和“它觉得应该怎么做”混在一起。

再往下，还有一个更现实的问题： **代码评审越来越像在猜谜。**

当 AI 已经把代码写完以后，你再去 review，很多时候其实是在反推：它到底是理解对了需求，还是只是刚好写出一份看起来能跑的实现？

所以你会发现，AI 编程里最容易出问题的，不一定是“写代码”这一步，而是更前面的那一步：

**在动手之前，到底有没有把意图、边界和行为变化说清楚。**

OpenSpec 补的，正是这一层。

## OpenSpec 是什么？它是一个 spec framework，不是执行引擎

我觉得理解 OpenSpec，最重要的一点不是记住它的命令，而是先搞明白它的定位。

OpenSpec 官方把自己定义成一个 **lightweight spec-driven framework** ，核心主张是 **Agree before you build** 。\[1\]

这句话翻译成人话，其实就是：

> **先对齐，再动手。**

也就是说，OpenSpec 并不是先让 AI 去写代码，再靠你回头收拾；而是先把一项需求沉淀成一组结构化工件，让你和 AI 对这次改动达成共识之后，再进入实现。

这里特别适合借一句更直接的定位句：

> **OpenSpec 是一个 spec framework，而不是一个执行引擎。**

这句话很重要，因为它直接划清了边界。

它不主打多代理编排，不主打上下文压缩，也不主打复杂流水线。它解决的是一件更基础、也更常被忽略的事： **把需求、设计和任务拆解从聊天记录里捞出来，变成仓库中的正式资产。**

## OpenSpec 到底在项目里放了什么？核心其实只有两层

OpenSpec 的结构比很多人想象中要轻。官方 Concepts 文档里，最核心的就是两块： `specs/` 和 `changes/` 。\[2\]

前者描述的是系统 **当前如何工作** ，后者描述的是你这次 **准备怎么改** 。

| 目录 | 作用 | 更适合怎么理解 |
| --- | --- | --- |
| openspec/specs/ | 当前系统行为的来源事实 | 系统今天是怎么工作的 |
| openspec/changes/ | 每次变更的完整材料 | 这次准备怎么改它 |

这个拆分看着简单，价值其实很大。

过去很多团队的问题是，代码会持续变化，但“原来系统是什么行为”“这次改动影响了什么规则”“为什么当时这么设计”，这些信息并没有被稳定保存下来。

而 OpenSpec 的思路是：每次改动都先形成一个 change folder，把这次变更涉及的内容打包在一起。通常里面会包含 `proposal.md` 、 `design.md` 、 `tasks.md` ，以及对应的 `spec delta` 。\[1\]\[2\]\[3\]

| 文件 | 它解决的问题 |
| --- | --- |
| proposal.md | 为什么要改，改动范围是什么 |
| design.md | 技术上准备怎么落地 |
| tasks.md | 具体分几步完成 |
| spec delta | 这次改动会让系统行为怎么变化 |

从这个角度看，OpenSpec 最有价值的地方不是“又多了几份 Markdown”，而是它把一次需求变更从模糊想法，变成了一个 **可审阅、可归档、可回溯** 的工程单元。

![](https://pic1.zhimg.com/v2-bc9205efc74fdddcdac8306c269a78ec_1440w.jpg)

## 为什么它比“写个更详细的 Prompt”更有用？

很多人第一次看到 OpenSpec，都会有一个自然反应：

这不就是把 prompt 写得更长一点吗？

表面看有点像，但本质不是一回事。

更长的 prompt，解决的是“这一次怎么跟 AI 说”；而 OpenSpec，解决的是“以后怎么持续让人和 AI 对同一个项目保持一致理解”。

这两者差的不是长度，而是层级。

Prompt 本质上还是对话内资产。它往往绑定当前会话、当前上下文、当前操作者。今天好用，不代表下周还好用；这次说清楚了，不代表换个同事、换个模型、换个 session 还能维持同样的理解。

而 OpenSpec 把这些东西正式沉淀进仓库里。

从此以后，需求边界、行为合同、设计取舍、任务拆解，不再只是“某次聊天中提过”，而是项目的一部分。

所以我更愿意把它理解成：

> **Prompt 是一次性表达，Spec 是长期协作记忆。**

这也是为什么现在越来越多人开始意识到，AI 编程真正缺的，不是“再聪明一点的模型”，而是 **能让模型稳定发挥的上下文骨架** 。

## OpenSpec 最适合补哪一层？我觉得它就是 AI Coding 里的“规范层”

这一点，是我看完很多相关框架后越来越明确的判断。

OpenSpec 最适合被理解成 AI 编程体系里的 **规范层** 。

什么意思？

它不是帮你解决所有问题，也不是想一口气接管整个 AI workflow。它主要解决的是：

**在真正执行之前，先把“做什么”固定下来。**

如果用更工程化一点的话说，它解决的是以下几类问题：

| 典型问题 | OpenSpec 的补位方式 |
| --- | --- |
| 需求经常漂 | 用 proposal 和 spec 把变更边界固定下来 |
| AI 老跑偏 | 先对齐行为变化，再进入实现 |
| 团队理解不一致 | 用结构化文档统一语义 |
| [Brownfield 项目](https://zhida.zhihu.com/search?content_id=273100929&content_type=Article&match_order=1&q=Brownfield+%E9%A1%B9%E7%9B%AE&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODMxMjgxMzIsInEiOiJCcm93bmZpZWxkIOmhueebriIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI3MzEwMDkyOSwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.nCOLktkdwHltwwk4M5pn-p8WkNQCLBusr-7UeifGZxw&zhida_source=entity) 难改 | 用 spec 明确当前行为与本次改动 |
| 代码 review 成本高 | 先 review 意图，再 review 实现 |

尤其是最后一点，我觉得特别重要。

OpenSpec 官方首页有一句话我很认同： **Review intent, not just code** 。\[1\]

以前我们评审 AI 生成代码，经常是在读实现、猜意图。

而有了 spec delta 之后，至少你可以先看清楚：这次改动究竟想改变哪些系统行为。评审的人不需要第一时间扎进代码，也能先判断方向对不对。

这其实是在把 review 从“事后猜它写对没”变成“事前确认它是不是在做对的事”。

## 它实际怎么用？流程并没有想象中重

如果只看概念，很多人会担心 OpenSpec 会不会太流程化。

但从官方 README 来看，它的工作流其实很克制。\[3\]

先安装：

```
npm install -g @fission-ai/openspec@latest
```

进项目初始化：

```
cd your-project
openspec init
```

然后，你就可以围绕一个具体需求发起 proposal，比如：

```
/opsx:propose 添加 remember me 登录功能
```

接着，它会生成一套围绕这次改动的变更目录，把 proposal、design、tasks、spec delta 准备好。你可以先审、先改、先补充，再让 AI 真正进入实现阶段。\[1\]\[3\]

整个过程其实可以压成一条很清晰的链路：

| 阶段 | 这一步在做什么 | 产物 |
| --- | --- | --- |
| 提出变更 | 明确要改什么 | proposal |
| 固化行为 | 明确系统行为会怎么变化 | specs delta |
| 确认方案 | 明确技术实现思路 | design |
| 拆成任务 | 明确执行顺序和颗粒度 | tasks |
| 进入实现 | 让 AI 按约束落代码 | code |
| 完成归档 | 把通过的变更并入主 specs | archive |

这看起来像是多加了一层步骤。

但实际价值是， **你把返工前移了。**

以前是代码都写出来了，才发现需求理解偏了；现在是先把偏差暴露在 proposal 和 spec 阶段，修起来成本要小得多。

## 哪些人最值得用 OpenSpec？

我觉得它不是只适合大团队，反而也很适合现在这批高频使用 AI Coding 的个人开发者。

因为个人开发者最容易吃的亏，恰恰就是“觉得自己都知道，所以懒得固化”。

今天你当然知道这次为什么这么改，但过一周以后，你未必还记得当时的边界；换个会话继续让 AI 接手时，它更不可能天然知道这些上下文。

所以只要你属于下面几类情况，我都会建议试一下：

| 场景 | 建议程度 | 原因 |
| --- | --- | --- |
| 临时脚本、小玩具 Demo | 可选 | 返工成本低，流程不一定必要 |
| 正在改已有业务项目 | 很建议 | 需要保护旧行为，控制影响范围 |
| 高频使用 AI 协作开发 | 很建议 | 能降低跑偏和重复解释的成本 |
| 多人协作写项目 | 很建议 | 更容易统一理解与追踪决策 |
| 需要长期维护的仓库 | 很建议 | 规格和变更会变成项目记忆 |

尤其是 brownfield 项目，也就是已有代码库改造，这一点 OpenSpec 很有现实意义。\[3\]

因为现实世界里，大多数人接手的都不是一张白纸，而是一套已经跑起来、但有历史约束、有业务包袱、有维护风险的系统。

在这种场景里，AI 最怕的就是“看懂了眼前代码，但没看懂系统真正的行为底线”。

而 Spec 恰好能把这些边界从隐性经验，变成显性合同。

## OpenSpec 真正值钱的，不是规范感，而是“公共记忆”

如果只把 OpenSpec 理解成“多写几份文档”，其实会低估它。

我觉得它真正值钱的地方，在于它让需求、设计和任务拆解不再只活在聊天窗口里，而是进入仓库，变成一种 **公共记忆** 。

这个公共记忆既服务于人，也服务于 AI。

对你自己来说，它让你过段时间回来看，依然知道当时为什么这么做。

对同事来说，它让别人接手时，不需要从代码里硬猜历史意图。

对 AI 来说，它让模型不必每次都从零开始“悟”你的项目规则，而是有明确的结构可以读取。

这就是为什么我越来越觉得，未来 AI 编程真正比拼的，不只是模型能力，而是你有没有把项目上下文组织成一个它能长期稳定使用的体系。

在这个体系里，OpenSpec 补的是最前面的那层：

**先把“要做什么”讲清楚。**

![](https://picx.zhimg.com/v2-4652401cc272785e691ea45f2cfb2fd9_1440w.jpg)

## 最后一句话总结：OpenSpec 不是让 AI 更聪明，而是让协作更可控

如果你现在还只是偶尔让 AI 帮你补点代码，那 OpenSpec 未必是第一优先级。

但如果你已经开始让 AI 真实参与项目开发，尤其是涉及老项目、多轮迭代、多人协作、需求容易漂移的场景，那它非常值得花一点时间试一试。

因为很多时候，AI 编程最麻烦的地方，不在“写不出来”，而在“没先对齐”。

OpenSpec 做的，就是把这件事正式补上。

它不保证 AI 永远不犯错。

但它能显著提高一件更重要的事： **你知道这次到底要改什么，也知道它为什么会这么写。**

这就已经很值钱了。

---

## 参考资料

\[1\] [OpenSpec 官方网站](https://link.zhihu.com/?target=https%3A//openspec.dev/)

\[2\] [OpenSpec Concepts 文档](https://link.zhihu.com/?target=https%3A//github.com/Fission-AI/OpenSpec/blob/main/docs/concepts.md)

\[3\] [OpenSpec GitHub README](https://link.zhihu.com/?target=https%3A//github.com/Fission-AI/OpenSpec)

发布于 2026-04-14 19:24・北京・包含 AI 辅助创作 作者对内容负责

赞同