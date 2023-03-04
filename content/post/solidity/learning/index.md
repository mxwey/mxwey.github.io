---
title: "Solidity" # Title of the blog post.
date: 2023-03-04T18:15:41+08:00 # Date of post creation.
description: "" # Description used for search engine.
summary: "初识Solidity"
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
  - Solidity
series:
  - Solidity
# comment: false # Disable comment if false.
---


# 基本概念
## 什么是Solidity？

Solidity是一门面向合约的、为实现智能合约而创建的高级编程语言。智能合约是在以太坊虚拟机（EVM）上运行的程序，它们可以控制以太坊状态中的账户的行为。
Solidity是一种使用花括号语法的语言，受到C++、Python和JavaScript的影响。Solidity是静态类型的，支持继承、库和复杂的用户自定义类型等特性。

## EVM是什么？

以太坊虚拟机（Ethereum Virtual Machine），这是一个运行在每个以太坊节点上的强大、沙盒化的虚拟栈，负责执行合约字节码。合约通常用高级语言（如solidity）编写，然后编译成EVM字节码。EVM是图灵完备的，意味着它能够执行任何逻辑计算步骤。EVM在执行过程中维护一个临时内存（作为一个字节寻址的字节数组），它不会在交易之间持久化。

## 合约

合约是solidity中的一个基本概念，类似于面向对象语言中的类。合约包含了持久化的数据（状态变量）和可以修改这些数据的函数。合约可以在以太坊或兼容EVM的区块链上创建和部署，每个合约都有一个唯一的地址。合约可以通过构造函数来初始化，也可以通过其他合约或外部账户来调用。合约是构建以太坊应用的基础模块。

## 智能合约

智能合约是一种特殊的计算机协议，它可以在区块链上制定和执行合同。智能合约的优点是它可以去掉中介，实现用户之间的自主交易，而且透明公平，不可篡改。智能合约也是一种以太坊账户，它有自己的余额和地址，可以接收和发送以太币。


## 智能合约的应用场景

智能合约有很多应用场景，比如：

- 去中心化组织：智能合约可以实现一种无需中介的自主协作模式，让参与者按照共识规则进行决策和分配。
- 数字货币：智能合约可以实现一种基于区块链的数字货币系统，让用户可以安全、快速、低成本地进行交易。
- 数字人民币：智能合约可以支持数字人民币的一些特殊场景，如消费红包、政府补贴、专项资金、预付费等。
- 银行支付：智能合约可以让开发者无缝整合来自全球各大银行的数据和流程，开发出在传统金融系统中无法实现的应用程序。


## 智能合约优势

用智能合约的原因是它比传统数字合约更具有以下优势：

- 安全性：智能合约在去中心化的网络中运行，不需要依赖第三方中介，也不会受到单点故障或者恶意篡改的影响。
- 可靠性：智能合约的执行由多个独立节点验证和处理，可以保证网络的稳定运行和合约的按时履行。
- 公平性：智能合约使用点对点的方式进行交易，减少了中心化机构的干预和利益损失。
- 高效性：智能合约可以自动化地完成协议制定和执行，节省了时间和成本。


## Solidity的优势和缺点

Solidity有一些优势和缺点，根据不同的情况和需求，你可以选择是否使用它。以下是一些常见的优势和缺点：

**优势**：

- Solidity受到C++、Python和JavaScript的影响，所以如果你已经熟悉这些语言中的一个，学习和使用Solidity会很容易。
- Solidity有很多开源的文档和案例，可以帮助你了解以太坊和EVM链上的应用功能，并利用其他人的产品构建更复杂的应用。
- Solidity是一种静态类型的面向对象（合约）语言，支持继承、库、接口等特性，可以提高代码的可读性、复用性和安全性。

**缺点**：

- Solidity还是一种相对年轻的语言，可能存在一些未知的漏洞或错误，需要不断地更新和修复。
- Solidity编译后的字节码在EVM上运行时会消耗气体（gas），这是一种交易费用。如果代码不够优化或过于复杂，可能会导致气体消耗过多或超出限制。
- Solidity目前只能在以太坊或兼容EVM的区块链上运行，这限制了它在其他平台上的应用范围。

# Solidity编程语言

## Hello world
```solidity
// 声明solidity版本
pragma solidity ^0.8.0;

// 定义一个合约
contract HelloWorld {

    // 定义一个字符串变量
    string public message;

    // 定义一个构造函数，初始化变量为"Hello World"
    constructor() {
        message = "Hello World";
    }

    // 定义一个函数，返回变量的值
    function getMessage() public view returns (string memory) {
        return message;
    }
}
```

## 如何运行

运行solidity合约有几种方式，你可以根据你的需求和环境选择合适的方式。以下是一些常见的方式：

