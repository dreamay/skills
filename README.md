# Skills

一组面向 AI Agent 的技能（Skill）集合，每个 Skill 以 `SKILL.md` 为核心定义文件，可被 AI 助手加载并在对应场景下自动触发执行。

## 技能列表

### code-dev-habits — 代码开发习惯

系统化熟悉和整理一个代码项目的技能。当用户说"熟悉下这个项目"、"分析下这个代码库"、"项目 onboarding"等时触发。执行流程包括：探索项目结构和核心代码 → 识别待完善点并生成 `TODO.md` → 按需更新 README / CLAUDE.md / API.md 等文档 → 可选推送到 GitHub。

### obsidian-vault — Obsidian 知识库管理

通用的 Obsidian 知识库管理技能，支持任意路径、任意 Vault。当用户提到"知识库"、"知识卡片"、"读书笔记"、"同步知识库"、"Obsidian"等时触发。支持首次自动初始化完整 Vault 结构，生成知识卡片、读书笔记、技术文章、周报等多种类型笔记，并维护 MOC 索引和标签体系。

### obsidian-llm-wiki — LLM Wiki 知识库管理

AI 驱动的三层架构知识库管理技能（inbox → wiki → schema），是 obsidian-vault 的进阶版本。采用 inbox 收集、wiki 结构化知识、schema 指令层的分离设计，支持概念卡片、对比分析、综述、读书笔记、实体档案等多种笔记类型，通过 INDEX.md 和 CHANGELOG.md 自动维护知识库索引与变更日志。

### repowiki — 项目文档集生成

为任意代码项目生成结构化、可维护的 RepoWiki 文档集。当用户提到"repowiki"、"项目 wiki"、"生成项目文档"等时触发。输出包含项目概述、核心架构、数据模型、API 文档、配置管理、扩展开发、监控运维等多个维度的 Markdown 文档，所有文档遵循统一模板、Mermaid 图表规范和源码引用标注。文件夹与文件名统一添加两位数字序号前缀（01、02…），固定阅读顺序。

## 目录结构

```
skills/
├── code-dev-habits/
│   ├── SKILL.md              # 技能定义
│   └── evals/
│       └── evals.json        # 评估用例
├── obsidian-vault/
│   ├── SKILL.md              # 技能定义
│   ├── reference.md          # 详细规范参考
│   └── templates.md          # 初始化模板
├── obsidian-llm-wiki/
│   ├── SKILL.md              # 技能定义（v3.0 三层架构）
│   ├── reference.md          # 详细规范参考
│   └── templates.md          # 初始化模板
├── repowiki/
│   ├── SKILL.md              # 技能定义
│   └── evals/
│       └── evals.json        # 评估用例
├── LICENSE
└── README.md
```

## 许可证

[Apache License 2.0](LICENSE)
