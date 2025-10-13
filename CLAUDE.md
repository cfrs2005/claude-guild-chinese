# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working code in this repository.

## 🚨 **非常重要：网站发布验证要求**

在每次说"网站发布完成符合诉求"之前，**必须**执行以下验证步骤：

```bash
# 1. 等待 GitHub Actions 构建完成 (至少 2 分钟)
gh run list --limit 1

# 2. 确认构建成功后，等待网站更新 (至少 1 分钟)
sleep 60

# 3. 使用 Playwright 验证网站真实效果
mcp__playwright__browser_navigate --url "https://claude-code.80aj.com"
mcp__playwright__browser_snapshot

# 4. 检查关键页面和功能
mcp__playwright__browser_navigate --url "https://claude-code.80aj.com/categories/"
mcp__playwright__browser_navigate --url "https://claude-code.80aj.com/tags/"
mcp__playwright__browser_navigate --url "https://claude-code.80aj.com/about/"

# 5. 验证文章链接是否正常
mcp__playwright__browser_navigate --url "https://claude-code.80aj.com/2025/10/10/weekly-2025-w41/"
```

**只有在通过以上所有验证后，才能说"网站发布完成符合诉求"！**

验证重点：
- ✅ 首页是否显示最新文章列表
- ✅ 侧边栏导航是否完整（分类、标签、关于）
- ✅ 文章链接是否能正常访问
- ✅ 主题样式是否正确加载
- ✅ 移动端是否正常显示

**违反此要求的后果**：交付的是未经验证的烂摊子！

## 📝 最近更新

### 2025-10-12：网站架构重大重构 - Blog 风格首页实现

**背景**：用户希望将传统文档网站转型为 blog 风格，首页展示最新文章摘要，简化导航结构。

**核心改动**：

1. **首页 Blog 化改造** (`index.md`)
   - 📝 实现真正的 blog 风格首页，展示最新3篇文章的500字详细摘要
   - 🎯 重新设计内容结构：最新文章 → 今日摘录 → 热门文章 → 快速导航
   - ✨ 添加丰富的文章描述，包含发布时间和核心内容概述
   - 🔗 优化内部链接结构，所有链接指向正确页面

2. **创建期刊归档页面** (`archive.md`)
   - 📅 完整的期刊历史记录，按季度分类
   - 📊 统计信息展示（总期刊数、分类统计）
   - 🏷️ 按主题分类（基础教程系列、动态内容系列）
   - 🔗 相关链接导航，方便用户跳转

3. **关于页面重构** (`about.md`)
   - 📋 包含原始首页内容：项目介绍、赞助信息、期刊表格
   - 💫 保留赞助商信息和推广链接
   - 📰 完整的期刊列表（倒序排列）
   - 🎯 项目说明和适合人群介绍

4. **导航结构简化**
   - 🧹 移除复杂的期刊层级导航，只保留5个主要入口
   - 🔗 修复所有导航链接，确保指向正确的页面路径
   - ✅ 验证所有页面都能正常访问，无404错误

5. **MkDocs 配置修复**
   - 📁 解决 `docs_dir` 配置问题，文件正确放置在 `docs/` 目录
   - 🔧 修复导航链接格式（去掉 `.md` 扩展名）
   - ✅ 解决空白页面问题，确保所有内容正常显示

**技术实现细节**：

```bash
# 文件结构调整
docs/
├── index.md                    # Blog 风格首页（新增）
├── archive.md                  # 期刊归档页（新增）
├── about.md                    # 关于页面（重构）
├── 相关项目推荐.md              # 附录内容
└── weekly/                     # 周刊内容目录
    ├── 2025-W35-快速上手.md
    ├── ...
    └── 2025-W41-Sonnet升级与社区项目点评.md

# MkDocs 配置修复
nav:
  - 首页: index.md
  - 期刊归档: 'archive/'        # 修复链接格式
  - 每日摘录: 'daily/'
  - 相关项目推荐: '相关项目推荐/'
  - 关于: 'about/'              # 修复链接格式
```

