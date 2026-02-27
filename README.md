# paper-revision Skill

一个用于 LaTeX 论文润色和修改的 Oh-My-ClaudeCode Skill。

## 功能

- 📄 **自动读取 PDF** - 直接从参考文献 PDF 中提取内容
- 🎯 **学习写作风格** - 分析参考文献的词汇、句式、结构模式
- ✍️ **LaTeX 感知编辑** - 保留所有 LaTeX 命令，仅修改文本内容
- 📊 **风格档案** - 生成可重用的写作风格配置文件
- 🔍 **变更追踪** - 使用 LaTeX diff 格式标记修改
- 📝 **审稿意见处理** - 解析审稿意见，生成逐点回复信（Rebuttal）
- 🎯 **优先级修改** - 支持指定重点参考某位审稿人的意见

## 前置要求

- [Oh-My-ClaudeCode](https://github.com/Yeachan-Heo/oh-my-claudecode) 已安装
- Claude Code 支持 PDF 读取

## 安装

将 `SKILL.md` 复制到你的 skills 目录：

```bash
# 方法 1：克隆后复制
git clone https://github.com/<your-username>/paper-revision-skill.git
cp -r paper-revision-skill/SKILL.md ~/.claude/skills/omc-learned/paper-revision/

# 方法 2：直接作为子模块
cd ~/.claude/skills/omc-learned/
git submodule add https://github.com/<your-username>/paper-revision-skill.git paper-revision
```

## 使用方法

### 1. 准备参考文献

```bash
# 在项目根目录创建文件夹
mkdir -p .omc/papers/refs
mkdir -p .omc/papers/profiles
```

将 2-5 篇参考论文（PDF 格式）放入 `.omc/papers/refs/` 目录。

### 2. 分析风格并修改论文

```bash
# 方式 1：直接使用默认配置
/paper-revision

# 方式 2：分步执行
/paper-revision analyze .omc/papers/refs/
/paper-revision revise latex/main.tex
```

### 3. 查看输出

修改后的文件会输出到 `revised/` 目录：
- `revised/main-revised.tex` - 修改后的 LaTeX 文件
- `revised/changes.md` - 修改说明文档
- `revised/main-diff.tex` - 带 diff 标记的版本

---

## 审稿意见回复（Rebuttal）

### 1. 准备审稿意见

```bash
mkdir -p .omc/papers/reviews
mkdir -p .omc/papers/response
```

将审稿意见放入 `.omc/papers/reviews/`：
- 支持 `.txt` 格式（直接复制粘贴）
- 支持 `.pdf` 格式（自动读取）

```
.omc/papers/reviews/
├── reviewer-1.txt
├── reviewer-2.txt
└── reviewer-3.txt
```

### 2. 分析审稿意见

```bash
# 分析审稿意见，生成修改计划
/paper-revision analyze-reviews

# 指定重点关注的审稿人
/paper-revision analyze-reviews --focus "Reviewer 2"
```

### 3. 根据意见修改论文

```bash
# 带审稿意见修改
/paper-revision revise latex/main.tex --with-reviews

# 指定优先级
/paper-revision revise latex/main.tex --focus "Reviewer 2's major concerns"
```

### 4. 生成回复信

```bash
/paper-revision generate-response
```

输出：
- `.omc/papers/response/response-to-reviewers.tex` - 逐点回复信
- 格式：审稿人意见 → 我们的回复 → 修改内容 → 原文定位

## 目录结构

```
your-project/
├── .omc/
│   └── papers/
│       ├── refs/           # 参考文献 PDF
│       │   ├── ref-1.pdf
│       │   ├── ref-2.pdf
│       │   └── ref-3.pdf
│       └── profiles/       # 生成的风格档案
│           └── style-profile.md
├── latex/
│   ├── main.tex            # 原始 LaTeX 文件
│   └── chapters/           # 章节文件
└── revised/                # 输出目录
    └── main-revised.tex
```

## 配置选项

在 `~/.claude/.omc-config.json` 中添加：

```json
{
  "paper-revision": {
    "refsFolder": ".omc/papers/refs/",
    "profileFolder": ".omc/papers/profiles/",
    "latexSourceFolder": "latex/",
    "outputFolder": "revised/",
    "minReferencePapers": 2,
    "maxReferencePapers": 5,
    "trackChanges": true,
    "diffFormat": "latex-diff",
    "preserveBib": true
  }
}
```

## 触发词

以下短语会激活此 skill：
- "paper revision"
- "revise my paper"
- "learn writing style"
- "academic writing"
- "polish my paper"
- "improve my writing"
- "修改论文"
- "润色论文"
- "latex revision"

## 工作原理

### Phase 1: 参考文献分析
1. 使用 `Read` 工具读取 PDF 内容（支持分块读取大文件）
2. 提取文本用于风格分析

### Phase 2: 风格学习
- 词汇模式（学术短语、模糊语、确定性表达）
- 句式结构（平均句长、主被动比例）
- LaTeX 模式（引用命令、章节结构）
- 段落组织（摘要、引言模板）

### Phase 3: 风格档案生成
创建 `.omc/papers/profiles/style-profile.md` 配置文件

### Phase 4: LaTeX 草稿修改
- 保留所有 LaTeX 命令（`\cite`, `\ref`, `\label`, 公式，图表）
- 仅修改命令之间的文本内容
- 应用学习到的风格模式

## 示例

### 示例 1：完整流程
```
用户："我把参考文献都放在 .omc/papers/refs/ 目录里了，帮我分析风格然后修改 latex/main.tex"

→ 扫描参考文献，提取 PDF 内容
→ 分析写作模式（包括 LaTeX 引用风格）
→ 直接修改 main.tex，保留所有 LaTeX 命令
```

### 示例 2：指定章节
```
用户："I've placed 3 ACL papers in .omc/papers/refs/, analyze them and create a style profile. Then revise my Introduction section in latex/chapters/intro.tex"

→ 分析 3 篇 ACL 论文
→ 提取 ACL 特定模式
→ 仅修改 introduction 章节
```

### 示例 3：Diff 格式
```
用户："润色我的论文，用 diff 格式显示修改"

→ 输出带 \added{} 和 \deleted{} 标记的 LaTeX 文件
→ 方便逐条审查
```

## 注意事项

- **PDF 大小**：>20 页的 PDF 会自动分块读取
- **参考文献数量**：建议 2-5 篇，太少学不到足够模式，太多会稀释风格
- **LaTeX 命令**：所有命令会被保留，不会被修改
- **Bib 文件**：`.bib` 文件不会被自动修改

## License

MIT
