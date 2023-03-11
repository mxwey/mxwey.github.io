---
title: "cocos一些优化方法" # Title of the blog post.
date: 2023-03-12T06:30:45+08:00 # Date of post creation.
description: "" # Description used for search engine.
summary: "cocos的一些优化方法"
featured: false # Sets if post is a featured post, making appear on the home page side bar.
draft: false # Sets whether to render this page. Draft of true will not be rendered.
toc: false # Controls if a table of contents should be generated for first-level links automatically.
# menu: main
usePageBundles: false # Set to true to group assets like images in the same folder as this post.
# featureImage: "/images/path/file.jpg" # Sets featured image on blog post.
# featureImageAlt: 'Description of image' # Alternative text for featured image.
# featureImageCap: 'This is the featured image.' # Caption (optional).
thumbnail: "/images/cocos.png" # Sets thumbnail image appearing inside card on homepage.
# shareImage: "/images/path/share.png" # Designate a separate image for social media sharing.
codeMaxLines: 10 # Override global value for how many lines within a code block before auto-collapsing.
codeLineNumbers: false # Override global value for showing of line numbers within code block.
# figurePositionShow: true # Override global value for showing the figure label.
categories:
  - 技术
tags:
  - cocos优化
series:
  - cocos
# comment: false # Disable comment if false.
---

# 缓存事件函数
在Cocos Creator 3中，通过添加事件监听器来响应用户操作，每次事件触发都会执行相应的事件处理函数。如果事件处理函数复杂或者执行时间较长，就会影响游戏的帧率。为了避免这种情况，我们可以使用缓存事件函数的方法。

具体做法是，先使用一个变量记录事件处理函数，然后在事件触发时调用该函数即可。这样，对于相同类型的事件，每次触发都可以直接调用缓存的事件处理函数，避免反复执行。

下面是一个示例代码：

```ts
export default class MyScene extends cc.Component {
    private onClickButton: () => void; // 记录按钮点击事件处理函数

    onLoad() {
        const button = this.node.getChildByName('button');
        this.onClickButton = this.handleButtonClick.bind(this);
        button.on('click', this.onClickButton);
    }

    private handleButtonClick() {
        // 处理按钮点击事件
    }
}
```


# 使用对象池

在游戏中，我们可能需要频繁创建和销毁一些游戏对象，比如子弹、敌人等。如果每次都重新创建这些对象，会导致频繁的内存分配和垃圾回收，影响游戏的性能。为了避免这种情况，我们可以使用对象池的方法。

具体做法是，先在游戏初始化时创建一定数量的对象，然后在需要使用时，从对象池中取出一个空闲对象，使用完毕后，将对象释放回对象池，而不是将其销毁。

下面是一个示例代码：

```ts
export default class Bullet extends cc.Component {
    static pool = new cc.NodePool(); // 对象池

    onLoad() {
        Bullet.pool.put(this.node); // 初始时将对象放入对象池
    }

    static create() {
        const bullet = Bullet.pool.get() || cc.instantiate(this.prefab);
        return bullet.getComponent(this);
    }

    onCollisionEnter() {
        Bullet.pool.put(this.node); // 碰撞后，将对象放回对象池
    }
}
```

# 合批渲染
在游戏中，如果有大量相似的游戏对象需要渲染，比如粒子效果、小型敌人等，可以考虑使用合批渲染的方法。

具体做法是，在游戏中创建一个大型游戏对象，然后将所有相似的游戏对象作为该大型对象的子节点，再将该大型对象进行渲染。这样，所有相似的游戏对象将被渲染成一个批次，从而提高渲染效率。

下面是一个示例代码：
```ts
export default class BatchRender extends cc.Component {
    onLoad() {
        const parentNode = this.node;

        // 创建子节点，并设置为相似的游戏对象
        for (let i = 0; i < 100; i++) {
            const childNode = new cc.Node();
            const sprite = childNode.addComponent(cc.Sprite);
            sprite.spriteFrame = this.spriteFrame;
            childNode.setPosition(i * 10, 0);
            parentNode.addChild(childNode);
        }

        // 合批渲染
        const assembler = new cc.Assembler();
        assembler.init(this);
        assembler.updateRenderData(this);
    }
}
```

# 使用异步加载
在游戏中，我们需要加载大量资源，比如场景、图片等。如果每次都同步加载这些资源，会导致游戏启动时间过长，影响用户体验。为了避免这种情况，我们可以使用异步加载的方法。

具体做法是，使用Cocos Creator 3中提供的资源加载接口cc.resources.load，将需要加载的资源路径作为参数传入即可。该接口会返回一个Promise对象，当资源加载完毕后，会自动触发then回调函数。

下面是一个示例代码：

```ts
export default class MyScene extends cc.Component {
    async onLoad() {
        const node = await cc.resources.load('prefab/bullet');
        this.node.addChild(node);
    }
}
```

# 总结

通过以上四个方法，我们可以提升Cocos Creator 3游戏的性能，让游戏更加流畅、稳定。除此之外，我们还可以使用其他一些优化方法，比如减少渲染次数、减少内存占用等。在实际制作过程中，我们需要根据游戏的具体情况选择合适的优化方法。
