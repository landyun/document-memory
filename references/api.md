# API 参考文档

## DocumentMemory 类

主要的文档记忆管理类。

### 构造函数

```python
DocumentMemory(project_name: str, project_path: str = ".")
```

**参数:**
- `project_name`: 项目名称（必需）
- `project_path`: 项目根路径，默认为当前目录

**示例:**
```python
from scripts.document_memory import DocumentMemory

# 创建新的文档记忆项目
doc = DocumentMemory("我的需求文档")
```

---

### save_version()

保存新版本。

```python
save_version(
    content: str,
    change_description: str = "",
    author: str = "AI Assistant",
    metadata: Optional[Dict] = None
) -> Dict
```

**参数:**
- `content`: 文档内容字符串
- `change_description`: 变更描述（推荐填写）
- `author`: 作者名称
- `metadata`: 附加元数据字典

**返回:**
版本信息字典，包含：
- `version`: 版本号（整数）
- `version_tag`: 版本标签（如 "v1"）
- `timestamp`: ISO格式时间戳
- `change_description`: 变更描述
- `author`: 作者
- `metadata`: 附加元数据

**示例:**
```python
version_info = doc.save_version(
    content="# 需求文档\n\n这是第一版...",
    change_description="初始版本，包含项目概述",
    author="张三",
    metadata={"source": "用户输入", "type": "requirements"}
)
print(f"已保存为 {version_info['version_tag']}")
```

---

### get_version()

获取特定版本的内容。

```python
get_version(
    version: Optional[int] = None,
    version_tag: Optional[str] = None
) -> Optional[str]
```

**参数:**
- `version`: 版本号（整数）
- `version_tag`: 版本标签（如 "v1"）

**返回:**
文档内容字符串，如果版本不存在返回 None。

**示例:**
```python
# 通过版本标签获取
content = doc.get_version(version_tag="v3")

# 通过版本号获取
content = doc.get_version(version=3)

# 获取最新版本
content = doc.get_version()
```

---

### get_version_history()

获取完整的版本历史。

```python
get_version_history() -> List[Dict]
```

**返回:**
版本信息列表，按时间顺序排列。

**示例:**
```python
history = doc.get_version_history()
for v in history:
    print(f"v{v['version']}: {v['change_description']}")
```

---

### get_version_info()

获取特定版本的元数据信息。

```python
get_version_info(
    version: Optional[int] = None,
    version_tag: Optional[str] = None
) -> Optional[Dict]
```

**参数:**
- `version`: 版本号
- `version_tag`: 版本标签

**返回:**
版本信息字典，不包含文档内容。

---

### compare_versions()

对比两个版本的差异。

```python
compare_versions(
    version1: str,
    version2: str,
    context_lines: int = 3
) -> Dict[str, Any]
```

**参数:**
- `version1`: 第一个版本标签（如 "v1"）
- `version2`: 第二个版本标签（如 "v2"）
- `context_lines`: 差异上下文行数，默认 3 行

**返回:**
差异分析字典，包含：
- `summary`: 变更摘要统计
- `diff_text`: unified diff 格式文本
- `section_changes`: 章节级变更分析
- `content1`: 旧版本完整内容
- `content2`: 新版本完整内容

**示例:**
```python
diff = doc.compare_versions("v1", "v3")
print(f"新增 {diff['summary']['added_lines']} 行")
print(f"删除 {diff['summary']['removed_lines']} 行")
```

---

### generate_change_report()

生成人类可读的变更报告。

```python
generate_change_report(version1: str, version2: str) -> str
```

**参数:**
- `version1`: 旧版本标签
- `version2`: 新版本标签

**返回:**
Markdown 格式的变更报告字符串。

**示例:**
```python
report = doc.generate_change_report("v2", "v5")
print(report)

# 保存到文件
with open("change_report.md", "w", encoding="utf-8") as f:
    f.write(report)
```

---

### export_history()

导出完整的版本历史报告。

```python
export_history(output_path: Optional[str] = None) -> str
```

**参数:**
- `output_path`: 输出文件路径（可选）

**返回:**
Markdown 格式的历史报告字符串。

**示例:**
```python
# 仅返回内容
history = doc.export_history()

# 保存到文件
doc.export_history("version_history.md")
```
