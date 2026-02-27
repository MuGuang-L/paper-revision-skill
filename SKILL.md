---
name: paper-revision
description: Learn writing style from reference papers and revise your LaTeX draft
triggers:
  - "paper revision"
  - "revise my paper"
  - "learn writing style"
  - "academic writing"
  - "polish my paper"
  - "improve my writing"
  - "修改论文"
  - "润色论文"
  - "latex revision"
agent: writer
model: sonnet
---

# Paper Revision Skill (LaTeX-Enhanced)

Learn writing style, language patterns, and structure from reference papers, then apply those patterns to revise your LaTeX manuscript. Supports direct editing of `.tex` files and integrates with LaTeX editor workflows.

## When to Use

This skill activates when:
- User wants to upload reference papers for style learning
- User needs help revising a LaTeX draft to match academic writing conventions
- User wants to improve the language quality of their paper
- User asks to "learn the writing style" from papers
- User wants their paper "polished" or "improved"
- User is working with LaTeX files and needs direct `.tex` editing

## What It Does

This skill follows a three-phase workflow with LaTeX-specific enhancements:

### Phase 1: Reference Papers Folder Setup

**Folder Structure:**
```
{worktree}/
├── .omc/
│   └── papers/
│       ├── refs/           # Reference papers (PDF or text)
│       │   ├── ref-1.pdf
│       │   ├── ref-2.pdf
│       │   └── ref-3.pdf
│       └── profiles/       # Generated style profiles
│           └── style-profile.md
├── latex/
│   ├── main.tex            # Main LaTeX file
│   ├── chapters/           # Chapter files
│   ├── sections/           # Section files
│   └── references.bib      # Bibliography
└── revised/
    └── draft-revised.tex   # Output revised draft
```

**User Workflow:**
1. Create `.omc/papers/refs/` folder in your project
2. Place 2-5 reference papers (PDF format) in the folder
3. Skill automatically detects and analyzes all papers in the folder

### Phase 2: Style Analysis

**Analysis Process:**
1. **PDF Extraction** - Use the `Read` tool to extract text from PDF papers
   - The `Read` tool natively supports PDF files - use it directly
   - For large PDFs (>10 pages), use the `pages` parameter to read in chunks (e.g., pages: "1-10", "11-20")
   - Read each PDF in `.omc/papers/refs/` sequentially until all content is extracted

2. **Writing Pattern Analysis:**
   - Sentence structure and length preferences
   - Transition phrases and connectors
   - Technical terminology usage
   - Paragraph organization patterns
   - Citation style (author-year, numbered, etc.)
   - Abstract and introduction structure
   - Results/discussion language patterns

3. **LaTeX-Specific Patterns:**
   - Section/subsection organization
   - Figure/table referencing style
   - Equation presentation
   - Citation commands (`\cite`, `\citet`, `\citep`)
   - Cross-referencing patterns (`\ref`, `\label`)

### Phase 3: Style Profile Creation

Create a structured style profile at `.omc/papers/profiles/style-profile.md`:

```markdown
# Style Profile

## Vocabulary Patterns
- Academic phrases: "we propose", "our results demonstrate", ...
- Hedging: "suggests", "may indicate", "potentially"
- Certainty: "clearly", "demonstrates", "establishes"

## Sentence Patterns
- Average sentence length: 20-25 words
- Passive/Active ratio: 40/60
- Common structures: [Pattern examples]

## LaTeX Patterns
- Citation style: \citet{author} for narrative, \citep{author} for parenthetical
- Section structure: Introduction → Related Work → Method → Experiments → Conclusion
- Figure refs: "Figure~\ref{fig:name}" (capitalized)
- Equation refs: "Eq.~\eqref{eq:name}"

## Section Templates
### Abstract
- Sentence 1: Problem statement
- Sentence 2: Gap identification
- Sentence 3: Proposed approach
- Sentence 4: Key results
- Sentence 5: Implications

### Introduction
- Paragraph 1: Broad context
- Paragraph 2: Specific problem
- Paragraph 3: Limitations of prior work
- Paragraph 4: Our contributions (bulleted)
```

### Phase 4: LaTeX Draft Revision

**Direct `.tex` File Editing:**

The skill can directly read and modify LaTeX files:

1. **Preserve LaTeX Structure:**
   - Keep all `\label{}` and `\ref{}` intact
   - Preserve `\cite{}` commands
   - Maintain math environments (`$$`, `\[`, `\begin{equation}`)
   - Don't modify figure/table environments

2. **Apply Style Improvements:**
   - Revise text content only (not LaTeX commands)
   - Improve sentence flow in paragraphs
   - Enhance academic tone
   - Fix grammar and language issues
   - Match learned section patterns

