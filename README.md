# ERC_Implementation

ERC的中文翻译与实现（主要是OpenZeppelin）的一些讨论。

仅作个人学习记录之用，才疏学浅挂一漏万，多有谬误敬请指正。

## 前言：从EIP-1说起

EIP-1发布于2015年10月27日，是关于EIP定义、分类、书写规范及提议流程的指南文档，原文见 [EIP-1: EIP Purpose and Guidelines](https://eips.ethereum.org/EIPS/eip-1)

### 什么是EIP？

EIP即Ethereum Improvement Proposal，译为太坊改进提案，可理解为与以太坊相关的新功能，新特性的描述性提案文档。

在[EIP-1](https://eips.ethereum.org/EIPS/eip-1)中，EIP定义如下

> EIP stands for Ethereum Improvement Proposal. An EIP is a design document providing information to the Ethereum community, or describing a new feature for Ethereum or its processes or environment. The EIP should provide a concise technical specification of the feature and a rationale for the feature. The EIP author is responsible for building consensus within the community and documenting dissenting opinions.
>
> EIP 是以太坊改进提案（Ethereum Improvement Proposal）的缩写。EIP 是向以太坊社区提供信息，或描述以太坊本身的新功能，或描述以太坊流程或环境的新特性的设计文档。 EIP 应提供该功能的简明技术规范和该功能的基本原理。 EIP 作者负责在社区内建立共识并记录不同意见。

EIP的提案来源于社区，绝大多数EIP提案并未被实际采用。对于一个提案，从想法到被实际采纳，需要经过 <ruby>
草稿<rp>(</rp><rt>Draft</rt><rp>)</rp></ruby> → <ruby>评阅<rp>(</rp><rt>Review</rt><rp>)</rp></ruby> → <ruby>终审<rp>(</rp><rt>Last Call</rt><rp>)</rp></ruby> → <ruby>定稿<rp>(</rp><rt>Final</rt><rp>)</rp>
</ruby> 四个阶段。

关于EIP的提案流程，详见 [EIP-1 #EIP Work Flow](https://eips.ethereum.org/EIPS/eip-1#eip-work-flow)

### EIP和ERC

*EIP: Ethereum Improvement Proposal*
*ERC: Ethereum Request for Comments* 

许多人会混淆EIP和ERC，因为在媒体报道中二者有时会互用，但实际上ERC是EIP中的一类，特指应用层面的标准。

在[EIP-1](https://eips.ethereum.org/EIPS/eip-1)中，对ERC的定义如下。

> **ERC**: application-level standards and conventions, including contract standards such as token standards ([EIP-20](https://eips.ethereum.org/EIPS/eip-20)), name registries ([EIP-137](https://eips.ethereum.org/EIPS/eip-137)), URI schemes, library/package formats, and wallet formats.
>
> **ERC**：应用程序层级的标准和约定，包括合约标准，例如代币标准（[EIP-20](https://eips.ethereum.org/EIPS/eip-20)）、名称注册（[EIP-137](https: //eips.ethereum.org/EIPS/eip-137))、URI 方案、库/包格式和钱包格式。

几种常见的ERC标准

> - [ERC20](https://docs.openzeppelin.com/contracts/4.x/erc20): 尽管受限于其本身的简洁性，ERC20仍是同质化资产的最广泛的代币标准。
>
> - [ERC721](https://docs.openzeppelin.com/contracts/4.x/erc721): 非同质化代币的实际解决方案，通常应用于收藏品和游戏。
>
> - [ERC777](https://docs.openzeppelin.com/contracts/4.x/erc777): 更丰富的同质化代币标准，基于以往的学习实践所建立，支持一些新的功能。向后兼容 ERC20。
>
> - [ERC1155](https://docs.openzeppelin.com/contracts/4.x/erc1155): 一种多代币的新标准，允许单个合约代表多个同质化和非同质化的代币，并支持批量操作以提高gas效率。
>
>   <p align=right>——原文见OpenZeppelin文档: <a href="https://docs.openzeppelin.com/contracts/4.x/tokens#different-kinds-of-tokens">Tokens</a></p>





# [EIP-20: 代币标准 （中译）](https://eips.ethereum.org/EIPS/eip-20)

| 作者     | [Fabian Vogelsteller](mailto:fabian@ethereum.org), [Vitalik Buterin](mailto:vitalik.buterin@ethereum.org) |
| -------- | ------------------------------------------------------------ |
| 状态     | Final                                                        |
| 类型     | Standards Track                                              |
| 类别     | ERC                                                          |
| 创建日期 | 2015-11-19                                                   |

## 目录

- [简要概括](#简要概括)
- [摘要](#摘要)
- [动机](#动机)
- [规范](#规范)
- [代币](#代币)
  - [函数](#函数)
  - [事件](#事件)
- [实现](#实现)
- [历史](#历史)
- [版权](#版权)


##  简要概括

代币(Token)<ruby>代币<rp>(</rp><rt>Token</rt><rp>)</rp></ruby>的一种标准接口。

##  摘要

以下的标准涉及在智能合约中进行代币标准API的实现。此标准提供了转移代币的基本功能，并允许代币获得授权，以供链上第三方使用。

##  动机

标准接口允许以太坊上的任何代币被其他应用程序（诸如钱包和去中心化交易所等）所重用。

##  规范

##  代币

###  函数

**注意**:

- 以下规范使用Solidity `0.4.17` （及以上版本）的语法。
- 调用者必须对`returns (bool success)`返回`false`进行处理。调用者不能假定`false`不会被返回。

####  `name`函数（可选）

返回代币的名称，例如：`MyToken`。

*可选 - 此方法可用于提高可用性，但接口和其他合约不得认为这些值必然存在。*

```solidity
function name() public view returns (string)
```

####  symbol 函数（可选）

返回代币名称的缩写，例如：`HIX`。

*可选 - 此方法可用于提高可用性，但接口和其他合约不得认为这些值必然存在。*

```solidity
function symbol() public view returns (string)
```

####  decimals 函数（可选）

返回代币使用的小数位数，例如`8`，意味着要将代币金额除以`100000000` 来获得其在用户处的表示。

*可选 - 此方法可用于提高可用性，但接口和其他合约不得认为这些值必然存在。*

```solidity
function decimals() public view returns (uint8)
```

> 译者注：`decimals`的作用仅限于展示，ERC20默认的小数位数是18位，关于对`decimals`函数的理解可参考[OpenZeppelin-ERC20: A note on decimals](https://docs.openzeppelin.com/contracts/4.x/erc20#a-note-on-decimals)

####  totalSupply 函数

返回代币的总供应量。

```solidity
function totalSupply() public view returns (uint256)
```

####  balanceOf 函数

返回地址为`_owner`的账户的余额。

```solidity
function balanceOf(address _owner) public view returns (uint256 balance)
```

####  transfer 函数

转账，向地址`_to`转移`_value`数量的代币，并且必定触发`Transfer`事件。如果函数调用者的账户没有足够的余额，该函数会抛出异常（`throw`）。

*注意*：数量为0的转账也应当被认为是正常的，并且触发`Transfer`事件。

```solidity
function transfer(address _to, uint256 _value) public returns (bool success)
```

####  transferFrom 函数

从地址`_from`向地址`_to`转移`_value`数量的代币，并且必定触发`Transfer`事件。

 `transferFrom`函数用于提款，允许合约以您的名义转移代币。例如，此函数可用于允许智能合约代表您转移代币，或者以子货币收取费用。此函数应当抛出异常（`throw`），除非账户`_from`已通过某些机制有意地向消息发送者授权不这样做。

*注意*：数量为0的转账也应当被认为是正常的，并且触发`Transfer`事件。

```solidity
function transferFrom(address _from, address _to, uint256 _value) public returns (bool success)
```

####  approve 函数

允许`_spender`从您的帐户中多次提款，最高额度为`_value`。 如果重新调用此函数，它会用新的`_value`覆盖当前额度。

**注意**：为防止如[这篇文章](https://docs.google.com/document/d/1YLPtQxZu1UAvO9cZ1O2RPXBbT0mooh4DYKjA_jp-RLM/)所描述的攻击向量（对攻击向量的讨论见[这里](https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729)），客户端应当确保以这样一种方式来创建用户界面：即对于同一个消费者，首先将额度设置为`0`，然后再设置为其他值。尽管如此，合约本身不应当对其作出强制，以实现对以往部署的合约的后向兼容。

```solidity
function approve(address _spender, uint256 _value) public returns (bool success)
```

> 译者注：关于`approve`攻击向量，最早于2016年11月29日，由Mikhail Vladimirov和Dmitry Khovratovich的这篇文章：[*ERC20 API: An Attack Vector on the Approve/TransferFrom Methods*](https://docs.google.com/document/d/1YLPtQxZu1UAvO9cZ1O2RPXBbT0mooh4DYKjA_jp-RLM/edit#heading=h.wqhvh2y0obwt)所提出，简要讲就是已获得授权的恶意用户使用更高的Gasprice在抢账户所有者修改额度之前提走两次授权的所有额度，上述将额度设置为0的操作是避免此攻击的一种有效手段，对此攻击解决方案的探讨见[这个github issue](https://github.com/ethereum/EIPs/issues/738)，此攻击的中文描述参考[这里](https://paper.seebug.org/679/)。

####  allowance 函数

返回`_spender`被允许从`_owner`提款的剩余额度.

```solidity
function allowance(address _owner, address _spender) public view returns (uint256 remaining)
```

> 译者注：在OpenZeppelin对ERC20的具体实现上，`allowance`默认为0，其值仅受`approve`和`transferFrom`函数影响，参考[OpenZeppelin-ERC20：allowance](https://docs.openzeppelin.com/contracts/4.x/api/token/erc20#IERC20-allowance-address-address-)

###  事件

####  Transfer 事件

一旦出现代币转账，包括数额为0的转账在内，必然触发`Transfer`事件.

创建新代币的代币合约应当在创建代币时触发一个将 `_from` 地址设置为 `0x0` 的 `Transfer` 事件。

```solidity
event Transfer(address indexed _from, address indexed _to, uint256 _value)
```

> 译者注：`Transfer`可译为转账，创建新代币时`_from`地址为`0x0`的转账类似于凭空印钱，是代币供应量的原始来源。

####  Approval 事件

所有对`approve(address _spender, uint256 _value)`函数的成功调用，都必然触发`Approve`事件

```solidity
event Approval(address indexed _owner, address indexed _spender, uint256 _value)
```

> 译者注：`Approval`可译为授权。

##  实现

目前以太坊网络上已部署了大量符合ERC20标准的代币。不同的团队编写了ERC20的不同实现，各自着有不同的权衡：有些侧重节省矿工费，有些侧重提高安全性。

####  以下是一些实现案例：

- [OpenZeppelin 实现](https://github.com/OpenZeppelin/openzeppelin-solidity/blob/9b3710465583284b8c4c5d2245749246bb2e0094/contracts/token/ERC20/ERC20.sol)
- [ConsenSys 实现](https://github.com/ConsenSys/Tokens/blob/fdf687c69d998266a95f15216b1955a4965a0a6d/contracts/eip20/EIP20.sol)

##  历史

与此标准相关的历史链接：

- Vitalik Buterin的原始提案: https://github.com/ethereum/wiki/wiki/Standardized_Contract_APIs/499c882f3ec123537fc2fccd57eaa29e6032fe4a
- Reddit讨论: https://www.reddit.com/r/ethereum/comments/3n8fkn/lets_talk_about_the_coin_standard/
- 原始发布 #20: https://github.com/ethereum/EIPs/issues/20

##  版权

通过[CC0](https://eips.ethereum.org/LICENSE)放弃版权和相关权利。

## 引用

请如下对此文档进行引用：

[Fabian Vogelsteller](mailto:fabian@ethereum.org), [Vitalik Buterin](mailto:vitalik.buterin@ethereum.org), "EIP-20: Token Standard," *Ethereum Improvement Proposals*, no. 20, November 2015. [Online serial]. Available: https://eips.ethereum.org/EIPS/eip-20.





实现举例：

使用 [OpenZeppelin Contracts Wizard](https://docs.openzeppelin.com/contracts/4.x/wizard) 创建的名为`CyberDJ`，缩写为`CDJ`，初始流通量为`100`的ERC20标准代币:

Remix工程文档见：

部署在测试网，合约地址: 0xD038D129aFBe391ad360caC11F07E806Ac540D4c





### 参考与延伸

[ERC20原文: EIP-20: Token Standard](https://eips.ethereum.org/EIPS/eip-20) 

[Ethereum官网对ERC-20代币标准的介绍](https://ethereum.org/en/developers/docs/standards/tokens/erc-20/)

https://docs.openzeppelin.com/contracts/4.x/erc20

OpenZeppelin属实学习宝库



https://yuanxuxu.com/2018/06/28/openzeppelin-erc721-code-analysis/



https://github.com/sec-bit/awesome-buggy-erc20-tokens/blob/master/ERC20_token_issue_list_CN.md



理解ETH打包机制

https://medium.com/blockchannel/life-cycle-of-an-ethereum-transaction-e5c66bae0f6e



https://github.com/kadenzipfel/smart-contract-attack-vectors
