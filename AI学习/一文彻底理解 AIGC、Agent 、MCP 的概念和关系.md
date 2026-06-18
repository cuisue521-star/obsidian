---
title: "一文彻底理解 AIGC、Agent 、MCP 的概念和关系"
source: "https://zhuanlan.zhihu.com/p/1928117140751362037"
author:
  - "[[腾讯技术工程​]]"
published:
created: 2026-06-18
description: "作者：willzhen 近两年 AI 技术发展迅猛，日新月异。大语言模型 (LLM)、AIGC、多模态、RAG、Agent、MCP 等各种相关概念层出不穷，若不深入了解，极易混淆。本文旨在简要介绍这些 AI 技术的核心概念、基本原理及其…"
tags:
  - "clippings"
---
127 人赞同了该文章

作者：willzhen

> 近两年 AI 技术发展迅猛，日新月异。大语言模型 (LLM)、 [AIGC](https://zhida.zhihu.com/search?content_id=260324626&content_type=Article&match_order=1&q=AIGC&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODE5NDI5NDgsInEiOiJBSUdDIiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6MjYwMzI0NjI2LCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.cG5vVIpDybxWRIYy6lpWiF1Tr3uP9s-EVv_RcbouqDY&zhida_source=entity) 、多模态、RAG、Agent、MCP 等各种相关概念层出不穷，若不深入了解，极易混淆。本文旨在简要介绍这些 AI 技术的核心概念、基本原理及其相互关系，主要帮助非 AI 行业的开发者建立基础认知。文中涉及的每项技术在其垂直领域都值得深入探索，本文仅作概念性和原理性的概述。如有疏漏或错误，欢迎指正。

近年来，人工智能领域涌现出许多新概念和新技术，其中AIGC、MCP和Agent成为了业界和学术界的热门话题。本文将深入浅出地介绍这三个概念，帮助读者全面理解它们的内涵、区别与联系，以及在实际应用中的价值。

### 一、AIGC

AIGC，全称为 AI Generated Content，意为“人工智能生成内容”。它指的是利用人工智能技术（尤其是大模型，如GPT、Stable Diffusion等）自动生成文本、图片、音频、视频等多种内容的过程。2022 年 11 月 30 日，OpenAI 的 ChatGPT 正式上线（基于 GPT-3.5），引爆了 AIGC 热潮。

![](https://pic2.zhimg.com/v2-85c5e650729757bb818018c31826ee51_1440w.jpg)

### 1.1 多模态技术

**单模态** ：只处理一种类型的数据，比如只处理文本（如GPT-3.5）、只处理图像（如图像识别模型）。

**多模态** ：能够同时处理两种及以上类型的数据。例如，既能理解图片内容，又能理解文本描述，甚至还能结合音频、视频等信息进行综合分析和生成。对应的场景有

| 场景 | 主流模型 |
| --- | --- |
| 文生图片 | DALL-E(OpenAI)、Imagen(Google)、Stable Diffusion(Stability AI)、混元文生图（腾讯）等 |
| 文生视频 | Sora(OpenAI)、Stable Video Diffusion(Stability AI) |
| 图生文（图片理解） | [GPT-4](https://zhida.zhihu.com/search?content_id=260324626&content_type=Article&match_order=1&q=GPT-4&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODE5NDI5NDgsInEiOiJHUFQtNCIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI2MDMyNDYyNiwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.o7iUiaX7Y26RPzgpnv8LDFaaSYmJ1GhuyZsCCnry6sk&zhida_source=entity) V(OpenAI)、Gemini(Google)、Qwen-VL（阿里） |
| 图文生视频 | Runway Gen-2(Runway AI)、Stable Video Diffusion(Stability AI) |
| 视频生文（视频理解） | Gemini 1.5 / Gemini Pro Vision（Google） |

### 1.2 RAG 技术

RAG（Retrieval-Augmented Generation，检索增强生成） 技术，是一种将信息检索（IR） 与大型语言模型（LLM） 的文本生成能力相结合的人工智能框架。其核心思想是：当 LLM 需要回答一个问题或生成文本时，不是仅依赖其内部训练时学到的知识，而是先从一个外部知识库中检索出相关的信息片段，然后将这些检索到的信息与原始问题/指令一起提供给LLM，让LLM基于这些最新、最相关的上下文信息来生成更准确、更可靠、更少幻觉的答案。

大型语言模型虽然拥有海量的知识和强大的语言理解与生成能力，但也存在一些关键限制：

1. 知识局限性/过时性： LLM 的知识主要来源于其训练数据截止日期之前的信息。对于训练数据之后发生的事件、新研究、最新数据或特定领域的细节，LLM 可能不知道或给出过时的信息。
2. 幻觉： 当 LLM 遇到其知识库中不明确或不存在的信息时，它可能会“捏造”出看似合理但事实上错误或不存在的答案。
3. 缺乏来源/可验证性： LLM 通常无法提供其生成答案的具体来源依据，使得验证答案的准确性变得困难。
4. 特定领域知识不足： 通用 LLM 可能缺乏对某个特定公司、组织或个人私有知识库的深入了解。

RAG 正是为了解决这些问题而诞生的。

![](https://pic3.zhimg.com/v2-4df1aea1049a4d2a89d76f1859548e78_1440w.jpg)

### 二、智能体 Agent

“智能体”（Agent）在计算机科学和人工智能领域指的是 **一个能够感知环境、自主决策并采取行动以实现特定目标的实体或系统** 。它可以是软件程序、机器人硬件，甚至是生物实体（如人类或动物），但在 AI 领域通常指 **软件智能体** 。

Agent 和 AIGC 最大的区别：

1. AIGC 主要以生成式任务为主，而 Agent 是可以通过自主决策能力完成更多通用任务的智能系统。
2. 常见的 AIGC 系统（文生文，文生图）的核心就是一个 **生成模型** ，而 Agent 是一个集 **F\*\*\*\*unction Call** **模型** （下文会详细介绍）、软件工程于一体的复杂的系统，需要处理模型和外界的信息交互。
3. Agent 可以集成 AIGC 能力完成某些特定的任务，也就是 AIGC 可以是 Agent 系统里面的一个子模块。

Agent 最大的特点是，借助 **F\*\*\*\*unction Call 模型，** 可以自主决策使用外接的一些工具来完成特定的任务。

### 2.1 Function Call 模型

### 2.1.1 什么是 Fucntion Call 模型

**Function Calling（函数调用）** 是大型语言模型的关键技术。前面有提到过 **RAG技术** 是为了解决模型无法和外接数据交互的问题，但是 **RAG** 的局限在于只赋予了模型检索数据的能力，而 **Function Calling** 允许模型理解用户请求中的潜在意图，并自动生成结构化参数来调用外部 **任何** 函数/工具，从而突破纯文本生成的限制，实现与真实世界的交互，比如可以调用查天气、发邮件、数学计算等工具。

Function Call 模型最早由 OpenAI 在 2023 年 6 月 13 正式提出并发布，首次在 GPT-4 模型上实现了 Function Calling 能力。OpenAI 作为大语言模型的领路人，其发布的模型的 API 协议都会行业标准，后面国内外新发布模型都会按照 OpenAI 的协议作为标准实现。截止目前，支持 Fucntion Calling 能力的主流模型如下表

| 模型 | 开发者 | 首次支持 Function Calling 时间 |
| --- | --- | --- |
| GPT-4 | OpenAI | 2023/06/13 |
| Claude-3 | [Anthropic](https://zhida.zhihu.com/search?content_id=260324626&content_type=Article&match_order=1&q=Anthropic&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODE5NDI5NDgsInEiOiJBbnRocm9waWMiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNjAzMjQ2MjYsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.krLZrR4oXB9WsjtTYtlV6V817YanmUYFc64ZkXlJFpg&zhida_source=entity) | 2024/03/04 |
| Gemini-2.0 | Google | 2024年12月 |
| DeepSeek-R1 | 深度求索公司 | 2025/02/21 |

除了上面的知名度高的模型，还有一些其他开源或闭源模型也支持了 Fucntion Calling 能力，但是截止目前为止，GPT-4 仍然是公认的 Fucntion Calling 能力最强的模型。

### 2.1.2 工作原理：三步闭环流程

Function Call 模型的工作流程如下图

![](https://pic2.zhimg.com/v2-815a4b2b2aedc35d3a40798d82f817bd_1440w.jpg)

步骤详解：

1. **定义函数** （开发者预设） 向 LLM 描述函数的用途、输入参数格式（ [JSON Schema](https://zhuanlan.zhihu.com/p/678584949) ），例如：  
	{  
	"name": "get\_current\_weather",  
	"description": "获取指定城市的天气",  
	"parameters": {  
	"type": "object",  
	"properties": {  
	"city": {"type": "string", "description": "城市名称"},  
	"unit": {"enum": \["celsius", "fahrenheit"\]}  
	},  
	"required": \["city"\]  
	}  
	}  
	name 是工具名称  
	description 是这个工具的用途  
	parameters 是这个工具需要的输入参数
2. **模型决策与生成参数** 用户提问： `“北京今天需要带伞吗？”` → LLM 识别意图需调用 `get_current_weather` → 生成结构化参数：  
	{"city": "北京", "unit": "celsius"}
3. **执行函数 & 返回结果**
- 程序调用天气API，获真实数据： `{"temp": 25, "rain_prob": 30%}`
- 将结果交回LLM，生成最终回复： `“北京今天25°C，降水概率30%，建议带伞。”`

### 2.1.3 核心优势：LLM 的“手和眼睛”

| 能力 | 传统LLM | 支持Function Calling的LLM |
| --- | --- | --- |
| 获取实时信息 | ❌ 依赖训练数据 | ✅ 调用搜索引擎/数据库 |
| 执行精准计算 | ❌ 常出错（如复杂数学） | ✅ 调用计算器/Python |
| 操作外部系统 | ❌ 无法执行 | ✅ 发送邮件/控制智能家居 |
| 返回结构化数据 | ❌ 文本难解析 | ✅ 输出标准JSON |

### 2.2 Agent

OpenAI 发布 Function Call 模型后，Agent 才开始发展。而 Agent 真正进入到公众视野，被大家广泛关注的事件是 2025年4月 [Manus](https://zhida.zhihu.com/search?content_id=260324626&content_type=Article&match_order=1&q=Manus&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODE5NDI5NDgsInEiOiJNYW51cyIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI2MDMyNDYyNiwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.dr-lmn80UA0mjh4gs6RJ3HEMjVvbsqq7PmeBPZimAXI&zhida_source=entity) 发布了通用智能体产品，引入了 **Computer Use 和 Browser Use** ，首次展现出智能体的强大能力。

### 2.2.1 Agent 的工作流程

实际上上文提到的 Function Call 模型的工作流程图，已经算是一个 Agent 的雏形了，不同点是，Agent 完成一次任务，实际上会循环调用模型，可能会调用多次 Function Calling，每次需要调用什么工具，完全由模型决策。一个最简单的 Agent 调用流程图如下：

![](https://pica.zhimg.com/v2-72561d357f5473b09da621daed432d7c_1440w.jpg)

比如有一个出行规划的智能体，这个智能体配置有天气查询、驾车规划、公共交通规划、骑行规划、步行规划等工具。用户询问“我在深圳，5月1日想去自驾去北京旅行，帮我规划一下出行方案。”，一个可能的具体的执行流程如下

![](https://picx.zhimg.com/v2-7960109b770d5ef25991fc17a159dc37_1440w.jpg)

### 2.2.2 怎么开发一个自己的 Agent

最简单的方法就是把 Agent 的提示词（prompt）、工具、llm 调用，工具执行都硬编码到代码中，这样确实可以快速开发一个特定功能的 Agent。这样的实现会带来一些问题

1. 提示词（prompt），工具需要调整的时候，需要改配置或者代码，灵活度不够高；
2. 如果要开发一个新功能的 Agent，整体代码可能需要重新实现一遍。

为了解决这一系列的问题， [coze](https://link.zhihu.com/?target=https%3A//www.coze.cn/home) 、 [dify](https://link.zhihu.com/?target=https%3A//cloud.dify.ai/apps) 、 [腾讯云智能体开发平台](https://link.zhihu.com/?target=https%3A//lke.cloud.tencent.com/lke%23/experience-center/home%3Forigin%3Dall) 等智能体开发平台相继出现。借助这些平台，开发者甚至不需要会编程，不需要服务器资源，就可以开发一个自己的Agent，Agent 的整个执行流程完全由平台在云上执行。智能体开发平台的架构一般包含 插件配置、Agent 配置、Agent 执行模块、插件执行模块，发布模块。

![](https://pica.zhimg.com/v2-b311c4bb5352fb8c086679e63584baf6_1440w.jpg)

插件配置：所有 Agent 的工具都统一管理起来，而不是散落在各个 Agent 内部，这样可以做到工具的复用。一般平台会自带一些插件，比如网络搜索、文件上传、AIGC 工具等，同时也支持开发者添加自己的自定义插件。

Agent 配置：配置 Agent 的 提示词 (prompt)，使用的模型，以及选择插件配置中的一批工具提供给模型做选择。

发布配置：开发者把自己的 Agent 开发调试稳定以后，发布成稳定版本就可以提供给用户使用了。

插件执行：执行某个特定的插件，返回结果。

Agent 执行：实现通用的 Agent 执行流程，调用插件执行模块实现工具调用。

下图是用腾讯云智能体开发平台，开发一个简单的 Agent 配置和实际执行效果图。

![](https://pic4.zhimg.com/v2-5fca5abc326ea1b02daeaf9622058905_1440w.jpg)

### 2.2.3 Multi-Agent

除了使用智能体开发平台快速开发自己的 Agent 以外，还可以使用 sdk 的方式进行开发。2025 年 3 月 11 日，OpenAI 重磅发布 [OpenAI Agent SDK](https://link.zhihu.com/?target=https%3A//openai.github.io/openai-agents-python/) ！AI 开发范式彻底颠覆！使用 sdk 可以快速配置一个自定义的 Agent 后执行，相比智能体开发平台，sdk 具有更高的灵活性和自主可控性。

同时，在 OpenAI Agent SDK 中，首次引入了 Mulit Agent 的概念。在此之前，通过智能体开发平台，我们开发出来的 Agent 都只是单 Agent。一个单 Agent 的能力有限，只能解决特定领域的一个任务，而一个复杂任务往往需要执行多个领域的任务才能完成。而 OpenAI Agent SDK 可以让开发者定义多个领域的 Agent，并且给这些 Agent 配置一些转交关系，允许某个 Agent 把特定的任务交给另外一个合适领域的 Agent 来执行，多个 Agent 之间协同和互动来完成一个复杂任务。

在 OpenAI Agent SDK 发布以后，coze，和腾讯云智能体开发平台都相继支持了 Multi-Agent 模式。

### 2.3 Agent 的发展

Agent 目前的发展还处于一个较初期的阶段，但是发展速度很快。在一些垂直领域比如代码生成 Cursor、智能客服、广告营销等方向已经有了比较好的落地。而更通用的 Agent 目前除了看到 Manus 落地以外，还没看到其他比较好的应用模式落地。相信随着时间发展，会有越来越好用，越来越通用的 Agent 应用诞生。

### 三、MCP

### 3.1 什么是 MCP

MCP（ [Model Context Protocol](https://link.zhihu.com/?target=https%3A//modelcontextprotocol.io/introduction) ，模型上下文协议）是由人工智能公司 **Anthropic** 于 **2024 年 11 月 24 日** 正式发布并开源的协议标准。Anthropic 公司是由前 OpenAI 核心人员成立的人工智能公司，其发布的 Claude 系列模型是为数较少的可以和 GPT 系列抗衡的模型。

### 3.2 为什么需要 MCP

MCP 协议旨在解决大型语言模型（LLM）与外部数据源、工具间的集成难题，被比喻为“AI应用的USB-C接口“。通过标准化通信协议，将传统的“M×N集成问题”（即多个模型与多个数据源的点对点连接）转化为“M+N模式”，大幅降低开发成本。

![](https://pica.zhimg.com/v2-80ce23319d1a9e7c7d36cc62c468bbee_1440w.jpg)

在 MCP 协议没有推出之前：

1. 智能体开发平台需要单独的插件配置和插件执行模型，以屏蔽不通工具之间的协议差异，提供统一的接口给 Agent 使用；
2. 开发者如果要增加自定义的工具，需要按照平台规定的 http 协议实现工具。并且不同的平台之间的协议可能不同；
3. “M×N 问题”：每新增一个工具或模型，需重新开发全套接口，导致开发成本激增、系统脆弱；
4. 功能割裂：AI 模型无法跨工具协作（如同时操作 Excel 和数据库），用户需手动切换平台。

没有标准，整个行业生态很难有大的发展，所以 MCP 作为一种标准的出现，是 AI 发展的必然需求。

总结：MCP 如何重塑 AI 范式：

| 维度 | 传统模式 | MCP 模式 | 变革价值 |
| --- | --- | --- | --- |
| 集成成本 | 每对接新工具需定制开发 | 一次开发，全网复用 | 开发效率提升 10 倍 |
| 功能范围 | 单一工具调用 | 多工具协同执行复杂任务链 | AI 从“助手”升级为“执行者” |
| 生态开放性 | 封闭式 API，厂商锁定 | 开源协议，社区共建工具库 | 催生“AI 应用商店”模式 |
| 安全可控性 | API 密钥暴露风险 | 数据不离域，权限分级管控 | 满足企业级合规需求 |

### 3.3 MCP 的发展情况

MCP 自 **2024 年 11 月 24 日 发布以来，** OpenAI、Google、微软、腾讯、阿里、百度等头部企业纷纷接入 MCP，推动其成为 **事实性行业标准。** 并且相继出现了 [mcp.so](https://link.zhihu.com/?target=https%3A//mcp.so/) 、 [mcpmarket](https://link.zhihu.com/?target=https%3A//mcpmarket.cn/) 等超大体量的 MCP 服务提供商。国内的头部企业也相继加入 MCP 服务商的竞争中，百度发布了 [千帆 MCP 广场](https://link.zhihu.com/?target=https%3A//sai.baidu.com/mcp%3Fref%3Dopeni.cn) **，** 阿里发布了 [魔搭 MCP 广场](https://link.zhihu.com/?target=https%3A//www.modelscope.cn/mcp%3Fcategory%3Dresearch-and-data%26page%3D1) 。在如此庞大的 MCP 市场下，开发者基本不需要开发自己的插件，直接使用 MCP 服务商的插件就可以直接开发大量 Agent。

同时很多头部企业，开始把自身原有的 API 业务开发成封装成 MCP 服务对外提供。比如

1. GitHub Copilot 提供 MCP 的方式生成代码；
2. AWS 2025 年 6月推出开源工具 Amazon Serverless MCP Server **，** 支持 Agent 直接操作云上资源，进行服务编排。
3. 腾讯地图、高德地图、百度地图均发布 MCP Server，支持在 Agent 中使用丰富的地图资源。
4. 腾讯云COS、阿里云OSS、百度网盘均已支持 MCP 协议的接入。

**未来趋势** ：

- **与 AIOS 融合** ：MCP 正成为 AI 操作系统（如华为鸿蒙 HMAF）的核心组件，实现跨设备智能调度；
- **生态挑战** ：大厂通过 MCP 构建“闭环生态”（如阿里集成高德地图），可能引发协议割裂，需推动跨平台协作标准。

MCP 不仅是技术协议，更是 **AI 生产力革命的基石** ——它让模型真正融入现实世界，成为人类工作的无缝延伸。

**总结**

整体上看，Agent 是在 AIGC、MCP 、大语言模型 LLM 等原子能力的基础上进行编排，以提供更复杂的 AI 应用。

发布于 2025-07-14 17:45・广东

赞同 127