# DeFi 项目判断

## 指标分析

### TVL（Total Value Locked）

- **意义**：锁仓资产总额，反映了项目规模和用户信任度。
- **怎么用**：
  - **纵向比较**：同一协议的历史走势（是否稳步增长还是短期暴涨暴跌）。
  - **横向比较**：和同类协议比（Curve vs Uniswap vs Balancer）。
  - **增长来源**：TVL 涨是因为币价涨，还是因为新增资金流入（需要链上数据交叉验证）。
- **工具**：
  - DefiLlama → 看多链 TVL 历史和分布
  - Token Terminal → TVL + 收益

### Token 分布 & 持仓集中度

- **意义**：看一个代币的筹码是否过于集中，防止被大户控盘。
- **核心指标**：
  - 前 10 大钱包持有比例（>40% 非常危险）
  - 团队/投资人解锁节奏（看 Token Unlocks 或 Messari）
  - 流通比例（总供应 vs 流通）
- **工具**：
  - DeBank → 查项目方钱包和大户
  - Etherscan → Token Holder 分布

## 空气盘

- **协议逻辑**
  - 收益来自真实业务（交易手续费、借贷利息、质押奖励）还是只是发新币？
  - 如果收益主要来自新发行的 Token → 很可能是“资金盘”。
- **激励来源**
  - 激励是临时拉新还是有长期业务支持？
  - 如果激励停了，用户还会用吗？
- **资金流动**
  - 资金是从外部用户流入，还是内部账户互转刷数据？
  - 看链上转账路径（Dune 仪表盘常有现成分析）

## 套利/轮动策略

- 流动性搬砖（跨平台套利）
  - 思路：不同平台价格有差 → 低买高卖
  - 限制：需要速度（机器人）+ 考虑 Gas
- 工具：1inch、Matcha（帮你自动找最佳路径）
- APY 套利
  - 思路：相同资产在不同平台的年化收益差
  - 例：USDC 在 Aave 3% APY，Curve 7% APY → 搬过去赚更多利息
- LP 再质押复利
  - 思路：把 LP Token 存去借贷平台 → 抵押借出稳定币 → 再投回 LP
  - 风险：价格波动 → 爆仓

## 风控系统

### 止盈

- 设置目标收益（如 +20%）自动撤资
- 使用 DeBank 的“监控”功能

### 风险

- Rug：查合约是否可升级、是否有 owner 权限
- 钓鱼：不要乱签合约
- 波动风险：稳定币池 vs 高波动代币池

## Aave实战分析

### Why Aave

