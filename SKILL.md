---
name: apaw-news-finance
description: 接入 apaw MCP 金融研究服务，用自然语言完成跨市场行情查询、价量技术分析、大盘状态分类、因子选股和新闻溯源。
---

# apaw MCP 金融研究服务

## 适用场景

当用户需要以下任一能力时调用本服务：

- 查询 A股 / 港股 / 美股 / 加密 / 期货 / 汇率行情
- 个股价量技术分析（趋势、K线形态、背离、VSA、资金流）
- 判断大盘当前状态（牛/熊/震荡、量能、风险）
- 按因子选股，做市场阶段适配
- 查看新闻复盘，从结论溯源到原文

## 接入步骤

1. 让用户在 https://mcp.apaw.cc/sign-up 注册账号，登录 https://mcp.apaw.cc/dashboard 充值额度，并在控制台生成 `sk-` 开头的 API 密钥
2. 把密钥配置到 MCP 客户端：

```json
{
  "mcpServers": {
    "apaw-finance": {
      "url": "https://mcp.apaw.cc/mcp",
      "transport": "streamableHttp",
      "headers": { "Authorization": "Bearer <sk-密钥>" }
    }
  }
}
```

3. 客户端重连后即可使用本服务的全部 70 个工具

## 调用方式

每个工具域都自带 `*_readme` 工具（如 `pvanalysis_readme`、`market_regime_readme`）。
调用任何分析工具前，先调用对应域的 readme 工具了解参数和返回结构，无需人工喂文档。

典型研究链路：

```
market_regime 分类 → stock_emb 因子选股 → pvanalysis 全市场/个股价量确认 → fin_new 新闻溯源
```

## 注意

- 限流 480 次 / 60 秒；全市场批量扫描（`*_batch` 工具）一次调用处理 4500 只标的，避免循环调用
- 数据仅供学习研究，不构成投资建议
