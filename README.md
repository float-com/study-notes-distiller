# Study Notes Distiller (学习笔记提炼专家)

> 🚀 一个专为 Codex 设计的任务专用技能（Skill），将多模态碎片化学习材料（带圈画/文字批注截图、视频时间戳、技术文章链接、口语化随记）自动提炼并结构化为生产级中文技术 Markdown 笔记。

---

## 💡 为什么需要它？

在日常技术学习过程中，我们常常面临以下痛点：
- **截图易散落**：课程视频或官方文档中截了大量关键步骤，但过后找不到对应上下文。
- **批注难沉淀**：截图中用红圈、箭头勾画的关键参数或手写随记无法转化为规范文档。
- **架构图不可编辑**：截图中的架构图/流程图无法二次检索或编辑。
- **发布排版繁琐**：向 CSDN、掘金、微信公众号等平台同步笔记时，图片顺序容易错乱。

`study-notes-distiller` 通过严格的输入要素提取规则、来源追溯机制与规范化输出模板，一键完成“多模态素材输入 $\rightarrow$ 结构化技术笔记输出”。

---

## ✨ 核心特性

- **🎯 批注深度解析**：重点识别截图中的圈画、高亮框与文字批注，将其作为核心要点（Key Takeaways）精准提炼。
- **📊 原生 Mermaid 支持**：自动将截图中的调用链路与架构图转换为原生 `flowchart` 或 `sequenceDiagram`，可复用可修改。
- **🖼️ 稳定截图资产归档 (`assets/`)**：自动归档截图并按 `图 01`、`图 02` 进行稳定编号索引，确保直接兼容 CSDN 等平台的图片上传映射流程。
- **🧠 知识思维导图**：文末内置 `mindmap` 知识树，全景提炼核心概念、链路与避坑事项。
- **🔍 事实边界与可溯源性**：严格区分原文事实、补充理解与未确认内容，杜绝胡乱脑补。

---

## 🛠️ 安装方式

### 方式一：通过 Codex Skill 目录手动安装（推荐）

1. 在本地 Codex 技能配置目录下创建文件夹：
   ```bash
   # 全局安装路径（Linux / macOS）
   mkdir -p ~/.codex/skills/study-notes-distiller

   # 或 Windows PowerShell:
   # New-Item -ItemType Directory -Path "$HOME\.codex\skills\study-notes-distiller" -Force
   ```

2. 在该目录下新建 `SKILL.md`，将仓库中的 `SKILL.md` 完整内容写入。

3. 在 Codex 对话框中输入 `/skills`，确认 `study-notes-distiller` 已加载并启用。

### 方式二：通过 Git 仓库安装

```bash
git clone https://github.com/float-com/study-notes-distiller.git ~/.codex/skills/study-notes-distiller
```

---

## 🚀 使用示例

### 场景一：配合视频教程与截图整理
在对话框中拖入你在视频中截取的带有红框标记的图片，并输入：

> `$study-notes-distiller` 这是我看《深入理解 MySQL 索引底层原理》P3 的截图和随手笔记（视频地址：https://... 时间戳 14:20）。请帮我整理为规范的 Markdown 学习笔记，重点提炼我在图 01 中红框圈出的聚簇索引回表逻辑。

### 场景二：官方文档/实操配置整理
> `$study-notes-distiller` 请根据当前文件夹内的截图 `config-1.png`、`config-2.png`，结合我附带的批注，整理出 Docker 网络配置的标准实操步骤，并生成 Mermaid 调用链路图。

---

## 📋 输出文档结构

生成的技术笔记默认遵循清晰的七段式结构：

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

本项目采用 [MIT License](LICENSE) 开源协议。