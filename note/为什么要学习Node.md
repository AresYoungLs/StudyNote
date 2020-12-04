# 为什么我们要学习Node
1. 前端的高级内容基本上都是基于node.js来开发的，也就是说node是他们实现的一个部分
2. 真实项目开发中
    - node将作为中间层
    - 类似：桥梁的作用，承接客户端和服务端
3. 扩展javascript的能力
    - 文件操作能力
    - 数据库操作能力
    - dns解析能力
    - os操作系统操作能力
# 什么是Node
- 真假判断
    1. Node是一个后端语言  <font color="#error">False</font>
        Node不是一个后端语言，但是可以做类似后端语言的功能
    2. Node是使用谷歌V8引擎  <font color="#green">True</font>
    3. Node是javascript的一个运行环境  <font color="#green">True</font>
    4. Node具有非阻塞I/O特点  <font color="#green">True</font>
    5. Node采用了Common.js规范  <font color="#green">True</font>
# 关键字解释
1. Google V8 Chrome V8引擎  
    谷歌浏览器的内核，是帮助浏览器解析javascript代码为机器代码（0101）的一种方式
2. js的一个运行环境  
    Node提供了一个可以执行javascript的环境
3. 非阻塞I/O  
    - I：Input   输入  
    - O：Output  输出
    - 非阻塞：有序的进入有序的离开，javascript是单线程，异步执行  
        当多个任务要执行时，所有的任务会按顺序执行  
        主线程任务  
        异步队列任务
    - 阻塞  
        同步执行
4. Common.js规范
     - require
     - module.exports
# 为什么要使用模块化