3. **Track Changes:**
   - Use LaTeX diff format: `\deleted{old}` and `\added{new}`
   - Or create a `.tex.diff` file with changes
   - Generate a summary document explaining key revisions

## Agent Delegation

```
Task(
  subagent_type="oh-my-claudecode:writer",
  model="sonnet",
  name="latex-style-analyzer",
  prompt="LATEX PAPER STYLE ANALYSIS TASK

Analyze reference papers and extract writing style patterns with LaTeX-specific focus.

Input: Reference papers from .omc/papers/refs/

Analysis Tasks:
1. Extract text content from PDF papers
2. Identify vocabulary patterns (academic phrases, hedging, certainty)
3. Analyze sentence structures (length, complexity, voice)
4. Map paragraph organization and transitions
5. Document LaTeX-specific patterns:
   - Citation commands used
   - Section/subsection structure
   - Figure/table/equation referencing style
6. Extract abstract and introduction templates

Output: Style profile at .omc/papers/profiles/style-profile.md"
)

Task(
  subagent_type="oh-my-claudecode:writer",
  model="sonnet",
  name="latex-paper-reviser",
  prompt="LATEX PAPER REVISION TASK

Revise the LaTeX draft using the learned style profile.

Input:
- Draft: latex/main.tex (or specified .tex file)
- Style profile: .omc/papers/profiles/style-profile.md

Revision Rules:
1. PRESERVE all LaTeX commands (\cite, \ref, \label, math, figures, tables)
2. MODIFY only text content between commands
3. Apply vocabulary patterns from style profile
4. Improve sentence flow and academic tone
5. Match section structure patterns

Output:
- Revised .tex file with tracked changes
- Change summary document
```

## Workflow

### Step 1: Setup Reference Folder

```bash
# User creates reference folder
mkdir -p .omc/papers/refs
mkdir -p .omc/papers/profiles
```

**User places reference papers:**
```
.omc/papers/refs/
├── acl-paper-1.pdf
├── acl-paper-2.pdf
└── acl-paper-3.pdf
```

### Step 2: Analyze References

Skill invocation:
```
/paper-revision analyze .omc/papers/refs/
```

Actions:
- Scan `.omc/papers/refs/` for all PDF files
- Extract text from each paper
- Analyze writing patterns
- Generate style profile at `.omc/papers/profiles/style-profile.md`

### Step 3: Specify LaTeX Draft

User provides the LaTeX file to revise:
```
/paper-revision revise latex/main.tex
```

Or specify a chapter/section:
```
/paper-revision revise latex/chapters/method.tex
```

### Step 4: Apply Revision

Actions:
- Read the specified `.tex` file
- Apply style profile
- Preserve LaTeX structure
- Write revised output to `revised/` folder

### Step 5: Review and Iterate

Output delivered:
- `revised/main-revised.tex` - Full revised LaTeX file
- `revised/changes.md` - Summary of all changes
- `revised/main-diff.tex` - Version with `\added{}` and `\deleted{}` markup

User can:
- Compile the revised `.tex` to verify formatting
- Request specific section re-revision
- Accept/reject individual changes

## Tool Usage

- Use `Read` to extract content from PDF and `.tex` files (Read tool natively supports PDF)
- For large PDFs, use `pages` parameter: `Read(path, pages="1-10")`
- Use `Edit` to make surgical changes to `.tex` content
- Use `Write` to save style profiles and revised drafts
- Use `Glob` to find all `.tex` files and PDFs in the project
- Use `Task` with `oh-my-claudecode:writer` agent for analysis and revision
- Use `project_memory_add_note` to persist style profiles

## File Locations

| Purpose | Location |
|---------|----------|
| Reference papers (input) | `{worktree}/.omc/papers/refs/` |
| Style profiles | `{worktree}/.omc/papers/profiles/style-profile.md` |
| LaTeX source | `{worktree}/latex/` or user-specified |
| Revised output | `{worktree}/revised/` |
| Change tracking | `{worktree}/revised/changes.md` |

## LaTeX-Specific Handling

### What Gets Preserved

```latex
% All commands preserved exactly
\cite{author2023}
\ref{fig:architecture}
\label{sec:method}
\begin{equation}
  E = mc^2
\end{equation}
\begin{figure}[t]
  \centering
  \includegraphics{architecture.pdf}
  \caption{System architecture}
  \label{fig:architecture}
\end{figure}
```

### What Gets Revised

