---
title: "Typescript 索引签名" # Title of the blog post.
date: 2023-03-02T09:25:20+08:00 # Date of post creation.
description: "" # Description used for search engine.
summary: "ts的索引签名学习笔记"
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
  - 技术
tags:
  - typescript
series:
  - typescript
# comment: false # Disable comment if false.
---

先看一段代码

```ts
interface IEventData {
    func: Function;
    target: any;
}

interface IEvent {
    [eventName: string]: IEventData[];
}
```

这段代码的含义是：

- 定义了一个接口 `IEventData`，它表示一个事件数据对象，它有两个属性：`func` 和 `target`。`func` 是一个函数类型，表示事件的回调函数；`target` 是任意类型，表示事件的目标对象。
- 定义了一个接口 `IEvent`，它表示一个事件对象，它可以有任意数量和名称的属性，每个属性的值是一个 `IEventData` 类型的数组。这个接口使用了索引签名 `[eventName: string]: IEventData[]`，表示属性名是字符串类型，属性值是 `IEventData` 类型的数组。


ts中的索引签名的应用场景：

索引签名的应用场景有很多，主要是当你想要定义一个对象，它可以有任意数量和类型的属性，而不是固定的属性名和类型。例如：
你可以使用索引签名来定义一个字典，它可以存储任意的键值对。例如：
```ts
interface IDictionary {
    [key: string]: string; // 索引签名，表示键是字符串，值也是字符串
}

let dict: IDictionary = {}; // 创建一个空的字典对象

dict["apple"] = "苹果"; // 添加一个单词和它的翻译
dict["banana"] = "香蕉"; // 添加另一个单词和它的翻译

console.log(dict["apple"]); // 输出 "苹果"
console.log(dict["orange"]); // 输出 undefined，因为没有这个单词
```

你也可以使用索引签名来定义一个数组，它可以存储任意类型的元素。例如：
```ts
interface IArray {
    [index: number]: any; // 索引签名，表示索引是数字，值是任意类型
}

let arr: IArray = []; // 创建一个空的数组对象

arr[0] = 1; // 添加一个数字元素
arr[1] = "hello"; // 添加一个字符串元素
arr[2] = true; // 添加一个布尔元素

console.log(arr[0]); // 输出 1
console.log(arr[3]); // 输出 undefined，因为没有这个元素
```

另外，你还可以使用索引签名来定义一些更复杂的对象，比如使用符号或者模板字面量作为键。例如：
```ts
interface ISymbol {
    [symbol: symbol]: string; // 索引签名，表示键是符号类型，值是字符串类型
}

let sym: ISymbol = {}; // 创建一个空的符号对象

let s1 = Symbol("foo"); // 创建一个符号 s1
let s2 = Symbol("bar"); // 创建另一个符号 s2

sym[s1] = "hello"; // 添加一个符号键值对
sym[s2] = "world"; // 添加另一个符号键值对

console.log(sym[s1]); // 输出 "hello"
console.log(sym[s2]); // 输出 "world"
```

```ts
interface ITemplate {
    [template: `hello-${string}`]: string; // 索引签名，表示键是以 hello- 开头的模板字面量类型，值是字符串类型
}

let temp: ITemplate = {}; // 创建一个空的模板对象

temp["hello-world"] = "Hello World!"; /
```
