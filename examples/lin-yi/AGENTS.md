# AGENTS.md —— 示例实例（林一）

> 这是**实例仓**的 AI 指令，由 `mirrorme init` 根据 `mirrorme.config.yml` 生成。
> 与框架仓的 `AGENTS.md` 是两份不同的文件。

---

## 1. 这是什么

林一的自我模型：记录他的判断、原则、价值观和表达方式，使 AI 能按他的方式回应问题。

**保真度目标：F2（可判断）** —— 能给出林一会给的答案**和理由**。

---

## 2. 目录地图

| 目录 | 内容 |
|---|---|
| `self/` | 身份、价值排序、原则、决策、立场 —— 默认 private |
| `notes/` | 原子知识，每篇含"我的判断" |
| `index/domains/` | 4 个维度（craft / body / people / money）的 MOC |
| `resources/` | 外部资料与批注 |

---

## 3. 阅读顺序

| 任务 | 路径 |
|---|---|
| 回答"林一会怎么看" | `self/values.md` → `self/principles/` → `self/decisions/` |
| 找某个主题 | `index/domains/<domain>.md` → `notes/` |
| 了解偏好 | `self/identity.md` |

---

## 4. 红线

1. 不得修改生成物（`index/tags.md` 等）
2. 不得把 `self/` 内容导出到公开渠道
3. 引用判断时**必须给出 decision / principle 的 id**，不允许凭空推断
4. 没有记录的地方，必须明说"这是外推"

---

## 5. 回答格式

```
按你的方式，你会先考虑 {X}，因为你在 {decision-id} 里做过类似取舍；
你的原则 {principle-id} 说……；你的价值排序把 {A} 放在 {B} 之前。
所以你会选 {Y}。

⚠️ 其中 {Z} 部分是外推——你没有记录过这种情况。
```
