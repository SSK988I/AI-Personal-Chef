# AI 私人厨师

基于 FastAPI + LangGraph 的智能厨师助手，支持多模态（图片+文本）输入，为用户推荐个性化食谱。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python](https://img.shields.io/badge/Python-3.13+-blue.svg)](https://www.python.org/downloads/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.128+-green.svg)](https://fastapi.tiangolo.com/)

## 功能特性

- 🍳 **多模态输入**：支持食材图片上传和文字描述
- 🤖 **智能推荐**：基于 AI 分析食材，推荐合适的食谱
- 📝 **流式对话**：实时流式响应，提升用户体验
- 💾 **会话管理**：支持多会话和历史记录
- ☁️ **云端存储**：图片存储于阿里云 OSS

## 技术栈

### 后端
- FastAPI - Web 框架
- LangGraph - Agent 编排
- 阿里百炼 (qwen3-omni-flash) - 多模态 AI 模型
- Tavily - 搜索引擎
- SQLite - 会话存储

### 前端
项目提供了简单的 HTML 静态文件，可直接访问 API

## 快速开始

### 1. 环境准备

```bash
# 克隆项目
git clone git@github.com:SSK988I/AI-Personal-Chef.git
cd 私厨

# 安装 uv (如果还没安装)
pip install uv

# 安装依赖
uv sync
```

### 2. 配置环境变量

复制 `.env.example` 到 `.env`（如果还没有，手动创建）：

```bash
cp .env.example .env
```

编辑 `.env` 文件，填入你的 API 密钥：

```env
# 阿里百炼
DASHSCOPE_API_KEY=your_api_key
DASHSCOPE_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1

# Tavily 搜索
TAVILY_API_KEY=your_tavily_key

# 阿里 OSS
OSS_ACCESS_KEY_ID=your_access_key
OSS_ACCESS_KEY_SECRET=your_secret
OSS_BUCKET=your_bucket_name
OSS_REGION=cn-beijing

# LangSmith (可选)
LANGSMITH_API_KEY=your_langsmith_key
LANGSMITH_TRACING=true
LANGSMITH_PROJECT=personal-chef

# DeepSeek (可选)
DEEPSEEK_API_KEY=your_deepseek_key
```

### 3. 启动后端

```bash
uv run python -m app.main
```

后端将在 `http://127.0.0.1:8001` 启动

### 4. 访问服务

后端启动后，你可以通过以下方式访问：
- API 文档：http://127.0.0.1:8001/docs
- 静态页面：http://127.0.0.1:8001 （访问 index.html）
- 直接测试 API：
  ```bash
  # 测试对话接口
  curl -X POST "http://127.0.0.1:8001/api/v1/chat/stream" \
    -H "Content-Type: application/json" \
    -d '{"message": "帮我推荐一些菜谱", "thread_id": "test"}'
  ```

## API 文档

启动后端后，访问：
- Swagger UI: `http://127.0.0.1:8001/docs`
- ReDoc: `http://127.0.0.1:8001/redoc`

### 主要接口

| 接口 | 方法 | 描述 |
|------|------|------|
| `/api/v1/chat/stream` | POST | 流式对话 |
| `/api/v1/chat/messages` | GET | 获取历史消息 |
| `/api/v1/chat/messages` | DELETE | 清空历史消息 |
| `/api/v1/oss/presign` | GET | 获取 OSS 预签名 URL |

## 项目结构

```
私厨/
├── app/                 # 后端代码
│   ├── api/v1/         # API 接口
│   ├── agents/         # Agent 逻辑
│   ├── models/         # 数据模型
│   ├── common/         # 公共工具
│   ├── db/             # 数据库（本地）
│   └── main.py         # 入口文件
├── 私厨-前端源码/       # 前端源码（未使用，参考静态文件）
├── app/static/          # 静态文件（HTML + CSS + JS）
├── .env.example        # 环境变量模板
├── .gitignore          # Git 忽略规则
├── pyproject.toml      # Python 依赖
└── README.md           # 项目说明
```

**注意**：
- 前端代码需要单独配置和启动
- `.env` 文件包含敏感信息，请勿提交到 Git
- 数据库文件会在首次运行时自动创建

## 开发指南

### 添加新的 Agent 工具

在 `app/agents/personal_chief.py` 中添加工具：

```python
from langchain_core.tools import tool

@tool
def my_custom_tool(input: str) -> str:
    """工具描述"""
    return "结果"
```

然后添加到 `agent = create_agent(tools=[...])`

## 常见问题

### Q: 如何处理 API 密钥安全？
A: 
1. 将 `.env.example` 复制为 `.env` 并填入你的密钥
2. 确保 `.env` 文件在 `.gitignore` 中
3. 不要将 `.env` 文件提交到 Git

### Q: 如何自定义 Agent 工具？
A: 参考 [开发指南](#开发指南) 部分添加新的工具

### Q: 数据存储在哪里？
A: SQLite 数据库文件默认创建在 `app/db/personal_chief.db`

## 许可证

MIT License - 详见 [LICENSE](LICENSE) 文件

## 贡献

欢迎提交 Issue 和 Pull Request！

## 联系方式

如有问题，请通过 GitHub Issues 联系。
