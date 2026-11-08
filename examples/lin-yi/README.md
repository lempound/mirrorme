# 示例实例：林一（虚构）

> ⚠️ **林一是虚构人物。** 姓名、经历、观点全部为示例，用于验证 `mirrorme` 框架的通用性。
> 本目录不含任何真实个人数据（框架仓铁律 L3）。

---

## 为什么要有这个示例

1. **证明抽象是对的**：换一个人，框架不该改一行代码
2. **给 `mirrorme init` 一个可对照的产出目标**
3. **提前暴露错误**：如果写林一时发现某个机制只能适配某个人，说明 spec 抽象错了

## 它与参考实例（框架作者）的差异

| 项 | 林一 | 框架作者 | 是否改框架 |
|---|---|---|---|
| 分类维度 | 4 维：`craft` / `body` / `people` / `money` | 7 维：`growth` / `ability` / … | ❌ 不改，只改 config |
| `modules.projects` | `false` | `true` | ❌ |
| 访谈频率 | 15 min/周 | 20 min/周 | ❌ |
| 保真度目标 | F2 | F1 | ❌ |

**两套配置，同一套 spec** —— 这就是框架与模板的区别。

---

## 怎么读这个示例

1. 先看 `mirrorme.config.yml`：林一的分类和作者完全不同，但没有一行代码需要改
2. 再看 `notes/20260920-design-system-delay.md`：注意四段结构，第 3 段是判断
3. 再看 `self/decisions/`：注意 `rationale` 写的是**当时真实的权衡**，不是事后合理化
4. 最后看 `AGENTS.md`：AI 的阅读顺序

---

## 目录

```
lin-yi/
├─ README.md                 本文件
├─ AGENTS.md                 AI 入口（由 init 生成）
├─ mirrorme.config.yml       实例配置
├─ mirrorme.lock             版本锁
├─ self/
│  ├─ identity.md            身份档案
│  ├─ values.md              价值排序
│  ├─ principles/            判断原则（条目）
│  └─ decisions/             决策记录（条目）
├─ notes/                    原子笔记
└─ index/domains/            MOC
```
