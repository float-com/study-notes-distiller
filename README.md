# Study Notes Distiller (学习笔记提炼专家)

> 🚀 一个面向 Codex 等 Agent 宿主的任务专用技能（Skill），将多模态碎片化学习材料（带圈画/文字批注截图、视频时间戳、技术文章链接、口语化随记）自动提炼并结构化为生产级中文技术 Markdown 笔记。

---

## 💡 为什么需要它？

在日常技术学习过程中，我们常常面临以下痛点：
- **截图易散落**：课程视频或官方文档中截了大量关键步骤，但过后找不到对应上下文。
- **批注难沉淀**：截图中用红圈、箭头勾画的关键参数或手写随记无法转化为规范文档。
- **架构图不可编辑**：截图中的架构图/流程图无法二次检索或编辑。
- **发布排版繁琐**：向 CSDN、掘金、微信公众号等平台同步笔记时，图片顺序容易错乱。

`study-notes-distiller` 通过明确的输入要素处理规则、来源追溯机制与规范化输出模板，一键完成“多模态素材输入 $\rightarrow$ 结构化技术笔记输出”。

---

## ✨ 核心特性

- **🎯 批注深度解析**：重点识别截图中的圈画、高亮框与文字批注，将其作为核心要点（Key Takeaways）完整提炼，不作遗漏。
- **📊 原生 Mermaid 支持**：自动将截图中的调用链路与架构图转换为原生 `flowchart` 或 `sequenceDiagram`，可复用可修改。
- **🖼️ 稳定截图资产归档 (`assets/`)**：将本次用到的截图完整复制到笔记旁的 `assets/` 目录（保留原文件名，重名自动加递增序号，不改动原图），正文与文末索引统一用 `图 01`、`图 02` 这类稳定编号引用，确保直接兼容 CSDN 等平台的逐张上传映射流程。
- **🧠 知识思维导图**：文末内置 `mindmap` 知识树，全景提炼核心概念、链路与避坑事项。
- **🔍 事实边界与可溯源性**：严格区分原文事实、补充理解与未确认内容，杜绝胡乱脑补。

---

## 🛠️ 安装方式

### 方式一：通过 Codex Skill 目录手动安装（推荐）

1. 在 Codex 技能目录下创建文件夹。用户级技能目录为 `$CODEX_HOME/skills`（未设置 `CODEX_HOME` 时默认 `~/.codex/skills`）：

   ```bash
   # Linux / macOS
   mkdir -p ~/.codex/skills/study-notes-distiller

   # Windows PowerShell
   New-Item -ItemType Directory -Path "$HOME\.codex\skills\study-notes-distiller" -Force
   ```

   > [!NOTE]
   > Codex 官方文档当前列出的用户级技能目录是 `$HOME/.agents/skills`（[Skills 文档](https://learn.chatgpt.com/docs/build-skills)），而 Codex 自带的 `skill-installer` / `skill-creator` 仍默认写入 `$CODEX_HOME/skills`（见 [openai/skills#420](https://github.com/openai/skills/issues/420)）。两者都是本地技能目录，若按上面的路径安装后找不到该技能，请将 `SKILL.md` 改放到 `$HOME/.agents/skills/study-notes-distiller/`。

2. 在该目录下新建 `SKILL.md`，将仓库中的 `SKILL.md` 完整内容写入。

3. 重启 Codex（或新开一个会话），然后运行 `/skills`，确认 `study-notes-distiller` 已出现在技能列表中；也可以在提示词中用 `$` 直接引用 `$study-notes-distiller` 调用它。

### 方式二：通过 Git 仓库安装

```bash
git clone https://github.com/float-com/study-notes-distiller.git ~/.codex/skills/study-notes-distiller
```

### 其他 Agent 宿主

本技能就是一个标准 `SKILL.md` 技能包，可被支持该格式的 Agent 宿主加载（Codex、DeepSeek Harness 等），区别只在于各宿主扫描的技能目录不同。

**Codex 的目录优先级**（仓库级 → 用户级 → 系统级，同时支持符号链接）：

| 作用域 | 路径 |
| --- | --- |
| REPO | `$CWD/.agents/skills`、`$CWD/../.agents/skills`、`$REPO_ROOT/.agents/skills` |
| USER | `$HOME/.agents/skills`（官方文档）／`$CODEX_HOME/skills`（`skill-installer` 默认） |
| ADMIN | `/etc/codex/skills` |
| SYSTEM | Codex 内置技能 |

**DeepSeek Harness（DSH）的目录优先级**：

| 优先级 | 来源 | 路径 |
| --- | --- | --- |
| 100 | `project-dsh` | `<项目根>/.dsh/skills` |
| 200 | `project-agents` | `<项目根>/.agents/skills` |
| 300 | `custom` | 宿主配置的 `customSkillDirs` |
| 400 | `user-dsh` | `$DSH_HOME/skills`（默认 `~/.dsh/skills`） |
| 500 | `user-agents` | `~/.agents/skills` |

以 DSH 为例，把仓库克隆或复制到上表任一用户级目录即可：

```bash
git clone https://github.com/float-com/study-notes-distiller.git ~/.agents/skills/study-notes-distiller
```

调用方式是输入 `/` 从技能候选中选择，或直接输入 `/study-notes-distiller`。注意各宿主的调用前缀不同：Codex 用 `$`（也支持 `/skills` 查看列表），DSH 用 `/`，请按宿主约定书写。

另外请勿混用两套目录：`$CODEX_HOME/skills`（`~/.codex/skills`）只对 Codex 生效，DSH 不扫描它；`~/.dsh/skills` 只对 DSH 生效，Codex 不扫描它。两边都认的只有 `~/.agents/skills`。

---

## 🚀 使用示例

### 场景一：配合视频教程与截图整理
在对话框中拖入你在视频中截取的带有红框标记的图片，并输入：

> `$study-notes-distiller` 这是我看《深入理解 MySQL 索引底层原理》P3 的截图和随手笔记（视频地址：https://... 时间戳 14:20）。请帮我整理为规范的 Markdown 学习笔记，重点提炼我在图 01 中红框圈出的聚簇索引回表逻辑。

### 场景二：官方文档/实操配置整理
> `$study-notes-distiller` 请根据当前文件夹内的截图 `config-1.png`、`config-2.png`，结合我附带的批注，整理出 Docker 网络配置的标准实操步骤，并生成 Mermaid 调用链路图。

---

## 📋 输出文档结构

生成的技术笔记默认遵循以下九部分结构（可按材料实际情况删减不适用章节，但“知识思维导图”为必备部分）：

1. **资料来源与溯源标识**（含视频时间戳/文档段落）
2. **本篇速览**（学到什么、核心结论）
3. **核心概念与定位**（定义、解决痛点）
4. **核心流程与架构图**（Mermaid 可视化）
5. **核心操作与配置步骤**（Step-by-Step 操作路径与预期验证）
6. **关键代码 / 配置清单**（带语法高亮与验证状态）
7. **核心批注与避坑指南**（`[!IMPORTANT]` 与 `[!WARNING]` 标注）
8. **截图索引**（规范相对路径与说明）
9. **知识思维导图**（Mermaid Mindmap 全景知识树）

---

## 📄 开源协议

本项目采用 MIT License 开源协议。（仓库当前尚未包含 `LICENSE` 文件，如需正式适用，请补充该文件。）