# JS Language Profile Normalization Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Normalize Python-oriented wording in `00-配置/` so the vault consistently treats JavaScript as the default learning, explanation, and solution language.

**Architecture:** Make one focused content pass over `00-配置/`, with `00-配置/学员档案.md` as the primary edit target and the remaining config files treated as verification targets. Preserve headings, tables, and checklist structure; only rewrite language-preference and learning-goal wording that conflicts with the JavaScript-first profile.

**Tech Stack:** Markdown, grep, git, Claude Code file edit tools

## Global Constraints

- Only modify files under `00-配置/`.
- Preserve existing Markdown structure, headings, tables, and checklist layout.
- Default solution language, explanation language, and code output language must all be JavaScript.
- If external material is in another language, the config should instruct AI to convert it to JavaScript before explaining it.
- Do not expand the work into `02-Wiki/`, `03-学习笔记/`, or unrelated template refactors.
- Only touch `00-配置/学习日志.md`, `00-配置/进度看板.md`, or `00-配置/全局索引.md` if they contain wording that conflicts with the JavaScript-first profile.

---

### Task 1: Confirm the exact Python-oriented wording in `00-配置/`

**Files:**
- Inspect: `00-配置/学员档案.md:13-24`
- Inspect: `00-配置/学员档案.md:72-74`
- Inspect: `00-配置/学习日志.md`
- Inspect: `00-配置/进度看板.md`
- Inspect: `00-配置/全局索引.md`

**Interfaces:**
- Consumes: Current Markdown content under `00-配置/`
- Produces: A verified inventory of Python-oriented wording to rewrite, limited to config content

- [ ] **Step 1: Re-run a scoped keyword search**

```bash
grep -RInE 'Python|\bpy\b|collections|heapq|bisect' ./00-配置
```

Expected: matches only lines that explicitly mention Python-oriented wording, with `00-配置/学员档案.md` as the primary hit.

- [ ] **Step 2: Read the hit locations before editing**

Inspect these current snippets in `00-配置/学员档案.md`:

```markdown
| 编程基础 | 有一定的 js/ts 代码基础                     |
- 刷题语言：JavaScript
  - Python 熟练度：不太熟，只作为参考
  - 当前目标：14 天完成 LeetCode Hot 100 第一轮
  - 学习方式：先自己思考，再看提示，最后看题解
  - 代码要求：所有代码优先使用 JavaScript
  - 讲解要求：遇到 Python 代码时，请转换成 JavaScript，并解释 JS 写法
```

and:

```markdown
### 次要目标
- [ ] 能够用 Python 标准库（collections、heapq、bisect 等）熟练解题
- [ ] 掌握常见的时空复杂度分析方法
```

Expected: these are the only lines that require wording changes for this task.

- [ ] **Step 3: Confirm the non-target files do not need edits**

Read `00-配置/学习日志.md`, `00-配置/进度看板.md`, and `00-配置/全局索引.md` and confirm they do not contain conflicting Python-first instructions.

Expected: no changes needed in those files unless a new conflicting line is discovered during review.

- [ ] **Step 4: Commit the inspection checkpoint if working on a branch**

```bash
git status --short
```

Expected: no content changes yet; if a reviewer wants an explicit checkpoint note, record that the edit scope is limited to `00-配置/学员档案.md`.

### Task 2: Rewrite `00-配置/学员档案.md` into a consistent JavaScript-first profile

**Files:**
- Modify: `00-配置/学员档案.md:13-24`
- Modify: `00-配置/学员档案.md:72-74`

**Interfaces:**
- Consumes: The verified hit list from Task 1
- Produces: A JavaScript-first learner profile that later AI sessions can follow without ambiguity

- [ ] **Step 1: Replace the language-profile block with JavaScript-first wording**

Update the block beginning at `00-配置/学员档案.md:13` so it reads exactly as follows:

