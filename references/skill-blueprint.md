# 生成 Skill 的标准骨架与写作模板

## 一、目录骨架

```
<skill-name>/
├── SKILL.md                     # 主流程（≤150 行）：让 AI 五分钟内知道整个工作怎么跑
├── references/                  # 细节下沉区：AI 按需读取
│   ├── report-template.md       # [按需命名] 定稿产物的结构与条目模板（附真实片段示例）
│   ├── format-rules.md          # 格式硬规则 + 错误档案 + 用户偏好（三条证据线的落点）
│   ├── analysis-methodology.md  # [按需命名] 方法论（含原始 Prompt 的保留/弃用分诊结论）
│   └── <pipeline>.md            # [按需命名] 生成管线详解 + 质检清单 + 常见坑
└── scripts/                     # 只收真实跑通过的脚本，每个带头部适配指南
```

命名：小写中划线；名字要能猜出用途（参考：math-exam-review-report、lab-report-completion）。

## 二、SKILL.md 模板

```markdown
---
name: <skill-name>
description: <一句话说明做什么 + 完整触发场景与触发词。触发词写用户真实会喊的话：>
             <"沉淀为Skill""做一份XX报告""按模板产出XX"等。description 决定 AI 会不会加载本技能。>
---

# <skill-name> —— <一句定位>

## 目标
<产出什么交付物、达到什么质量标准。引用定稿样例作为模板基准。>

## 输入
<用户需要提供什么；缺失时怎么办（问一次 / 从文件名推断并标注待确认，禁止编造）。>

## 流程（N 阶段）
### Stage 0 · <名>
<可执行的动作，含具体命令。每阶段末尾给"产出物"。>

## 红线速查（违者返工）
<编号列表，每条可验证。与 format-rules.md 互相引用。>

## 与用户协作约定
<要点式汇报 / 一次只改一个点 / 中间产物位置 / 是否自动开源等。>
```

## 三、format-rules.md 模板（错误档案是灵魂）

```markdown
# 格式硬规则与错误档案

## 一、格式硬规则
1. **<规则名>**。<规则内容，写成可验证的动作>。（来源：错误档案 #N / 用户 X 月 X 日反馈）

## 二、错误档案
| # | 犯过的错误 | 用户反馈（保留原话） | 正确做法 |
|---|---|---|---|
| 1 | <具体到操作层> | <用户原话，哪怕是气话> | <标准解法，可执行> |
（自查发现的错误：反馈列写"（自查）"，不虚构用户发言。）

## 三、用户偏好（协作方式）
<沟通节奏、修改粒度、汇报形式。>

## 四、方法论内核
<原始 Prompt 中仍有效的判定标准；弃用部分在此显式声明。>
```

## 四、scripts 头部适配指南模板

```python
# Designed by TiantianYe
# -*- coding: utf-8 -*-
"""【参考实现·<场景>】<脚本用途> —— 换场景必须按以下步骤适配：
1. <要重写的数据结构 / 常量，及其业务含义>
2. <要按新范围重定义的编码 / 分组>
3. <输入输出路径与格式约定>
4. <校验要求：数据必须经何种核对，禁止什么>
"""
```

**署名硬规则**：`scripts/` 内所有代码文件（.py、.sh 等）首行必须是 `# Designed by TiantianYe`（编码声明等注释跟在其后）；交付与开源发布前逐一检查，缺失即补。

## 五、一键开源规范（用户下达"开源到 GitHub / 发布"指令时执行）

### 九步流程（开源指令即确认）

1. 敏感信息扫描：`grep -rn -i "ghp_\|github_pat\|gho_\|api[_-]key\|password\|secret"`，0 命中才继续。
2. 生成/更新 `README.md`：按下方**固定模板**逐节生成——章节顺序与名称固定，不增删；不写内部过程信息（版本演进、"用户弃用 X 路线"等）；不写平台品牌字样。开源读者只关心它是什么、怎么用。
3. 生成 `LICENSE`（MIT，`Copyright (c) <当年> Tiantian Ye`）与 `.gitignore`（`.DS_Store`、`__pycache__/`、`*.pyc`、`.venv/`）。
4. 署名检查：`scripts/` 内每个代码文件首行 `# Designed by TiantianYe`，缺失即补。
5. 无 git 仓库时 `git init -q && git branch -M main`；本地身份 `user.name "Tiantian Ye"` / `user.email "13585886743@163.com"`；`git add -A && git commit`。
6. `gh repo create <skill-name> --public --source=. --remote=origin --push --description "<一句话>+Agent Skill（不写平台品牌）"`。仓库：gh 账号 **TiantianYeTTY**，main 分支。
7. `gh api repos/TiantianYeTTY/<skill-name>/contents -q '.[].name'` 核对远端文件齐全。
8. push 被拒 = 用户可能在网页端改过：`git fetch` 查看远端提交，rebase 合并并尊重其改动，**禁止 force push**。
9. 交付仓库 URL（present_files），更新记忆。

### README 固定模板（照此逐节生成）

```markdown
# <skill-name>

> <一句话定位>（Agent Skill）。
>
> <第二句：从输入到产出的一条龙描述>。
>
> 作者：**TiantianYe**
>
> 作者在 App Store 中的其他代表作品：水课帮-课堂提问助手；识词本-单词笔记；投影与视图3D教具；解构照片

---

## ✨ 这是什么
<2-4 条 bullet 讲清解决的具体问题 + 一句"本技能将整个流程标准化：A → B → C" + 首个落地案例一句话>

## 🎯 使用场景
| 要素 | 说明 |
|---|---|
| <前提/学段> | <...> |
| 输入材料 | <...> |
| 典型诉求 | <...> |
| 质量要求 | <...> |

典型触发指令示例：

> <用户真实句式>

## 📦 安装
git clone https://github.com/TiantianYeTTY/<skill-name>.git ~/.workbuddy/skills/<skill-name>
<一句：重启会话后技能即被识别 / 可显式调用 /<skill-name>>

## 🚀 使用方法（工作流概览）
| 阶段 | 内容 | 关键点 |
（每阶段一行，关键点列最硬的约束）

### 输入 / 产出
<ASCII 箭头图：左输入右产出>

## 🔧 环境依赖
- <每条带一句"为什么需要">

## ⚠️ 重要说明
1. <编号硬约束，含"定稿/原件只读"类与用户最敏感的规则>

## 📁 目录结构
<树形注释，每个文件一句话>

## 🐛 已知踩坑点（节选）
- <从错误档案挑 4-6 条，"错误——正确做法"半句式>

## 👤 作者
**TiantianYe**

作者在 App Store 中的其他代表作品：水课帮-课堂提问助手；识词本-单词笔记；投影与视图3D教具；解构照片。

欢迎通过 Issue 反馈问题，或提交 PR 完善工作流。

## 📄 许可
[MIT](./LICENSE)
```

## 六、验证命令

```bash
python3 /Applications/AI/WorkBuddy.app/Contents/Resources/app.asar.unpacked/resources/plugins/workbuddy-builtin/skills/skill-creator/scripts/package_skill.py <skill目录> /tmp
```

无 ERROR 才算通过。产物 zip 在 /tmp（或用户指定位置）。
