---
title: "cocos学习笔记（二）" # Title of the blog post.
date: 2023-02-22T11:08:17+08:00 # Date of post creation.
description: "一些cocos的最佳实践" # Description used for search engine.
summary: "一些cocos的最佳实践"
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
  - cocos
tags:
  - cocos
series:
  - cocos
# comment: false # Disable comment if false.
---

### 1. ui切换方式、优缺点

UI 切换是指在游戏中根据不同的状态或需求，切换不同的 UI 界面，比如从主菜单切换到游戏场景，或者从游戏场景切换到暂停菜单等。UI 切换有多种实现方式，以下是一些常见的设计思路：

#### 实现方式：

- 如果 UI 是固定不变的，比如主菜单、暂停界面等，可以使用场景切换的方式来实现，每个 UI 对应一个场景文件，通过 cc.director.loadScene 来加载和切换。
- 如果 UI 是动态变化的，比如对话框、提示框等，可以使用预制体的方式来实现，每个 UI 对应一个预制体文件，通过 cc.instantiate 来创建和销毁。
- 如果 UI 是复杂且频繁切换的，比如商店、背包等，可以使用组件化的方式来实现，每个 UI 对应一个组件脚本，通过 cc.Component 的 enable 和 disable 来控制显示和隐藏。


#### 优缺点：

- 使用场景文件来存储不同的 UI 界面，然后通过加载和切换场景来实现 UI 切换。这种方式比较简单，但是会增加场景文件的数量和加载时间。
- 使用预制体（Prefab）来存储不同的 UI 界面，然后通过动态加载和销毁预制体来实现 UI 切换。这种方式可以减少场景文件的数量和加载时间，但是需要管理预制体的生命周期和内存占用。
- 使用节点激活/隐藏来控制不同的 UI 界面的显示/隐藏，然后通过修改节点层级和渲染顺序来实现 UI 切换。这种方式可以避免频繁加载和销毁资源，但是需要管理节点树结构和性能消耗。

#### 代码示例：


- 使用场景文件来实现 UI 切换，你可以使用以下代码来加载和切换场景：

```javascript
// 加载并切换到指定场景
cc.director.loadScene('MyScene');

// 或者使用 Asset Bundle 来加载指定 bundle 中的场景
cc.assetManager.loadBundle('bundleName', function (err, bundle) {
    if (err) {
        return console.error(err);
    }
    bundle.loadScene('MyScene', function (err, scene) {
        cc.director.runScene(scene);
    });
});
```

- 使用预制体（Prefab）来实现 UI 切换，你可以使用以下代码来动态加载和销毁预制体：

```javascript
// 加载预制体资源
cc.resources.load('prefabs/MyUI', cc.Prefab, function (err, prefab) {
    if (err) {
        return console.error(err);
    }
    // 实例化预制体节点
    var node = cc.instantiate(prefab);
    // 将节点添加到当前场景中
    cc.director.getScene().addChild(node);
});

// 销毁预制体节点
node.destroy();
```

- 使用节点激活/隐藏来实现 UI 切换，你可以使用以下代码来控制节点的显示/隐藏：

```javascript
// 激活或隐藏节点
node.active = true; // 显示节点
node.active = false; // 隐藏节点

// 修改节点层级和渲染顺序（假设 node1 和 node2 是同级兄弟节点）
node1.setSiblingIndex(0); // 将 node1 放在最底层
node2.setSiblingIndex(1); // 将 node2 放在 node1 的上面

// 或者使用以下方法交换两个节点的位置（假设 node1 和 node2 是同级兄弟节点）
node1.swapSibling(node2); // 交换 node1 和 node2 的位置

```

### 2. emit和dispatchEvent的区别、应用场景、举例

#### emit和dispatchEvent的区别是：

- emit是直接触发一个事件，不会进行事件传递，只有注册了该事件的节点才能监听到。
- dispatchEvent是派发一个事件，会进行事件传递，也就是冒泡机制，事件会从发起节点向上层节点传递，直到根节点或者被停止。

#### 应用场景如下：

- emit适用于自定义事件，或者不需要事件传递的情况，比如一个按钮点击事件，只需要在按钮节点上注册监听即可。
- dispatchEvent适用于系统事件，或者需要事件传递的情况，比如一个触摸事件，需要在多个节点上监听触摸开始、移动、结束等状态。


#### 举例


- emit的使用：

```javascript
// 在装备介绍框的脱掉按钮上添加点击事件
this.node.on('click', function (event) {
    // 关闭介绍框
    this.node.active = false;
    // 通知主页面装备已脱掉
    cc.find('Canvas').emit('equip-off', this.equipId);
}, this);

// 在主页面上监听装备脱掉事件
cc.find('Canvas').on('equip-off', function (event) {
    // 获取装备id
    let equipId = event.detail;
    // 更新装备状态
    this.updateEquip(equipId);
}, this);
```

- dispatchEvent的使用：

```javascript
// 在触摸节点上添加触摸事件
this.node.on(cc.Node.EventType.TOUCH_START, function (event) {
    // 派发触摸开始事件，会向上传递到父节点
    event.target.dispatchEvent(new cc.Event.EventCustom('touch-start', true));
}, this);

// 在父节点上监听触摸开始事件
this.node.parent.on('touch-start', function (event) {
    // 获取触摸位置
    let touchPos = event.getLocation();
    // 处理触摸逻辑
    this.handleTouch(touchPos);
}, this);
```

### 3. UI控制器

- 首先，你需要在资源管理器中创建一个预制体文件夹，用来存放不同的 UI 界面的预制体资源¹。
- 然后，你需要在层级管理器中创建一个空节点，用来作为 UI 管理器，负责加载和切换不同的 UI 界面。你可以给这个节点添加一个脚本组件，用来编写控制逻辑。
- 接下来，你需要在脚本组件中定义一些变量和方法，用来存储和操作预制体资源。比如：

```javascript
// 定义一个数组变量，用来存储不同的 UI 预制体资源
@property({type: [cc.Prefab]})
uiPrefabs = [];

// 定义一个变量，用来存储当前显示的 UI 节点
@property({type: cc.Node})
currentUI = null;

// 定义一个方法，用来根据索引加载并显示对应的 UI 预制体
showUI(index) {
    // 检查索引是否有效
    if (index < 0 || index >= this.uiPrefabs.length) {
        return;
    }
    // 销毁当前显示的 UI 节点（如果有）
    if (this.currentUI) {
        this.currentUI.destroy();
        this.currentUI = null;
    }
    // 实例化新的 UI 节点
    var node = cc.instantiate(this.uiPrefabs[index]);
    // 将节点添加到当前场景中
    cc.director.getScene().addChild(node);
    // 更新当前显示的 UI 节点
    this.currentUI = node;
}
```

- 最后，你需要在编辑器中将不同的 UI 预制体资源拖拽到 uiPrefabs 数组变量中，并且在合适的时机调用 showUI 方法来切换不同的 UI 界面。
