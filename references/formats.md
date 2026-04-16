# 数据格式说明

## 目录结构

```
.doc-memory/
├── index.json              # 项目索引（未来扩展）
└── projects/
    └── {project-id}/
        ├── meta.json       # 项目元数据
        ├── history.json    # 版本历史
        └── versions/
            ├── v1.md
            ├── v2.md
            └── ...
```

---

## meta.json - 项目元数据

```json
{
  "project_name": "项目需求分析",
  "project_id": "项目需求分析",
  "created_at": "2026-04-16T10:30:00.123456",
  "current_version": 5,
  "description": "项目描述（可选）"
}
```

**字段说明:**
- `project_name`: 项目显示名称
- `project_id`: 项目ID（用于目录名）
- `created_at`: ISO 8601 格式的创建时间
- `current_version`: 当前最新版本号
- `description`: 项目描述（可选）

---

## history.json - 版本历史

```json
[
  {
    "version": 1,
    "version_tag": "v1",
    "timestamp": "2026-04-16T10:30:00.123456",
    "change_description": "初始版本",
    "author": "AI Assistant",
    "metadata": {}
  },
  {
    "version": 2,
    "version_tag": "v2",
    "timestamp": "2026-04-16T11:20:00.123456",
    "change_description": "添加用户管理模块",
    "author": "张三",
    "metadata": {
      "source": "用户修改",
      "reviewed": false
    }
  }
]
```

**字段说明:**
- `version`: 版本号（整数，递增）
- `version_tag`: 版本标签（v + 版本号）
- `timestamp`: ISO 8601 格式的保存时间
- `change_description`: 变更描述
- `author`: 作者名称
- `metadata`: 自定义元数据（可选）

---

## 版本文件 (v1.md, v2.md, ...)

纯文本文件，保存该版本的完整文档内容。

---

## compare_versions() 返回格式

```json
{
  "summary": {
    "added_lines": 15,
    "removed_lines": 5,
    "total_changes": 20,
    "version1": "v1",
    "version2": "v2"
  },
  "diff_text": "--- v1\n+++ v2\n@@ -1,5 +1,8 @@\n # 标题\n\n+新增内容\n 原有内容\n-删除内容\n 修改内容\n",
  "section_changes": [
    {
      "type": "added",
      "section": "新章节",
      "content": "新章节内容..."
    },
    {
      "type": "modified",
      "section": "旧章节",
      "old": "旧内容...",
      "new": "新内容..."
    },
    {
      "type": "removed",
      "section": "已删除章节"
    }
  ],
  "content1": "v1的完整内容...",
  "content2": "v2的完整内容..."
}
```
