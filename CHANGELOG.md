# Changelog

All notable changes to this project will be documented here.

---

## [2026.4] - 2026-04-06

### Core Optimizations

- **引入防穿仓数学引擎 (Newton-Raphson)**
  为 Sell (平仓) 编写了真实的 3 代币一元三次方程求解逻辑。如果遇到大额砸盘，会自动产生滑点，保证池子资金绝对不会变负数。

- **AMM 份额置换逻辑**
  Buy (买入) 不再是单纯的做加法，而是按照 `1 USDC = 1胜 + 1平 + 1负` 的规则自动分配储备。

- **重构赔率公式**
  池子里的数值现在代表"流动性份额"，按照反比算价（份额越稀缺，价格越高），完美契合 AMM 定律。
