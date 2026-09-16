# 通用 AI 智能体 Skill 集合

本项目是一组通用的 AI 智能体自定义 Skill，采用 `SKILL.md` 标准格式定义，覆盖**项目从零构建、执行计划生成、GitHub 首次发布**三个标准化工作流，帮助 AI 在执行相关任务时遵循统一、可验收的流程规范。

适用于所有支持 `SKILL.md` / 插件机制的 AI 智能体平台，包括但不限于豆包（Doubao）、Claude、Cursor、Coze、Dify、LangChain 类 Agent 框架等。

## 项目解决什么问题

在使用 AI 辅助开发时，常见痛点包括：需求没问清就动手写代码、项目做完没有说明文档、代码变更缺乏影响评估、发布到 GitHub 时遗漏步骤或泄露敏感信息。

本项目通过三个结构化 Skill，将上述场景分别固化为**带确认门控和验收标准的标准化流程**，让 AI 的执行过程可预测、可干预、可追溯，减少返工和安全风险。

## 主要功能

本集合包含以下三个 Skill：

### 1. project-builder — 通用项目从零构建
- 三阶段工作流：需求澄清 → 项目制作 → 文档生成
- 需求澄清阶段一次只问一个问题，用户可随时叫停
- 项目制作前先规划结构，遇决策点暂停询问
- 强制生成五段式 README（解决什么问题、主要功能、安装方法、使用方法、输入输出示例）
- 支持中断恢复和回到需求澄清

### 2. project-execution-planner — 项目执行计划生成
- 将分步或自由文本需求转化为工程级执行计划 Markdown 文档
- 8 步流程：输入校验 → 冲突检测 → 方案选定 → 任务拆解 → 代码影响评估 → 风险校验 → 生成计划 → 增量迭代
- 子任务带优先级（P0/P1/P2）、依赖关系、涉及模块和验收标准
- 代码变更评估影响面与兼容性，功能移除必查依赖
- 含风险与校验项、交付物清单、回滚方案
- 支持需求变更的增量更新与版本记录

### 3. github-project-publisher — GitHub 项目首次发布
- 五阶段工作流：完善 README → 敏感信息排查 → 初始化 Git 仓库 → 发布到 GitHub → v0.1.0 Tag 与 Release
- 敏感信息排查阶段绝不执行 Git 操作，防止密码/Token/隐私泄露进版本历史
- 关键节点设确认门控（仓库名称、可见性、是否发 Release、Release 内容）
- 统一 main 分支和 Initial commit 规范
- 自动生成带标题、发布说明和功能列表的 GitHub Release

## 安装方法

### 环境要求
- 任意支持 `SKILL.md` 格式或自定义插件/Skill 机制的 AI 智能体平台
- 操作系统：Windows / macOS / Linux（Skill 本身为 Markdown 定义，跨平台）
- `github-project-publisher` Skill 执行时需本地已安装 Git 和 GitHub CLI（`gh`）

### 安装步骤

1. 克隆或下载本仓库到本地：
   ```bash
   git clone https://github.com/gaigaiz/The-production-and-sharing-of-SKILL.git
   ```

2. 将需要使用的 Skill 文件夹复制到目标智能体平台对应的 Skill / 插件目录。以下为常见平台示例：

   **豆包（Doubao）：**
   - Windows：`%USERPROFILE%\AppData\Local\Doubao\User Data\Default\.doubao\agent_mode\workspace\.user_skills\`
   - 或客户端设置中指定的自定义 Skill 目录

   **其他平台：**
   - Claude / Cursor：放入对应插件或自定义指令目录
   - Coze / Dify：通过平台的插件/Skill 上传功能导入
   - 自研 Agent 框架：将 `SKILL.md` 作为系统提示词或工具定义加载

3. 复制后重启对应客户端或新开会话，Skill 即会被识别和加载。

> 每个 Skill 文件夹内需包含 `SKILL.md` 主文件；带 `references/` 子目录的 Skill 需一并复制，否则参考模板无法读取。

## 使用方法

Skill 加载后，在对应智能体的对话中通过自然语言触发即可，无需手动调用命令。

| Skill | 触发说法示例 |
|-------|-------------|
| project-builder | "帮我做一个 XX 工具"、"从零开始做个 XX 项目"、"我想开发一个 XX" |
| project-execution-planner | "帮我做个项目计划"、"把需求拆成执行计划"、"评估代码变更影响"、"拆解这个需求" |
| github-project-publisher | "发布到 GitHub"、"初始化仓库"、"打 Tag 发 Release"、"把这个项目推到 GitHub" |

触发后，AI 会按照对应 Skill 定义的阶段流程逐步执行，并在需要确认的节点主动询问。

## 输入输出示例

### 示例：使用 project-execution-planner 生成执行计划

**输入：**
```
帮我做个项目计划：给现有用户系统加一个手机号登录功能，
第 1 步：数据库加手机号字段；第 2 步：后端写登录接口；第 3 步：前端加登录页面。
```

**输出（摘要）：**
生成一份 `执行计划.md`，包含：
- 项目概述与原始需求原文
- 工程前置约束（技术栈、选定方案）
- 任务拆解表（T-01 数据库迁移 P0、T-02 后端接口 P1、T-03 前端页面 P1，含依赖和涉及文件）
- 代码变更规则（每个任务的变更类型、影响面、兼容性）
- 风险与校验项（如数据迁移风险、接口兼容性校验）
- 交付物清单与回滚方案

### 示例：使用 github-project-publisher 发布项目

**输入：**
```
把当前项目发布到 GitHub。
```

**输出（执行过程）：**
1. 检查并完善 README.md（五章节）
2. 扫描敏感信息并报告，用户确认无问题
3. `git init` → `git branch -M main` → `git add .` → `git commit -m "Initial commit"`
4. 用户确认仓库名称和 Public 可见性后，`gh repo create` + `git push -u origin main`
5. 用户确认后创建 v0.1.0 Tag 并发布 GitHub Release
6. 汇报仓库 URL 和 Release URL

---

## 项目结构

```
.
├── project-builder/
│   ├── SKILL.md
│   └── references/
│       ├── clarification-questions.md
│       └── doc-template.md
├── project-execution-planner/
│   ├── SKILL.md
│   └── references/
│       └── plan-template.md
├── github-project-publisher/
│   └── SKILL.md
├── project-builder-Skill作用总结.md
├── project-execution-planner-Skill作用总结.md
├── github-project-publisher-Skill作用总结.md
├── README.md
└── LICENSE
```

## License

MIT License
