---
title: 记录idea使用的插件
categories:
  - 后端
tags:
  - IntellJ IDEA
author: geepair
date: 2020-04-16 20:49:00
---

记录一些idea中自用的插件，以防止丢失。
<!-- more -->

# idea插件
* .ignore
* Alibaba Java Coding Guidelin
* CodeMaker
* Free MyBatis Plugin
* GenerateAllSetter
* GenerateSerialVersionUID
* Grep Console
* GsonFormat
* HighlightBracketPair
* Key Promoter X
* Lombok
* Maven Helper
* POJO to Json
* Rainbow Brackets
* RestfulToolkit
* SequenceDiagram
* String Manipulation
* Translation

## 介绍

#### .ignore
可以自定义.gitignore文件模板，项目右键就可以直接生成.gitignore文件，自定义过滤文件。

#### Alibaba Java Coding Guidelin
《阿里巴巴Java开发规约》，将不符合规约的代码按Blocker/Critical/Major三个等级显示在下方，甚至在IDEA上，我们还基于Inspection机制提供了实时检测功能，编写代码的同时也能快速发现问题所在。

![pasted-11](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106153918.png)

#### CodeMaker
通过 Velocity 支持自定义代码模板来生成代码。
* 支持增加自定义代码模板（Velocity）
* 支持选择多个类作为代码模板的上下文

#### Free MyBatis Plugin
可在mapper和对应的xml之间跳转

其他功能：
* 支持生成语句，@Param注释和xml的映射器
* 在xml中支持一些有用的mapper重命名
* 支持mapper xml中select语句的正确结果类型
* 支持mapper xml的正确无法解析的属性值
* 支持在重命名mapper接口时重构mapper xml文件的名称
* 支持mapper xml中基于id的标记的重构
* 支持查找映射器接口和映射器xml元素的用法
* 突出显示mapper xml的冲突元素为错误
* 自动注册映射器为spring bean
* 在编辑sql时，Mapper参数在xml中自动完成
* ＃{} yourParameter
* 基于@Param注解
* 协会支持

生成 （alt+enter）

#### GenerateAllSetter
可以快速针对已有的model实体对象的属性生产set代码

#### GenerateSerialVersionUID
自动生成序列化的ID

#### Grep Console
通过expression表达式过滤日志、给不同级别的日志或者给不同pattern的日志加上背景颜色与上层颜色。

![pasted-12](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106153939.png)

#### GsonFormat
弹出一个输入框，将想要转换成实体类的json粘贴到输入框内，点击ok，GsonFormat就会自动为你生成对应的实体类。

#### HighlightBracketPair
括号匹配高亮

#### Key Promoter X
键盘快捷方式
折叠Project，会自动提示

![pasted-13](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106153950.png)

#### Lombok
要引入相关的 Lombok jar包，然后开启idea注解在编译阶段起作用
@Data
@Getter
@Setter
...

![pasted-14](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106153959.png)

#### Maven Helper
帮助解决Maven依赖冲突

#### POJO to Json
实体类转换成JSON字符串
#### Rainbow Brackets
匹配的彩虹括号

#### RestfulToolkit
* 根据 URL 直接跳转到对应的方法定义
* 提供了一个 Services tree 的显示窗口
* 一个简单的 http 请求工具
* 在请求方法上添加了有用功能: 复制生成 URL;,复制方法参数...
* 其他功能: java 类上添加Convert to JSON 功能，格式化 json 数据

#### SequenceDiagram
自动生成类的时序图，当类中逻辑比较复杂时，可以比较直观的查看
#### String Manipulation
字符串工具，很多功能
#### Translation
嵌入的翻译功能

----

补充：
- idea右下角显示内存使用

![pasted-15](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106154022.png)

- 类似eclipse的自动构建 不用每次修改后都要等待一段时间make

![pasted-16](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106154031.png)

- 自动导包，不用每次去点击提示消息之后导入

![pasted-17](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106154052.png)