- **代表性强且资料齐全** — Aave 是 DeFi 最大的借贷市场之一，TVL/收入/治理/多链都做得很成熟，适合做“从基础到进阶”的尽调实操。[DefiLlama](https://defillama.com/protocol/aave)[Token Terminal](https://tokenterminal.com/explorer/projects/aave?utm_source=chatgpt.com)
- **数据源丰富** — 可在 DefiLlama、Token Terminal、Dune、Etherscan 等多渠道交叉验证（正好满足你学会用这些工具的目标）。[DefiLlama](https://defillama.com/protocol/aave)[Token Terminal](https://tokenterminal.com/explorer/projects/aave/metrics/revenue?utm_source=chatgpt.com)[Dune](https://www.dune.com/alecaruso/aave-v3-dashboard?utm_source=chatgpt.com)
- **风险/事件样本充足** — Aave 有“外围合约被利用”“多次审计与补丁”等事件，是练习如何判定‘协议内核安全 vs 外围风险’的好对象。[SolidityScan](https://blog.solidityscan.com/aave-repay-adapter-hack-analysis-aafd234e15b9?utm_source=chatgpt.com)[aave.com](https://aave.com/security?utm_source=chatgpt.com)

### 数据快照

- **总 TVL（Combined）**：约 **$38.52B**；以太坊占主导（≈ $34.6B），其余在 Arbitrum、Base、Avalanche 等链上分布。[DefiLlama](https://defillama.com/protocol/aave)
- **年化费用 / 年化收入（平台层面）**：Fees（Annualized）≈ **$891.5M**；Revenue（Annualized）≈ **$126.6M**（DefiLlama 报表）；Token Terminal 也有可交叉的收入细分数据。[DefiLlama](https://defillama.com/protocol/aave)[Token Terminal](https://tokenterminal.com/explorer/projects/aave/metrics/revenue?utm_source=chatgpt.com)
- **借出总额（被借走）**：约 **$27.43B**（表示借贷市场活跃）。[DefiLlama](https://defillama.com/protocol/aave)
- **Protocol Treasury**：约 **$243M**（可用于回购/治理激励/短缺应急）；AAVE 代币总量上限 16,000,000，持币地址数庞大。[DefiLlama](https://defillama.com/protocol/aave)[Ethereum (ETH) Blockchain Explorer](https://etherscan.io/token/0x7fc66500c84a76ad7e9c93437bfc5ac33e2ddae9?utm_source=chatgpt.com)
- **安全事件示例**：2024 年外围 Repay Adapter 被利用造成约 $56k（外围/非核心合约），说明外围合约风险值得关注。[SolidityScan](https://blog.solidityscan.com/aave-repay-adapter-hack-analysis-aafd234e15b9?utm_source=chatgpt.com)

### 步骤 1 — TVL 与 链上分布（DefiLlama）

- **为什么**：TVL 显示有多少用户资金信任协议；链分布揭示跨链风险与流动性集中度。
- **在哪看**：打开 `https://defillama.com/protocol/aave`。 [DefiLlama](https://defillama.com/protocol/aave)
- **怎么做（可复现）**：
  - 进页面后看最上方“Total Value Locked”与“TVL by Chain”表格。
  - 点 “TVL” 图表右上角 → 下载 CSV 或在页面用时间选择器查看历史。
  - （可选）用 DefiLlama API 抓取：参考 DefiLlama API 文档（`/protocol/{slug}`），或者直接调用 `https://api.llama.fi/protocols` 再过滤 `slug: aave`。（查 docs 在 DefiLlama API）[DefiLlama API Docs](https://api-docs.defillama.com/?utm_source=chatgpt.com)[DefiLlama](https://docs.llama.fi/coin-prices-api?utm_source=chatgpt.com)
  - ![img](https://jweybcsoid.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDNlODlkMDllZmIwMDk5ZjE1OTQ1YjA0YzM4M2E5ZDRfb2Ezczh1dTFWcG9kcEh1OHprV3B0RmE4WW40NTBKakxfVG9rZW46VzZDQ2JuN3NNb3FRSmh4bnFaMWMzcXlFbnNiXzE3NTQ4MTU3MDk6MTc1NDgxOTMwOV9WNA)
- **如何解读**：
  - TVL 突增但同时没有收入上升 → 可能是短期激励（airdrop / farming）导致的“虚假 TVL”
  - TVL 主要集中在单一链（例如 Ethereum）→ 跨链风险相对小但主网拥堵时成本高；分散在多链→ 需要检查每条链的市场深度与 oracle 风险。

### 步骤 2 — 收入 / 费率（Token Terminal + DefiLlama）

- **为什么**：TVL 大不等于可持续营收，想知道协议“能不能自洽”要看 fee → treasury → earnings。
- **在哪看**：Token Terminal 的 Aave 页面 （Revenue / Fees）和 DefiLlama 的 Fees/Revenue 列表。[Token Terminal](https://tokenterminal.com/explorer/projects/aave/metrics/revenue?utm_source=chatgpt.com)[DefiLlama](https://defillama.com/protocol/aave)
- **怎么做**：
  - 打开 Token Terminal 的 Aave 页面，查看最近 30d / 365d 的 Revenue、Fees。[Token Terminal](https://tokenterminal.com/explorer/projects/aave?utm_source=chatgpt.com)
  - 计算关键比率：`Revenue / TVL`（年化或 30d），`Fees / TVL`。
    1. 示例：用刚抓的数值：Revenue ≈ $126.58M，TVL ≈ $38.523B → `126.58M / 38.523B ≈ 0.328%` 年化（约 0.33%）。[DefiLlama](https://defillama.com/protocol/aave)
    2. ![img](https://jweybcsoid.feishu.cn/space/api/box/stream/download/asynccode/?code=YTY4MzY5NjM4NTAzMWJiNDU4MTdkOWIyMmVhNDZlZDVfdUhGQ28zZEhqVEFTZkhTd01rOVZScmQ5UEZFVThpaWtfVG9rZW46SDViVGIxQzhVb2lHeWN4a3NoamNYZDQ2bkNiXzE3NTQ4MTU3MDk6MTc1NDgxOTMwOV9WNA)
- **如何解读**：
  - `Revenue/TVL` 很低（<1%）→ 表示用户通过放贷赚到的利息总体不高，协议的盈利能力偏小（这在借贷协议中常见）；但高于同类协议则可能更健康。
  - 看 `Fees` 与 `Incentives` 的比率：若激励（发代币补贴）远高于收入，说明协议大量依赖补贴拉动使用，停激励风险大。

### 步骤 3 — Tokenomics 与 持仓集中（Etherscan、DefiLlama Unlocks）

- **为什么**：代币分布/解锁节奏直接影响治理被控与抛售压力。
- **在哪看**：AAVE 代币页（Etherscan）查看 `Holders`、`Transfers`；DefiLlama / Token Logic 查看 Treasury / unlock 日历。[Ethereum (ETH) Blockchain Explorer](https://etherscan.io/token/0x7fc66500c84a76ad7e9c93437bfc5ac33e2ddae9?utm_source=chatgpt.com)[DefiLlama](https://defillama.com/protocol/aave)
- **怎么做**：
  - 打开 Etherscan 的 AAVE Token 页面 → 点击 “Holders” → 导出或记录前 10/20 地址的占比。[Ethereum (ETH) Blockchain Explorer](https://etherscan.io/token/0x7fc66500c84a76ad7e9c93437bfc5ac33e2ddae9?utm_source=chatgpt.com)
  - ![img](https://jweybcsoid.feishu.cn/space/api/box/stream/download/asynccode/?code=N2QxZTgzYTNlYmU1OTZkZTc5ZjJmYjM4MTBiYzBjNmZfY3liaFM2aXA1SnNjemVuYWNiWWNra0VkcXJMR1dpNWtfVG9rZW46TXlJZWJIQVFNb1dNWmx4dlVmWWM0TWF2bnZkXzE3NTQ4MTU3MDk6MTc1NDgxOTMwOV9WNA)

  - 检查“Own Tokens / Treasury”项目（DefiLlama 有列出 Protocol Own Tokens），看 DAO 手里有多少代币可用于回购/激励。[DefiLlama](https://defillama.com/protocol/aave)
  - ![img](https://jweybcsoid.feishu.cn/space/api/box/stream/download/asynccode/?code=NDNhNjI3MjMwZTdlNzUzYTM0OWM1MjY3YmE2NjZhNTdfUFJFWUE1NVQxbW5ldnNwM0hLeXNYekpTMDMwR0cxNGtfVG9rZW46U2hRQmJpSEhib3RpQnV4TG9TemNiMmdRblZiXzE3NTQ4MTU3MDk6MTc1NDgxOTMwOV9WNA)

  - 去项目 Unlocks（日历）看未来 6–12 个月的代币解锁量（大解锁月构成抛售风险）。
- **如何解读**：
  - 若前 10 地址合计占比 > 30–40%，应警惕大户操纵或抛售风险。
  - 若 DAO / Treasury 持大量代币 → 可用于长期激励或回购，但也要看治理投票动力（是否分散）。

### 步骤 4 — 用户行为与市场指标（Dune）

- **为什么**：看谁在用（活跃用户）、借贷利用率、清算频率等，判断市场健康与实时风险。
- **在哪看**：Dune 上 Aave 的公开 dashboard（很多作者做了很好的分析板）。示例：`Aave V3 Dashboard`、`AAVE Liquidations` 等。[Dune](https://www.dune.com/alecaruso/aave-v3-dashboard?utm_source=chatgpt.com)[Dune](https://dune.com/KARTOD/AAVE-Liquidations?_bhlid=aab7d45c1767eb996feae8df4c92dfbfe2ef5ba1&utm_campaign=aave-up-6m-in-bloodbath&utm_medium=newsletter&utm_source=chatgpt.com)
- **怎么做**（复现一个常用查询）：
  - 打开 Dune 上的 Aave Dashboard（例如 `https://dune.com/alecaruso/aave-v3-dashboard`）。 [Dune](https://www.dune.com/alecaruso/aave-v3-dashboard?utm_source=chatgpt.com)
  - 查 `Daily deposits`, `Daily borrows`, `Utilization rate by asset`, `Daily liquidations`。这些图表通常已做好，你可以直接 `Fork` 或查看 SQL。
  - 如果要自写 SQL，步骤是：在 Dune 新建 Query → 在左侧 Schema 搜索 `aave_v3` → 找到 `deposits/borrows/liquidations` 表 → 写 `SELECT date_trunc('day', block_time) day, sum(amount) FROM aave_v3.deposits GROUP BY day ORDER BY day;`（注意：表名以 Dune 的 schema 为准，先用 Schema Explorer 确认表名与字段）。[Dune](https://dune.com/data/contracts/aave_v3.Pool?utm_source=chatgpt.com)[Dune](https://www.dune.com/allez_labs/aaverateshistory?utm_source=chatgpt.com)
- **如何解读**：
  - 高 `utilization rate`（接近 100%）→ 借款利率上涨、清算风险上升；
  - 突发清算高峰 → 说明杠杆者承压，可能带来连锁清算（短期套利机会也可能出现）。

### 步骤 5 — 安全/合约检查（Etherscan + 官方安全页 + 历史事件）

- **为什么**：核心合约 vs 周边合约的责任不同；外围 bug 常见但对整体风险评估非常重要。[aave.com](https://aave.com/security?utm_source=chatgpt.com)[SolidityScan](https://blog.solidityscan.com/aave-repay-adapter-hack-analysis-aafd234e15b9?utm_source=chatgpt.com)
- **在哪看**：Aave 安全页面（audit 列表）、Etherscan 合约页面（查看是否为 proxy、管理员地址、是否 source verified）、历史安全事件（查 news / security blogs）。[aave.com](https://aave.com/security?utm_source=chatgpt.com)[SolidityScan](https://blog.solidityscan.com/aave-repay-adapter-hack-analysis-aafd234e15b9?utm_source=chatgpt.com)
- **怎么做**：
  - 到 `aave.com/security` 看最新的 audit 报告列表（哪些模块被审核、哪些是 periphery）。[aave.com](https://aave.com/security?utm_source=chatgpt.com)
  - 在 Etherscan 打开 `Pool` 合约 → 检查 “Contract” 是否 Verified、是否使用 Proxy（Proxy 管理者就是额外的信任/治理向量）。[aave.com](https://aave.com/docs/resources/addresses?utm_source=chatgpt.com)
  - 检索最近 12–24 个月的安全事件（Google / security blogs / PeckShield 报告），判断是否为“核心合约被盗”还是“外围工具被利用”。示例：2024 年 Repay Adapter（外围）被利用，影响有限但值得关注外围模块。[SolidityScan](https://blog.solidityscan.com/aave-repay-adapter-hack-analysis-aafd234e15b9?utm_source=chatgpt.com)
- **如何解读**：
  - 若只出现外围合约 exploit（小额），核心合约多年稳定 → 风险相对低，但外围生态（UI、adapter、桥）会影响用户资金路径安全。
  - 若核心合约曾直接被攻击并造成重大短缺 → 高风险，慎入。

### 步骤 6 — 治理 / Treasury / Roadmap（Aave Governance + Dune）

- **为什么**：治理能改变费率、上新资产、部署到新链；Treasury 决定激励长龙头。[Aave](https://governance.aave.com/t/arfc-aave-governance-dashboard-on-dune/22038?utm_source=chatgpt.com)[DefiLlama](https://defillama.com/protocol/aave)
- **在哪看**：governance.aave.com（提案 & 表决历史）、Dune 的 Governance Dashboard（Aave 提案指标）。[Aave](https://governance.aave.com/t/arfc-aave-governance-dashboard-on-dune/22038?utm_source=chatgpt.com)
- **怎么做**：
  - 查看最近 12 个月的 Governance 提案（有哪些通过、哪些未通过、投票参与度如何）。
  - 检查 Treasury 的支出记录（是否常用于奖励而非回购/减债）。DefiLlama 的 Treasury 字段也可以快速查看数值。[DefiLlama](https://defillama.com/protocol/aave)
- **如何解读**：
  - 高参与 + 理性投票 → 治理体制健康；低参与 + 多数由少数地址决定 → 风险较高。
  - Treasury 长期净支出大于收入 → 可持续性疑问（会消耗 DAO 资产）。

### 结论

在上面给出数字：Revenue ≈ $126.58M，TVL ≈ $38.523B

Revenue/TVL = 126,580,000 / 38,523,000,000 ≈ 0.003286 => 0.3286% 年化

对 Aave 来说，协议层年化收入相对 TVL 很小（≈0.33%），这意味着用户放币在 Aave 的总体回报主要来自借款人支付利息（分散到供应端），协议本身也不能靠高费用拿到很大份额。这个比率也告诉我们：若协议通过发代币激励来拉动 TVL，必须留心激励与真实收入的差距（是否可持续）。数据来源：DefiLlama / Token Terminal。[DefiLlama](https://defillama.com/protocol/aave)[Token Terminal](https://tokenterminal.com/explorer/projects/aave/metrics/revenue?utm_source=chatgpt.com)