```markdown
| 编程基础 | 有一定的 JavaScript / TypeScript 代码基础        |
| 算法基础 | （力扣hot100做了80道，一遍）                  |
| 目标   | 14 天速通 LeetCode Hot 100，形成自己的算法知识体系 |
| 可用时间 | 每天约 4-6 小时                          |
| 学习周期 | （7.24）～ （8.6）（14 天）                 |
- 刷题语言：JavaScript
  - JavaScript / TypeScript 基础：已有一定代码基础，JavaScript 作为主刷题语言
  - 当前目标：14 天完成 LeetCode Hot 100 第一轮
  - 学习方式：先自己思考，再看提示，最后看题解
  - 代码要求：所有代码优先使用 JavaScript
  - 讲解要求：默认使用 JavaScript 讲解；若参考资料是其他语言，需要先转换成 JavaScript 再解释对应写法
  - 当前重点：掌握题型模板，而不是死记答案
```

Expected: the learner profile no longer describes Python proficiency as a reference point and instead states a JavaScript-first learning setup.

- [ ] **Step 2: Replace the secondary goal with JavaScript problem-solving tooling**

Update the `### 次要目标` list so it reads exactly as follows:

```markdown
### 次要目标
- [ ] 能够熟练使用 JavaScript 常见解题工具（Array、Map、Set、栈/队列模拟、二分模板等）
- [ ] 掌握常见的时空复杂度分析方法
```

Expected: the learner goal measures progress using JavaScript tools and templates instead of Python standard-library familiarity.

- [ ] **Step 3: Review the edited file for structure preservation**

Check that `00-配置/学员档案.md` still preserves:

```markdown
## 一、基本信息
## 二、知识底子评估
## 三、学习习惯
## 四、学习目标
## 五、学习进度概览（由 AI 自动更新）
```

Expected: only wording changes, no accidental heading, table, or checklist damage.

- [ ] **Step 4: Commit the content rewrite**

```bash
git add '00-配置/学员档案.md'
git commit -m "docs: unify learner profile around JavaScript"
```

Expected: one commit containing only the intended content normalization.

### Task 3: Verify the directory is free of conflicting Python-oriented guidance

**Files:**
- Verify: `00-配置/学员档案.md`
- Verify: `00-配置/学习日志.md`
- Verify: `00-配置/进度看板.md`
- Verify: `00-配置/全局索引.md`

**Interfaces:**
- Consumes: The edited `00-配置/学员档案.md` from Task 2
- Produces: A verified `00-配置/` directory with no remaining Python-first guidance relevant to this spec

- [ ] **Step 1: Re-run the scoped keyword search**

```bash
grep -RInE 'Python|\bpy\b|collections|heapq|bisect' ./00-配置
```

Expected: no matches, or only matches that are clearly not language-guidance conflicts and do not require edits.

- [ ] **Step 2: Re-read the final learner-language rules**

Confirm the final `00-配置/学员档案.md` contains these rules:

```markdown
- 刷题语言：JavaScript
- 代码要求：所有代码优先使用 JavaScript
- 讲解要求：默认使用 JavaScript 讲解；若参考资料是其他语言，需要先转换成 JavaScript 再解释对应写法
- [ ] 能够熟练使用 JavaScript 常见解题工具（Array、Map、Set、栈/队列模拟、二分模板等）
```

Expected: the config unambiguously sets JavaScript as the default solution and explanation language.

- [ ] **Step 3: Check the working tree summary**

```bash
git diff -- '00-配置/'
```

Expected: only intended wording changes under `00-配置/学员档案.md`, unless a reviewer explicitly approved extra config-file fixes.

- [ ] **Step 4: Record completion status**

Use this completion summary when handing off:

```markdown
已完成 `00-配置/` 语言画像统一：当前默认刷题、题解输出与讲解语言均为 JavaScript；未扩散修改到其他学习内容目录。
```

Expected: the implementation closes with a concise statement that maps back to the approved spec.
