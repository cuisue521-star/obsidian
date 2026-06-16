---
title: "JMeter接口关联【正则表达式提取器】"
source: "https://zhuanlan.zhihu.com/p/541013011"
author:
  - "[[软件测试开发区公众号：软件测试开发区]]"
published:
created: 2026-04-17
description: "1、正则表达式提取器介绍如果有这样的情况：一个完整的操作流程，需要先完成某个操作，获得某个值或数据信息，然后才能进行下一步的操作，也就是常说的接口关联，将上一个请求的响应结果作为下一个请求的参数。 在…"
tags:
  - "clippings"
---
[收录于 · 软件测试](https://www.zhihu.com/column/c_1527668402965852160)

**1、 [正则表达式](https://zhida.zhihu.com/search?content_id=208358223&content_type=Article&match_order=1&q=%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NzY1OTI3NTQsInEiOiLmraPliJnooajovr7lvI8iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyMDgzNTgyMjMsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.o8LWjeyeJQc4G5DoK0OYtboaUGye1mhqk5zqE_TjMVQ&zhida_source=entity) 提取器介绍**

如果有这样的情况：一个完整的操作流程，需要先完成某个操作，获得某个值或数据信息，然后才能进行下一步的操作，也就是常说的接口关联，将上一个请求的响应结果作为下一个请求的参数。

在JMeter中，可以利用 **正则表达式提取器** 来帮助我们完成这一动作。

**2、正则表达式提取器界面详解**

添加 **正则表达式提取器** 组件操作：选中“ [取样器](https://zhida.zhihu.com/search?content_id=208358223&content_type=Article&match_order=1&q=%E5%8F%96%E6%A0%B7%E5%99%A8&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NzY1OTI3NTQsInEiOiLlj5bmoLflmagiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyMDgzNTgyMjMsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.HxtJcb6ulDLuLOIgYBiWLw9bhjWby831IbG26lBNEq0&zhida_source=entity) ”右键—>添加—>后置处理器—>正则表达式提取器。

界面如下图所示：

![](https://pic2.zhimg.com/v2-e1be14f13b8528064734f4cb3fba9f0f_1440w.jpg)

下面是 **正则表达式提取器** 组件的详细说明：

- **名称** ： **正则表达式提取器** 组件的自定义名称，见名知意最好。
- **注释** ：即添加一些备注信息，对该 **正则表达式提取器** 组件的简短说明，以便后期回顾时查看。

**（1）Apply to：作用范围（返回内容的取值范围）**

- Main sample and sub-samples：作用于父节点的取样器及对应子节点的取样器。
- Main sample only：仅作用于父节点的取样器。
- Sub-samples only：仅作用于子节点的取样器。
- JMeter Variable Name to use：作用于JMeter变量(输入框内可输入JMeter的变量名称)，从指定变量中提取需要的值。

**（2）Field to check：要检查的响应字段**

1. 主体：响应报文的主体，最常用。
2. Body(unescaped)：是替换了所有的HTML转义符的响应主体内容，注意HTML转义符处理时不考虑上下文，因此可能有不正确的转换，不太建议使用。（即：替换了转义码的Body部分）
3. Body as a Document：从不同类型的文件中提取文本，注意这个选项比较影响性能。
4. Response Headers：从响应头信息中提取数据，如果你使用中文语言的JMeter，会看到这一项是信息头，这是中文翻译问题，应以英文为准。
5. Request Headers：从请求头信息中提取数据。
6. URL：从请求URL中提取数据。
7. 响应码（Response Code）：提取响应状态码，比如：200、404等。
8. 响应信息（Response Message）：响应信息中提取数据。

**（3）第三部分内容**

- 引用名称（Name of created variable）：定义提取值的引用变量名称。
- 正则表达式（Regular Expression）：用于提取值的正则表达式。
- 模板（Template ($i$ where iis capturing group number, starts at 1)）：正则表达式的提取模式。如果正则表达式有多个提取结果，则结果是数组形式，模板为$1$，$2$，表示把解析到的第几个值赋给变量,从1开始匹配，以此类推。若只有一个结果，则只能是$1$。
- 匹配数字（0代表随机）：正则表达式匹配数据的结果可以看做一个数组，表示如何取值：0代表随机取值，正数n则表示取第n个值，比如1代表第一个，2代表第二个，以此类推。负数则表示提取所有符合条件的值，如-1。
- 缺省值（Default Value）：匹配失败时候的默认值；通常用于后续的逻辑判断，一般通常为特定含义的英文大写组合，比如：ERROR等。
- Use empty defau't value：使用空值为默认值。

**3、正则表达式提取器的使用**

**需求：**

1. 访问网易官网，获取title值。
2. 将title值放入百度搜索框，进行搜索。

**（1）测试计划内包含的元件**

**添加元件操作步骤** ：

1. 创建测试计划。
2. 创建线程组：选中“测试计划”右键 —> 添加 —> 线程（用户） —> 线程组。
3. 在线程组下，添加取样器“HTTP请求”组件：选中“线程组”右键 —> 添加 —> 取样器 —> HTTP请求。
4. 在取样器下，添加后置处理器“正则表达式提取器”组件：选中“取样器”右键 —> 添加 —> 后置处理器 —> 正则表达式提取器。
5. 在线程组下，添加监听器“察看结果树”组件：选中“线程组”右键 —> 添加 —> 监听器 —> 察看结果树。

*提示：需要重复添加的组件这里不重复描述。* 最终测试计划中的元件如下：

![](https://pic1.zhimg.com/v2-05e44fe83efad04399913ac1a91447f6_1440w.jpg)

点击运行按钮，会提示你先保存该脚本，脚本保存完成后会直接自动运行该脚本。

**（2）网易首页请求界面内容**

非常简单的Get请求，之前说了很多次了，这里就不做解释了。界面内容如下图所示：

![](https://pic3.zhimg.com/v2-ae2337b1a36d4c09bfa963dea29589a6_1440w.jpg)

**（3）正则表达式提取器界面内容**

我们在编辑 **正则表达式提取器** 组件之前，一般先请求一下需要提取返回数据的接口。

因为我们需要先查看一下，需要提取的数据在什么位置，如下图所示：

![](https://pic1.zhimg.com/v2-b3cb9d7e4d323acda4212afac306daa0_1440w.jpg)

然后选择RegExp Tester视图模式，先手动编写正则表达式，看看是否能够取到需要的数据。

如下图所示：

![](https://pic2.zhimg.com/v2-3e11376b33738add6c048f1bf91ac879_1440w.jpg)

之后我们就可以编写 **正则表达式提取器** 组件界面了，如下：

编写引用名称、正则表达式、选择第几个模版，匹配数据选择。

![](https://pic4.zhimg.com/v2-244dd1674175d5bff6c061c7b8c1ad45_1440w.jpg)

**正则表达式提取器** 组件提取出来的数据，会存储在线程变量中，供其他后续接口使用。

**（4）百度首页请求界面内容**

填写接口的基本请求信息，然后把 **正则表达式提取器** 提取出来的数据，作为 [参数化](https://zhida.zhihu.com/search?content_id=208358223&content_type=Article&match_order=1&q=%E5%8F%82%E6%95%B0%E5%8C%96&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NzY1OTI3NTQsInEiOiLlj4LmlbDljJYiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyMDgzNTgyMjMsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.WLTMOTnsDDVcpAf9jFLdDJOVItkj5H54EsgEbxvT418&zhida_source=entity) 变量应用到请求中。

如下图所示：

![](https://pic3.zhimg.com/v2-a466bb48501756f1a121e4599894f0d6_1440w.jpg)

**（5）查看结果**

我们可以看到在第二个请求中，拿到了第一个请求提取出来的数据“网易”。如下图所示：

![](https://picx.zhimg.com/v2-b5c9aff9ce0a3b0a7bbec13a6ea22a81_1440w.jpg)

*提示：可以添加Debug PostProcessor（调试后置处理器），或者Debug Sampler（调试取样器），来查看 **正则表达式提取器** 中，提取出的内容是否正确。*

*注意：正常跑用例时删除或禁用它们。*

**4、总结**

**正则表达式提取器** 可以用于对任何文本的提取。提取完参数后，相当于把参数以key-value的形式放到参数池，以便后面的请求使用。

***注意：不能超前引用。***

**正则表达式提取器** 和 **XPath提取器** 的区别：

1. 正则表达式提取器可以用于对页面任何文本的提取，提取的内容是根据正则表达式在页面内容中进行文本匹配；
2. XPath提取器则可以提取返回页面任意元素的任意属性；
3. 如果需要提取的文本是页面上某元素的属性值，建议使用XPath Extractor;
4. 如果需要提取的文本在页面上的位置不固定，或者不是元素的属性，建议使用正则表达式提取器。

**5、正则表达式简单说明**

**正则表达式（Regular Expression）** ：使用正则表达式解析响应结果，（） **表示提取字符串中的部分值** ，请不要使用||，除非你本身需要匹配这个字符。

1. .：代表匹配任意一个字符。
2. \[\]：表示取值范围。比如：\[0-9\]代表匹配0-9之间任意一个数字。\[a-z\]代表匹配a-z之间任意一个字符。\[A-Z\]代表匹配A-Z之间任意的一个字符。
3. +：匹配前面的子表达式一次或多次。
4. ?：代表匹配一次或一次也没有。这个符号还有特殊的用法，当放到\*号后面的时候，标识取到的数据是非贪婪的。说明：贪婪与非贪婪模式是两种不同的表达式匹配行为，贪婪模式在整个表达式匹配成功的前提下，尽可能多的匹配，而非贪婪模式在整个表达式匹配成功的前提下，尽可能少的匹配，即匹配第一个。
5. \*：匹配前面的子表达式零次或多次。
6. \\d：匹配一个数字字符，等价于\[0-9\]。
7. \\w：匹配包括下划线的任何单词字符，等价于\[A-Za-z0-9\_\]。
8. {3}：代表匹配3次，示例如下：\[1-9\]\[0-9\]{4,14}：代表前面的子表达式，至少匹配4次，最多不超过14次。1\[358\]\\d{9}：匹配手机号。\[a-zA-Z0-9\_\]+@\[a-zA-Z0-9\]+\\.\[a-zA-Z\]+：匹配邮箱。

***提示：常用正则表达式查询： [tool.oschina.net/upload](https://link.zhihu.com/?target=http%3A//tool.oschina.net/uploads/apidocs/jquery/regexp.html)***

---

欢迎关注我：  
[@软件测试开发区](https://www.zhihu.com/people/6601de30063273e418342e69d6f4e49d)  
持续分享软件测试干货！！！

发布于 2022-07-13 11:24[正则表达式](https://www.zhihu.com/topic/19577832)[jmeter](https://www.zhihu.com/topic/19804390)[软件测试](https://www.zhihu.com/topic/19562409)