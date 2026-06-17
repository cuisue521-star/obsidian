---
source: https://mp.weixin.qq.com/s/IyB5vFWFNiDS7uT-TKdydA
description: 2 分钟教你用 Claude Code 和 Codex 对接国内模型，保姆级教程
tags:
  - clippings
---
它是一个跨平台桌面应用，把 Claude Code、Claude Desktop、Codex、Gemini CLI、OpenCode、OpenClaw、Hermes Agent 这七个 AI 编程 CLI 的供应商配置、MCP 服务器、Prompts、Skills、代理、会话和成本追踪全部收进一个界面。

很多人想学 AI 编程，想耍一耍目前最流行的 Claude Code 和 Codex 编程工具，结果一上手就卡在了第一步。

要么没有国外的订阅账号，登录都登录不上；要么好不容易开通了，发现官方额度死贵，对话一会儿额度就耗光了；再加上时不时还有封号的风险，整的提心吊胆。

咱们怎么能因为「用不了工具」这种事，就把学 AI 编程的劲头给浇灭了呢！

其实 Claude Code 和 Codex 都支持切换模型，咱们用国内的大模型（比如 DeepSeek、Qwen、智谱 GLM）来驱动它们就行，量大管饱，不用魔法，也不怕封号。

这篇文章我就手把手教你，用一个开源神器 **CC Switch** ，几分钟带你把国内模型接到 Claude Code 和 Codex 里。我会以 DeepSeek 为例，看完你就能跑通整套流程，想换哪家模型都是一样的操作。


## CC Switch 是什么

Claude Code、Codex 这些命令行工具，每一个的配置格式都不一样。如果你想给它们换个模型供应商，得自己去翻文档，手动编辑 JSON、TOML 或者 `.env` 文件，填一堆 Base URL、API Key、模型名之类的参数。说不定改错一个字符就跑不起来，想在几个模型之间来回切换就更麻烦了。。。

CC Switch 就是来解决这个痛点的。它是一个免费开源的跨平台桌面应用，把 Claude Code、Claude Desktop、Codex、Gemini CLI、OpenCode、OpenClaw、Hermes Agent 这七个 AI 编程 CLI 的供应商配置、MCP 服务器、Prompts、Skills、代理、会话和成本追踪全部收进一个界面。

> 开源指路：https://github.com/farion1231/cc-switch

