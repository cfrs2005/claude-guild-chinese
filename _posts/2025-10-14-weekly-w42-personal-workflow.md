---
layout: post
title: "🚀 第8期：Claude Code 个人工作流深度配置"
date: 2025-10-14 12:00:00 +0800
categories: [周刊, 实战教程]
tags: [工作流, Everything is a File, 自定义命令, MCP, 效率提升]
---

## 2025年10月14日 | 第8期

> 发布日期: 2025年10月14日 | [返回首页](/) | [期刊归档](/categories/)

---

## 📌 写在前面

**来源**: 本文基于 Anthropic Claude Code 团队 Demo Designer **Thariq Shihipar** 的[真实工作流分享](https://x.com/ThariqS)

**核心观点**:
> "Everything is a File" - Claude Code 不只是代码助手，它能像你一样使用电脑上的文件系统。

当 Anthropic 内部团队都在用 Claude Code 管理工作生活时，这套方法论值得我们深挖。

---

## 💡 核心哲学：Everything is a File

### 为什么这个理念很重要？

**传统 AI 助手的痛点**:
```text
❌ 上下文丢失 - 每次对话都要重新解释背景
❌ 数据孤岛 - 无法访问你的笔记、代码、待办事项
❌ 工作流割裂 - 在 10 个工具间来回切换
```

**Claude Code 的优势**:
```text
✅ 文件即记忆 - 通过文件系统持久化所有上下文
✅ 统一入口 - 代码、笔记、待办、日志都在一个地方
✅ 智能搜索 - 像你一样在文件系统中查找信息
```

**Linus 式点评**:
> "这不是什么黑科技，就是把 Unix 哲学用对了。文件系统本来就是用来组织数据的，别搞那些花里胡哨的 GUI 工具了。"

---

## 🏗️ 目录结构设计

### Thariq 的完整配置

```bash
~/ (家目录)
├── Claude.MD                    # 核心配置文件
├── .claude/
│   └── commands/
│       ├── journal.md           # /journal 命令
│       └── todo.md              # /todo 命令
│
├── memories/                    # 长期记忆（重要！）
│   ├── work-context.md
│   ├── project-decisions.md
│   └── technical-notes.md
│
├── journal/                     # 每日日志
│   ├── 2025-10-14.md
│   ├── 2025-10-13.md
│   └── ...
│
├── todos/                       # 待办事项（按主题分类）
│   ├── claudecode.todos.md
│   ├── home.todos.md
│   └── learning.todos.md
│
├── projects/                    # 活跃项目
│   ├── website-redesign.md
│   └── api-integration.md
│
├── ideas/                       # 创意收集
│   └── brainstorm.md
│
├── code/claude-scripts/         # 自定义脚本
│   ├── notes-cli.js             # Apple Notes 集成
│   └── automation.sh
│
├── Repos/                       # 代码项目
├── memes/                       # 灵感素材（是的，memes）
└── Desktop/                     # 截图和临时文件
```

### 设计原则

**1. 扁平化优先**
```text
✅ ~/memories/work-context.md
❌ ~/documents/work/context/2024/october/notes.md
```
> **Linus**: "超过 3 层目录就是在折磨自己。文件系统不是俄罗斯套娃。"

**2. 命名要见名知意**
```text
✅ claudecode.todos.md          # 清晰
✅ 2025-10-14.md                # 标准日期格式
❌ stuff.md                      # 垃圾命名
❌ untitled-3.md                 # 懒惰
```

**3. Markdown 统治一切**
- 人类可读
- Git 友好
- Claude 原生支持
- 跨平台通用

---

## ⚙️ 核心配置：Claude.MD

### 完整配置文件

创建 `~/Claude.MD`:

```markdown
# Claude Configuration

## About Me
- Name: [你的名字]
- Role: [你的职位]
- Focus: [你的工作重点]
- Timezone: Asia/Shanghai

## Directory Structure
- **~/Desktop**: 截图和临时文件
- **~/Downloads**: 最近下载
- **~/Documents**: 文档和个人资料

## Working Directories
- **Memory**: `~/memories` - 长期记忆（技术笔记、决策记录）
- **Journal**: `~/journal` - 每日工作日志
- **To dos**: `~/todos` - 待办事项（按主题分文件）
- **Projects**: `~/projects` - 活跃项目的上下文

## Instructions
Before answering queries:
1. Search ~/memories/ for relevant technical notes
2. Check ~/projects/ for project-specific context
3. Review ~/journal/ for recent activities

After completing work:
1. Ask if anything should be saved to memories
2. Update relevant project file in ~/projects/
3. Suggest adding to journal if significant

## Tools & Scripts
- **Apple Notes CLI**: `~/code/claude-scripts/notes-cli.js`
  - Search: `node notes-cli.js search "query"`
  - Create: `node notes-cli.js create "title" "content"`

## Usage Notes
- I use you for general tasks, not just coding
- Help me maintain organized notes and todos
- Proactively search my files for context
```

---

## 🛠️ 自定义命令实战

### 1. `/journal` - 日志管理

**创建 `.claude/commands/journal.md`**:

```markdown
Create or update journal entry for today or specified date.

Usage:
- `/journal` - Open today's entry
- `/journal some thoughts` - Add to today
- `/journal 2025-10-14 content` - Specific date

Format: `~/journal/YYYY-MM-DD.md`

Template:
# Journal - YYYY-MM-DD

## Morning

## Afternoon

## Evening

## Notes

Always show current content after update.
```

**使用示例**:

```bash
# 场景1：开始新的一天
你: /journal
Claude: 创建了 ~/journal/2025-10-14.md，今天想记录什么？

# 场景2：快速记录想法
你: /journal 今天解决了 Redis 连接池的内存泄漏问题，原因是...
Claude: 已添加到 ~/journal/2025-10-14.md 的 Notes 部分

# 场景3：补充历史记录
你: /journal 2025-10-10 那天的会议决定用 PostgreSQL 而不是 MongoDB
Claude: 已更新 ~/journal/2025-10-10.md
```

### 2. `/todo` - 待办事项管理

**创建 `.claude/commands/todo.md`**:

```markdown
Manage TODO lists organized by topic in separate markdown files.

Usage:
- `/todo list` - Show all todos across all files
- `/todo add [topic] task description` - Add new todo
- `/todo complete [topic] task description` - Mark as done
- `/todo context [topic]` - Search memories for relevant context

Instructions:
1. Each topic has its own file: `~/todos/{topic}.todos.md`
2. Format:
   # {Topic} TODOs

   ## In Progress
   - [ ] Task 1

   ## Backlog
   - [ ] Task 2

   ## Done
   - [x] Completed task

3. Before adding todos, search user's code/projects for context
```

**使用示例**:

```bash
# 场景1：添加待办（Claude 会自动搜索相关代码）
你: /todo add claudecode 实现 MCP 服务器的健康检查接口
Claude:
  搜索到相关代码：src/mcp/server.ts
  已添加到 ~/todos/claudecode.todos.md:
  - [ ] 实现 MCP 服务器的健康检查接口
        参考：server.ts:45 的现有端点结构

# 场景2：标记完成
你: /todo complete claudecode 实现 MCP 服务器的健康检查接口
Claude: 已移动到 Done 部分

# 场景3：查看所有待办
你: /todo list
Claude:
  claudecode.todos.md (3 个待办)
  home.todos.md (1 个待办)
  learning.todos.md (5 个待办)
```

### 3. Memory 系统 - 长期记忆

**在 `Claude.MD` 中添加指令**:
```markdown
## Memory Management
Before answering queries:
1. Search `~/memories/` for relevant context
2. If missing context, ask user to provide and save to memories

After completing significant work:
1. Ask: "Should I save anything to memories?"
2. Suggest what to save (decisions, patterns, lessons learned)
3. Update appropriate memory file
```

**Memory 文件示例**:

`~/memories/project-decisions.md`:
```markdown
# Project Decisions

## Database Choice (2025-10-10)
**Decision**: PostgreSQL over MongoDB
**Reason**:
- 需要 ACID 保证
- 复杂查询优化更好
- 团队更熟悉 SQL
**Trade-off**: 失去了 schema-less 的灵活性
```

---

## 🔌 高级集成：系统工具对接

### 1. Apple Notes 集成

**创建 `~/code/claude-scripts/notes-cli.js`**:

```javascript
#!/usr/bin/env node

const { execSync } = require('child_process');

function executeAppleScript(script) {
  try {
    return execSync(`osascript -e '${script}'`, { encoding: 'utf-8' });
  } catch (error) {
    console.error('Error executing AppleScript:', error.message);
    process.exit(1);
  }
}

const commands = {
  search: (query) => {
    const script = `
      tell application "Notes"
        set matchingNotes to {}
        repeat with aNote in notes
          if (name of aNote contains "${query}") or (body of aNote contains "${query}") then
            set end of matchingNotes to name of aNote
          end if
        end repeat
        return matchingNotes
      end tell
    `;
    console.log(executeAppleScript(script));
  },

  create: (title, content) => {
    const script = `
      tell application "Notes"
        make new note at folder "Notes" with properties {name:"${title}", body:"${content}"}
      end tell
    `;
    executeAppleScript(script);
    console.log(`Created note: ${title}`);
  },

  list: () => {
    const script = `
      tell application "Notes"
        set noteList to name of every note
        return noteList
      end tell
    `;
    console.log(executeAppleScript(script));
  }
};

const [,, command, ...args] = process.argv;

if (commands[command]) {
  commands[command](...args);
} else {
  console.log('Usage: notes-cli <search|create|list> [args]');
}
```

**赋予执行权限**:
```bash
chmod +x ~/code/claude-scripts/notes-cli.js
```

### 2. MCP 集成（终极方案）

**常用 MCP 服务器**:

```bash
# Notion
claude mcp add --transport http notion https://mcp.notion.so

# GitHub
claude mcp add --transport http github https://mcp.github.com

# Linear
claude mcp add --transport http linear https://mcp.linear.app

# Slack
claude mcp add --transport http slack https://mcp.slack.com
```

---

## 🎯 实战工作流示例

### 场景1：开始新的一天

```bash
# 1. 晨间检查
你: /journal
Claude: 创建 2025-10-14.md，准备记录今天的工作

你: 今天有什么安排？
Claude:
  查询 ~/todos/ 发现:
  高优先级:
  - [claudecode] 修复 MCP 健康检查 bug
  - [work] 准备下午的技术分享

  从 memories 中找到上下文:
  - MCP bug 相关决策在 memories/project-decisions.md
  - 技术分享主题：Claude Code 最佳实践
```

### 场景2：解决技术问题

```bash
你: Redis 连接池怎么配置来着？
Claude:
  搜索 ~/memories/technical-notes.md:

  ## Redis 连接池最佳实践
  - 最大连接数: 20
  - Idle timeout: 300s
  - 记得手动 quit() 避免泄漏

  还需要看具体代码吗？
```

### 场景3：项目复盘

```bash
你: 帮我总结这周在"网站重构"项目上的进展
Claude:
  查看 ~/projects/website-redesign.md:

  本周完成:
  ✅ 前端组件库升级到 React 19
  ✅ API 接口全部迁移到 RESTful 规范
  ✅ 数据库索引优化（查询速度提升 40%）

  未完成:
  ⏳ 单元测试覆盖率还在 60%（目标 80%）
  ⏳ 移动端适配进度 70%

  要记录到 journal 吗？
```

---

## 🚀 快速开始：30 分钟搭建你的工作流

### Step 1: 创建目录结构（5分钟）

```bash
# 在家目录创建
cd ~

# 创建核心目录
mkdir -p memories journal todos projects ideas code/claude-scripts

# 验证
tree -L 1 ~/memories ~/journal ~/todos ~/projects
```

### Step 2: 配置 Claude.MD（10分钟）

```bash
# 创建配置文件
cat > ~/Claude.MD << 'EOF'
# Claude Configuration

## About Me
- Name: [你的名字]
- Role: [你的职位]
- Focus: [你的工作重点]

## Working Directories
- **Memory**: `~/memories` - 长期记忆
- **Journal**: `~/journal` - 每日日志
- **To dos**: `~/todos` - 待办事项
- **Projects**: `~/projects` - 活跃项目

## Instructions
Before answering queries:
1. Search ~/memories/ for relevant notes
2. Check ~/projects/ for project context

After completing work:
1. Ask if anything should be saved to memories
2. Update relevant project file
EOF
```

### Step 3: 创建自定义命令（10分钟）

```bash
# 创建命令目录
mkdir -p ~/.claude/commands

# /journal 命令
cat > ~/.claude/commands/journal.md << 'EOF'
Create or update journal entry.

Usage:
- `/journal` - Open today's entry
- `/journal thoughts` - Add to today

Format: `~/journal/YYYY-MM-DD.md`
EOF

# /todo 命令
cat > ~/.claude/commands/todo.md << 'EOF'
Manage TODOs by topic.

Usage:
- `/todo list` - Show all
- `/todo add [topic] description` - Add new
- `/todo complete [topic] description` - Mark done

File: `~/todos/{topic}.todos.md`
EOF
```

### Step 4: 创建初始文件（5分钟）

```bash
# 创建第一个 memory 文件
cat > ~/memories/setup.md << 'EOF'
# Setup Notes

## Initial Configuration (2025-10-14)
- Created directory structure
- Configured ~/Claude.MD
- Set up /journal and /todo commands
EOF

# 创建今天的 journal
DATE=$(date +%Y-%m-%d)
cat > ~/journal/$DATE.md << EOF
# Journal - $DATE

## Morning
- Set up Claude Code personal workflow

## Notes
- Following "Everything is a File" philosophy
EOF

echo "✅ 初始文件创建完成！"
```

---

## 💎 进阶技巧

### 1. 智能提醒系统

**在 `Claude.MD` 添加**:
```markdown
## Daily Routines
### Morning (9:00)
1. Show today's todos
2. Check journal for yesterday's unfinished items
3. Summarize Slack/Linear mentions

### Evening (18:00)
1. Review accomplishments
2. Suggest adding work to memories
3. Update project progress
4. Prompt for journal entry
```

### 2. 项目模板系统

**创建 `~/templates/project.md`**:
```markdown
# [Project Name]

## 项目目标
[一句话描述]

## 技术决策

### YYYY-MM-DD: [决策标题]
**问题**:
**方案**:
**原因**:

## 进度
- [ ] YYYY-MM-DD:
```

### 3. 周期性复盘

**创建 `~/.claude/commands/weekly-review.md`**:
```markdown
Generate weekly review from journals and projects.

Steps:
1. List journal entries from past 7 days
2. Extract completed tasks from todos
3. Summarize progress from projects
4. Suggest what to move to memories

Save to: `~/journal/weekly-YYYY-Www.md`
```

---

## 🎯 常见问题

### Q1: 文件会不会越来越多？

**A**: 定期归档

```bash
# 每月归档 journal
mkdir -p ~/journal/archive/2025-10
mv ~/journal/2025-10-*.md ~/journal/archive/2025-10/
```

### Q2: Memory 文件应该多大？

**最佳实践**:
```bash
# ❌ 一个超大文件
~/memories/notes.md (2000 行)

# ✅ 按主题拆分
~/memories/
├── docker.md (200 行)
├── redis.md (150 行)
└── postgresql.md (180 行)
```

### Q3: 如何多设备同步？

**方案: Git 同步**（推荐）
```bash
# 初始化 Git 仓库
cd ~
git init
git add Claude.MD memories/ journal/ todos/ projects/
git commit -m "Initial setup"

# 推送到私有仓库
git remote add origin git@github.com:你的用户名/claude-workspace.git
git push -u origin main
```

---

## 🔥 Linus 式总结

### 核心是什么？

**不是 AI 魔法**，是**回归本质**:

```text
✅ 文件系统本来就是用来组织数据的
✅ Markdown 本来就是人类可读的
✅ 搜索本来就是查找文件的基本操作
```

### 为什么它有效？

**1. 数据结构优先**
```text
数据结构清晰了，一切都变简单：
- Memory: 长期知识（按主题）
- Journal: 时间序列（按日期）
- Projects: 项目上下文（按项目）
- Todos: 待办事项（按主题）
```

**2. 消除特殊情况**
```text
不需要为不同类型的信息设计不同工具：
代码、笔记、待办、日志 → 全部用 Markdown 文件
```

**3. 实用主义**
```text
只解决一个问题：
"我怎么快速找到我需要的信息？"

答案：文件系统 + 搜索 + Claude
```

### 如果只能给一条建议

**从最简单的开始**:

```bash
# 今天就做这 3 件事（15分钟）
1. 创建 ~/Claude.MD（5分钟）
2. 创建 ~/memories/setup.md（5分钟）
3. 问 Claude 一个需要上下文的问题（1分钟）

先跑起来，再优化。
```

---

## 📚 参考资源

**原始分享**:
- Thariq 的 X 帖子: [https://x.com/ThariqS](https://x.com/ThariqS)
- Claude.MD 配置: [Gist](https://gist.github.com/ThariqS/2c35650d1cb81e87392e9f0fee5d5a10)
- /journal 命令: [Gist](https://gist.github.com/ThariqS/ae5ddce4e81cb92a781dc4597768adad)
- /todo 命令: [Gist](https://gist.github.com/ThariqS/25d515eca8bea795b743149b91aa5d16)

**相关文档**:
- [第35期: 快速上手](/posts/weekly-w35-getting-started/)
- [第36期: 命令系统](/posts/weekly-w36-command-system/)
- [第40期: 命令参考](/posts/weekly-w40-command-reference/)

---

**本期编辑**: cfrs2005
**发布时间**: 2025年10月14日
**特别感谢**: Thariq Shihipar (Anthropic Claude Code Team)
**欢迎反馈**: [GitHub Issues](https://github.com/cfrs2005/claude-guild-chinese/issues)
