# FateStar Ziwei Remote MCP

中文在前，English below.

---

## 中文

> FateStar 托管的紫微斗数（Zi Wei Dou Shu / Purple Star Astrology）远程 MCP 接入文档。支持 Claude / Cursor / 任意远程 MCP 客户端生成完整命盘、6 层运限，并可调用「郑大钱」AI 解读。

**排盘免费、匿名可用**；**郑大钱解读**需 `FSFSKey`，按积分规则计费。
本仓库是远程 MCP 的公开接入说明，不包含本地 stdio server 源码，也不提供当前可用的 `npx` 安装包。

### 接入方式

支持远程 MCP 的客户端，配置同一个 URL 即可：

```jsonc
{
  "mcpServers": {
    "ziwei": { "url": "https://www.fatestar.top/api/mcp" }
  }
}
```

要使用郑大钱解读（付费），加上 `Authorization` Header：

```jsonc
{
  "mcpServers": {
    "ziwei": {
      "url": "https://www.fatestar.top/api/mcp",
      "headers": { "Authorization": "Bearer FSFSKey20260606XXXXXXXXXXXXXXXXXXXX" }
    }
  }
}
```

如果某个 MCP 客户端暂不支持自定义 Header，也可以在调用 `ziwei_reading` 时把 `apiKey` 作为工具参数传入。优先推荐 Header，因为它不会混入普通对话参数。

### 工具列表

| 工具 | 作用 | 计费 |
| --- | --- | --- |
| `ziwei_chart` | 完整本命盘：五行局 / 命主身主 / 十二宫 / 生年四化 / 格局识别 / 夹宫 / 真实出生四柱 | 免费 |
| `ziwei_transits` | 6 层运限：大限 / 小限 / 流年 / 流月 / 流日 / 流时，每层带命宫 + 四化 + 双轨干支 | 免费 |
| `ziwei_reading` | 「郑大钱」AI 解读：在排盘结果上叠加 FateStar 知识引擎与郑大钱人格 | 付费，扣积分 |

排盘（`ziwei_chart` / `ziwei_transits`）匿名免费；只有 `ziwei_reading` 需要 `FSFSKey`。

### 工具参数

三个工具共享出生参数；`ziwei_transits` 多 4 个运限目标，`ziwei_reading` 多一个 `question`。

| 参数 | 必填 | 说明 |
| --- | --- | --- |
| `year` `month` `day` | 是 | 出生年月日（阳历；`calendarType=lunar` 时为农历） |
| `hour` | 是 | 出生小时 `0-23`（24 小时制，不是时辰地支） |
| `gender` | 是 | `male` / `female` |
| `minute` | 否 | 出生分钟，配合 `longitude` 做真太阳时精修 |
| `calendarType` | 否 | `solar`（默认）/ `lunar` |
| `isLeapMonth` | 否 | 农历闰月，仅 `lunar` 有效 |
| `longitude` `timezoneOffset` | 否 | 经度 + 时区，传入后启用真太阳时修正 |
| `targetYear` `targetMonth` `targetDay` `targetHour` | 否 | 仅 `ziwei_transits`：运限目标 |
| `question` | 否 | 仅 `ziwei_reading`：要问郑大钱的问题 |
| `apiKey` | 否 | 仅 `ziwei_reading`：`FSFSKey`，也可由 Authorization Header 提供 |

### 拿一个 Key

排盘不需要 Key。郑大钱解读需要 Key：访问 <https://www.fatestar.top> → 注册免费会员 → 做新手任务领积分 → 开发者中心创建 `FSFSKey`。免费会员每天 3 积分（北京时间 21:00 重置）。

### 双轨干支铁律

运限输出带两套干支，严禁混用：

- **真实干支**：万年历六十甲子，对外讲「X 日签约 / X 月运势」用这个。
- **宫位编号**：紫微五虎遁推算，仅用于推四化，不能当日子说出口。

### 开源边界与 know-how 保护

本仓库只公开远程 MCP 的接入方式，不公开 FateStar 的核心 know-how。

- 公开的是协议说明、工具参数、接入示例和安全建议。
- 不公开本地 server 源码、郑大钱人格训练、私有 prompt、知识库、RAG / rerank 策略、评测集、模型路由、反幻觉规则、计费风控或服务端实现。
- `ziwei_reading` 在 FateStar 服务端执行；MCP 客户端只收到最终结构化结果，不会收到原始 prompt、检索上下文或知识库内容。
- MIT License 只覆盖本仓库中的说明与示例；FateStar 品牌、托管服务、郑大钱人格、知识引擎、训练数据与后台策略仍为 FateStar 私有资产。

