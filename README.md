# FateStar Ziwei MCP Server

> 紫微斗数 (Zi Wei Dou Shu / Purple Star Astrology) 排盘引擎 MCP server —— 让 Claude / Cursor / 任意 MCP 客户端直接生成完整命盘 + 调「郑大钱」AI 命理师深度解读。

**排盘免费、匿名**(不要 key);**郑大钱解读**付费(需 `fs_live_` key)。引擎 FateStar 自建,102 颗星 · 三合派四化 · 真太阳时 · 0 第三方排盘库。

两种接入,都是 MCP 标准协议,**无需任何桥接代理**:

- **远程 Streamable HTTP** —— 零安装,配置里填一个 URL
- **本地 stdio** —— `npx @fatestar/ziwei-mcp`,自带引擎,**断网也能排盘**

[English below ↓](#english)

---

## Features

| 工具 | 作用 | 计费 |
| --- | --- | --- |
| `ziwei_chart` | 完整本命盘:五行局 / 命主身主 / 十二宫(主辅煞杂 + 庙旺亮度 + 生年四化 + 空宫借星 + 大限年龄 + 长生十二神)/ 格局识别(含古籍出处)/ 夹宫 / 真实出生四柱 | 免费 |
| `ziwei_transits` | 6 层运限:大限 / 小限 / 流年 / 流月 / 流日 / 流时,每层带命宫 + 四化 + 双轨干支 | 免费 |
| `ziwei_reading` | 「郑大钱」AI 命理师:知识引擎 + 古籍锚定深度断盘问事 | 付费(扣积分) |

排盘(`ziwei_chart` / `ziwei_transits`)匿名免费;只有 `ziwei_reading` 需 `fs_live_` key。

---

## 安装

### 远程 Streamable HTTP(推荐,零安装)

支持 MCP 的客户端,配置里填一个地址即可:

```jsonc
{
  "mcpServers": {
    "ziwei": { "url": "https://www.fatestar.top/api/mcp" }
  }
}
```

要用郑大钱解读(付费),加上 key:

```jsonc
{
  "mcpServers": {
    "ziwei": {
      "url": "https://www.fatestar.top/api/mcp",
      "headers": { "Authorization": "Bearer fs_live_xxxxxxxx" }
    }
  }
}
```

### 本地 stdio(npx,自带引擎,断网可用)

偏好本地进程的客户端,用 npx 拉开源包(引擎已 bundle 进包,排盘 0 网络):

```jsonc
{
  "mcpServers": {
    "ziwei": { "command": "npx", "args": ["-y", "@fatestar/ziwei-mcp"] }
  }
}
```

郑大钱解读把 key 放进 env:

```jsonc
{
  "mcpServers": {
    "ziwei": {
      "command": "npx",
      "args": ["-y", "@fatestar/ziwei-mcp"],
      "env": { "FATESTAR_API_KEY": "fs_live_xxxxxxxx" }
    }
  }
}
```

> 远程 = 始终最新、零安装;stdio = 本地排盘、断网可用。二选一,排盘能力一致。

重启客户端,即可对 AI 说:「帮我排 1990 年 7 月 23 日早上 8 点出生男性的紫微命盘」。

---

## Agent 客户端速查

只要客户端支持 MCP,远程 / 本地任选其一即可接入。地址统一 `https://www.fatestar.top/api/mcp`。

| Agent | 远程 (HTTP) | 本地 (stdio) | 配置位置 |
| --- | --- | --- | --- |
| Claude Desktop / Claude Code | 填 url | `npx @fatestar/ziwei-mcp` | `claude_desktop_config.json` |
| Cursor | 填 url | `npx` | `~/.cursor/mcp.json` |
| OpenCode | 填 url | `npx` | `opencode.json` |
| Codex / OpenClaw / Cline / 飞书 CLI / 其他 | 填 url | `npx` | 各自 MCP 配置 |

---

## 工具参数(Available Tools)

三个工具共享出生参数;`ziwei_transits` 多 4 个运限目标,`ziwei_reading` 多一个 `question`。

| 参数 | 必填 | 说明 |
| --- | --- | --- |
| `year` `month` `day` | ✅ | 出生年月日(阳历;`calendarType=lunar` 时为农历) |
| `hour` | ✅ | 出生小时 `0–23`(24 小时制,**不是**时辰地支) |
| `gender` | ✅ | `male` / `female` |
| `minute` | | 出生分钟(配合 `longitude` 做真太阳时精修) |
| `calendarType` | | `solar`(默认)/ `lunar` |
| `isLeapMonth` | | 农历闰月(仅 `lunar` 有效) |
| `longitude` `timezoneOffset` | | 经度 + 时区,传了即启用真太阳时修正 |
| `targetYear` `targetMonth` `targetDay` `targetHour` | | 仅 `ziwei_transits`:运限目标(默认当年 + 本命农历月日 + 出生时辰) |
| `question` | | 仅 `ziwei_reading`:要问郑大钱的问题 |
| `apiKey` | | 仅 `ziwei_reading`:`fs_live_` key(也可由环境变量 `FATESTAR_API_KEY` 提供) |

### 拿一个 key

排盘不需要 key。郑大钱解读:到 https://www.fatestar.top 注册免费会员 → 做新手任务领积分 → 开发者中心创建 `fs_live_` key。免费会员每天 3 积分(北京时间 21:00 重置)。

---

## ⚠️ 双轨干支铁律

运限输出带两套干支,**严禁混用**:

- **真实干支** —— 万年历六十甲子,对外讲「X 日签约 / X 月运势」用这个。
- **宫位编号** —— 紫微五虎遁推算,**仅用于推四化**,严禁当日子说出口。

---

## 想要 Skill 版 / REST API?

- **Skill**(Claude / Codex 装个会排盘的技能):https://github.com/LouisLin0723/fatestar-ziwei-skill
- **REST API / 完整开发者文档**:https://www.fatestar.top/docs

---

<a name="english"></a>
## English

**Zi Wei Dou Shu (Purple Star Astrology) charting engine as an MCP server.** Three tools:

- `ziwei_chart` — full natal chart (free)
- `ziwei_transits` — six transit levels: decade / minor-year / annual / monthly / daily / hourly (free)
- `ziwei_reading` — the 郑大钱 (Zheng Da Qian) AI master's expert reading (paid, needs an `fs_live_` key)

102 stars, sanhe four-transformations school, true solar time, **zero third-party charting library**. Charting is free and anonymous.

Two ways to connect, both standard MCP, **no bridge proxy needed**:

```jsonc
// Remote Streamable HTTP (zero install)
{ "mcpServers": { "ziwei": { "url": "https://www.fatestar.top/api/mcp" } } }

// Local stdio (open-source npm package, bundles the engine — charts offline)
{ "mcpServers": { "ziwei": { "command": "npx", "args": ["-y", "@fatestar/ziwei-mcp"] } } }
```

Birth params: `year` `month` `day` `hour` (0–23) `gender` (`male`/`female`) required; `minute` `calendarType` (`solar`/`lunar`) `isLeapMonth` `longitude` `timezoneOffset` optional. `ziwei_transits` adds `targetYear/Month/Day/Hour`; `ziwei_reading` adds `question` + `apiKey`.

Skill version → https://github.com/LouisLin0723/fatestar-ziwei-skill · Full docs → https://www.fatestar.top/docs

---

MIT licensed · Powered by [FateStar](https://www.fatestar.top) · 排盘引擎自建,只讲真话。
