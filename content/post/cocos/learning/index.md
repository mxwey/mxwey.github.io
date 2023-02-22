---
title: "Cocos学习笔记" # Title of the blog post.
date: 2023-02-20T16:32:34+08:00 # Date of post creation.
description: "" # Description used for search engine.
summary: "Cocos学习笔记"
featured: false # Sets if post is a featured post, making appear on the home page side bar.
draft: false # Sets whether to render this page. Draft of true will not be rendered.
toc: false # Controls if a table of contents should be generated for first-level links automatically.
# menu: main
usePageBundles: true # Set to true to group assets like images in the same folder as this post.
# featureImage: "/images/path/file.jpg" # Sets featured image on blog post.
# featureImageAlt: 'Description of image' # Alternative text for featured image.
# featureImageCap: 'This is the featured image.' # Caption (optional).
thumbnail: "images/cocos.png" # Sets thumbnail image appearing inside card on homepage.
# shareImage: "/images/path/share.png" # Designate a separate image for social media sharing.
codeMaxLines: 10 # Override global value for how many lines within a code block before auto-collapsing.
codeLineNumbers: false # Override global value for showing of line numbers within code block.
figurePositionShow: true # Override global value for showing the figure label.
categories:
  - 技术
tags:
  - cocos
series:
  - cocos
# comment: false # Disable comment if false.
---

1. 获取节点方法

```ts
// 获取主节点
this.node.children[0];
this.node.getChildByName("xxx");
cc.find("xxx")
// 获取父节点
this.node.getParent();
// 设置父节点
this.node.setParent('xxx')
// 移除子节点
// 移除父节点

// 访问位置
this.node.x
this.node.y
// 设置位置
this.node.setPosition(3,4);

// 颜色
this.node.color = cc.Color.RED;

// 节点开关
this.node.active = false;

// 组件开关
this.enabled = false;

// 获取zujian 
let sprite = this.getComponent(cc.Sprite);
// 获取子组件
this.getComponentInChildren(cc.Sprite);

```

2. 资源动态加载
3. 场景管理

- 创建场景
> 文件->新建场景
- 切换/加载场景
```ts
cc.director.loadScene('scene name');

// 带参数
cc.director.loadScene('scene name',function(){
  // 切换完场景的操作
});
// 问题：资源大时，很卡，需要进度条
cc.director.preloadScene('scene name',function(){
  // 场景预记载
cc.director.loadScene('scene name');
});

```

4. 键鼠操作

```ts
// 监听鼠标 cc.Node.EventType.xxx 定义各种事件的枚举
this.node.on(cc.Node.EventType.xxx, function(event){

});

// 键盘监听
this.systemEvent.on(cc.SystemEvent.EventType.xxx,function(e){
  console.debug(event.keyCode);
  // event.keyCode 通过枚举定义：cc.macro.KEY.xxx
})
```


5. 触摸事件

```ts
// 与鼠标类似 详见cc.Node.EventType枚举
this.node.on(cc.Node.EventType.TOUCH_START,function(e){
  // 多指触摸的区分 触摸编码
  console.debug('编码：'+e.getID())
  // 获取触摸位置
  console.debug(e.getLocation())
})
```

6. 自定义事件

```ts
// 定义自定义事件
this.node.on('myevent1',function(e){
  console.debug("自定义事件")
})
// 触发自定义事件
this.node.emit('myevent1')

// 另一种事件触发 - 事件分发
// 可以给父节点发（冒泡）
this.node.dispatchEvent(new cc.Event.EventCustom("myevent1", true))
```

7. 碰撞检测

- 添加碰撞组件

> 右侧面板->添加组件->添加碰撞组件

- 步骤：
  - 开启碰撞`cc.director.getCollisionManager().enabled = true`
  - 定义碰撞周期事件

```ts
import { _decorator, Component, Node } from 'cc';
const { ccclass, property } = _decorator;

@ccclass('NewComponent')
export class NewComponent extends Component {
    start() {
      // 开启碰撞检测
      cc.director.getCollisionManager().enabled = true;
    }

    // 碰撞事
    // other 碰撞到别人的碰撞体
    // self 自己
    onCollisionEnter(other, self){
      console.debug('碰撞发生，用碰撞标签区分哪个物体：'+other.tag)
    }

    // 碰撞持续 只要碰撞发生一直会调用
    onCollisionStay(other){

    }

    // 碰撞结束
    onCollisionStay(other){

    }
}

```


