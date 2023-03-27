### 获取web3实例
```ts
import Web3 from 'web3';

const getWeb3Instance = (chainId: string | number) => {
  let web3;
  if (typeof window !== 'undefined') {
    const url = URLS[Number(chainId)][0];
    console.log('[[getWeb3Instance.url]]', url);
    if (!url) {
      return null;
    }
    // https://goerli.infura.io/v3/{infuraKkey}
    web3 = new Web3(url);
  }

  return web3;
};

```

### 构造合约实例
```ts
const web3Contract = new web3.eth.Contract(
  stakeAbi, // abi json file
  STAKE_CONTRACT_ADDRESS // 合约地址
);

```

### 获取合约方法的gaslimit

```ts

// 获取abi Data
  const web3HashData = web3Contract.methods.myContractMethod([
    // 根据abi结构传递参数
      pubkey,
      withdrawal_credentials,
      signature,
      deposit_data_root,
    ]);

// 计算
  const web3bgasLimit = await web3.eth.estimateGas({
      to: STAKE_CONTRACT_ADDRESS,
      from: walletAddress,
      data: web3HashData.encodeABI(),
      // 看合约是否要求，tranfer value
      value: parseEther('32.05')
    });
    

```

### 获取实时的gasPrice

```ts
 const web3gasPrice = await web3.eth.getGasPrice();
```



### 发送一笔交易

```ts
const res = await contract?.methods
    .myContractMethod([
      pubkey,
      withdrawal_credentials,
      signature,
      deposit_data_root,
    ])
    .send({
      from: walletAddress,
      value: parseEther('32.05')
      to: STAKE_CONTRACT_ADDRESS,
    });

```