---
title: "Typescript实现一个事件系统" # Title of the blog post.
date: 2023-03-02T09:37:07+08:00 # Date of post creation.
description: "" # Description used for search engine.
summary: "用ts实现一个简单的事件系统"
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
codeMaxLines: 100 # Override global value for how many lines within a code block before auto-collapsing.
codeLineNumbers: true # Override global value for showing of line numbers within code block.
figurePositionShow: true # Override global value for showing the figure label.
categories:
  - 
tags:
  - typescript
series:
  - typescript
# comment: false # Disable comment if false.
---
#### 事件系统
```ts
// event.ts
interface IEventData {
    func: Function;
    target: any;
}

interface IEvent {
    [eventName: string]: IEventData[];
}

class EventDispatcher {
    private static events: IEvent = {}; // 创建一个空的事件对象

    // 注册一个事件监听器
    public addEventListener(eventName: string, func: Function, target: any): void {
        // 如果没有这个事件名，则创建一个空数组
        if (!this.events[eventName]) {
            this.events[eventName] = [];
        }
        // 将事件数据对象添加到数组中
        this.events[eventName].push({ func, target });
    }

    // 移除一个事件监听器
    public removeEventListener(eventName: string, func: Function, target: any): void {
        // 如果有这个事件名，则遍历数组
        if (this.events[eventName]) {
            let arr = this.events[eventName];
            for (let i = 0; i < arr.length; i++) {
                let data = arr[i];
                // 如果找到匹配的函数和目标，则移除该元素
                if (data.func === func && data.target === target) {
                    arr.splice(i, 1);
                    break;
                }
            }
        }
    }

    // 触发一个事件
    public dispatchEvent(eventName: string): void {
        // 如果有这个事件名，则遍历数组
        if (this.events[eventName]) {
            let arr = this.events[eventName];
            for (let data of arr) {
                // 调用每个回调函数，并传入目标对象作为参数
                data.func.call(data.target);
            }
        }
    }
}

```

#### 这段代码可以用来创建一个事件分发器的实例，然后给它添加、移除和触发事件监听器。例如，你可以这样使用它：

```ts
// 创建一个事件分发器的实例
let dispatcher = new EventDispatcher();

// 定义一个回调函数
function onTest(event: any) {
  console.log("Test event triggered");
}

// 给事件分发器添加一个名为"test"的事件监听器，并传入回调函数和目标对象
dispatcher.addEventListener("test", onTest, this);

// 触发"test"事件
dispatcher.dispatchEvent("test");

// 移除"test"事件监听器
dispatcher.removeEventListener("test", onTest, this);
```
```