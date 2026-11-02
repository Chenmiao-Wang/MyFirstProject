
---
## SOP｜将 Zotero 指定文件夹中的全部文献标题按格式批量导入 Obsidian

> **用途**：把 Zotero 某个分类（Collection）中的文献标题，批量整理到同一篇 Obsidian 笔记；每篇标题以普通正文显示，标题之间自动插入 Markdown 水平分隔线。 
> **适用环境**：Windows、Zotero、Excel、Notepad++、Obsidian。  
> **方法**：Zotero导出CSV → Excel复制原始 Title 列 → Notepad++正则替换 → Obsidian。  
> **版本说明**：不同软件版本的菜单文字可能不同。

### 1. 目标效果

将原始标题：
```text
Paper A: Analysis of "genetic risk"
Paper B: Single-cell transcriptomics
Paper C: Multi-omics integration
```

转换为下面的 Markdown：
```markdown
Paper A: Analysis of "genetic risk"

---
Paper B: Single-cell transcriptomics

---
Paper C: Multi-omics integration
```

### 2. 前提条件

- Zotero中已经创建目标 Collection，并存有需要导出的文献条目。
- Excel能打开导出的 CSV。
- Windows已安装 Notepad++ 和 Obsidian。
- 本 SOP 只处理标题文本，不创建独立文献笔记、不导入 PDF、不保持 Zotero 自动同步。

### 3. 操作步骤

#### Step 1｜从 Zotero 导出目标分类

1. 在 Zotero 左侧找到目标 Collection（文件夹）。
2. 右键选择”导出...”。
3. 设置.csv格式以及UTF-8。
4. 如需包含子分类文献，先核对导出选项或分别导出子分类，以免漏掉条目。
![[Pasted image 20261102201611.png|161]]

#### Step 2｜从 Excel 复制原始标题

1. 用 Excel 打开 CSV。
2. 找到列名Title的原始列，确认每个单元格只有一篇标题。
3. 仅选中文献标题数据（不要选表头）。
4. `Ctrl + C` 复制。

> **踩过的坑**：不要使用 `TEXTJOIN` 合并整个标题范围；它会把许多标题放进一个单元格，不符合本SOP 的一行一篇标题！

#### Step 3｜在 Notepad++ 检查文本

1. 打开 Notepad++，新建空白文件。
2. `Ctrl + V` 粘贴。
3. 核对内容是**一行一篇文献标题**，例如：
```text
Paper A
Paper B
Paper C
```

 >**踩过的坑**：若直接在excel中进行格式转换，复制到obsidian中时，每行都会自带引号，这是excel自带的bug，无法解决。但若直接全局删除 `"`，则会破坏标题里本来就有的引号，故换用NotePad++进行查找替换。

#### Step 4｜批量插入 Markdown 分隔线

在 Notepad++ 按 `Ctrl + H` 打开“替换”：

- 查找目标：`\R`
- 替换为：`\r\n\r\n---\r\n`
- 查找模式：正则表达式
- 点击全部替换
![[Pasted image 20261102202439.png|497]]
这条正则把后面紧跟非空白文本的换行替换成：换行 + 空行 + `---` + 换行。这样不会给第一篇标题前面加空行，也不会自动在最后一篇标题后添加分隔线。

**替换前：**
```text
Paper A
Paper B
Paper C
```

**替换后：**
```markdown
Paper A

---
Paper B

---
Paper C
```

> 若更希望分隔线上下均留空行，可将“替换为”改为 `\r\n\r\n---\r\n\r\n`。

#### Step 5｜复制到 Obsidian

1. 在 Notepad++ 中 `Ctrl + A` 全选、`Ctrl + C` 复制。
2. 在 Obsidian 中打开或新建一篇文献目录笔记。
3. `Ctrl + Shift + V` 粘贴为纯文本。
4. 也可在替换完成后，直接在 Notepad++ 中选择 “文件 → 另存为” ，将文件保存Obsidian仓库中，其中文件名填写：XXXX.md，编码选择 UTF-8。
5. 切换到实时预览或阅读视图，确认 `---` 渲染为水平线、标题显示为普通正文。

---
**经验总结**：先复制原始单行标题，再用 Notepad++ 在“行与行之间”插入 Markdown 分隔符，比在 Excel 的单元格里嵌入换行符更容易避免多余双引号和重复拼接。
