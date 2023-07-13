# 使用教程

1. 引入依赖

```
<dependency>
  <groupId>ai.stardust</groupId>
  <artifactId>dingbot-spring-boot-starter</artifactId>
  <version>1.0.0-SNAPSHOT</version>
</dependency>
```

2. 配置机器人属性

首先在报警群中添加自定义机器人，并配置安全设置，保存好webhook 与密钥。在yaml文件中绑定配置信息。DingBot 提供两个属性配置类，分别绑定机器人配置和负责人信息。

![](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/v1GXn461mAAbODQ4/img/4258a43b-f9ef-458c-86b3-f38f5c68a142.png)![](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/v1GXn461mAAbODQ4/img/3c2615a4-b552-4064-b973-2698fea4beef.png)

```
dingtalk:
  robots:
    robotMappings:
      {机器人key1}:
        webhook: {机器人webhook1}
        secret: {安全设置中的密钥1}
      {机器人key2}:
        webhook: {机器人webhook2}
        secret: {安全设置中的密钥2}
   responsible:
    modulesMapping:
      {模块1}: [负责人1id，负责人2id]
      {模块2}: [负责人1id，负责人2id]
```

3. 注入DingTalkSender类，调用方法

```
@Resource
private DingTalkSender dingTalkSender;
```

# 实现细节

1. 消息模版

目前支持四种消息模版，分别为Text 、Markdown 、LinkTemplate 与ActionCard。以及四个模版的抽象父类，抽象父类中包含robotKey 与at 成员信息，并包含一个将对象转化为json字符串的方法。

2. MarkdownBuilder

Markdown构造器，支持链式构造Markdown格式的内容，支持添加标题，引用，链接，图片，表格。

```
// MarkdownBuilder example1
String markdownText = MarkdownBuilder.Writer().title("一级标题", 1)
                .title("上面是一级标题，下面是二级标题。", 0)
                .title("二级标题", 2)
                .bold("这是一段加粗的文本")
                .cite("这是一段引用文本").write().toString();
// MarkdownBuilder example2
MarkdownBuilder.Writer resumeMessage = MarkdownBuilder.Writer();
resumeMessage.tableHeader("**算法ID**", "**算法名称**", "**算法地址**");
resumeMessage.tableBody("1", "gcn", "url1");
resumeMessage.tableBody("2", "fcn", "url2");
resumeMessage.tableBody("3", "cnn", "url3");
String markdownText = resumeMessage.write().toString();
```

3. DingTalkSender

DingTalkSender 对外提供使用机器人发送消息的多个方法。

|     |     |
| --- | --- |
| 方法  | 描述  |
| send(AbstractDingTalkTemplate message) |     |
| sendMarkdown(DingTalkMarkdownTemplate message) |     |
| sendMarkdown(String text, String title, String robotKey, boolean isAtAll) |     |
| sendMarkdown(String text, String title, String robotKey, List<String> ats) |     |
| sendText(String content, String robotKey, boolean isAtAll) |     |
| sendText(String content, String robotKey, List<String> ats) |     |
| sendLink(String text, String title, String messageUrl, String picUrl, String robotKey, boolean isAtAll) |     |
| sendLink(String text, String title, String messageUrl, String picUrl, String robotKey, List<String> ats) |     |
