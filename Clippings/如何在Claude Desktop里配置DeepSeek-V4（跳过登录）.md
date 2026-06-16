---
title: 如何在Claude Desktop里配置DeepSeek-V4
source: https://www.aixq.cc/25214.html
author:
published: 2026-04-29
created: 2026-05-18
description: 我猜你肯定会用CC Switch来给Claude Code CLI配置各种不同模型了，那Claude Desktop你会么？ 如果答案是NO，那请按照下面的操作步骤，5分钟就能让Claude Desk…
tags:
  - clippings
tool: Claude
---
我猜你肯定会用CC Switch来给Claude Code CLI配置各种不同模型了，那[Claude Desktop]()你会么？

如果答案是NO，那请按照下面的操作步骤，5分钟就能让Claude Desktop用上DeepSeek-V4啦～

![image](https://tu.aixq.cc/wp-content/uploads/2026/04/20260429074513516.jpg!ys)

1.

在Claude官网下载Claude Desktop：https://code.claude.com/docs/en/desktop-quickstart

![image](https://tu.aixq.cc/wp-content/uploads/2026/04/20260429074514100.jpg!ys)

2.

安装（我是Mac版本，安装过程就不啰嗦了）

![image](https://tu.aixq.cc/wp-content/uploads/2026/04/20260429074515152.jpg!ys)

3.

启动Claude Desktop：Get Started

![image](https://tu.aixq.cc/wp-content/uploads/2026/04/20260429074516721.jpg!ys)

4.

登录界面：不用登录，直接进下一步

![image](https://tu.aixq.cc/wp-content/uploads/2026/04/20260429074517783.jpg!ys)

5.

找到Help–>Troubleshooting–>Enable Developer Mode（打开开发者模式）

![image](https://tu.aixq.cc/wp-content/uploads/2026/04/20260429074518804.jpg!ys)

6.

点击“Enable”，之后Claude Desktop会重启

![image](https://tu.aixq.cc/wp-content/uploads/2026/04/20260429074520362.jpg!ys)

7.

重启之后，会在菜单栏多出一个Developer的入口，选择【Configure Third-Party Inference】

![image](https://tu.aixq.cc/wp-content/uploads/2026/04/20260429074521192.jpg!ys)

在这儿页面做如下配置： 1.

Connect模式选择【Gateway】

2.

Gateway base URL填：https://api.deepseek.com/anthropic

3.

Gateway API Key填写从DeepSeek官方控制台（https://platform.deepseek.com/api\_keys）创建的API KEY

> 当然，你也可以使用KIMI、MiniMax、GLM、MiMo等模型，你只要找到对应的兼容Anthropic的Base URL，然后把API KEY贴进去就行了

![image](https://tu.aixq.cc/wp-content/uploads/2026/04/20260429074522301.jpg!ys)

9.

在之前的窗口往下划，添加两个模型【deepseek-v4-pro】和【deepseek-v4-flash】：

> 由于DeepSeek-V4支持百万上下文，记得打开【Offer 1M-Context variant】选项

![image](https://tu.aixq.cc/wp-content/uploads/2026/04/20260429074523571.jpg!ys)

10.

![image](https://tu.aixq.cc/wp-content/uploads/2026/04/20260429074524136.jpg!ys)

11.

点击【Relaunch Now】，重启应用：

![image](https://tu.aixq.cc/wp-content/uploads/2026/04/20260429074525949.jpg!ys)

![image](https://tu.aixq.cc/wp-content/uploads/2026/04/20260429074526606.jpg!ys)

12.

Claude Desktop重启之后，你应该可以进入程序主界面了：

> 顺便吐个槽，Claude Desktop和Codex Desktop，你们俩是不是越长越像了？！果然是天下文章一大抄！

![image](https://tu.aixq.cc/wp-content/uploads/2026/04/20260429074527217.jpg!ys)

13.

目前Claude Desktop默认是在【Cowork】状态，如果要写Code，可以点击左上角切换到【Code】模式：

![image](https://tu.aixq.cc/wp-content/uploads/2026/04/20260429074528948.jpg!ys)

14.

这个时候，你应该可以在右下角的模型区看到4个模型：

> 如果是写Code，建议选择DeepSeek-V4-Pro 1M版本

![image](https://tu.aixq.cc/wp-content/uploads/2026/04/20260429074529205.jpg!ys)

15.

选择一个项目（上图中的【Select Folder】），然后随便输入点什么，看API KEY是不是可以正常工作了：

![image](https://tu.aixq.cc/wp-content/uploads/2026/04/20260429074530518.jpg!ys)

恭喜！

你现在可以在Claude Desktop上使用DeepSeek-V4系列大模型了！

此外，全自动运行模式需要手动在Settings里打开：

![image](https://tu.aixq.cc/wp-content/uploads/2026/04/20260429074532694.jpg!ys)

然后再在主界面选择【Bypass permissions】，当然，全自动有风险，操作需谨慎：

![image](https://tu.aixq.cc/wp-content/uploads/2026/04/20260429074533903.jpg!ys)

Have fun～

CC Switch地址：https://ccswitch.io/zh/