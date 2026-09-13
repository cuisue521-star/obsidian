`steps: 
- action: goto 
  value: https://www.baidu.com 
- action: aiInput 
   value: Midscene.js desc: 百度首页搜索输入框 
- action: aiTap desc: 蓝色百度一下按钮 
- action: aiWaitFor desc: 搜索结果列表展示出来 
- action: aiAssert desc: 页面存在 Midscene github 相关结果`