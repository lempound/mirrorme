# mirrorme · 自我模型框架

> **给每个人一套搭建自己"自我模型"的框架。**
> 知识是原料，判断是产物，性格是形状。
>
> 我的个人知识库是它的**第一个实例（reference instance）**，不是它本身。

---

## 这个仓库是什么

`mirrorme` 是一个**框架**，不是模板。区别在于：

| | 模板 | 框架（本项目） |
|---|---|---|
| 别人怎么用 | fork，填内容，从此分叉 | 装工具 + 写配置，结构受保护 |
| 框架升级 | 无法升级 | `mirrorme upgrade` 迁移数据 |
| 分类方式 | 写死 | 每个人的 taxonomy 不同 |
| 能否评测 | 各自为准 | 统一的 F1/F2/F3 保真度评测 |

**核心主张**：一个人能被"继承"的部分，不是他说过的知识，而是他**做判断的方式**。

---

## 它做什么，不做什么

| 做 | 不做 |
|---|---|
| 记录你的判断、原则、价值观、表达习惯、记忆 | 承诺"意识上传" |
| 让任意 AI 读取后，按**你的方式**回答新问题 | 替代你在法律/医疗/财务上的真实意愿 |
| 用保真度阶梯量化"像不像你" | 用模糊的"像"来宣传 |
| 保证数据零工具也能用（纯 Markdown） | 把你锁进某个厂商或某个 App |

---

## 三层结构

```
spec/      规范 —— 与具体人无关的契约（schema、config、问题库、评测）
engine/    引擎 —— 实现规范的代码（CLI、索引、检索、迁移）
instance/  实例 —— 某个人的数据（self/、notes/、config）
```

铁律：**规范 > 引擎 > 实例**；引擎可换，规范不能飘；实例零工具可读。

---

## 保真度阶梯

| 级别 | 能力 | 需要 |
|---|---|---|
| **F1 可复述** | 准确说出你的观点、经历、偏好 | 内嵌判断的笔记 |
| **F2 可判断** | 面对新问题，给出你会给的答案和理由 | + 原则库 + 20 条以上决策记录 |
| **F3 可延续** | 行为层面难以与你区分 | + 语言样本 + 事件记忆 + 反应模式 |

---

## 五分钟上手

> 状态：**v0.1 草案，尚未实现 CLI**。当前阶段先用 AI 按 `spec/` 生成实例。

```bash
# 目标形态（M2 之后）
pip install mirrorme        # 若 PyPI 包名被占，用 mirrorme-selfmodel
mirrorme init               # 10 个问题的访谈，生成你的实例
mirrorme capture "想法"      # 随手记
mirrorme interview          # 每周 20 分钟的 AI 访谈
mirrorme check              # 校验数据契约
mirrorme eval               # 跑保真度评测
mm init                     # 短别名，与 mirrorme init 等价
```

---

## 仓库结构

```
mirrorme/
├─ README.md
├─ AGENTS.md              # 本仓库对 AI 的指令
├─ LICENSE                # MIT（框架）
├─ spec/                  # 规范层（核心）
│  ├─ schema-v1.md        # 内容契约
│  ├─ config-v1.md        # 实例配置规范
│  ├─ questions-v1.md     # 访谈问题库（规则）
│  └─ questions-v1.yml    # 访谈问题库（数据）
├─ templates/             # 8 类模板
│  ├─ note.md  resource.md  journal.md
│  ├─ decision.md  position.md  principle.md
│  └─ moc.md  LEGACY.md
├─ engine/                # 引擎层（M2）
├─ examples/              # 示例实例（脱敏）
└─ docs/                  # 面向用户的概念与指南
```

---

## 状态

- [x] 设计完成（工作区 `/docs` 下的 01–04 号文档）
- [x] 框架化结构确定（spec / engine / instance 分离）
- [x] spec v0.1-draft：`schema` / `config` / `questions`
- [x] `templates/`：8 类模板
- [x] `examples/`：示例实例「林一」（4 维度，与参考实例完全不同）
- [x] 我的实例骨架（`lempound/mirrorme-instance`，private）
- [ ] 我的实例跑通（M2）
- [ ] 首次外部试用（M3）

**spec v1 不会现在冻结**：先用真实实例跑 2 个月，再反推规范。没有真实用例的规范不值得写。

---

## 伦理红线

本框架**禁止**用于：伪造他人身份、未经授权采集第三方信息、替代他人在法律/医疗/财务上的真实意愿。
详见 `docs/ethics.md`（待写）与 `templates/LEGACY.md`。

---

## 许可

框架代码与规范：MIT。实例数据：由使用者自选（默认 CC BY-SA 4.0）。