![图片](https://mmbiz.qpic.cn/mmbiz_png/LlSQOKIxJ1FeibicHkggVnaOKf5bLAj8HicHY7eh0E9LVzZN2PZuUAibvX29O8MfOjiceG0aAMxibK3po7Ndf4wktD58aN0HEjsbrRjZJPUy0rJWg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=1)

CC Switch 内置了 50 多个供应商预设，DeepSeek、Qwen、Kimi、智谱 GLM、MiniMax 这些都有，你不用自己手动改配置文件，点几下就能一键切换模型，还能从系统托盘里快速切换。

![图片](https://mmbiz.qpic.cn/mmbiz_png/LlSQOKIxJ1HeSgvcRTHsmoHGYneVjGiaHkr5t477Yo3loVRicrZKN432wibFW4LBCBJ7SAT28vB2jWqD6a6WknUouhcOFRcNmOZqdv4xHfLicE4/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=2)

下面我就用它来带大家实操一遍。

## 安装 CC Switch

到 CC Switch 官网 或者 GitHub Releases 页面，根据你的操作系统选择对应的安装方式。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/LlSQOKIxJ1FQfdoW4rH7pCvscTIhAEkaRepvXQBOn0S2hP8v6na9NMATtHrpTe2lwiaIgyjJahsnrCdemjPx2iajayxaaxkEn7U3gMWdiapKYw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=3)

Mac 用户推荐直接用 Homebrew 一行命令安装，装完直接就能用：

```
brew install --cask cc-switch
```
![图片](https://mmbiz.qpic.cn/mmbiz_png/LlSQOKIxJ1FqiazzGSg2rD31jFer6uHeicwic9uzgmqANZK5cKczTyibay3aWc8n80Dcg52ue2LSNYL1nxnC0LQXhk0uUF1nC61sBMEEudOJicvM/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=4)

Windows 用户从 Releases 页面下载 `.msi` 安装包，双击运行即可；Linux 用户根据发行版选择 `.deb` 、`.rpm` 或 `.AppImage` 。

安装完成后启动 CC Switch，主界面会出现在桌面或者系统托盘里。

下面我们就分别给 Claude Code 和 Codex 接上 DeepSeek 了。不过在那之前，得先把 DeepSeek 的 API Key 准备好。

## 准备大模型 API Key

不管接哪个工具，都得先有一个 DeepSeek 的 API Key。

到 DeepSeek 开放平台 注册登录，进入 API keys 页面，点击创建一个新的 key。

![图片](https://mmbiz.qpic.cn/mmbiz_png/LlSQOKIxJ1FlMqxKicIiaS4T4xMqW5Kdicgmics3ibjTBGupCPcBwcbHdDqPia0mS82loaxF3vvb2R4wPwtgzJ8Jy0lPeuourtHOQ4h3zVTPEAK0M/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=5)

注意，key 只会在创建时完整显示这一次，记得当场复制保存好，后面在 CC Switch 里要用到它。

## 把 DeepSeek 接入  [[如何在Claude Desktop里配置DeepSeek-V4（跳过登录）]] 

我们先从简单的 Claude Code 开始。

### 先装好 Claude Code

简单介绍一下 Claude Code。它是 Anthropic 推出的 AI 编程工具，直接在终端里运行，你跟它聊天描述需求，它就能自主分析项目、写代码、跑命令、修 Bug，全程自主执行。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/LlSQOKIxJ1Eb12DIqC6WicpGyeSWR8A8Xt5oJMmurZibYicQGjNeEGTkNA8vEf94tibickaATrmZ5CQoYHqsNMnoojT2apqWGO4q82Abz6aVV0Ls/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=6)

安装 Claude Code 很简单，先确保电脑里有 Node.js 环境，没有的话去 Node 官网 下个傻瓜式安装包。然后一行命令搞定：

```
npm install -g @anthropic-ai/claude-code
```

装好后在终端输入 `claude` 就能进入对话界面，首次使用需要登录。但很多同学没有 Anthropic 的官方账号，登录这步就卡住了，根本没法直接用。

别急，下面就用 CC Switch 把它切换成 DeepSeek。

### 用 CC Switch 切换模型

打开 CC Switch，在顶部应用栏选择 **Claude** ，然后点击「添加供应商」：

![图片](https://mmbiz.qpic.cn/mmbiz_png/LlSQOKIxJ1GjZIQNheQSTXGB5toHWLL2ftDhGU0R3gTyy4AyQ7mQLrticicgb6zkXdPpo931kCwpHIxvibiaJ8Xem4fyu20bTqX3u2B4VWKo9Hc/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=7)

在预设的模型供应商列表里选择 **DeepSeek** ：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/LlSQOKIxJ1FS8G5E6tspDpPvQWjHNQI9vQnG8ibrWEOMPGKWtU8ibGkwpTswrWkia4GuYibIvfmOEbWqzBgIicGic9YQqgSsBKic9eoibyCRNrC55qQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=8)

接下来填写刚才在 DeepSeek 开放平台创建好的 API Key：

![图片](https://mmbiz.qpic.cn/mmbiz_png/LlSQOKIxJ1G5Z0bAvWAGOuUNic3eJEtLauQdJWxRxboZSXj87mzuxa7HoZWRYvlE5GRIpdbIMCONs1vmloPqroCwPCaFPxzht7ZVgWXiaZhOg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=9)

其余字段基本不用动，CC Switch 的 DeepSeek 预设已经帮你把模型都配好了，里面内置了 DeepSeek-V4-Pro 和 DeepSeek-V4-Flash 两个版本，主模型默认就是 Pro，相比 Flash，它的 Agent 能力和复杂推理更强，更适合写代码。

如果你想用上 DeepSeek V4 的百万 tokens 超长上下文，还能在这里直接勾选开启 1M 模式，告诉 Claude Code 这个模型能吃下这么长的上下文，不用自己去改配置。

![图片](https://mmbiz.qpic.cn/mmbiz_png/LlSQOKIxJ1GjDNw6HaybqPzaDiaNggHnaHZFm2u8FjNqrrIUDVJPsRicmdldwujygN5jf0OugaOcX2EvvRh8wibtFYJ7oOXIFmhm5B223WdPibA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=10)

填完点右下角的「添加」按钮。这里能看到 Claude Code 的 JSON 配置文件，CC Switch 干的活儿就是帮你可视化地修改它，省去手动编辑的麻烦。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/LlSQOKIxJ1GEbKKp2iadeicGO75svbRick0Vw2hOia8N2yIqRE1icuYpAn8EIAlNKatK0kuNr5eWI4AYbO0juqgnxj3xE7eQYEep7Bu2cbnBxUz8/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=11)

最后点击启用 DeepSeek 模型：

![图片](https://mmbiz.qpic.cn/mmbiz_png/LlSQOKIxJ1FC58JnuGuBCkRBxbFMnE3OYckMwWiayWSkjTVn6e5MyLVYjpJs4NgiaMib1chK7ttR8S35NPGEeo0yPYicEWKQJkfNxRC99b5y1pE/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=12)

重新进入 Claude Code，左上角能看到当前用的模型。我们让它自报家门，输入一句：你是什么模型？

AI 能正常给出回复，就说明切换成功了：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/LlSQOKIxJ1FnErBN1xA1sGI7Qs5OnHWe23xhxmZsvIM9jhdFYXs1jPJeGP0prQYZsyvicqzxhRJOdZx6AApjiacEqqjNdwibtsM4UQ9tk3kD3A/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=13)

你可能会发现，用 CC Switch 给 Claude Code 接 DeepSeek，整套流程格外简单。这是因为 DeepSeek 提供了兼容 Anthropic 协议的接口，而 Claude Code 本身就是按这个协议来通信的，CC Switch 直接把配置写进 `settings.json` 就能用了。

## 二、把 DeepSeek 接到 Codex

搞定 Claude Code，我们再来看 Codex。它是 OpenAI 推出的 AI 编程工具，最近的热度堪称炸裂。

![图片](https://mmbiz.qpic.cn/mmbiz_png/LlSQOKIxJ1FWibXfsqusktFvLJ3rkf515UM97sFXrhb4TLL3oVp1jAsZlzXYUSTw8BLfRvjhdxJMU8bZXicibM02FDJcne9Rke4T3nZjDsP2D8/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=14)

Codex 有两种形态，一种是在终端里跑的命令行版 Codex CLI，一种是带图形界面的桌面 APP。

命令行版的安装方式跟 Claude Code 类似，一行命令就能搞定：

```
npm install -g @openai/codex
```

装好后在终端输入 `codex` 就能进入对话界面，首次使用同样需要登录 OpenAI 账号。没有账号的话，就要用 CC Switch 来切换模型。

![图片](https://mmbiz.qpic.cn/mmbiz_png/LlSQOKIxJ1Emb2AuAPcgR4ibJQFu53l7AmLz0oU0dQcz0OqkxS1uLSb5krZE1Gicze1bXVTgcsX0ic8aHtuqve89V2MSSVv6YXWFJSumn4p574/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=15)


### Codex 接入原理

既然 Codex 的配置都放在 `~/.codex/config.toml` 这个文件里，那我直接把里面的接口地址 `base_url` 改成 DeepSeek 的不就行了？

能想到这说明你很聪明，但是这么干大概率会翻车，直接给你报一个 404 错误。

问题出在协议上。Codex 用的是 OpenAI 的 **Responses API** ，而 DeepSeek 这些国内模型走的是 **Chat Completions API** ，这俩压根儿不是一套东西。就好比你打电话，号码是拨通了，可你说中文、对方只懂法语，照样聊不到一块儿去。

![图片](https://mmbiz.qpic.cn/mmbiz_png/LlSQOKIxJ1Gpwx6RFxrI5pmm3ltiaaFcgt2ChRZiaJqCmKvMgJcqPXrnUI8deSa5CSzPNrMgqhRFUgqDg2aPZ8c7GoNEibsibJw5dhCjxKjxCoA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=17)

所以 Codex 接国内模型，关键在于中间得有个「翻译」，把 Codex 发出的请求转换成 DeepSeek 能听懂的格式。

好在 CC Switch 已经把这件事给我们办妥了。它的「本地路由」功能会在你电脑上起一个轻量级的代理服务，请求的流转过程是这样的：

```
Codex → CC Switch → DeepSeek → CC Switch → Codex
```

整个转发对 Codex 完全透明，它自己还以为在访问 OpenAI 官方接口呢，实际上请求已经被 CC Switch 偷偷转给 DeepSeek 了。这样既保留了 Codex 的原汁原味体验，又能用上便宜的国内模型，岂不美哉？

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/LlSQOKIxJ1FAveG3zMNHpJXckgnUzvpGCiazJKlwiamZR3iamrxnBccJL3Q4ksgOoPicEbmsgmkVNDwnb0LicZ8A6s2uIQmrn2seQ9rJRg6CUN74/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=18)

### 用 CC Switch 配置

打开 CC Switch，在顶部应用栏切换到 **Codex** ，点击「添加供应商」：

![图片](https://mmbiz.qpic.cn/mmbiz_png/LlSQOKIxJ1HgttNPFN6ZgPAxRWGtqVa5hAz5IX7I22Vyw4hjXiaoHicmibcmBJhhTw3pThrJtzYJmKRxTtuRFzwKEykZ24ic1icnvFdOWU4e8Xrw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=19)

在预设里搜索并选择 **DeepSeek** ，跟前面给 Claude Code 配置时的步骤一样：

![图片](https://mmbiz.qpic.cn/mmbiz_png/LlSQOKIxJ1GRBFHib7bqkOb9j4qpyh6yxr9qxQgcOOaTR2YF19ewvn1wR5RAfOoea16D3iaPNCZ9UTR3jeVJJuCYicrUmN6YRTxqZfLWc31ibLo/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=20)

填入你的 DeepSeek API Key，其余字段保持默认：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/LlSQOKIxJ1EbMC1kkAUhze35OI4j1BshROzBDJvibBjm1bn9DkXfTPWrARdFW5TGYZt3KQKPE64KTCDnJwLn2g7X0QhgCaT9LVAI4ibibq3GCs/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=21)

跟 Claude Code 一样，模型这些 CC Switch 都已经预设好了，其他字段不用动。

要特别注意的是，这一步的关键是得 **开启「本地路由映射」** ，然后点右下角的「添加」按钮保存就好。

![图片](https://mmbiz.qpic.cn/mmbiz_png/LlSQOKIxJ1FTCP5HwibgyomPiaPqQ6TMhec9jciaq2GeXhOMQYFsSqEBYrdp84d5R5GBpp7ymCnoGg9b6L2PxbG7e93Sle6kySHUINsOr2Ejo8/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=22)

回到主页后，选择启用 DeepSeek：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/LlSQOKIxJ1HYCHoibnIibDDaWdShib7vjWYWoLutX80RsuHgfWe8bs6HibicklVK64uwHDurEcjjMCRctXsstiboORPSd08PQHMONGFEPSqA2iaCDU/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=23)

但是到目前为止，我们还不能在 Codex 中正常使用 DeepSeek，对话会直接报前面说的 404 错误：

![图片](https://mmbiz.qpic.cn/mmbiz_png/LlSQOKIxJ1FvoRKhGneJiaQYRHIT59JzjJPu1OjuUo40P1PKAVoQcSzdXRziazgPWfj4YetRJ0YFT6ClPvT7bBrFo3dibe30NI5Lp22sh6b6Rk/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=24)

### 开启本地路由

切换到 DeepSeek 后，系统会提示你开启路由。点击左上角的「设置」按钮进入设置页面：

![图片](https://mmbiz.qpic.cn/mmbiz_png/LlSQOKIxJ1FVicaNN2ABHHXfIryAdghSChuLynxl0klRxLCaQa799gKVibSrAqNnRgrcrDVWiaRvjBColvQal0VOywhvtQiagmQwsvKGTgsajJg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=25)

找到路由设置菜单，把本地路由的「路由总开关」打开，然后选择启用 Codex 路由：

![图片](https://mmbiz.qpic.cn/mmbiz_png/LlSQOKIxJ1FbtMPCu0MSicEYFjDBtBtl7XUiblIYZfGo8kH1cUL9ic0gmanwAA7xQArA9b2xtqIWHIFo8y0mRovMGLeia8TQqynZmHN76PEDHW0/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=26)

这一步就是让 CC Switch 的本地代理正式接管 Codex 的请求，前面说的协议转换全靠它。

大功告成！

重新打开 Codex CLI，就能看到已经切换为 DeepSeek 模型了。同样让它自报家门，能正常对话就说明切换成功了：

![图片](https://mmbiz.qpic.cn/mmbiz_png/LlSQOKIxJ1Fjib8mSUYY1d3bCT499picpGKpQ5agQIocZ3lueTCpFDTb88NrzB7ZNReUJkfKwSBicia6XpeAujUzD8GdiapBnfQXBSMLYmRCTHR8/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=27)

你会发现，AI 嘴上还说自己是基于 GPT-5 的 Codex。这是因为 Codex 会给模型注入一套自己的系统提示词，让它默认以为自己是官方模型，但实际干活的底层已经换成 DeepSeek 了。

再来试试 Codex 桌面 APP。因为它和命令行版共用 `~/.codex` 这套配置，CC Switch 切换之后直接打开就能用，同样问它是什么模型，底层跑的也是 DeepSeek：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/LlSQOKIxJ1EYFAOq6LkymLuqPvmhqq3Ffo455YdsrcLer08w9pDprLzp49o6WLzf3Xnvib2DibEFT8PFO46yTibMFLKdZbIMEEUNQDEzh3icCF0/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=28)

如果想改回来，反向操作即可，把路由关掉、再启用默认配置就行：

![图片](https://mmbiz.qpic.cn/mmbiz_png/LlSQOKIxJ1G7tkhNaqpWplTpaOSVgwYVrdia2pjS3bq42PI83b6Twia8ibF6Wvb8CuiaWWxEzA6laGJichmdibbmAb4WTicQheq27GTN8G8iaSvVFME/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=29)

怎么样，是不是比想象中简单多了？

可能你已经买了其他平台的 Coding Plan，比如智谱 GLM、Qwen、Kimi、MiniMax 等等。

想对接别家大模型的话，操作几乎一模一样。在 CC Switch 里选对应的供应商预设（没有预设就自定义一个），把人家给你的专属 Base URL 和 API Key 填进去就行。对接 Claude Code 能直接生效，对接 Codex 时记得同样开启本地路由。


看到这里你会发现，现在学 AI 编程的门槛真的低到不能再低了。

一个开源工具 + 国内大模型，几分钟就能把官方收费、还要魔法的 Claude Code、Codex 跑起来，成本可能就是官方的零头。说真的，工具和模型都不该成为你学习路上的拦路虎。

