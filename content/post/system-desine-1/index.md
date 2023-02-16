---
title: "系统设计（一）" # Title of the blog post.
date: 2023-02-16T10:44:02+08:00 # Date of post creation.
description: "系统分析师笔记-系统设计（一）" # Description used for search engine.
featured: false # Sets if post is a featured post, making appear on the home page side bar.
draft: false # Sets whether to render this page. Draft of true will not be rendered.
toc: false # Controls if a table of contents should be generated for first-level links automatically.
# menu: main
usePageBundles: true # Set to true to group assets like images in the same folder as this post.
# featureImage: "/images/path/file.jpg" # Sets featured image on blog post.
# featureImageAlt: 'Description of image' # Alternative text for featured image.
# featureImageCap: 'This is the featured image.' # Caption (optional).
# thumbnail: "/images/path/thumbnail.png" # Sets thumbnail image appearing inside card on homepage.
# shareImage: "/images/path/share.png" # Designate a separate image for social media sharing.
codeMaxLines: 10 # Override global value for how many lines within a code block before auto-collapsing.
codeLineNumbers: false # Override global value for showing of line numbers within code block.
figurePositionShow: true # Override global value for showing the figure label.
categories:
  - 系统分析师
tags:
  - 系统设计
# comment: false # Disable comment if false.
---

#### 1、架构设计

目的：需求分配，了解需求，将满足职责的需求分配到组件上

#### 2、架构风格：
|风格|特点|
|-|-|
|数据流风格|批处理，管道-过滤器
|调用/返回风格|主程序/子程序，面向对象，层次结构
|独立构件风格|进程通信，事件驱动系统
|虚拟机风格|解释器，规则系统
|仓库风格|数据库系统，黑板系统，超文本系统


#### 3、基于服务的架构（SOA）

优点：标准化，更利于互联互通

##### SOA实现方式：


![Web Service实现](images/webservice.jpg)

![ESB实现：服务请求着与服务器提供者之间解藕](images/esb.jpg)


#### 4.微服务

特点：
- 小、且专注于做一件事情
- 轻量级的通信机制
- 松耦合、独立部署

区别：
|单块架构|微服务架构|
| - | - |
|紧耦合|松耦合|
|所有功能在一个进程中|功能在不同的微服务进程中|

优势：
- 技术异构性。不同微服务使用不同的技术
- 弹性。
- 扩展。
- 简化部署。
- 与组织结构相匹配。
- 可组合性。
- 对可替代性的优化。

挑战：
- 分布式系统的复杂性
- 运维成本
- 部署自动化
- DevOps与组织结构
- 服务间依赖测试
- 服务间依赖管理

#### 5、微服务与SOA
|微服务|SOA|
|-|-|
|能拆分的就拆分|是整体的，服务能放一起的都放一起|
|纵向业务划分|是水平分多层|
|由单一组织负责|按层级划分不同部门的组织负责|
|细粒度|粗粒度|
|两句话可以解释明白|几百字只相当于SOA的目录|
|独立的子公司|类似大公司里面划分了一些业务单元（BU）|
|组件小|存在较复杂的组件|
|业务逻辑存在于每个服务中|业务逻辑横跨多个业务领域|
|使用轻量级的通信方式，如HTTP|企业服务总线（ESB）充当了服务之间通信的角色|


|微服务架构实现|SOA实现|
|-|-|
|团队级，自底向上|企业级，自顶向下|
|一个系统拆分多个服务，粒度细|服务由多个子系统组成，粒度大|
|无集中式总线，松散的服务架构|企业服务总线，集中式的服务架构|
|集成方式简单（HTTP/REST/JSON）|集成方式复杂（ESB/WS/SOAP）|
|服务能独立部署|单块架构系统，相互依赖，部署复杂|

