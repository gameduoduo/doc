# 新手入门
--------
## 安装
Ethers的各类（Class）和功能，在[@ethersproject](https://www.npmjs.com/search?q=%40ethersproject%2F)组织之下，是可以手动从子包中输入的，但对于新手来说，伞包（umbrella package）才是最容易地入门方式。
```
/home/ricmoo> npm install --save ethers
```
----
## 输入
### Nodejs          
*<p align="right"> node.js require </p>*
```
const { ethers } = require("ethers");
```
*<p align="right">ES6 or TypeScript </p>*
```
import { ethers } from "ethers";
```
### Web浏览器
基于安全的原因，更好地操作是将[ethers library](https://cdn.ethers.io/lib/ethers-5.1.esm.min.js)拷贝到本地web服务器，从而服务自己。
为了可以更快地试用或进行原型开发，你可以从我们提供的CDS中，加载它到你自己的Web应用中。
*<p align="right">ES6 in the Browser</p>*
```
<script type="module">
    import { ethers } from "https://cdn.ethers.io/lib/ethers-5.2.esm.min.js";
    // Your code here...
</script>
```
*<p align="right">ES3 (UMD) in the Browser</p>*
```
<script src="https://cdn.ethers.io/lib/ethers-5.2.umd.min.js"
        type="application/javascript"></script>

```
------
## 通用术语
这个部分需要工作...
<table>
  <tr>
      <th>Provider</th>
      <td>在ethers中，provider是一个类，这个为一个到Ethereum网的连接提供一个缩略语。provider为到Blockchain和它的状态提供只读访问 。</td>
  </tr>
  <tr>
      <th>Signer</th>
      <td>Signer是一个类，通常通过一些方式直接或间接访问私人密钥，它可以签署信息和业务到授权网络，并向你的账户收取一些执行操作的费用。</td> 
  </tr>
  <tr>
      <th>Contract</th>
      <td>Contract是一个缩略语，代表在Ethereum网络上，一个专门的contract的连接，以便所有应用程序都可以像一个正常的JavaScript对象来使用它。</td> 
  </tr>
  </table> 

---
## 连接到Ethereum：MetaMask
一个最快和最容易地开始在Ethereum上开发的方法是使用[MetaMask](https://blank)。MetaMask是浏览器的扩展。
它主要提供：
* 1个到Ethereum网络的连接（[Provider](https://docs.ethers.org/v5/single-page/#/v5/api/providers/provider/)）。
* 持有你的私人密钥并可以签署信息 （[Signer](https://docs.ethers.org/v5/single-page/#/v5/api/signer/-%23-Signer)）。

*<p align="right"> Connecting to MetaMask</p>*
```
// A Web3Provider wraps a standard Web3 provider, which is
// what MetaMask injects as window.ethereum into each page
const provider = new ethers.providers.Web3Provider(window.ethereum)

// MetaMask requires requesting permission to connect users accounts
await provider.send("eth_requestAccounts", []);

// The MetaMask plugin also allows signing transactions to
// send ether and pay to change state within the blockchain.
// For this, you need the account signer...
const signer = provider.getSigner()
```
----
##连接到Ethereum：RPC
[JSON-RPC API](https://github.com/ethereum/wiki/wiki/JSON-RPC)是和Ethereum进行交互的另一种流行的方法，它可以应用于所有主要的Ethereum节点启动实施（例如[Geth](https://geth.ethereum.org/)和[Parity](](https://www.parity.io/))），和许多第三方的Web业务（如：[INFURA](https://www.infura.io/)）。
它主要提供：
* 1个到Ethereum网络的连接（[Provider](https://docs.ethers.org/v5/single-page/#/v5/api/providers/provider/)）。
* 持有你的私人密钥并可以签署信息 （[Signer](https://docs.ethers.org/v5/single-page/#/v5/api/signer/-%23-Signer)）。
*<p align="right">Connecting to an RPC client</p>* 
```
// If you don't specify a //url//, Ethers connects to the default 
// (i.e. ``http:/\/localhost:8545``)
const provider = new ethers.providers.JsonRpcProvider();

// The provider also allows signing transactions to
// send ether and pay to change state within the blockchain.
// For this, we need the account signer...
const signer = provider.getSigner()
```   
## 查询Blockchain
一旦你有一个[Provider](https://docs.ethers.org/v5/single-page/#/v5/api/providers/provider/),你便拥有了一个到Blockchain的只读连接。你可以用来查询当前状态，抓取历史日志、查看部署的代码等等。
*<p align="right">Basic Queries</p>*
```javascript
// Look up the current block number
await provider.getBlockNumber()
// 16987688

// Get the balance of an account (by address or ENS name, if supported by network)
balance = await provider.getBalance("ethers.eth")
// { BigNumber: "182334002436162568" }

// Often you need to format the output to something more user-friendly,
// such as in ether (instead of wei)
ethers.utils.formatEther(balance)
// '0.182334002436162568'

// If a user enters a string in an input field, you may need
// to convert it from ether (as a string) to wei (as a BigNumber)
ethers.utils.parseEther("1.0")
// { BigNumber: "1000000000000000000" }
```
