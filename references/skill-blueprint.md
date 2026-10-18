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

## 五、开源规范（用户确认发布 GitHub 时）

- 仓库：gh 账号 **TiantianYeTTY**；git 提交身份 `Tiantian Ye <13585886743@163.com>`；main 分支。
- **README 格式对齐 TiantianYeTTY/lab-report-completion**：`# 仓库名` → 顶部 blockquote（一句话定位 + 作者 **TiantianYe** + App Store 代表作品签名行）→ `## ✨ 这是什么` → `## 🎯 使用场景`（要素表 + 典型触发指令示例引用块）→ `## 📦 安装`（git clone 到 `~/.workbuddy/skills/`）→ `## 🚀 使用方法（工作流概览）`（阶段表 + 输入/产出 ASCII 箭头图）→ `## 🔧 环境依赖` → `## ⚠️ 重要说明`（编号硬约束）→ `## 📁 目录结构` → `## 🐛 已知踩坑点（节选）` → `## 👤 作者` → `## 📄 许可`。
- 作者签名行（固定文案）：`作者在 App Store 中的其他代表作品：水课帮-课堂提问助手；识词本-单词笔记；投影与视图3D教具；解构照片`
- Python 文件首行：`# Designed by TiantianYe`
- README 不写内部过程性信息（版本演进、"用户已弃用 X 路线"之类）——开源读者只关心它是什么、怎么用。
- 发布前：敏感信息扫描（token/key/password）→ 首次 commit → `gh repo create <name> --public --source=. --push` → `gh api` 核对远端文件齐全。
- 用户网页端可能自行改过 README：push 被拒时先 `git fetch` 看远端提交，rebase 合并并尊重其改动，禁止 force push。

## 六、验证命令

```bash
python3 /Applications/AI/WorkBuddy.app/Contents/Resources/app.asar.unpacked/resources/plugins/workbuddy-builtin/skills/skill-creator/scripts/package_skill.py <skill目录> /tmp
```

无 ERROR 才算通过。产物 zip 在 /tmp（或用户指定位置）。
