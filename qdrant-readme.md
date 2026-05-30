# Qdrant 向量检索系统

基于 Qdrant + FastAPI + MySQL 的短视频素材向量检索系统。

## 功能

- 📦 **向量检索**：输入文案，毫秒级匹配最合适的视频素材
- 🏷️ **标签过滤**：支持按标签/分类组合筛选
- 🔄 **MySQL 双向同步**：增删改自动同步到 Qdrant
- 🐘 **PHP SDK**：一行代码集成到现有 PHP 项目

## 技术栈

| 组件 | 技术 |
|------|------|
| 向量数据库 | Qdrant |
| API 服务 | Python FastAPI |
| 关系数据库 | MySQL 8.0 |
| 向量模型 | BGE-small-zh / OpenAI |
| 部署 | Docker Compose |

## 快速开始

### 1. 部署 Qdrant + MySQL

```bash
cd deploy
docker-compose up -d
```

### 2. 启动 API

```bash
cd backend
python3 -m venv venv
source venv/bin/activate
pip install -e .
python main.py
```

### 3. 全量同步

```bash
cd backend
python app/sync_service.py
```

### 4. 测试

```bash
python test_search.py
```

## API 文档

启动后访问 http://localhost:8000/docs

### 向量检索

```json
POST /api/v1/search
{
    "query": "AI技术正在改变世界",
    "tags": "科技,教程",
    "category": "教程",
    "limit": 10
}
```

### PHP 调用

```php
require 'php-client/VectorSearchClient.php';
$client = new VectorSearchClient('http://localhost:8000');
$result = $client->search('AI技术趋势', tags: '科技', limit: 5);
print_r($result);
```

## 项目结构

```
├── backend/
│   ├── main.py              # API 入口
│   ├── app/
│   │   ├── api.py            # 路由
│   │   ├── database.py       # 数据库配置
│   │   ├── embedding.py      # 向量生成
│   │   ├── models.py         # 数据模型
│   │   ├── qdrant_client.py  # Qdrant 封装
│   │   └── sync_service.py   # 同步服务
│   └── .env.example
├── php-client/
│   └── VectorSearchClient.php
├── deploy/
│   ├── docker-compose.yml
│   ├── init.sql
│   └── deploy.sh
├── test_search.py
└── README.md
```

## 性能

| 数据量 | 检索耗时 |
|--------|---------|
| 5万条 | < 50ms |
| 50万条 | < 200ms |
| 100万条 | < 500ms (需调优) |

## 网页 Demo 前端

打开 `frontend/index.html` 即可体验向量检索功能。

支持：
- 输入文案自动匹配素材
- 标签/分类过滤
- 5 个示例搜索一键体验
- API 不可用时自动使用内置 Demo 数据