- 在线模式：你可以使用Remix IDE，这是一个在线的集成开发环境，可以让你编写、编译、部署和调用solidity合约。你只需要在浏览器中打开Remix IDE，输入或导入你的合约代码，然后选择一个编译器版本和一个网络（可以是本地或远程的以太坊节点），就可以运行你的合约了。
- 离线模式：你也可以在本地运行solidity合约，这需要一些前提条件和步骤。首先，你需要下载并安装node.js，并全局安装Truffle和ganache-cli。然后，你需要创建一个Truffle项目，并配置一个开发网络。接着，你需要编写并部署你的合约代码。最后，你可以使用Truffle控制台或web3.js来与你的合约交互。
- 其他模式：除了上述两种方式，还有一些其他的工具或平台可以帮助你运行solidity合约，例如Hardhat、OpenZeppelin、Etherscan等。它们各有各的特点和优势，你可以根据自己的喜好和需求选择使用。





## 用solidity实现投票智能合约

用solidity实现投票智能合约的方法有很多，不同的方法可能有不同的优缺点和适用场景。这里我给你一个简单的例子，仅供参考：

```solidity
// 声明solidity版本
pragma solidity ^0.8.0;

// 定义一个投票合约
contract Voting {

    // 定义一个候选人结构体，包含名字和得票数
    struct Candidate {
        string name;
        uint voteCount;
    }

    // 定义一个映射，记录每个地址是否已经投过票
    mapping(address => bool) public voters;

    // 定义一个数组，存储所有候选人
    Candidate[] public candidates;

    // 定义一个事件，当有人投票时触发
    event Voted(address voter, uint candidateId);

    // 定义一个构造函数，初始化一些候选人
    constructor() {
        candidates.push(Candidate("Alice", 0));
        candidates.push(Candidate("Bob", 0));
        candidates.push(Candidate("Charlie", 0));
    }

    // 定义一个函数，返回候选人的数量
    function getCandidateCount() public view returns (uint) {
        return candidates.length;
    }

    // 定义一个函数，让用户投票给某个候选人
    function vote(uint candidateId) public {
        // 检查用户是否已经投过票
        require(!voters[msg.sender], "You have already voted.");

        // 检查候选人是否存在
        require(candidateId < candidates.length, "Invalid candidate.");

        // 记录用户已经投过票
        voters[msg.sender] = true;

        // 增加候选人的得票数
        candidates[candidateId].voteCount++;

        // 触发投票事件
        emit Voted(msg.sender, candidateId);
    }
}
```

## 其他示例

Solidity是一种用于编写智能合约的高级编程语言，它支持多种区块链平台，如以太坊、波卡、币安智能链等。

为了演示如何用Solidity编写一个智能合约系统，我将参考中的一个例子，实现一个简单的银行系统，让用户可以存款、取款和转账。

首先，我们需要定义一个合约名为Bank，并声明一些变量和事件：

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.7;

// 定义一个名为Bank的合约
contract Bank {
    // 定义一个映射类型的变量balance，用于存储每个地址的余额
    mapping(address => uint) public balance;
    
    // 定义一个事件类型的变量Deposit，用于记录每次存款的信息
    event Deposit(address indexed sender, uint amount, uint balance);
    
    // 定义一个事件类型的变量Withdraw，用于记录每次取款的信息
    event Withdraw(address indexed sender, uint amount, uint balance);
    
    // 定义一个事件类型的变量Transfer，用于记录每次转账的信息
    event Transfer(address indexed sender, address indexed receiver, uint amount);
}
```

接下来，我们需要定义一些函数来实现银行系统的功能：

```solidity
// 在Bank合约内继续添加以下代码

// 定义一个名为deposit的函数，用于让用户存款
function deposit() public payable {
    // 增加调用者地址对应的余额
    balance[msg.sender] += msg.value;
    
    // 触发Deposit事件，并传递相关参数
    emit Deposit(msg.sender, msg.value, balance[msg.sender]);
}

// 定义一个名为withdraw的函数，用于让用户取款
function withdraw(uint amount) public {
    // 检查调用者地址对应的余额是否足够取款金额
    require(balance[msg.sender] >= amount);
    
    // 减少调用者地址对应的余额
    balance[msg.sender] -= amount;
    
    // 将取款金额发送给调用者地址
    payable(msg.sender).transfer(amount);
    
    // 触发Withdraw事件，并传递相关参数
    emit Withdraw(msg.sender, amount, balance[msg.sender]);
}

