---
theme: seriph
background: https://cover.sli.dev
title: 小红书自动搜索评论工具 (MCP Server 2.0)
info: |
  ## 小红书MCP工具介绍
  从部署环境到登录使用的完整流程演示

  基于Playwright开发的小红书自动化工具
class: text-center
transition: slide-left
---

# 小红书自动搜索评论工具

## MCP Server 2.0 完整流程介绍

<div class="text-2xl mt-8">
基于 Playwright 开发的小红书自动化工具
</div>

<div class="text-lg mt-4 opacity-80">
支持搜索、内容获取、智能评论发布等功能
</div>

---
layout: center
---

# 项目概述

## 主要特点

- **深度集成AI能力**：利用MCP客户端（如Claude）的大模型能力
- **模块化设计**：笔记分析、评论生成、评论发布三个独立模块
- **强大的内容获取能力**：集成多种获取笔记内容的方法
- **持久化登录**：首次登录后无需重复登录
- **两步式评论流程**：分析笔记 → AI生成评论 → 发布

---
layout: two-cols
---

# 核心功能

::left::

## 1. 用户认证与登录
- **持久化登录**：支持手动扫码登录
- **登录状态管理**：自动检测登录状态

## 2. 内容发现与获取
- **智能关键词搜索**：支持多关键词搜索
- **多维度内容获取**：四种不同的获取方法
- **评论数据获取**：获取笔记评论信息

::right::

## 3. 内容分析与生成
- **笔记内容分析**：提取关键信息和领域
- **AI评论生成**：基于笔记内容生成自然评论
- **多类型评论支持**：
  - 引流型：引导关注或私聊
  - 点赞型：简单互动获取好感
  - 咨询型：以问题形式增加互动
  - 专业型：展示专业知识建立权威

---
layout: center
---

# 部署环境准备

## 环境要求

- **Python 环境**：Python 3.8 或更高版本
- **系统依赖**：支持 Windows、macOS、Linux
- **浏览器支持**：Playwright 支持的 Chromium

---
layout: two-cols
---

# 安装步骤

::left::

## 1. Python 环境准备
```bash
# 检查Python版本
python --version
# 或
python3 --version
```

## 2. 项目获取
```bash
# 克隆或下载项目到本地
git clone <repository-url>
cd Redbook-Search-Comment-MCP2.0-main
```

::right::

## 3. 创建虚拟环境
```bash
# 创建虚拟环境
python3 -m venv venv

# 激活虚拟环境
# Windows
venv\Scripts\activate
# macOS/Linux
source venv/bin/activate
```

---
layout: two-cols
---

# 安装依赖

::left::

## 4. 安装Python依赖
```bash
# 激活虚拟环境后安装
pip install -r requirements.txt
pip install fastmcp
```

**主要依赖包：**
- playwright>=1.40.0
- fastapi>=0.95.1
- uvicorn>=0.22.0
- pandas>=2.1.1
- mcp[cli]

::right::

## 5. 安装浏览器
```bash
# 安装Playwright浏览器
playwright install
```

**可选：Docker部署**
```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
COPY xiaohongshu_mcp.py .
RUN pip install -r requirements.txt
RUN playwright install chromium
```

---
layout: center
---

# MCP Server 配置

## 在MCP客户端中配置

### Claude for Desktop 配置示例

**Windows配置：**
```json
{
    "mcpServers": {
        "xiaohongshu MCP": {
            "command": "C:\\path\\to\\venv\\Scripts\\python.exe",
            "args": [
                "C:\\path\\to\\xiaohongshu_mcp.py",
                "--stdio"
            ]
        }
    }
}
```

**macOS配置：**
```json
{
    "mcpServers": {
        "xiaohongshu MCP": {
            "command": "/path/to/venv/bin/python3",
            "args": [
                "/path/to/xiaohongshu_mcp.py",
                "--stdio"
            ]
        }
    }
}
```

---
layout: center
---

# 启动服务器

## 两种启动方式

### 1. 直接运行
```bash
# 激活虚拟环境
source venv/bin/activate  # 或 venv\Scripts\activate

# 运行MCP服务器
python3 xiaohongshu_mcp.py
```

### 2. 通过MCP客户端启动
- 配置好MCP客户端后
- 按照客户端操作流程启动
- 服务器将在后台运行

---
layout: center
---

# 登录流程

## 首次使用登录

### 工具函数
```
mcp0_login()
```

### 在MCP客户端中的使用
```
帮我登录小红书账号
```
或
```
请登录小红书
```

### 登录步骤
1. 调用登录工具
2. 浏览器自动打开小红书页面
3. 点击登录按钮
4. 扫码登录
5. 系统自动检测登录成功
6. 保存登录状态供后续使用

---
layout: two-cols
---

# 主要功能操作

::left::

## 1. 搜索笔记
**工具函数：**
```
mcp0_search_notes(keywords="关键词", limit=5)
```

**使用示例：**
```
帮我搜索小红书笔记，关键词为：美食
```
```
帮我搜索小红书笔记，关键词为旅游，返回10条结果
```

