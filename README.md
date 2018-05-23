# -
一个很受欢迎的以太坊教程：http://xc.hubwiz.com/course/5a952991adb3847553d205d1
本以太坊教程主要是介绍：搭建一个开发环境、编写编译一个智能合约。

### 以太坊是什么

以太坊（Ethereum）是一个开源的有智能合约功能的公共区块链平台。通过其专用加密货币以太币（Ether）提供去中心化的虚拟机（“以太虚拟机” Ethereum Virtual Machine）来处理点对点合约。

以太坊的概念首次在2013至2014年间由程序员Vitalik Buterin，受比特币启发后提出，大意为“下一代加密货币与去中心化应用平台”，在2014年通过ICO众筹得以开始发展。目前以太币是市值第二高的加密货币，仅次于比特币。

### 以太坊区块链是什么？

以太坊区块链有 2 个主要组件：

- 数据存储：网络中每笔交易都存储在区块链上。当你部署合约时，就是一笔交易。当你执行合约功能时，也是另一笔交易。所有的这些交易都是公开的，每个人都可以看到并进行验证。这个数据永远也无法篡改。为了确保网络中的所有节点都有着同一份数据拷贝，并且没有向区块链中写入任何的无效数据，以太坊使用一个叫做工作量证明的算法来保证网络安全。

- 代码：就数据的层面而言，区块链就是存储交易。在以太坊的世界里，你可以通过一个叫 Solidity 的语言编写逻辑/应用代码（也就是智能合约）。然后用 solidity 编译器将代码编译为以太坊字节码，并将字节码部署到区块链上（也有一些其他的语言可以写合约，不过 solidity 是到目前为止用得最多也是相对更容易的选择）。所以，以太坊不仅仅会存储交易数据，它还会存储和执行智能合约代码。

可以简单的理解以太坊区块链的作用就是存储数据和代码，并在 EVM（Ethereum Virtual Machine，以太坊虚拟机）中执行代码。

### 要准备的基础知识

为了进行以太坊开发，你应该对以下语言/技术有基本了解：

-  熟悉某种面向对象语言（如Python，Java，go）
-  HTML/CSS/Javascript
- 基本的命令行交互如Linux shell命令
- 理解数据库的基本概念

为了构建以太坊去中心化应用即Dapp（Decentralized application），以太坊有一个非常方便的 JavaScript 库即 web3.js，你也可以在一些 js 框架中直接引入该库构建应用，比如 react，angular，vue 等。

### 示例：一个以太坊投票应用