### 相关链接

- Skill 版：<https://github.com/LouisLin0723/fatestar-ziwei-skill>
- REST API / 完整开发者文档：<https://www.fatestar.top/docs>

---

## English

> FateStar Ziwei Remote MCP is the hosted remote MCP endpoint for Zi Wei Dou Shu (Purple Star Astrology). It lets Claude, Cursor, and other MCP clients generate full natal charts, six transit levels, and paid Zheng Da Qian AI readings.

Charting is free and anonymous. Zheng Da Qian readings require an `FSFSKey` and are charged by credits.
This repository documents the hosted remote MCP endpoint. It does not contain a local stdio server source tree and does not provide a currently usable `npx` package.

### Connection

For clients that support remote MCP, configure one URL:

```jsonc
{
  "mcpServers": {
    "ziwei": { "url": "https://www.fatestar.top/api/mcp" }
  }
}
```

For paid Zheng Da Qian readings, add an `Authorization` header:

```jsonc
{
  "mcpServers": {
    "ziwei": {
      "url": "https://www.fatestar.top/api/mcp",
      "headers": { "Authorization": "Bearer FSFSKey20260606XXXXXXXXXXXXXXXXXXXX" }
    }
  }
}
```

If a client cannot send custom headers, pass `apiKey` as a `ziwei_reading` tool argument. The header is preferred because it keeps the key out of normal tool arguments.

### Tools

| Tool | Purpose | Billing |
| --- | --- | --- |
| `ziwei_chart` | Full natal chart: Five-Element class, soul/body masters, twelve palaces, birth-year transformations, pattern detection, neighboring-palace checks, true birth pillars | Free |
| `ziwei_transits` | Six transit levels: decade, minor-year, annual, monthly, daily, hourly; each with palace, transformations, and dual Gan-Zhi fields | Free |
| `ziwei_reading` | Zheng Da Qian AI reading powered by FateStar's knowledge engine and persona layer | Paid credits |

`ziwei_chart` and `ziwei_transits` are free and anonymous. Only `ziwei_reading` requires an `FSFSKey`.

### Parameters

All tools share birth parameters. `ziwei_transits` adds target transit parameters. `ziwei_reading` adds `question`.

| Parameter | Required | Description |
| --- | --- | --- |
| `year` `month` `day` | Yes | Birth date. Solar by default; lunar when `calendarType=lunar` |
| `hour` | Yes | Birth hour `0-23`, using the 24-hour clock |
| `gender` | Yes | `male` / `female` |
| `minute` | No | Birth minute, used with `longitude` for true solar time |
| `calendarType` | No | `solar` default / `lunar` |
| `isLeapMonth` | No | Leap lunar month, only valid for lunar input |
| `longitude` `timezoneOffset` | No | Enables true solar time correction |
| `targetYear` `targetMonth` `targetDay` `targetHour` | No | Transit target fields, only for `ziwei_transits` |
| `question` | No | Reading question, only for `ziwei_reading` |
| `apiKey` | No | `FSFSKey`, only for `ziwei_reading`; can also be provided by Authorization header |

### Get a Key

Charting does not need a key. Zheng Da Qian readings need one. Visit <https://www.fatestar.top>, sign up, claim starter credits, and create an `FSFSKey` in the developer center. Free members receive 3 credits per day, resetting at 21:00 Beijing time.

### Dual Gan-Zhi Rule

Transit output contains two Gan-Zhi fields. Do not mix them:

- **Real Gan-Zhi**: use this when speaking about calendar dates.
- **Palace code**: used only for Ziwei transformation calculations. Do not present it as a calendar date.

### Open-Source Boundary and Know-How Protection

This repository documents how to connect to FateStar Remote MCP. It does not publish FateStar's core know-how.

- Public content includes protocol docs, tool parameters, examples, and security guidance.
- It does not include local server source code, Zheng Da Qian persona training, private prompts, knowledge base content, RAG / reranking logic, evaluation sets, model routing, anti-hallucination rules, billing controls, or backend implementation.
- `ziwei_reading` runs on FateStar's hosted server. MCP clients receive the final structured result, not raw prompts, retrieval context, or private corpus content.
- The MIT License applies to this repository's docs and examples. FateStar's brand, hosted service, Zheng Da Qian persona, knowledge engine, training data, and backend strategy remain proprietary FateStar assets.

### Links

- Skill version: <https://github.com/LouisLin0723/fatestar-ziwei-skill>
- REST API / full developer docs: <https://www.fatestar.top/docs>

---

MIT licensed. Powered by [FateStar](https://www.fatestar.top).
