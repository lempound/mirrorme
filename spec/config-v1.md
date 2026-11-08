# spec/config-v1 —— 实例配置规范

> 版本：**v0.1-draft**
> 文件：实例仓库根目录的 `mirrorme.config.yml`
> 原则：**配置项越少越好**。每个配置项都意味着一条测试路径和一份文档。

---

## 1. 完整示例（Reference Instance）

```yaml
# ==== 身份 ====
spec_version: 1
config_version: 1

identity:
  name: "周逸凡"
  lang: zh                    # zh | en | mixed
  timezone: Asia/Shanghai

# ==== 分类（每个人的维度都不同，框架不写死） ====
taxonomy:
  max_domains_per_note: 3
  require_domain: true
  domains:
    - id: growth
      label: 成长
      question: 我在往哪走？
    - id: ability
      label: 能力
      question: 我能做什么？
    - id: skill
      label: 技能
      question: 我会什么？
    - id: thinking
      label: 思考
      question: 我怎么想？
    - id: insight
      label: 见识
      question: 我知道什么？
    - id: attitude
      label: 态度
      question: 我怎么看待？
    - id: character
      label: 性格
      question: 我是谁？

# ==== 模块开关（决定目录骨架） ====
modules:
  projects: true              # monorepo：是否启用 projects/
  journal: true
  resources: true
  skills: true

# ==== 保真度目标（决定启用哪些采集面） ====
fidelity:
  target: F1                  # F1 | F2 | F3
  interview_weekly_minutes: 20
  interview_questions_per_session: 4

# ==== 隐私 ====
privacy:
  default_visibility: private
  never_export:
    - self/memory/
    - self/voice/samples/
    - self/affect.md
    - self/LEGACY.md
  blocked_name_patterns: []   # 第三方姓名等，导出时脱敏

# ==== 生成物 ====
agents:
  targets: [agents-md, claude-md, cursor]   # agents-md | claude-md | cursor | llms-txt
  max_agents_md_lines: 150

index:
  generate: [tags, timeline, index-json]

publish:
  enabled: false              # 第一版不发布
  engine: quartz

# ==== 结构强制 ====
structure:
  enforce_note_sections: true # note 四段结构
  require_resource_annotation: true

# ==== 生命周期 ====
lifecycle:
  seed_ttl_days: 60
  review_reminder_days: 7
```

---

## 2. 字段说明与默认值

| 路径 | 类型 | 默认 | 说明 |
|---|---|---|---|
| `spec_version` | int | 必填 | 数据契约版本，必须与 engine 兼容 |
| `config_version` | int | `1` | 配置自身版本（用于迁移） |
| `identity.name` | string | 必填 | 显示名；不要求真实姓名 |
| `identity.lang` | enum | `zh` | 默认语言，写入新文件 |
| `identity.timezone` | string | 系统时区 | IANA 时区 |
| `taxonomy.domains[]` | object[] | 必填 | `id` / `label` / `question` |
| `taxonomy.max_domains_per_note` | int | `3` | 单篇最多维度数 |
| `taxonomy.require_domain` | bool | `true` | 是否强制至少一个维度 |
| `modules.*` | bool | 视模块 | 决定目录骨架 |
| `fidelity.target` | enum | `F1` | 决定采集面与提醒强度 |
| `fidelity.interview_weekly_minutes` | int | `20` | 访谈时长上限 |
| `fidelity.interview_questions_per_session` | int | `4` | 每次提问数上限 |
| `privacy.default_visibility` | enum | `private` | 新文件默认可见性 |
| `privacy.never_export[]` | string[] | 见示例 | 路径前缀黑名单，导出时强制排除 |
| `privacy.blocked_name_patterns[]` | string[] | `[]` | 导出时替换为 `[他人]` |
| `agents.targets[]` | enum[] | `[agents-md]` | 生成哪些入口文件：`agents-md` / `claude-md` / `cursor` / `llms-txt`（供 ChatGPT 等网页版上传） |
| `agents.max_agents_md_lines` | int | `150` | 硬上限，超了 CI 失败 |
| `publish.enabled` | bool | `false` | 是否启用发布 |
| `structure.enforce_note_sections` | bool | `true` | 关闭将导致 F2 无法达成 |
| `lifecycle.seed_ttl_days` | int | `60` | 种子笔记超期阈值 |

---

## 3. 约束

| # | 约束 | 理由 |
|---|---|---|
| 1 | `taxonomy.domains[].id` 必须匹配 `^[a-z][a-z0-9-]*$` | 可做文件名与 URL |
| 2 | 每个 domain 必须有 `question` | 没有问题的维度无法判断归属 |
| 3 | domain 数量 3–12 | 少于 3 无分类意义，多于 12 记不住 |
| 4 | domain `id` 创建后不可改（改名走迁移） | 已有笔记的 frontmatter 引用它 |
| 5 | `never_export` 优先级高于任何单文件 `visibility: public` | 防误导出 |
| 6 | `structure.enforce_note_sections: false` 时 `mirrorme check` 必须警告 | 否则 F2 静默失效 |

---

## 4. 与框架的关系

- **框架代码不包含任何 domain 定义**。7 个人生维度只属于本实例。
- 框架提供的是 `taxonomy` 机制（结构 + 校验 + MOC 生成）。
- 换一个用户，domain 可以是"工作/家庭/健康/兴趣"，框架不需要改一行代码。

---

## 5. 版本演进

| 变更 | 版本 | 迁移 |
|---|---|---|
| 新增可选配置项 | minor | 无 |
| 修改默认值 | minor | 无（但需在 CHANGELOG 标注行为变化） |
| 删除/重命名配置项 | **major** | `mirrorme upgrade` 自动改写 `mirrorme.config.yml` |

**禁止**在实例里写 `# 手工改这行`——所有个性化必须落在配置项上，否则升级会覆盖。