[以太坊教程](http://xc.hubwiz.com/course/5a952991adb3847553d205d1)示例中，我们将会构建一个简单的去中心化投票应用。所谓去中心化应用，就是一个不只存在于某一中心化服务器上的应用。在网络中成百上千的电脑上，会运行着非常多的应用副本，这使得它几乎不可能出现宕机的情况。你将会构建一个投票应用，在这个应用中，你可以初始化参与选举的候选者，并对候选者投票，而这些投票将会被记录在区块链上。你将会经历编写投票合约，部署到区块链并与之交互的整个过程。你将会了解什么是一个合约，将合约部署到区块链上并与之交互意味着什么。

本质上，区块链就像是一个分布式数据库，这个数据库维护了一个不断增长的记录链表。如果熟悉关系型数据库，你应该知道一张表里有很多行的数据。现在，对数据进行批（batch）量处理（比如每批 100 行），并将每个处理的批次相连。就可以形成一个区块链了！在区块链里，每个批次的数据就叫一个块（block），块里的每一行就叫一笔交易（transaction）。

现在，你对以太坊已经有了基本了解，我们可以开始构建投票的 dapp 了。这将会加强你对以太坊的认识，并且初略了解以太坊的功能。

### 以太坊开发环境搭建

#### Linux

示例是 Ubuntu 16.04 下的学习环境搭建，你只需要成功安装了 nodejs 和 npm，就可以继续项目的下一步了。

我们通过 npm 安装 ganache 和 web3 包来为[以太坊教程](http://xc.hubwiz.com/course/5a952991adb3847553d205d1)提供支撑。我们也需要安装 solc来编译合约。

下面是安装过程：
```
$ sudo apt-get update
$ curl -sL https://deb.nodesource.com/setup_7.x -o nodesource_setup.sh
$ sudo bash nodesource_setup.sh
$ sudo apt-get install nodejs
$ node --version
v7.4.0
$ npm --version
4.0.5
$ mkdir -p ethereum_voting_dapp/chapter1
$ cd ethereum_voting_dapp/chapter1
$ npm install ganache-cli web3@0.20.1 solc
$ node_modules/.bin/ganache-cli
```
如果安装成功，运行命令node_modules/.bin/ganache-cli，应该能够看到下面的输出。

```
Ganache CLI v6.0.3 (ganache-core: 2.0.2)

Available Accounts
==================
(0) 0x5c252a0c0475f9711b56ab160a1999729eccce97
(1) 0x353d310bed379b2d1df3b727645e200997016ba3
(2) 0xa3ddc09b5e49d654a43e161cae3f865261cabd23
(3) 0xa8a188c6d97ec8cf905cc1dd1cd318e887249ec5
(4) 0xc0aa5f8b79db71335dacc7cd116f357d7ecd2798
(5) 0xda695959ff85f0581ca924e549567390a0034058
(6) 0xd4ee63452555a87048dcfe2a039208d113323790
(7) 0xc60c8a7b752d38e35e0359e25a2e0f6692b10d14
(8) 0xba7ec95286334e8634e89760fab8d2ec1226bf42
(9) 0x208e02303fe29be3698732e92ca32b88d80a2d36

Private Keys
==================
(0) a6de9563d3db157ed9926a993559dc177be74a23fd88ff5776ff0505d21fed2b
(1) 17f71d31360fbafbc90cad906723430e9694daed3c24e1e9e186b4e3ccf4d603
(2) ad2b90ce116945c11eaf081f60976d5d1d52f721e659887fcebce5c81ee6ce99
(3) 68e2288df55cbc3a13a2953508c8e0457e1e71cd8ae62f0c78c3a5c929f35430
(4) 9753b05bd606e2ffc65a190420524f2efc8b16edb8489e734a607f589f0b67a8
(5) 6e8e8c468cf75fd4de0406a1a32819036b9fa64163e8be5bb6f7914ac71251cc
(6) c287c82e2040d271b9a4e071190715d40c0b861eb248d5a671874f3ca6d978a9
(7) cec41ef9ccf6cb3007c759bf3fce8ca485239af1092065aa52b703fd04803c9d
(8) c890580206f0bbea67542246d09ab4bef7eeaa22c3448dcb7253ac2414a5362a
(9) eb8841a5ae34ff3f4248586e73fcb274a7f5dd2dc07b352d2c4b71132b3c73f0

HD Wallet
==================
Mnemonic:   cancel better shock lady capable main crunch alcohol derive alarm duck umbrella
Base HD Path: m/44'/60'/0'/0/{account_index}

Listening on localhost:8545
```
为了便于测试，ganache 默认会创建 10 个账户，每个账户有 100 个以太。如果你还不懂什么是账户，把它想象成存钱的银行账户就可以了（以太（Ether，ETH）就是以太坊生态系统中的 钱/货币）。你需要用这个账户创建交易，发送/接收以太。

#### MacOS

如果你还没有安装 homebrew，请按照 https://brew.sh/ 的指示安装 homebrew。homebrew 是一个包管理器，它可以帮助我们安装开发所需的所有其他软件。按照下面的指示安装所有其他所需的包。
```
$ brew update
$ brew install nodejs
$ node --version
v7.10.0
$ npm --version
4.2.0
$ mkdir -p ethereum_voting_dapp/chapter1
$ cd ethereum_voting_dapp/chapter1
$ npm install ganache-cli web3@0.20.1 solc
$ node_modules/.bin/ganache-cli
```

我们通过 npm 安装 ganache 和 web3 包。我们也需要安装 solc 来编译合约。

如果安装成功，运行命令node_modules/.bin/ganache-cli，应该能够看到右图所示的输出。
```
Ganache CLI v6.0.3 (ganache-core: 2.0.2)
Available Accounts
==================
(0) 0x5c252a0c0475f9711b56ab160a1999729eccce97
(1) 0x353d310bed379b2d1df3b727645e200997016ba3
(2) 0xa3ddc09b5e49d654a43e161cae3f865261cabd23
(3) 0xa8a188c6d97ec8cf905cc1dd1cd318e887249ec5
(4) 0xc0aa5f8b79db71335dacc7cd116f357d7ecd2798
(5) 0xda695959ff85f0581ca924e549567390a0034058
(6) 0xd4ee63452555a87048dcfe2a039208d113323790
(7) 0xc60c8a7b752d38e35e0359e25a2e0f6692b10d14
(8) 0xba7ec95286334e8634e89760fab8d2ec1226bf42
(9) 0x208e02303fe29be3698732e92ca32b88d80a2d36

Private Keys
==================
(0) a6de9563d3db157ed9926a993559dc177be74a23fd88ff5776ff0505d21fed2b
(1) 17f71d31360fbafbc90cad906723430e9694daed3c24e1e9e186b4e3ccf4d603
(2) ad2b90ce116945c11eaf081f60976d5d1d52f721e659887fcebce5c81ee6ce99
(3) 68e2288df55cbc3a13a2953508c8e0457e1e71cd8ae62f0c78c3a5c929f35430
(4) 9753b05bd606e2ffc65a190420524f2efc8b16edb8489e734a607f589f0b67a8
(5) 6e8e8c468cf75fd4de0406a1a32819036b9fa64163e8be5bb6f7914ac71251cc
(6) c287c82e2040d271b9a4e071190715d40c0b861eb248d5a671874f3ca6d978a9
(7) cec41ef9ccf6cb3007c759bf3fce8ca485239af1092065aa52b703fd04803c9d
(8) c890580206f0bbea67542246d09ab4bef7eeaa22c3448dcb7253ac2414a5362a
(9) eb8841a5ae34ff3f4248586e73fcb274a7f5dd2dc07b352d2c4b71132b3c73f0

HD Wallet
==================
Mnemonic:   cancel better shock lady capable main crunch alcohol derive alarm duck umbrella
Base HD Path: m/44'/60'/0'/0/{account_index}

Listening on localhost:8545
```
为了便于测试，ganache 默认会创建 10 个账户，每个账户有 100 个以太。如果你还不懂什么是以太坊账户，把它想象成存钱的银行账户就可以了（以太（Ether, ETH）就是以太坊生态系统中的钱/货币）。你需要用这个账户创建交易，发送/接收以太。

####  Windows

- 安装 Visual Studio Community Edition。如果你选择定制安装，那么至少应该安装 Visual C++（目前的版本是 VS 2017）
- 安装 Windows SDK for Windows
- 安装 Python 2.7 如果你还没有安装的话，并且确保将它加入到环境变量 PATH
- 安装 git 如果你还没有安装并加入到 PATH 
- 安装 OpenSSL。确保选择了正确的安装包，并且只安装完整版（而不是轻装版）。你必须将 OpenSSL 安装到推荐安装的位置 -- 不要改变安装路径
- 下载和安装 node v8.1.2 。不推荐使用版本 v6.11.0 搭配 VS2017
- 执行命令 npm install ganache-cli web3@0.20.1 solc

### Solidity Contracts

现在已经安装好 ganache 并运行，我们将会开始编写第一个以太坊智能合约。

我们会使用 solidity 编程语言来编写合约。如果你熟悉面向对象编程，学习用 solidity 写合约应该非常简单。我们会写一个叫做 Voting 的合约（可以把合约看成是面对对象编程语言的一个类），这个合约有以下内容：

- 一个构造函数，用来初始化一些候选者。
- 一个用来投票的方法（对投票数加 1）
- 一个返回候选者所获得的总票数的方法

当你把合约部署到区块链的时候，就会调用构造函数，并只调用一次。与 web 世界里每次部署代码都会覆盖旧代码不同，在区块链上部署的合约是不可改变的，也就是说，如果你更新合约并再次部署，旧的合约仍然会在区块链上存在，并且数据仍在。新的部署将会创建合约的一个新的实例。

```
pragma solidity ^0.4.18;

contract Voting {

  mapping (bytes32 => uint8) public votesReceived;
  bytes32[] public candidateList;

  function Voting(bytes32[] candidateNames) public {
    candidateList = candidateNames;
  }

  function totalVotesFor(bytes32 candidate) view public returns (uint8) {
    require(validCandidate(candidate));
    return votesReceived[candidate];
  }

  function voteForCandidate(bytes32 candidate) public {
    require(validCandidate(candidate));
    votesReceived[candidate]  += 1;
  }

  function validCandidate(bytes32 candidate) view public returns (bool) {
    for(uint i = 0; i < candidateList.length; i++) {
      if (candidateList[i] == candidate) {
        return true;
      }
    }
    return false;
   }
}
```
将右侧代码拷贝到一个叫做 Voting.sol 的文件中，并保存到 chapter1 目录下面。

**代码和解释**

- Line 1. 我们必须指定代码将会哪个版本的编译器进行编译
- Line 3. mapping 相当于一个关联数组或者是字典，是一个键值对。mapping votesReceived 的键是候选者的名字，类型为 bytes32。mapping 的值是一个未赋值的整型，存储的是投票数。
- Line 4. 在很多编程语言中，仅仅通过 votesReceived.keys 就可以获取所有的候选者姓名。但是，但是在 solidity 中没有这样的方法，所以我们必须单独管理一个候选者数组 candidateList。
- Line 14. 注意到 votesReceived[key] 有一个默认值 0，所以你不需要将其初始化为 0，直接加1 即可。

你也会注意到每个函数有个可见性说明符（visibility specifier）（比如本例中的 public）。这意味着，函数可以从合约外调用。如果你不想要其他任何人调用这个函数，你可以把它设置为私有（private）函数。如果你不指定可见性，编译器会抛出一个警告。最近 solidity 编译器进行了一些改进，如果用户忘记了对私有函数进行标记导致了外部可以调用私有函数，编译器会捕获这个问题。 [这里](http://solidity.readthedocs.io/en/develop/miscellaneous.html#function-visibility-specifiers) 可以看到所有的可见性说明符。

你也会在一些函数上看到一个修饰符 view。它通常用来告诉编译器函数是只读的（也就是说，调用该函数，区块链状态并不会更新）。所有的修饰符都可以在 [这里](http://solidity.readthedocs.io/en/develop/miscellaneous.html#modifiers) 看到。

### 编译智能合约

我们将会使用上一节安装的 solc 库来编译代码。如果你还记得的话，之前我们提到过 web3js 是一个库，它能够让你通过 RPC 与区块链进行交互。我们将会在 node 控制台里用这个库部署合约，并与区块链进行交互。

首先，在终端中运行 node 进入 node 控制台，初始化 web3 对象，并向区块链查询获取所有的账户。

>确保与此同时 ganache 已经在另一个窗口中运行

为了编译合约，先从 Voting.sol 中加载代码并绑定到一个 string 类型的变量，然后像下边这样对合约进行编译。

```
$ node

In the node console
> Web3 = require('web3')
> web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:8545"));
> web3.eth.accounts
['0x5c252a0c0475f9711b56ab160a1999729eccce97'
'0x353d310bed379b2d1df3b727645e200997016ba3'
'0xa3ddc09b5e49d654a43e161cae3f865261cabd23'
'0xa8a188c6d97ec8cf905cc1dd1cd318e887249ec5'
'0xc0aa5f8b79db71335dacc7cd116f357d7ecd2798'
'0xda695959ff85f0581ca924e549567390a0034058'
'0xd4ee63452555a87048dcfe2a039208d113323790'
'0xc60c8a7b752d38e35e0359e25a2e0f6692b10d14'
'0xba7ec95286334e8634e89760fab8d2ec1226bf42'
'0x208e02303fe29be3698732e92ca32b88d80a2d36']


> code = fs.readFileSync('Voting.sol').toString()
> solc = require('solc')
> compiledCode = solc.compile(code)
```

当你成功地编译好合约，打印 compiledCode 对象（直接在 node 控制台输入 compiledCode 就可以看到内容），你会注意到有两个重要的字段，它们很重要，你必须要理解：

- 1.compiledCode.contracts[':Voting'].bytecode: 这就是 Voting.sol 编译好后的字节码。也是要部署到区块链上的代码。
- 2.compiledCode.contracts[':Voting'].interface: 这是一个合约的接口或者说模板（叫做 abi 定义），它告诉了用户在这个合约里有哪些方法。在未来无论何时你想要跟任意一个合约进行交互，你都会需要这个 abi 定义。你可以在这里 看到 [ABI](https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI) 的更多内容。

教程参考汇智网的DAPP开发入门教程，如果大家等不及博客更新，也可以直接访问这个[以太坊教程](http://xc.hubwiz.com/course/5a952991adb3847553d205d1)。
