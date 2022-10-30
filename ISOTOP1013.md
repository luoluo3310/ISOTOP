# IISOTOP1013合约极简教程

如果你要写Dapp的前端，在使用IISOTOP1013合约时，需要用到`ethers.js`。

## 1. ethers.js简述

`ethers.js`是一个完整而紧凑的开源库，用于与以太坊区块链及其生态系统进行交互。

与更早出现的`web3.js`相比，它有以下优点：

1. 代码更加紧凑：`ethers.js`大小为116.5 kB，而`web3.js`为590.6 kB。

2. 更加安全：`Web3.js`认为用户会在本地部署以太坊节点，私钥和网络连接状态由这个节点管理（实际并不是这样）；`ethers.js`中，`Provider`提供器类管理网络连接状态，`Wallet`钱包类管理密钥，安全且灵活。

3. 原生支持`ENS`。

使用方法也很简单
``` js
import { ethers } from "ethers";
```

## 2. 获得工厂合约对象

要使用IISOTOP1013合约，首先需要获得工厂合约对象
 ```javascript
     const Factory = await ethers.getContractAt('IIsotopFactory',FactoryContractAddr);
 ```
 其中`FactoryContractAddr`是工厂合约地址，`Factory`是生成的工厂合约对象
 
## 3.  查询子合约地址并获得`IISOTOP`对象
  然后查询子合约地址，使用`IISOTOP1013.getContractsDeployed()`方法获得`IISOTOP1013Addr`

  ```js
      let IISOTOP1013AddrArray = new Array();
      IISOTOP1013AddrArray = await Factory.getContractsDeployed();
      IISOTOP1013Addr = IISOTOP1013AddrArray[IISOTOP1013AddrArray.length-1];
  ```
之后根据`IISOTOP1013Addr`获得对象`IISOTOP`，就可以调用它的各种方法了
```js
    const  IISOTOP = await  ethers.getContractAt("IISOTOP1013",IISOTOP1013Addr);
```
## 4.  铸造NFT

获得NFT子合约地址之后，就可以调用IISOTOP1013合约下面的各种方法了，比如mint`IISOTOP.mint(address, uint) `
  ```js
      let  waiter = await  IISOTOP.mint(MyAddr1,5) ; //铸造5个nft
      await  waiter.wait()
  ```
  这里加入`waiter.wait()`的目的是等待区块确认
## 5.  查询某地址拥有的NFT数量
查询address地址拥有的NFT数量使用` IISOTOP.balanceOf(address)`方法
  ```js
      const  balance = await  IISOTOP.balanceOf(address);
  ```
## 6. 转移NFT
转移NFT需要用到`IISOTOP.transferFrom(from,to,tokenID)`方法
  ```js
     let  waiter = await  IISOTOP.transferFrom(from,to,tokenID);
     await  waiter.wait();
  ```
  其中from，to是地址，tokenID是NFT编号
## 7. 查询某NFT ID对应的所有人
查询某NFT对应所有人使用` IISOTOP.ownerOf(tokenID)`方法
  ```js
     owneraddr = await  IISOTOP.ownerOf(tokenID);
  ```
## 8. 查询NFT ID对应的授权地址
查询NFT ID对应的授权地址` IISOTOP.getApproved(tokenID)`方法
  ```js
     approvedaddr = await  IISOTOP.getApproved(tokenID);
  ```
  ## 9. 授权NFT给对应的地址
授权NFT给对应的地址使用` IISOTOP.approved(address,tokenID)`方法
  ```js
    let  waiter = await  IISOTOP.approve(address,tokenID);
    await  waiter.wait();
  ```
## 10. 设置NFT元数据
设置NFT元数据使用` IISOTOP.setBaseURI(baseURI)`方法
  ```js
    await  IISOTOP.setBaseURI(baseURI);
  ```
## 11. 查询账户下所有NFT的ID
查询账户下所有NFT的ID使用` IISOTOP.tokensOfOwner(address)`方法
  ```js
    await  IISOTOP.tokensOfOwner(address);
  ```
## 12 查询账户下ID从A-B中包含的NFT
查询账户下ID从A-B中包含的NFT使用` IISOTOP.tokensOfOwnerIn(address,start,stop)`方法
  ```js
    await  IISOTOP.tokensOfOwnerIn(address,start,stop);
  ```
  其中start表示开始的ID，类型为uint256，stop表示结束的ID，类型为uint256，address表示需要查询的地址
## 13. 查询NFT的使用者user
查询NFT的使用者user`IISOTOP.userOf(tokenID)`方法
  ```js
    await  IISOTOP.userOf(tokenID);
  ```
## 14. 设置NFT的使用者user
设置NFT的使用者user使用` IISOTOP.setUser(tokenID,address,time)`方法
  ```js
    let  waiter = await  IISOTOP.setUser(tokenID,address,time);
    await  waiter.wait();
  ```
其中time为uint64类型
## 15. 检查NFT的租借时间
检查NFT的租借时间使用`IISOTOP.userExpires(tokenID)`方法
  ```js
    await  IISOTOP.userExpires(tokenID);
  ```

