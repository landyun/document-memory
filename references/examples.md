# 使用示例

## 示例1: 基本的需求文档管理

```python
from scripts.document_memory import DocumentMemory

# 初始化项目
doc = DocumentMemory("电商平台需求分析")

# 第一版需求
v1_content = """
# 电商平台需求文档

## 1. 项目概述
搭建一个电商平台，支持用户购物和商家管理商品。

## 2. 用户功能
- 用户注册登录
- 浏览商品
- 购物车
- 下单支付
"""

doc.save_version(
    content=v1_content,
    change_description="初始版本，包含基本框架",
    author="产品经理"
)

# 几天后，更新需求
v2_content = """
# 电商平台需求文档

## 1. 项目概述
搭建一个电商平台，支持用户购物和商家管理商品。

## 2. 用户功能
- 用户注册登录（支持手机号和邮箱）
- 浏览商品（支持分类筛选和搜索）
- 购物车
- 下单支付（支持支付宝、微信）

## 3. 商家功能
- 商品管理
- 订单处理
- 数据统计
"""

doc.save_version(
    content=v2_content,
    change_description="添加商家功能模块，完善用户功能",
    author="产品经理"
)

# 对比两个版本
diff = doc.compare_versions("v1", "v2")
print(f"新增 {diff['summary']['added_lines']} 行")
```

---

## 示例2: 交互式迭代

模拟与用户反复修改需求的场景：

```python
from scripts.document_memory import DocumentMemory

doc = DocumentMemory("项目需求")

# 初始需求
content = "# 需求文档"
doc.save_version(content, "初始版本")

# 第一次修改
content = """# 需求文档
## 用户管理
- 用户注册
- 用户登录
"""
doc.save_version(content, "添加用户管理模块", "AI")

# 第二次修改
content = """# 需求文档
## 用户管理
- 用户注册（支持手机/邮箱）
- 用户登录
- 密码重置
## 商品管理
- 商品列表
- 商品详情
"""
doc.save_version(content, "完善用户管理，添加商品管理", "用户")

# 查看历史
history = doc.get_version_history()
for v in history:
    print(f"{v['version_tag']} - {v['change_description']}")

# 对比 v1 和 v3
report = doc.generate_change_report("v1", "v3")
print(report)
```

---

## 示例3: 命令行使用

```bash
# 初始化项目
python scripts/document_memory.py init --name "我的需求"

# 从文件保存版本
python scripts/document_memory.py save --name "我的需求" --file req.md --message "第一版"

# 列出版本
python scripts/document_memory.py list --name "我的需求"

# 对比版本
python scripts/document_memory.py diff --name "我的需求" v1 v2

# 导出历史
python scripts/document_memory.py export --name "我的需求" --output history.md
```

---

## 示例4: 集成到AI工作流

```python
def handle_requirements_update(user_input, current_content, doc_memory):
    """
    处理用户需求更新的AI工作流
    """
    # 1. 分析用户输入
    changes = analyze_user_changes(user_input)

    # 2. 修改文档
    new_content = apply_changes(current_content, changes)

    # 3. 保存新版本
    version_info = doc_memory.save_version(
        content=new_content,
        change_description=user_input[:100],  # 用用户输入前100字作为描述
        author="AI Assistant"
    )

    # 4. 生成变更报告
    if version_info["version"] > 1:
        prev_version = f"v{version_info['version'] - 1}"
        curr_version = version_info["version_tag"]
        report = doc_memory.generate_change_report(prev_version, curr_version)
        return new_content, report

    return new_content, None
```

---

## 示例5: 版本恢复

```python
# 获取历史版本并恢复
doc = DocumentMemory("项目需求")

# 假设当前是 v5，但想回到 v3
v3_content = doc.get_version(version_tag="v3")

# 将 v3 保存为新版本 v6
doc.save_version(
    content=v3_content,
    change_description="恢复到 v3 的内容",
    author="用户"
)
```
