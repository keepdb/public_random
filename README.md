# Public Random

**公开、可验证、低成本的社会可信随机数方案**

> 每分钟可产出 · 多链混合 · 承诺揭示 · 7 天延迟开奖

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![GitHub](https://img.shields.io/badge/GitHub-keepdb%2Fpublic__random-blue)](https://github.com/keepdb/public_random)

---

## 这是什么？

Public Random 是一个**开放、可验证、不依赖单一中心化服务**的随机数生成方案。

它通过以下组合实现社会可信的随机性：

1. **提前公开承诺值**（Commit）
2. **多链区块数据持续混合**（Ethereum + Bitcoin + 其他链）
3. **到达目标时间后揭示盐值**（Reveal）
4. **最终种子公开可复现**

任何人都可以独立验证结果，无需信任任何单一方。

---

## 官方定位

本仓库是 **Public Random 的官方规范、参考实现与承诺值注册入口**。

| 内容 | 说明 |
|------|------|
| **方法与规则** | 完全公开，任何人可按相同规则自行实现与验证 |
| **代码（SDK / CLI / 工具）** | MIT 许可，可自由使用、修改、集成 |
| **官方承诺值** | **仅以本仓库 [`commitments/`](./commitments/) 目录为准** |
| **命名与品牌** | “Public Random” 指本项目的官方实现与注册；非官方实现请勿使用易混淆名称冒充官方 |

我们欢迎生态使用与兼容实现，但**社会可信的权威记录只认本仓库**。  
这样既能保持开放，又能避免信任焦点被稀释。

---

## 核心设计

### 流程概览

```text
1. 提前公开承诺值
   commitment = keccak256(target_time || salt)

2. 从 hash₀ = 0x00...00 开始
   持续多轮混合：
   nextHash = keccak256(
     prevHash +
     ETH区块数据 +
     BTC区块数据 +
     其他链区块数据
   )

3. 到达目标时间后
   - 公开 salt
   - 验证 commitment
   - 计算最终种子：
     final_seed = keccak256(hash_final || salt)
