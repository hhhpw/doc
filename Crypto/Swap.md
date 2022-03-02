## 何为 Swap

基础公式

X\*Y = K

其中，X、Y 代表池子中代币 A 和代币 B 数量

价格：Pa = Y/X Pb = X/Y

1、买入 A 代币后的计算：

买入 n 个 A 代币，需要支付的 B 代币数量为 m，

由 （X-n）\* （Y+m）= K

得出 m = K/（X-n）- Y

价格变化为：Pa' = K/（X-n）²

2、卖出 a 个 A 代币，可以得到的 B 代币数量 b 为，

由 （X+a）\* （Y-b）= K

b = Y - K/(X+a)

价格变化为：Pa' = K/（X+a）²

实例说明：

假设我们在 Uniswap 的某个池里投入 50 个 STC 和 50 个 USDT 。

假设市场中 STC 与 USDT 的汇率刚好是 1:1。因为该 Uniswap 资金池中分别有 50 个 STC 和 50 个 USDT，因此，按上述常数积的等式规则，a \* b = 2500 。对于任何交易，Uniswap 都需要保证，池中库存的 STC 数和 USDT 数相乘等于 2500。

假设一位客户进入我们的 Uniswap 池来买一个 STC。她应该支付多少个 USDT 呢？如果她买走一个 STC，我们的池里就剩下 49 个 STC，而 49\* b 依然需要等于 2500。这样 USDT 的总数 b 就等于 51.02。由于之前池中有 50 个 USDT，因此我们还需要 1.02 个 USDT 。

价格也从 1 变成了 51.02/49 = 1.04 USDT

## 做市算法

- AMM（Automated Market Maker 自动做市）

[参考资料](https://www.zhiguf.com/focusnews_detail/266844)
代表 DEX： uni、sushi

- PMM（Proactive Market Maker 主动做市）

  1.资金利用率高，PMM 跟 AMM 一样，都是在零到正无穷大的价格范围内提供流动性，但是 PMM 价格曲线在预言机价格（市价）附近的区域明显更平坦。也就是说，PMM 将大多数资金聚集在市价附近，可以满足更加活跃和频繁的交易，从而提高了资金利用率。

  2.单一风险敞口，PMM 允许做市商存入某交易对的任意单边资产，而不用承担持有两种资产的风险；比如为 ETH-USDT 交易对提供流动性，在 AMM 中，如果 ETH 下跌，你要承担 ETH 下跌的带来的账面损失，而当 ETH 上涨，你的 ETH 又会减少。在 DODO 这里则不会出现，你只需要持有一种资产即可参与做市，从而避免了无常损失带来的损失。

  3.交易滑点低，更低的交易成本。通过 PMM 引入预言机指导价格，将更多资金聚集在市场中间价附近，所以可以提供

  Base Token 和 Quote Token#
  Base 和 quote 是两个被反复提到的概念，有两个方法可以区分他们：
  在交易对中，base 是连字符前边的代币，quote 是后边的。
  在交易中，价格往往是表示多少个 quote token 可以买 1 个 base token 。
  比如，在 ETH - USDC 交易对中， ETH 是 base token , quote token。
  PMM 参数#
  PMM 算法中有 4 个参数：

  B0：base token 回归目标值 - 做市商总充值
  Q0​：quote token 回归目标值 - 做市商总充值
  B：base token 资产 - 当前资产池 base token 中的数量
  Q：quote token 资产 - 当前资产池中 quote token 中的数量

  在最初没有任何交易的时候，资产池处于平衡的状态。base token 和 quote token 位于回归目标。即 B=B0 和 Q=Q0
  ​
  当交易者售出 base token 时，base token 资产池的余额高于回归目标；相反 quote token 资产池余额低于回归目标；在这种情况下，PMM 将会尝试出售多出的 base token ，让资产池回归到平衡状态。

  同理，当交易者购入 base token 时，quote token 资产池中的余额会高于回归目标；base token 余额低于回归目标。PMM 将会尝试出售多出的 quote token，让资产池回归平衡状态。

  参数 R 会在回归过程起到非常关键的作用。资产池越偏离平衡状态，R 就越偏离 1。当 PMM 给出的价格与市价存在差价时，套利者就可以来搬砖帮助资产池回到平衡状态。

## 如何做市