## 2. 获取笔记内容
**工具函数：**
```
mcp0_get_note_content(url="笔记URL")
```

**使用示例：**
```
帮我获取这个笔记的内容：
https://www.xiaohongshu.com/search_result/xxxx
```

::right::

## 3. 获取笔记评论
**工具函数：**
```
mcp0_get_note_comments(url="笔记URL")
```

**使用示例：**
```
帮我获取这个笔记的评论：
https://www.xiaohongshu.com/search_result/xxxx
```

## 4. 发布智能评论
**工具函数：**
```
mcp0_post_smart_comment(url="笔记URL", comment_type="类型")
```

**使用示例：**
```
帮我为这个笔记写一条专业类型的评论：
https://www.xiaohongshu.com/explore/xxxx
```

---
layout: center
---

# 评论类型详解

## 四种评论类型

| 类型 | 描述 | 适用场景 | 示例 |
|------|------|----------|------|
| **引流型** | 引导用户关注或私聊 | 增加粉丝或私信互动 | "很棒的内容！可以加个关注吗？" |
| **点赞型** | 简单互动获取好感 | 增加曝光和互动率 | "太喜欢这个分享了！👍" |
| **咨询型** | 以问题形式增加互动 | 引发博主回复 | "请问这个方法具体怎么操作呢？" |
| **专业型** | 展示专业知识建立权威 | 建立专业形象 | "从专业角度来看，这个方法很科学..." |

---
layout: center
---

# 工作原理

## 两步式智能评论流程

### 1. 笔记分析阶段
- 调用 `post_smart_comment` 工具
- 获取笔记的标题、作者、内容等信息
- 分析笔记所属领域和关键词
- 返回结构化的笔记信息给MCP客户端

### 2. 评论生成与发布阶段
- MCP客户端（如Claude）基于分析结果生成评论
- 用户可以预览和修改生成的评论
- 调用 `post_comment` 工具发布评论
- 返回发布结果反馈

---
layout: two-cols
---

# 实际工作流程示例

::left::

## 用户请求
```
帮我为这个小红书笔记写一条专业类型的评论：
https://www.xiaohongshu.com/explore/xxxx
```

## Claude响应
```
我会帮您写一条专业类型的评论。
让我获取笔记内容并生成评论。

[调用post_smart_comment工具]
```

::right::

## 工具返回分析结果
```json
{
  "title": "上海春假旅游攻略",
  "author": "旅行达人小明",
  "content": "详细的上海旅游攻略...",
  "domain": "旅游",
  "keywords": ["上海", "春假", "旅游", "攻略"]
}
```

## Claude生成并发布评论
```
我已经获取到笔记信息，这是一篇关于上海春假旅游的笔记。
基于内容，我生成并发布了以下专业评论：

"上海春假旅游攻略很详细！作为一个经常去上海的旅行者，我特别欣赏您对交通和美食的推荐。春季的上海确实是个好去处！"

[调用post_comment工具]

评论已成功发布！
```

---
layout: center
---

# 代码结构

## 项目文件组织

- **xiaohongshu_mcp.py**：核心功能实现
  - 登录功能
  - 搜索功能
  - 内容获取功能
  - 评论发布功能
- **login_script.py**：独立的登录测试脚本
- **search_script.py**：搜索功能测试脚本
- **get_content_script.py**：内容获取测试脚本
- **test_mcp.py**：MCP服务器测试脚本
- **requirements.txt**：Python依赖列表
- **Dockerfile**：Docker容器配置
- **README.md**：详细使用文档

---
layout: center
---

# 常见问题与解决方案

## 连接失败
- ✅ 确保使用虚拟环境Python的完整绝对路径
- ✅ 确保MCP服务器正在运行
- ✅ 尝试重启MCP服务器和客户端

## 登录问题
- ✅ 检查浏览器数据目录权限
- ✅ 确保Playwright浏览器已正确安装
- ✅ 登录超时可重试

## 功能异常
- ✅ 检查网络连接
- ✅ 确认小红书页面结构未变更
- ✅ 查看控制台错误日志

---
layout: center
---

# 总结

## 项目优势

- **智能化**：集成AI能力生成自然评论
- **自动化**：从搜索到发布的全流程自动化
- **模块化**：清晰的代码结构，易于维护扩展
- **跨平台**：支持多种操作系统和部署方式
- **易集成**：标准MCP协议，易于接入各种客户端

## 适用场景

- **内容营销**：自动发布相关评论增加曝光
- **社交互动**：批量处理笔记评论，提高互动效率
- **数据收集**：自动收集小红书内容和评论数据
- **研究分析**：分析热门话题和用户偏好

---
layout: center
class: text-center
---

# 感谢使用！

## 小红书自动搜索评论工具 (MCP Server 2.0)

<div class="text-lg mt-8">
如有问题，请查看项目文档或提交Issue
</div>

<div class="text-sm mt-4 opacity-60">
基于 JonaFly/RednoteMCP 优化开发
</div>
