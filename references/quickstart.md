# 快速入门指南

## 5分钟快速上手

### 前置要求

如果你需要使用Word文档功能，先安装依赖：
```bash
pip install python-docx
```

### 第1步：导入并初始化

```python
from scripts.document_memory import DocumentMemory

# 创建一个文档记忆项目
doc = DocumentMemory("我的需求文档")
```

### 第2步：保存第一版

#### 方式A：从文本/Markdown保存
```python
# 你的文档内容
content = """
# 项目需求

## 功能列表
1. 用户登录
2. 数据查询
3. 报表导出
"""

# 保存版本
doc.save_version(
    content=content,
    change_description="初始版本，包含基本功能",
    author="我"
)
```

#### 方式B：从Word文档导入
```python
# 直接从.docx文件导入
doc.import_from_word(
    docx_path="requirements.docx",
    change_description="从Word文档导入",
    author="我"
)
```

### 第3步：修改后保存新版本

```python
# 修改后的内容
content = """
# 项目需求

## 功能列表
1. 用户登录（支持手机/邮箱）
2. 数据查询（支持筛选和搜索）
3. 报表导出（Excel/PDF）
4. 数据可视化
"""

# 保存新版本
doc.save_version(
    content=content,
    change_description="完善功能描述，添加数据可视化",
    author="我"
)
```

### 第4步：查看历史记录

```python
history = doc.get_version_history()
for v in history:
    print(f"{v['version_tag']}: {v['change_description']}")
```

### 第5步：对比版本差异

#### 文本格式报告
```python
# 生成变更报告
report = doc.generate_change_report("v1", "v2")
print(report)
```

#### 导出为Word文档
```python
# 导出特定版本为Word
doc.export_to_word("v2_output.docx", version_tag="v2")

# 导出差异对比为Word
doc.export_diff_to_word("diff_report.docx", "v1", "v2")
```

就这么简单！你的文档现在有了完整的版本历史记录。
