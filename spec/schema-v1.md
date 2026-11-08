# spec/schema-v1 —— 内容契约

> 版本：**v0.1-draft**（未冻结，将先由真实实例反推，见 R20）
> 适用：`instance/` 下所有内容 `.md` 文件，`README.md` / `AGENTS.md` / 项目源码除外

---

## 0. 设计目标

| 目标 | 含义 |
|---|---|
| **机器可校验** | 每个字段的类型、取值范围可由脚本判定 |
| **只加不改** | 小版本只新增可选字段，不改既有语义 |
| **零工具可读** | 没有工具时，字段仍是人类可读的 YAML |
| **最小必填** | 人工最多填 3 个字段，其余由工具补齐 |

---

## 1. 通用 frontmatter

```yaml
---
schema_version: 1
id: 20261108-ai-entry-protocol
title: AI 入口协议的设计取舍
type: note
status: growing
domain: [thinking, skill]
tags: [agents-md, llms-txt]
summary: 为什么用 AGENTS.md 做唯一权威，以及 llms.txt 只能由脚本生成。
created: 2026-11-08
updated: 2026-11-08
lang: zh
visibility: private
source: https://agents.md/
related: [20261101-pkm-architecture]
confidence: 0.9
review_after: 2027-02-08
---
```

---

## 2. 字段定义

| 字段 | 类型 | 必填 | 人工填 | 约束 | 说明 |
|---|---|---|---|---|---|
| `schema_version` | int | ✅ | ❌ | `= 1` | 契约版本 |
| `id` | string | ✅ | ❌ | `^[0-9]{8}-[a-z0-9-]+$`，全实例唯一，= 文件名 | 稳定地址 |
| `title` | string | ✅ | ✅ | 1–80 字 | 可改 |
| `type` | enum | ✅ | ❌ | 见 §3 | 由模板决定 |
| `status` | enum | ✅ | ❌ | `seed｜growing｜evergreen｜archived` | 默认 `seed` |
| `domain` | string[] | ✅ | ✅ | 1–`config.taxonomy.max_domains_per_note` 个，且每项 ∈ taxonomy | `[0]` 为主维度 |
| `tags` | string[] | ❌ | ✅ | ≤ 8，`^[a-z0-9-]+$` | 自由词 |
| `summary` | string | ✅ | ✅ | 8–120 字，单句 | **检索主字段** |
| `created` | date | ✅ | ❌ | `YYYY-MM-DD` | 创建日 |
| `updated` | date | ✅ | ❌ | `YYYY-MM-DD` | 工具维护 |
| `lang` | enum | ❌ | ❌ | `zh｜en｜mixed` | 默认取 config |
| `visibility` | enum | ✅ | ❌ | `public｜private` | 默认取 config |
| `source` | string | ❌ | ✅ | URL 或引文 | 溯源 |
| `related` | string[] | ❌ | ✅ | 有效 `id` 或项目 slug | 显式关联 |
| `confidence` | number | ❌ | ✅ | `0–1` | 主观可信度 |
| `review_after` | date | ❌ | ✅ | `> created` | 到期复核 |

**人工必填只有 3 个**：`title`、`summary`、`domain`。其余由 `mirror new` 自动生成，`mirror check` 自动补齐。

---

## 3. 类型（type）

| type | 用途 | 所在目录 | 额外要求 |
|---|---|---|---|
| `note` | 原子知识 | `notes/` | 见 §5 四段结构 |
| `resource` | 外部资料 | `resources/` | 必须有"我的批注"段 |
| `journal` | 时间性记录 | `journal/` | 无 |
| `decision` | 决策记录 | `self/decisions/` | 见 §6 |
| `position` | 立场（可演化） | `self/positions/` | 必须有 `as_of` |
| `principle` | 判断原则 | `self/principles/` | 必须有"适用场景 + 反例" |
| `moc` | 索引笔记 | `index/domains/` | 无 |
| `skill` | 可执行方法论 | `skills/<name>/SKILL.md` | 遵守 Agent Skills 规范 |

**扩展方式**：不新增 type。任何新形态先用 `type: note` + `tags` 表达，积累 10 个真实用例后再评估是否升级为类型。

---

## 4. 自定义字段

实例需要框架未定义的字段时，一律加 `x_` 前缀：

```yaml
x_project: dsh-plugin
x_source_type: podcast
```

- 工具必须**忽略且保留** `x_*` 字段（迁移、导出不得丢弃）
- `x_*` 永不进入规范

---

## 5. `note` 强制四段结构

框架的核心机制：**写知识的同时写判断**。

```markdown
## 1. 客观面：它是什么
## 2. 与我的关系：我为什么在意它
## 3. 我的判断：我认为它对/错/有用/没用，为什么
## 4. 我的疑问：我还没想明白的地方
```

- 第 2/3/4 段由 `mirror check` 校验是否存在（标题匹配即可，内容不设长度要求）
- 可用 `config.structure.enforce: false` 关闭（**关闭后 F2 无法达成**，`mirror check` 会警告）
- 第 4 段写入"暂无"时，条目会进入 `self/unknowns.md`

---

## 6. `decision` 扩展字段

在通用字段之外追加：

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `situation` | string | ✅ | 一句话情境 |
| `options` | string[] | ❌ | 当时考虑的选项 |
| `chose` | string | ✅ | 最终选择 |
| `rationale` | string | ✅ | **我的推理，≥ 3 句**（核心字段） |
| `tradeoff` | string | ❌ | 我放弃了什么 |
| `outcome` | string | ❌ | 事后结果（后期补） |
| `verdict` | enum | ❌ | `right｜wrong｜mixed｜pending` |
| `pattern` | string | ❌ | 可复用的判断模式 |

**`rationale` 是 F2 的唯一燃料**。没有它，决策记录只是"我做过什么"，不是"我怎么想"。

---

## 7. 校验规则（`mirror check` 必须实现）

| # | 规则 | 级别 |
|---|---|---|
| 1 | `id` 与文件名一致，且全实例唯一 | error |
| 2 | 必填字段齐全、类型正确、格式合法 | error |
| 3 | `domain` 每项存在于 `config.taxonomy.domains` | error |
| 4 | `related` 指向的 id 存在 | warning |
| 5 | `note` 四段结构完整 | warning（`enforce: true` 时为 error） |
| 6 | 相对链接目标存在 | warning |
| 7 | `review_after` 已过期 | info |
| 8 | `seed` 状态超过 `config.lifecycle.seed_ttl_days` | info |
| 9 | 生成物文件含 `<!-- GENERATED -->` 头 | error |
| 10 | `visibility: private` 的文件出现在导出清单中 | error |

---

## 8. 版本演进

| 变更类型 | 版本号 | 要求 |
|---|---|---|
| 新增可选字段 | minor（1.0 → 1.1） | 无需迁移 |
| 新增类型 | minor | 需文档 + 校验更新 |
| 字段改名/删除/语义变化 | **major** | 必须提供迁移脚本 + 迁移测试 + 至少 2 个月弃用期 |

**冻结规则**：spec 在真实实例使用满 2 个月后才冻结 v1.0。