**验证结果**：
- ✅ 首页：完美展示 blog 风格，500字详细摘要
- ✅ 归档页：完整的期刊目录，分类清晰
- ✅ 关于页：包含所有原始内容，链接正确
- ✅ 导航：所有5个主要页面都能正常访问
- ✅ 链接：无404错误，所有内部链接正常工作

**关键洞察**：
- MkDocs 没有内置 blog 功能，需要手写首页实现 blog 效果
- `docs_dir` 配置是关键，必须在单独目录而不是根目录
- 导航链接需要去掉 `.md` 扩展名，指向目录路径
- 页面URL格式：`/archive/` 而不是 `/archive.md`

**本地预览**：
```bash
mkdocs serve -a localhost:8001  # 使用8001端口避免冲突
```

**效果验证**：
- 首页：http://localhost:8001/ （blog 风格，丰富摘要）
- 归档：http://localhost:8001/archive/ （完整期刊目录）
- 关于：http://localhost:8001/about/ （项目介绍信息）

---

### 2025-10-10：网站样式重大升级 - 高级紫色主题

**背景**：用户反馈网站样式传统，缺乏现代感和高级感，希望采用紫色/白色/黑色配色方案。

**改动内容**：

1. **创建高级紫色主题** (`stylesheets/premium-theme.css`)
   - 🎨 紫色渐变配色系统（主色 #7c3aed Violet-600）
   - 🌈 完整的 CSS Variables 设计系统
   - 🎭 优雅的深色模式适配
   - ✨ 现代化阴影和圆角系统

2. **顶部导航栏重设计**
   - 紫色渐变背景 + 磨砂玻璃效果
   - 高度增加到 4rem，更有存在感
   - 搜索框圆角设计，hover 紫色光晕效果

3. **侧边栏导航优化**
   - 卡片化导航项，圆角 12px
   - Hover 时紫色渐变背景 + 右移动画
   - 激活项带紫色阴影

4. **标题系统全面升级**
   - H1: 2.5rem，紫色渐变文字效果
   - H2: 2rem，紫色左边框 + 淡紫色背景卡片
   - H3: 1.5rem，浅紫色左边框
   - H4: 1.25rem（**修复比正文小的问题**），紫色渐变背景
   - 所有标题增加呼吸感间距

5. **内容区域卡片化**
   - 白色卡片容器，24px 大圆角
   - 紫色调阴影系统
   - 深色模式深紫色背景