```latex
% Text content between commands gets improved
% BEFORE:
\section{Introduction}
In this paper we do thing. The method is good.
We get better results than before.

% AFTER (revised with academic tone):
\section{Introduction}
In this paper, we present a novel approach to X.
Our method demonstrates significant improvements over prior work.
Experimental results show that our approach achieves state-of-the-art performance.
```

### Citation Style Detection

The skill detects and matches the citation style from reference papers:

| Detected Pattern | Applied Revision |
|-----------------|------------------|
| `\citep{}` for parenthetical | Use `\citep{}` consistently |
| `\citet{}` for narrative | Use `\citet{}` for "Author et al. showed" |
| Numbered `[1]` | Maintain numbered style |
| Author-year `(Smith, 2023)` | Maintain author-year style |

## Examples

<Good>
User: "我把参考文献都放在 .omc/papers/refs/ 目录里了，帮我分析风格然后修改 latex/main.tex"
Action: 扫描参考文献目录，提取 PDF 内容，分析写作模式（包括 LaTeX 引用风格），然后直接修改 main.tex 文件，保留所有 LaTeX 命令。
Why good: 明确的目录结构，LaTeX 文件路径清晰。
</Good>

<Good>
User: "I've placed 3 ACL papers in .omc/papers/refs/, analyze them and create a style profile. Then revise my Introduction section in latex/chapters/intro.tex"
Action: Analyze the 3 ACL papers, extract ACL-specific patterns (citation style, section structure), create profile, then revise only the introduction chapter.
Why good: Scoped revision target, clear reference set.
</Good>

<Good>
User: "润色我的论文，用 diff 格式显示修改"
Action: 修改论文，输出带\added{}和\deleted{}标记的 LaTeX 文件，方便用户逐条审查。
Why good: 明确的输出格式要求，便于审阅。
</Good>

<Bad>
User: "修改论文"
Action: 不知道参考文献在哪里，不知道要修改哪个文件。
Why bad: 缺少必要信息。应该询问参考文献位置和目标文件。
</Bad>

<Bad>
User: "把这些 PDF 都改了" (提供 10 个 PDF 在 latex/ 目录)
Action: latex/ 目录应该包含 LaTeX 源文件，不是 PDF。PDF 是参考文献，应该放在 .omc/papers/refs/。
Why bad: 目录用途混淆。应该澄清：参考文献放 .omc/papers/refs/，LaTeX 源文件放 latex/。
</Bad>

## Escalation And Stop Conditions

- **Large PDF handling**: If PDF > 20 pages, read in chunks using `pages` parameter (e.g., "1-15", "16-30")
- **Wrong file type**: If user places PDFs in `latex/` directory, clarify the folder structure
- **Compilation issues**: If revised `.tex` doesn't compile, investigate:
  - Did we accidentally modify a LaTeX command? → Fix immediately
  - Encoding issues? → Ensure UTF-8
- **Large files**: If `.tex` > 50 pages, suggest chapter-by-chapter revision
- **Bibliography**: Do NOT modify `.bib` files automatically - citation style should be preserved

## Final Checklist

- [ ] Reference papers placed in `.omc/papers/refs/`
- [ ] Style profile generated at `.omc/papers/profiles/style-profile.md`
- [ ] Target `.tex` file identified
- [ ] All LaTeX commands preserved (citations, references, math, figures, tables)
- [ ] Text content revised to match style profile
- [ ] Changes tracked with `\added{}` / `\deleted{}` or separate diff file
- [ ] Revised file compiles without errors (user verifies)
- [ ] Change summary document provided

## Configuration

Optional settings via `~/.claude/.omc-config.json`:

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

## Quick Start

```bash
# 1. Setup folders
mkdir -p .omc/papers/refs .omc/papers/profiles revised

# 2. Place 2-5 reference papers (PDF) in .omc/papers/refs/

# 3. Invoke skill
/paper-revision

# Or be specific:
/paper-revision analyze .omc/papers/refs/
/paper-revision revise latex/main.tex

# 4. Review output in revised/
```

## Integration with LaTeX Editors

### VS Code + LaTeX Workshop

1. Install "LaTeX Workshop" extension
2. After revision, open `revised/main-revised.tex`
3. Use built-in diff viewer to compare with original
4. Compile to verify formatting

### Overleaf

1. Download `revised/main-revised.tex`
2. Upload to Overleaf project
3. Use Overleaf's "Track Changes" feature (if available)
4. Compile to verify

### Vim/Neovim + vimtex

1. Set vimtex to compile from `revised/` folder
2. Use `:VimtexCompile` to verify
3. Use fugitive.vim for diff viewing

Task: {{ARGUMENTS}}