// 定义一个名为transfer的函数，用于让用户转账给其他地址
function transfer(address receiver, uint amount) public {
     // 检查调用者地址对应的余额是否足够转账金额 
     require(balance[msg.sender] >= amount);

     // 减少调用者地址对应的余额 
     balance[msg.sender] -= amount;

     // 增加接收者地址对应的余额 
     balance[receiver] += amount;

     // 触发Transfer事件，并传递相关参数 
     emit Transfer(msg.sender, receiver, amount);
}
```

至此，我们就完成了一个简单的银行系统智能合约。要运行和部署这个合约，我们可以使用Remix IDE或者Truffle框架等工具。具体步骤如下：

- 使用Remix IDE：
  - 打开https://remix.ethereum.org/网站，在左侧面板中创建一个新文件，并将上述代码复制粘贴到文件中。
  - 在右上角选择Solidity环境，并在左侧面板中选择编译器选项卡。
  - 点击Compile按钮，编译合约代码。
  - 在左侧面板中选择部署与运行选项卡。
  - 在Environment下拉菜单中选择JavaScript VM（这是一种模拟环境，在浏览器中运行虚


## Solidity工程目录结构示例

Solidity工程目录结构可能会因为不同的工具和框架而有所差异，但一般来说，一个典型的Solidity工程目录结构如下：

```text
project/
├── contracts/  // 存放合约源代码文件
│   ├── Bank.sol  // Bank合约文件
│   └── ...  // 其他合约文件
├── migrations/  // 存放部署脚本文件
│   ├── 1_initial_migration.js  // 初始部署脚本
│   ├── 2_deploy_contracts.js  // 部署Bank合约脚本
│   └── ...  // 其他部署脚本
├── test/  // 存放测试代码文件
│   ├── bank.test.js  // 测试Bank合约的JavaScript文件
│   └── ...  // 其他测试文件
├── node_modules/  // 存放依赖模块文件（不需要上传到版本控制系统）
├── package.json  // 定义项目的元数据和依赖关系
└── truffle-config.js  // 定义Truffle框架的配置选项（如果使用Truffle框架）
```

以上是一个使用Truffle框架的Solidity工程目录结构的例子。如果使用其他工具或框架，如Hardhat，则可能会有一些不同，例如：

- Hardhat会在根目录下创建一个名为hardhat.config.js的配置文件，而不是truffle-config.js。
- Hardhat会在根目录下创建一个名为scripts/的子目录，用于存放执行任务的脚本文件。
- Hardhat会在根目录下创建一个名为cache/的子目录，用于存放编译和运行时产生的缓存数据（不需要上传到版本控制系统）。

具体地，一个使用Hardhat框架的Solidity工程目录结构可能如下：

```text
project/
├── contracts/ 
│   ├── Bank.sol 
│   └── ...  
├── scripts/ 
│   ├── deploy.js 
│   └── ...  
├── test/ 
│   ├── bank.test.js 
│   └── ...  
├── node_modules/
├── cache/
├── package.json  
└── hardhat.config.js  
```

无论使用哪种工具或框架，建议遵循以下一些通用原则来组织Solidity工程目录结构：

- 将合约源代码文件统一存放在contracts/子目录下，并按照功能或模块进行分类和命名。
- 将测试代码文件统一存放在test/子目录下，并与合约源代码文件保持一致的名称和结构。
- 将部署脚本或任务脚本统一存放在migrations/或scripts/子目录下，并按照执行顺序进行编号和命名。
- 使用package.json来管理项目的元数据和依赖关系，并避免将不必要的模块或数据上传到版本控制系统。


## 框架

Solidity语言有很多流行的框架，可以帮助开发者更方便地编写、测试和部署智能合约。以下是一些比较知名的框架：

- **Truffle**：Truffle是一个专门用来进行Solidity智能合约开发、部署和测试的软件工具集，它提供了一个命令行界面，可以让开发者快速创建、编译、迁移和测试智能合约。Truffle还支持多种网络环境，包括本地、公共和私有链。
- **Hardhat**：Hardhat是一个基于TypeScript的Solidity开发环境，它提供了一个强大的任务执行器，可以让开发者自定义和组合不同的任务，如编译、测试、部署和调试智能合约。Hardhat还支持使用JavaScript或TypeScript编写测试代码，并提供了一个内置的网络模拟器，可以让开发者在本地运行以太坊节点，并模拟各种场景。
- **OpenZeppelin**：OpenZeppelin是一个提供了一系列可重用和安全的Solidity智能合约库的项目，它包含了很多常用的功能模块，如代币标准、访问控制、数学运算、安全机制等。OpenZeppelin还提供了一些工具和服务，如Defender（用于管理和保护智能合约）、Upgrades（用于升级智能合约）等。
- **Remix**：Remix是一个基于浏览器的Solidity集成开发环境（IDE），它可以让开发者在线编写、编译、部署和调试智能合约。Remix还支持使用JavaScript或Solidity编写测试代码，并提供了一些插件来扩展其功能，如分析器（用于检查代码质量）、调试器（用于跟踪交易执行过程）等。

以上只是一部分流行的Solidity框架，还有很多其他的框架可以选择，例如Brownie（基于Python的Solidity开发环境）、Embark（基于JavaScript的区块链应用平台）、Waffle（基于Ethers.js的轻量级测试框架）等。不同的框架有不同的特点和优势，建议根据自己的需求和喜好来选择适合自己的框架。


## 开发IDE

Solidity开发IDE有很多种，可以根据自己的喜好和需求来选择。以下是一些比较流行的IDE：

- **Remix**：Remix是一个基于浏览器的IDE，它用于开发智能合约，也是目前比较推荐的一款开发以太坊智能合约的IDE，特别是对于新手来说。Remix提供了一个简洁的界面，可以让开发者在线编写、编译、部署和调试智能合约。Remix还支持使用JavaScript或Solidity编写测试代码，并提供了一些插件来扩展其功能，如分析器、调试器等。
- **VScode**：VScode是一个轻量级的代码编辑器，它支持多种语言和平台，包括Solidity。VScode可以通过安装插件来增强其功能，例如Solidity Visual Developer（用于语法高亮、自动补全、错误检查等）、Ethereum Remix（用于连接Remix IDE进行智能合约开发）等。
- **IntelliJ IDEA**：IntelliJ IDEA是一个强大的集成开发环境（IDE），它支持多种语言和框架，包括Solidity。IntelliJ IDEA可以通过安装插件来支持Solidity开发，例如IntelliJ-Solidity（用于语法高亮、自动补全、错误检查等）、web3j-intellij-plugin（用于与web3j库进行交互）等。
- **Atom**：Atom是一个可定制的文本编辑器，它支持多种语言和平台，包括Solidity。Atom可以通过安装插件来增强其功能，例如language-solidity（用于语法高亮、自动补全等）、atom-solidity-linter（用于错误检查等）、etheratom（用于编译、部署和测试智能合约）等。
- **Embark**：Embark是一个基于JavaScript的区块链应用平台，它提供了一个命令行界面和一个图形界面来进行Solidity智能合约开发、部署和测试。Embark还支持与其他工具和服务进行集成，如IPFS（用于存储文件）、Orbit（用于通信）、Etherscan（用于查看交易）等。
- **Yakindu**：Yakindu是一个基于Eclipse的IDE，它专注于状态机建模和代码生成。Yakindu可以通过安装插件来支持Solidity开发，例如Yakindu Solidity Tools（用于语法高亮、自动补全、错误检查、代码生成等）。

以上只是一部分流行的Solidity IDE，还有很多其他的IDE可以选择，例如EthFiddle（基于浏览器的在线代码编辑器）、Brownie（基于Python的Solidity开发环境）、Waffle（基于Ethers.js的轻量级测试框架）等。不同的IDE有不同的特点和优势，建议根据自己的需求和喜好来选择适合自己的IDE。


## Solidity学习资源

Solidity的学习资源有很多种，可以根据自己的基础和目标来选择。以下是一些比较常用的资源：

- **Solidity官方文档**：这是最权威和最全面的Solidity学习资源，它包含了Solidity的语法、特性、安全、工具等方面的内容，以及一些示例代码和练习题。Solidity官方文档有英文版和中文版，建议根据自己的语言水平来选择。
- **合约编程语言 Solidity - 知乎**：这是一个知乎专栏，由Peter Chen撰写，他是一个区块链开发者和教育者。这个专栏介绍了Solidity的基本概念、语法、数据类型、函数、控制结构、继承、接口等方面的内容，以及一些实战案例和项目。这个专栏适合有一定编程基础但不熟悉Solidity的读者。
- **文科生无编程经验，想自学solidity，有什么建议吗？ - 知乎**：这是一个知乎问题，由多位回答者提供了不同的建议和资源。这个问题适合没有编程经验但想学习Solidity的读者。一些常见的建议包括：先学习一门基础编程语言（如Python或JavaScript），再学习区块链和智能合约的原理和概念，然后再学习Solidity；利用在线课程或视频教程来跟随老师进行实践；参与社区讨论或项目贡献来提高自己的能力等。

以上只是一部分流行的Solidity学习资源，还有很多其他的资源可以选择，例如CryptoZombies（一个在线游戏化教程），Mastering Ethereum（一本关于以太坊开发的书籍），OpenZeppelin（一个开源智能合约库）等。不同的资源有不同的特点和优势，建议根据自己的需求和喜好来选择适合自己的资源。
