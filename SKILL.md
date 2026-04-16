---
name: document-memory
description: 文档记忆与版本管理skill，支持需求分析等文档的多版本保存、变更记录、版本对比和差异可视化。支持Markdown、纯文本和Word文档(.docx)格式。适用于：(1)需要保存文档迭代历史的场景，(2)多次修改调整需求文档，(3)需要对比不同版本之间的差异，(4)跟踪文档变更过程，(5)导入/导出Word格式文档。
---

# Document Memory Skill - 文档记忆与版本管理

本skill提供文档版本管理功能，支持保存多个版本、记录变更历史、对比版本差异。支持Markdown、纯文本和Word文档(.docx)格式。

## 核心功能

1. **版本保存** - 自动保存文档的每个版本，记录时间戳和变更说明
2. **变更追踪** - 记录每次修改的原因、内容和作者信息
3. **差异对比** - 智能分析两个版本之间的差异，支持可视化展示
4. **历史回溯** - 查看文档的完整演变过程，随时恢复到任意版本
5. **Word格式支持** - 支持从Word导入和导出为Word文档

## 目录结构

文档记忆系统使用以下目录结构：

```
.doc-memory/
├── projects/              # 项目目录
│   └── {project-id}/     # 具体项目
│       ├── meta.json      # 项目元数据
│       ├── versions/      # 版本文件
│       │   ├── v1.md
│       │   ├── v2.md
│       │   └── ...
│       └── history.json   # 版本历史记录
└── index.json            # 项目索引
```

## 工作流程

### 1. 初始化项目

首次使用时，创建文档记忆项目：

```python
from scripts.document_memory import DocumentMemory

# 初始化项目
doc_memory = DocumentMemory(project_name="项目需求分析", project_path=".")
```

### 2. 保存新版本

每次完成文档修改后，保存新版本：

```python
# 保存新版本
version_info = doc_memory.save_version(
    content=document_content,
    change_description="添加了用户管理模块的详细需求",
    author="AI Assistant"
)
```

### 3. 查看版本历史

获取文档的所有版本记录：

```python
# 获取版本历史
history = doc_memory.get_version_history()
for version in history:
    print(f"v{version['version']}: {version['change_description']} ({version['timestamp']})")
```

### 4. 对比版本差异

对比任意两个版本之间的差异：

```python
# 对比两个版本
diff = doc_memory.compare_versions(version1="v1", version2="v2")
print(diff['summary'])  # 差异摘要
print(diff['details'])  # 详细差异
```

### 5. 获取特定版本

```python
# 获取特定版本的内容
content = doc_memory.get_version(version="v1")
```

### 6. Word文档支持

首先需要安装依赖：
```bash
pip install python-docx
```

从Word文档导入：
```python
# 从.docx文件导入并保存为新版本
version_info = doc_memory.import_from_word(
    docx_path="requirements.docx",
    change_description="从Word文档导入需求",
    author="产品经理"
)
```

导出为Word文档：
```python
# 导出特定版本为.docx
doc_memory.export_to_word(
    output_path="output.docx",
    version_tag="v2"  # 可选，默认最新版本
)
```

导出版本差异为Word：
```python
# 导出差异对比报告为Word
doc_memory.export_diff_to_word(
    output_path="diff_report.docx",
    version1="v1",
    version2="v2"
)
```

## 脚本使用

### 命令行接口

skill提供了便捷的命令行工具：

```bash
# 初始化项目
python scripts/document_memory.py init --name "需求分析"

# 从文本/Markdown保存版本
python scripts/document_memory.py save --name "需求分析" --file requirements.md --message "初始版本"

# 从Word文档导入
python scripts/document_memory.py import-word --name "需求分析" --file requirements.docx --message "从Word导入"

# 列出版本
python scripts/document_memory.py list --name "需求分析"

# 对比版本（文本格式）
python scripts/document_memory.py diff --name "需求分析" v1 v2

# 导出为Word文档
python scripts/document_memory.py export-word --name "需求分析" --version v2 --output output.docx

# 导出版本差异为Word
python scripts/document_memory.py diff-word --name "需求分析" v1 v2 --output diff.docx

# 导出版本历史
python scripts/document_memory.py export --name "需求分析" --output history.md
```

## 差异分析格式

版本对比返回结构化的差异数据：

```json
{
  "summary": {
    "added_lines": 15,
    "removed_lines": 5,
    "modified_lines": 3,
    "total_changes": 23
  },
  "changes": [
    {
      "type": "added",
      "section": "用户管理",
      "content": "添加了用户角色权限管理..."
    },
    {
      "type": "modified",
      "section": "系统架构",
      "old": "单体架构",
      "new": "微服务架构"
    }
  ],
  "html_report": "<html>...</html>"
}
```

## 参考文档

- 详细的API文档请参考 [references/api.md](references/api.md)
- 数据格式说明请参考 [references/formats.md](references/formats.md)
- 使用示例请参考 [references/examples.md](references/examples.md)

## 最佳实践

1. **频繁保存** - 每次完成有意义的修改后立即保存版本
2. **清晰描述** - 使用具体的变更描述，避免"更新了一些内容"这样的模糊描述
3. **定期对比** - 在重大变更前后对比版本差异，确保变更符合预期
4. **结构化内容** - 使用Markdown标题分层组织文档，便于差异分析定位到具体章节

## 触发场景

当用户提及以下内容时，立即使用此skill：

- "保存这个版本"
- "看看改了什么"
- "对比一下两个版本"
- "我想看看历史记录"
- "恢复到之前的版本"
- "需求又变了，帮我记一下"
- "这个文档修改了好几次"