6. **代码块优化**
   - 行内代码：紫色渐变背景 + 边框
   - 代码块：顶部紫色渐变装饰条
   - 深色背景 (#0f0f1e) + 紫色边框

7. **表格现代化**
   - 紫色渐变表头
   - 分离边框设计
   - Hover 行淡紫色背景

8. **其他细节**
   - 紫色滚动条
   - 引用块淡紫色背景 + 紫色左边框
   - 告示块紫色调优化
   - 链接紫色 hover 下划线动画
   - 页面加载淡入动画

9. **mkdocs.yml 配置更新**
   - 主题色改为 `deep purple`
   - 强调色改为 `purple`
   - 加载 `premium-theme.css`
   - 修复 emoji 配置（使用 Material 9.4+ 新语法）

10. **本地开发环境配置**
    - 创建 `docs/` 目录用于 MkDocs
    - 使用软链接避免文件重复（`docs/` → 根目录）
    - 创建 `javascripts/extra.js`（避免 404）
    - **重要**：软链接不会影响 GitHub Pages 部署

**技术细节**：

```bash
# 新增文件
stylesheets/premium-theme.css     # 高级紫色主题（600+ 行）
javascripts/extra.js              # 占位 JS 文件
docs/                             # MkDocs 工作目录（软链接）

# 修改文件
mkdocs.yml                        # 主题配色 + CSS 加载
stylesheets/extra.css             # 保留原有微信公众号风格样式

# 软链接结构（仅用于本地预览）
docs/index.md -> ../index.md
docs/weekly -> ../weekly
docs/相关项目推荐.md -> ../相关项目推荐.md
docs/stylesheets -> ../stylesheets
docs/javascripts -> ../javascripts
```

**设计哲学**：
- 🎯 **现代化**：采用 2024 年流行的渐变、毛玻璃、大圆角设计
- 💎 **高级感**：紫色调 + 精致阴影 + 动画过渡
- 🌓 **深色模式**：完整适配，紫色调延续
- 📱 **响应式**：移动端自适应字体大小
- ⚡ **性能优先**：纯 CSS 实现，无 JS 依赖

**效果预览**：
- 本地预览：`mkdocs serve` → http://127.0.0.1:8000
- 部署后：https://claude-code.80aj.com（约 2-5 分钟生效）

**后续优化方向**：
- [ ] 添加页面切换动画
- [ ] 优化移动端侧边栏展开效果
- [ ] 考虑添加暗色主题切换按钮的紫色光晕
- [ ] 优化代码高亮配色（配合紫色主题）

---

## 项目概述

这是一个 **Claude Code 周刊** 项目，以周刊形式追踪官方最新特性、分享实战经验和深度技术点评。项目使用 MkDocs Material 主题构建静态文档网站，通过 GitHub Pages 自动部署到 https://claude-code.80aj.com

**项目结构**：
```
# 周刊内容（weekly/ 目录统一管理）
weekly/
├── index.md                           # 周刊索引页
├── 2025-W35-快速上手.md               # 第35期（基础教程）
├── 2025-W36-命令系统.md               # 第36期（基础教程）
├── 2025-W37-开发工作流.md             # 第37期（基础教程）
├── 2025-W38-智能任务.md               # 第38期（基础教程）
├── 2025-W39-协同架构.md               # 第39期（基础教程）
├── 2025-W40-命令参考.md               # 第40期（基础教程）
└── 2025-W41-Sonnet升级与社区项目点评.md  # 第41期（动态周刊）

# 附录和配置
index.md                    # 首页
README.md                   # GitHub 项目说明
相关项目推荐.md             # 附录：精选工具和资源
mkdocs.yml                  # MkDocs 配置文件（主要）
_config.yml                 # Jekyll 配置（回退方案）
CLAUDE.md                   # 项目开发指南（本文件）
```

**内容组织原则**：
- **基础教程**（第35-40期）：静态、系统化的核心功能教程，帮助新手快速入门
- **动态周刊**（第41期起）：追踪最新特性、模型升级、社区动态，每周五更新
- **附录**：相关工具和资源推荐，按需更新

## 常用命令

### 本地预览文档
```bash
# 安装依赖（首次运行）
pip install mkdocs-material mkdocs-git-revision-date-localized-plugin

# 启动本地开发服务器（默认 http://127.0.0.1:8000）
mkdocs serve

# 指定端口
mkdocs serve -a localhost:8080
```

### 构建静态网站
```bash
# 构建到 site/ 目录
mkdocs build

# 清理并重新构建
mkdocs build --clean
```

### 部署
```bash
# 手动部署到 gh-pages 分支（通常不需要，CI 自动处理）
mkdocs gh-deploy
```

项目使用 GitHub Actions 自动部署（见 [.github/workflows/pages.yml](.github/workflows/pages.yml)），推送到 main 分支会自动触发部署。

## 文档编写规范

### Markdown 格式
- 使用 **GitHub Flavored Markdown** 语法
- 标题层级：`##` 开始（`#` 保留给文件标题）
- 代码块必须指定语言：` ```bash`、` ```python` 等
- 使用 MkDocs Material 扩展功能：
  - 告示块 (Admonitions): `!!! note`、`!!! warning`
  - 表格、任务列表、图标 emoji 等

### 文件命名约定
- 周刊文件：`2025-WXX-标题.md`（遵循 ISO 8601 周编号）
- 使用中文文件名（项目特性）
- 首页和附录文件直接命名：`index.md`、`相关项目推荐.md`

**示例**：
- `2025-W35-快速上手.md`（第35期，基础教程）
- `2025-W41-Sonnet升级与社区项目点评.md`（第41期，动态周刊）

### Front Matter（YAML 头）
每个文档文件应包含：
```yaml
---
title: 页面标题
description: 页面描述
nav_order: 导航顺序（数字）
---
```

## 项目架构注意事项

### 双配置系统
项目同时维护两套配置以确保兼容性：
- **mkdocs.yml**：主要配置，用于 MkDocs Material 构建
- **_config.yml**：Jekyll 配置，作为 GitHub Pages 的回退方案

**修改导航时**，两个文件都需要更新以保持一致性。

### 特殊目录
- `.specstory/`：SpecStory 扩展自动生成的 AI 聊天历史，可忽略
- `.playwright-mcp/`：Playwright 测试相关截图和资源，可忽略
- 两者都不是构建流程的一部分

### 静态资源
- 文档内图片直接引用本地文件：`![描述](./image.png)`
- 外部链接图片：`![描述](https://...)`
- 确保图片路径相对于 Markdown 文件位置

## 常见任务

### 发布新一期周刊

**每周五发布流程**：

1. **创建周刊文件**
   ```bash
   # 文件命名：YYYY-Www.md（如 2025-W42.md）
   # 复制上一期模板
   cp weekly/2025-W41.md weekly/2025-W42.md
   ```

2. **编写内容**
   - 必须包含的部分：
     - 🔥 本周热点（官方更新、模型升级等）
     - 💡 实战技巧（真实可验证的使用经验）
     - 🛠️ 工具生态观察（社区项目点评）
     - 📝 本周思考（技术观点或趋势分析）
     - 🎯 下周预告

   - **内容原则**（Linus 式标准）：
     - ❌ **禁止编造数据**：没有数据就不写，空着比造假强一万倍
     - ✅ **真实可验证**：所有点评基于实际使用或代码审查
     - ✅ **技术优先**：犀利但准确的技术分析，避免泛泛而谈
     - ✅ **实用主义**：解决实际问题，拒绝理论空谈

3. **更新索引和链接**
   ```bash
   # 需要修改 4 个文件（保持一致性）
   vim weekly/index.md      # 更新"最新一期"和往期表格
   vim index.md             # 更新首页"动态内容"部分
   vim README.md            # 更新 GitHub 项目说明
   vim mkdocs.yml           # 确认导航配置正确
   ```

4. **提交和发布**
   ```bash
   # 提交更改
   git add weekly/ index.md README.md
   git commit -m "发布周刊第XX期

   ## 核心内容
   - [简要列出本期亮点]

   🤖 Generated with [Claude Code](https://claude.com/claude-code)
   Co-Authored-By: Claude <noreply@anthropic.com>"

   # 创建 tag（版本号：v2025.XX，XX为周数）
   git tag -a v2025.42 -m "Claude Code 周刊 - 2025年第42周

   [发布说明详情]

   发布日期：YYYY-MM-DD"

   # 推送
   git push origin main && git push origin v2025.42

   # 创建 GitHub Release
   gh release create v2025.42 \
     --title "📰 Claude Code 周刊 - 2025年第XX周" \
     --notes "[完整的发布说明]"
   ```

5. **质量检查清单**
   - [ ] 所有数据真实可查（无编造内容）
   - [ ] 外部链接有效（项目存在且可访问）
   - [ ] 代码示例可运行（如有）
   - [ ] 4个文件链接一致（weekly/index.md, index.md, README.md, mkdocs.yml）
   - [ ] Front Matter 正确（title, description, date）
   - [ ] Tag 命名规范（v2025.XX）

### 添加新周刊期数
1. 在 `weekly/` 目录创建 `2025-WXX-标题.md` 文件
2. 添加 YAML front matter（title, description, date）
3. 更新 [mkdocs.yml](mkdocs.yml) 的 `nav:` 部分（在"动态周刊"分类下）
4. 更新 [weekly/index.md](weekly/index.md) 添加到周刊索引表格
5. 更新 [README.md](README.md) 和 [index.md](index.md) 的周刊列表

### 修改样式
- CSS：编辑 `stylesheets/extra.css`（如果存在）
- JavaScript：编辑 `javascripts/extra.js`（如果存在）
- 主题配置：修改 [mkdocs.yml](mkdocs.yml) 的 `theme:` 部分

### 验证链接
```bash
# 检查内部链接
mkdocs build --strict

# 手动检查外部链接（使用第三方工具）
# linkchecker http://127.0.0.1:8000/
```

## 协作指南

### Git 工作流
- 主分支：`main`（自动部署）
- 提交信息：清晰描述变更内容
- PR 标题：简明扼要，如"修正第35期代码示例"、"新增第42期周刊"

### 内容更新原则

**基本规范**：
- 保持术语一致性（如 "Claude Code" 而非 "ClaudeCode"）
- 周刊间交叉引用使用相对链接：`[第35期](./weekly/2025-W35-快速上手.md)`
- 代码示例必须经过验证，确保可运行
- 外部链接定期检查有效性

**内容诚信原则**（来自实际教训）：

1. **禁止编造数据**
   - ❌ 错误示例：`GitHub Stars: 328 ⭐ (周增长 +15)` - 编造的数字
   - ✅ 正确做法：如果没有真实数据，就不写这一部分
   - **Linus 原则**："如果你不知道，就说不知道。空着比撒谎强一万倍。"

2. **真实可验证**
   - 所有推荐项目必须真实存在且可访问
   - 点评必须基于实际使用或代码审查
   - 数据必须可溯源（提供链接或来源）

3. **避免过度包装**
   - ❌ 错误：`最全面的`、`最好的`、`史上最强` 等绝对化表述
   - ✅ 正确：具体描述功能和适用场景
   - 让数据和事实说话，而不是形容词

4. **链接准确性**
   - 所有 URL 必须指向正确地址
   - 定期检查外部链接有效性
   - GitHub 仓库链接格式：`https://github.com/用户名/仓库名`

5. **目录结构清晰**
   - 主次分明：基础教程（第35-40期）→ 动态周刊（第41期起）→ 附录
   - 所有周刊统一在 weekly/ 目录管理
   - 导航一致：mkdocs.yml、weekly/index.md、README.md、index.md 保持同步

### 内容语言规范
- 技术术语保留英文（如 "slash command"、"MCP"）
- 界面文本使用中文
- 代码注释和变量名遵循原始约定
- 周刊内容采用 Linus 式犀利但专业的技术分析风格

## 故障排查

### MkDocs 构建失败
- 检查 YAML 语法：`python -m yaml mkdocs.yml`
- 验证 Markdown 格式：确保代码块闭合、表格格式正确
- 查看完整错误输出：`mkdocs build --verbose`

### GitHub Pages 部署问题
- 检查 [Actions](https://github.com/cfrs2005/claude-guild-chinese/actions) 日志
- 确认 Pages 设置：Settings → Pages → Source 应为 "GitHub Actions"
- 验证 CNAME 文件内容：`claude-code.80aj.com`

### 本地预览样式异常
- 清理缓存：删除 `site/` 目录，重新 `mkdocs build`
- 检查浏览器控制台错误
- 验证 CSS/JS 文件路径

## 项目依赖

### Python 包
```bash
mkdocs>=1.5.0
mkdocs-material>=9.0.0
mkdocs-git-revision-date-localized-plugin>=1.2.0
```

### 安装完整依赖
```bash
# 如果项目后续添加 requirements.txt
pip install -r requirements.txt

# 或使用当前配置
pip install mkdocs-material mkdocs-git-revision-date-localized-plugin
```

## GitHub 项目配置

### About 部分设置

在 GitHub 仓库页面手动配置（路径：仓库首页 → 右上角 ⚙️）:

**Description** (简短描述，不包含 URL):
```
🚀 最全面的 Claude Code 中文学习指南 | 深挖未公开功能，提升开发效率 60-80%
```

**Website** (专门字段):
```
https://claude-code.80aj.com
```

**Topics** (标签，便于检索):
```
claude-code, chinese, tutorial, ai-coding, documentation
```

**为什么这样设置**:
- Description 字段用于简短描述，URL 会显得冗余
- Website 字段专门用于链接，会在仓库顶部显示为可点击按钮
- Topics 帮助用户通过标签发现项目

## 周刊内容模板

### 标准周刊结构

```markdown
---
title: Claude Code 周刊 - 2025年第XX周
description: [本期核心主题简述]
date: YYYY-MM-DD
---

# 📰 2025年第XX周 (MM月DD-DD日)

> 发布日期: YYYY-MM-DD | [返回周刊目录](./index.md) | [返回首页](../index.md)

---

## 🔥 本周热点

### [主要话题标题]

[官方更新、模型升级、重要事件等]

---

## 💡 实战技巧

### [技巧标题]

[真实可验证的使用经验、配置方法、最佳实践]

---

## 🛠️ 工具生态观察

### [分类标题]

| 项目 | 描述 | GitHub 链接 | 提及日期 |
|------|------|-------------|----------|
| **项目名** | 简短描述 | [用户名/仓库名](链接) | YYYY-MM-DD |

### 💭 深度点评

#### 1. **项目名** - 一句话定位

**现状/亮点/槽点**: [具体描述]

**点评**:
> [基于实际使用或代码审查的技术分析]
>
> **适用场景**: [明确说明适合谁用]

**快速开始**（如有）:
```bash
[可运行的示例代码]
```

---

## 📝 本周思考

**问题**: [值得思考的技术问题]

**我的观点**:
> [有深度的技术观点或趋势分析]

---

## 🎯 下周预告

下周计划深度解析:
- [主题1]
- [主题2]
- [主题3]

---

[下一期 →](./2025-WXX.md) | [周刊目录](./index.md) | [返回首页](../index.md)

---

**本期编辑**: cfrs2005
**发布时间**: YYYY年MM月DD日
**欢迎反馈**: [GitHub Issues](https://github.com/cfrs2005/claude-guild-chinese/issues)
```

### 内容质量标准

**✅ 好的点评示例**:
```markdown
**点评**:
> 这是本期最值得关注的项目！代码索引是 AI 代码助手的核心能力，官方回退后，社区能自己解决这个问题是件好事。
>
> **技术亮点**:
> - 使用向量数据库（Zilliz/Milvus）做语义搜索，而不是简单的关键词匹配
> - 支持增量索引，不需要每次都重新扫描整个代码库
>
> **建议**: 如果你的项目超过 50 个文件，强烈建议尝试。
```

**❌ 差的点评示例**:
```markdown
**点评**:
> 这个项目很厉害！功能强大，值得使用！
```

## 外部资源

- **原始英文指南**：[Cranot/claude-code-guide](https://github.com/Cranot/claude-code-guide)
- **MkDocs 官方文档**：https://www.mkdocs.org/
- **Material for MkDocs**：https://squidfunk.github.io/mkdocs-material/
- **在线网站**：https://claude-code.80aj.com
- **GitHub 仓库**：https://github.com/cfrs2005/claude-guild-chinese

---

## 项目管理要点总结

### 核心原则（来自实际教训）

1. **诚实第一** - 不编造数据，不夸大其词
2. **结构清晰** - 基础教程（第35-40期）、动态周刊（第41期起）、附录分开
3. **链接准确** - 所有 URL 指向正确地址
4. **质量优先** - 犀利但准确的技术分析
5. **实用主义** - 解决实际问题，拒绝空谈

### 常见错误及修正

| 错误 | 正确做法 |
|------|----------|
| 编造社区数据 | 删除或使用真实数据 |
| URL 写在 description | 使用 Website 字段 |
| 目录结构混乱 | 明确分层：基础教程→动态周刊→附录 |
| 泛泛而谈的点评 | 具体技术分析+适用场景 |
| 过时的 GitHub Pages 链接 | 更新为自定义域名 |
