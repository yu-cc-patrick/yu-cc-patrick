# yu-cc-patrick

**Crypto market data systems — from raw exchange and on-chain data to executable signals.**

我在做加密市场的自动化数据管道和预警系统。不追求大而全，偏好小而完整、每天真在运行、结果可验证的东西。

## 精选项目

### 01 · 资金费率极端事件研究

**772 个币种 · 11 次极端事件 · 3 类驱动机制**

扫描币安全部 USDT-M 永续的历史资金费率，定位「结算周期缩短到 1h + 负费率」的极端事件，并追踪事件结束后的价格走势。

结论：**高负费率通常是陷阱，不是做多机会。** A 类（暴涨逼空）和 B 类（暴雷砸盘）事件退出后 14 天价格继续下跌 21–34%，吃费率完全覆盖不了本金回撤；只有 C 类（股票永续休市）的结构性折价才是可重复的机会。

`Python` `binance.vision` `matplotlib` · [Repository](https://github.com/yu-cc-patrick/binance_funding_research)

### 02 · 资金费率全市场预警 · 运行中

**全部 USDT-M 永续 · 每 5 分钟 · 241 commits**

每 5 分钟轮询币安全部永续合约，检测「结算周期降到 1h」和「|费率| > 1%」两类事件，状态切换时推送 Telegram。

工程上的关键点：币安封锁美区 IP（HTTP 451），而 GitHub Actions runner 在美区。系统用一个部署在 Render 新加坡区的 relay 做透明中转，让免费 CI 也能稳定跑。

`Python` `GitHub Actions` `Binance fAPI` `Render` · [Repository](https://github.com/yu-cc-patrick/funding-rate-alert)

### 03 · 链上美股永续预警 · 运行中

**约 87 个标的 · 自动识别 · 77 commits**

同一套架构，监控范围收窄到 `underlyingType == EQUITY` 的链上美股永续（AAPLUSDT、MSFTUSDT 等）。标的由交易所 `exchangeInfo` 自动识别，新上市自动纳入，无需维护清单。

单独做一个的原因：美股永续在传统市场休市时段缺少定价锚，费率行为和加密原生币完全不同。

`Python` `GitHub Actions` `Binance fAPI` · [Repository](https://github.com/yu-cc-patrick/binance-equity-funding-alert)

### 04 · Solana 庄钱包追踪 · 运行中

**存活 ≥6h · 市值 ≥$300K · 前 20 大持仓 · 58 commits**

每 15 分钟从 DexScreener 发现 Solana 新代币，筛选存活 ≥6 小时且市值 ≥$300K 的标的，通过 Solana RPC 拉取前 20 大持仓地址，新地址实时推送 Telegram。

核心思路：用「同一地址在多个币的前 20 大持仓中反复出现」作为行为指纹，把隐藏的关联钱包浮出来。

`Python` `Solana RPC / Helius` `DexScreener` · [Repository](https://github.com/yu-cc-patrick/solana-whale-tracker)

### 05 · 全链金狗看板 · 在线

**7 条链 · 每日更新**

每天自动扫描 GMGN 上 SOL / BSC / Base / ETH 等 7 条链的早期 Meme 币，按「至少 3 个 KOL 参与 + 历史最高市值 ≥$5M + 发币 ≤60 天」筛选，生成可直接浏览的看板。

→ **[打开在线看板](https://yu-cc-patrick.github.io/golden-dog-dashboard/)**

> 公开仓库只包含渲染后的页面，抓取脚本与钱包数据留在本地。

### 06 · 放量信号回测

**2,012 个事件 · 168 个质量样本**

把「5 分钟成交量放大 10 倍」定义成可计算的事件：以 30 天滚动均量为基准，聚合连续触发 bar，统计全市场分布并按流动性分层。

这是事件检测研究，不是完整策略回测——它回答"放量事件长什么样、在哪里发生"，下一步才接前瞻收益统计。

`Python` `binance.vision` · [Repository](https://github.com/yu-cc-patrick/volume-spike-research)

### 07 · 元监控

**监控那些监控器的监控器**

GitHub Actions 有两种静默死亡：任务失败没人看，以及仓库 60 天无提交后被平台自动停用。这个项目每天巡检所有预警仓库的 workflow 结论、运行时效和仓库活跃度，异常推 Telegram，正常发心跳。

`Python` `GitHub REST API` `GitHub Actions` · [Repository](https://github.com/yu-cc-patrick/monitor-health)

## 技术栈

**Data** Python · Binance fAPI · Solana RPC (Helius) · DexScreener · binance.vision
**Infrastructure** GitHub Actions · Render · macOS launchd · Telegram Bot API
**Analysis** matplotlib · 滚动窗口统计 · 事件研究

## 联系

Open to conversations on crypto market structure, on-chain data, and AI × crypto.

Email: chenrun437@gmail.com
