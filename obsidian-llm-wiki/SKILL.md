---
name: obsidian-vault
version: 3.0.0
description: LLM Wiki 知识库管理技能。三层架构（inbox → wiki → schema），支持任意路径、任意 Vault，自动发现与首次初始化。当用户提到"知识库"、"知识卡片"、"读书笔记"、"帮我写一张"、"同步知识库"、"Obsidian"、"vault"、"wiki"时触发。
description_zh: LLM Wiki 知识库管理。自动发现 Vault 路径，首次使用自动初始化完整结构。支持知识卡片、读书笔记、技术文章等内容生成。
---

# LLM Wiki 知识库管理

三层架构的 AI 驱动知识库：inbox（收集）→ wiki（结构化知识）→ schema（指令层）。适用于任意电脑、任意路径。

---

## 第一步：定位 Vault

每次操作前，先确定 Vault 路径：

1. **用户指定了路径** → 直接使用
2. **用户未指定** → 询问用户 Vault 在哪里

确定路径后，检查 `SCHEMA.md` 是否存在：
- **存在** → 已初始化的 LLM Wiki，读取 SCHEMA.md 获取规则，然后按需操作
- **不存在** → 全新 Vault，执行下方「首次初始化」

---

## 首次初始化

当目标 Vault 尚未初始化（无 `SCHEMA.md`）时，自动创建完整结构。

### 初始化内容

1. **创建目录树**（见 [templates.md](templates.md) 「目录树」章节）
2. **创建元文件**（内容见 templates.md）：
   - `.user-preferences.md` — 用户偏好
   - `SCHEMA.md` — 指令层，AI 行为规范
   - `INDEX.md` — 知识库入口索引 + 统计
   - `CHANGELOG.md` — 变更日志

初始化完成后告知用户：LLM Wiki 结构已就绪，可以开始使用了。

---

## 核心原则（必须遵守）

1. **只写一篇**：用户问什么就写什么，不衍生创建关联文件
2. **外链引用**：相关概念用外部 URL，不创建内部空文件。只有目标文件已存在时才用 `[[wiki link]]`
3. **精简总结**：知识卡片 800-1000 字，图文并茂，不写论文
4. **图文并茂**：架构图用 Mermaid，对比用表格，流程用 Mermaid 时序图
5. **面向快速理解**：假设用户是技术人员，直接进入重点

---

## 写作风格

- 中文为主，技术术语保留英文（如 Namespace、Cgroups）
- 禁止套话："值得注意的是"、"本文将介绍..."、"总之"、"综上所述"
- 不写教科书式开头和结尾
- 代码示例只放最核心的片段，不要贴完整配置

---

## 知识库结构

```
{vault-root}/
├── inbox/                      # 收集箱（原始资料、草稿、附件）
├── wiki/                       # 维基层（AI 管理的结构化知识）
│   ├── concepts/               # 概念：知识卡片 + 深度文章
│   ├── comparisons/            # 对比：A vs B
│   ├── synthesis/              # 综述：跨概念横向分析
│   ├── notes/                  # 笔记：读书/课程/论文
│   └── entities/               # 实体：工具/框架/人物档案
├── SCHEMA.md                   # 指令层（AI 行为规范）
├── INDEX.md                    # 知识库入口索引 + 统计
├── CHANGELOG.md                # 变更日志
└── .user-preferences.md        # 用户偏好（最高优先级）
```

完整结构详见 [reference.md](reference.md)。

---

## Frontmatter 规范

每篇笔记必须包含：

```yaml
---
type: 笔记类型    # 见下方枚举
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: []
ai_generated: true
---
```

### type 枚举与目标文件夹

| type | 目标文件夹 | 命名格式 |
|------|-----------|---------|
| concept | wiki/concepts/ | 概念名称.md |
| comparison | wiki/comparisons/ | A-vs-B.md |
| synthesis | wiki/synthesis/ | 主题名称.md |
| source-note | wiki/notes/ | 书名或论文-笔记.md |
| entity | wiki/entities/ | 实体名称.md |
| raw | inbox/ | 原标题.md |

---

## 知识卡片结构（最常用）

```markdown
## 一句话定义
（1-2 句话说清楚）

## 核心原理
（Mermaid 图或表格 + 简短说明）

## 关键命令/代码
（最常用的 5-10 行，不贴大段配置）

## 优缺点速查
| 优点 | 缺点 |
|------|------|
| ... | ... |

## 适用场景
（3-5 个，每个一句话）

## 延伸阅读
- [标题](外部URL)
```

**字数**：800-1000 字，**禁止**超过 1200 字。

---

## 链接策略

**默认用外部 URL**：
```markdown
## 延伸阅读
- [Kubernetes 官方文档](https://kubernetes.io/docs/)
```

**仅在以下情况用 `[[wiki link]]`**：
1. 目标文件已存在于知识库中
2. 用户明确要求
3. 概念高度相关且知识库中已有

---

## 操作流程

### 写入新笔记后

1. 确认 frontmatter 完整
2. 延伸阅读使用外部 URL
3. 更新 `INDEX.md` 对应分区并刷新统计
4. 更新 `CHANGELOG.md` 追加条目
5. 添加合适的 tags（从已有标签选择）

### 文件已存在时

**禁止直接覆盖**。先询问用户：
- **更新补充**：在现有基础上补充
- **完全重写**：覆盖旧内容
- **取消**

### 删除文件时（5 步完整执行）

1. 删除目标文件
2. 从 `INDEX.md` 移除对应条目
3. 扫描其他笔记清理 `[[wiki link]]` 引用
4. 更新 `INDEX.md` 统计数字
5. 在 `CHANGELOG.md` 记录删除日志

### 知识库同步

触发词："帮我同步一下知识库"

1. 扫描实际文件统计数量
2. 修正 `INDEX.md` 统计数字
3. 新文件添加到 `INDEX.md`（**只增不删用户文件**）
4. 清理 `INDEX.md` 中不存在的断链
5. 检查笔记内断链并提醒
6. 在 `CHANGELOG.md` 记录同步结果

### 定期校验

- **每天第一次操作**：自动完整校验（统计 + INDEX.md 链接）
- **其他时间**：仅用户说"同步"时才校验

---

## 常用触发指令

| 用户说 | 操作 |
|--------|------|
| "帮我写一张关于 XX 的知识卡片" | 创建 concept → wiki/concepts/ |
| "帮我写《XX》的读书笔记" | 创建 source-note → wiki/notes/ |
| "帮我对比 XX 和 YY" | 创建 comparison → wiki/comparisons/ |
| "帮我写一篇关于 XX 的技术文章" | 创建 synthesis → wiki/synthesis/ |
| "帮我解读 XX 这篇论文" | 创建 source-note → wiki/notes/ |
| "帮我删掉 XX" | 删除 + 善后 |
| "帮我同步一下知识库" | 完整同步 |
| "知识库里有哪些关于 XX 的内容" | 搜索查询 |
| "初始化知识库" | 首次初始化 LLM Wiki 结构 |

## 详细规范

更多细节（完整标签注册表、各类型详细生成规则）见 [reference.md](reference.md)。

初始化模板（目录树、元文件完整内容）见 [templates.md](templates.md)。
