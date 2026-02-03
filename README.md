# Ragflow
本地知识库问答系统 - Docker + Ollama + RAGflow
## 项目概述
这是一个完全本地化的知识库问答系统，基于 Docker 容器化技术，集成 Ollama 模型服务和 RAGflow 检索增强生成框架。系统支持多模态模型对话，提供安全、私密的本地知识管理和智能问答功能。
<img width="1919" height="915" alt="a5561c66801a3feafec81a62ac1dfe97" src="https://github.com/user-attachments/assets/42d70ebc-e5e3-49b1-9787-3c723aff6532" />

---

## 核心特性
- 🚀 **完全本地部署**：所有数据与模型处理均在本地完成，保障数据隐私。
- 🔧 **容器化部署**：使用 Docker / Docker Compose 简化环境配置与管理。
- 🧠 **多模型支持**：Qwen3-VL:4B（视觉语言多模态）、DeepSeek-R1:8B（纯文本）。
- 📚 **智能检索（RAG）**：基于 RAGflow 实现高效知识检索与上下文增强。

---

## 系统架构
```
用户界面 (Web UI)
       ↓
    RAGflow
       ↓
     Ollama
       ↓
本地知识库 + 向量数据库
```

---

## 环境要求
### 硬件
- 内存：最低 16GB，推荐 32GB 以上
- 存储：至少 50GB 可用空间（用于模型和知识库）
- GPU：可选，支持 CUDA 的 GPU 可加速推理

### 软件
- 操作系统：Linux (Ubuntu 20.04+)、macOS 或 Windows (WSL2)
- Docker: >= 20.10
- Docker Compose: >= 2.0
- Git：用于版本管理

---

## 快速开始
1. 下载docker、ollama
```bash
具体的网上都有部署教程
```

2. 下载模型（可选）
<img width="1911" height="913" alt="image" src="https://github.com/user-attachments/assets/97e8da0d-a8d9-46a0-850f-d3121105b5c4" />

```bash
#下载需要的模型
# 将模型文件放置在 models/ 目录下（例如 Qwen3-VL:4B, DeepSeek-R1:8B）
```

3. 配置环境
```bash
cp .env.example .env
```
编辑 `.env`：
```env
# Ollama 配置
OLLAMA_HOST=0.0.0.0
OLLAMA_NUM_PARALLEL=2
OLLAMA_MODELS_PATH=/root/.ollama/models

# RAGflow 配置
RAGFLOW_PORT=9380
RAGFLOW_WORKERS=4
RAGFLOW_API_KEY=your_api_key_here

# 模型配置
MODEL_QWEN=qwen2.5-vl:4b
MODEL_DEEPSEEK=deepseek-r1:8b
EMBEDDING_MODEL=all-minilm

# 系统资源限制
MEMORY_LIMIT=16g
CPU_LIMIT=8
```

4. 启动服务
```bash
docker-compose up -d
```

5. 验证服务状态
```bash
# 查看容器与日志
docker-compose ps
docker-compose logs -f ollama
docker-compose logs -f ragflow
```

6. 访问服务
- RAGflow Web UI: http://localhost:9380
- Ollama API: http://localhost:11434
- API 文档: http://localhost:9380/api/docs

---

## 模型管理
### 拉取模型
```bash
# 通过 Ollama 拉取模型
docker exec -it ollama ollama pull qwen2.5-vl:4b
docker exec -it ollama ollama pull deepseek-r1:8b
# 查看已安装模型
docker exec -it ollama ollama list
```

### 模型切换
在 RAGflow 界面：进入设置 → 模型配置 → 从下拉菜单选择目标模型。
<img width="1848" height="953" alt="27db303f5e686af3076f906c368be2b2" src="https://github.com/user-attachments/assets/198e413e-110c-4c9c-8447-8102026bea7f" />

---

## 知识库管理
1. 创建知识库（RAGflow Web UI）
- 点击“新建知识库”，填写名称和描述，选择嵌入模型与检索策略。
<img width="1919" height="963" alt="580925cc16a9965fa13ae7173625da2b" src="https://github.com/user-attachments/assets/1b7c4cbe-76ef-435b-812b-f15b0b898740" />

2. 上传文档（支持）
- PDF、Word (.doc/.docx)、Excel、PowerPoint、纯文本 (.txt)、网页 (.html)、电子书 (.epub)


3. 文档处理（RAGflow 自动）
- 解析文档 → 文本分割 → 生成嵌入 → 建立向量索引

---
## UI界面
1. 创建html文件

2. 将iframe嵌入网站处于所需位置
<img width="1919" height="923" alt="image" src="https://github.com/user-attachments/assets/d040ea9e-8a9d-4e0f-a786-81d1df65ed86" />

3. 运行html文件
<img width="1917" height="922" alt="823f9bc1e276c50ae22ae854c3971704" src="https://github.com/user-attachments/assets/51eb75ea-2703-4325-a684-423290cac02b" />


---
## 使用指南
### 基础问答
- 访问 RAGflow Web UI，选择目标知识库，输入问题，系统将检索并生成答案。

### 多模态对话（Qwen3-VL）
- 上传图片或含图文档，输入与视觉内容相关的问题，模型将结合图像与文本回答。

---

## API 使用示例
```python
import requests

API_KEY = "your_api_key_here"

def ask_question(question, knowledge_base_id):
    url = "http://localhost:9380/api/v1/chat/completions"
    headers = {"Authorization": f"Bearer {API_KEY}", "Content-Type": "application/json"}
    data = {
        "model": "qwen2.5-vl:4b",
        "messages": [{"role": "user", "content": question}],
        "knowledge_base_id": knowledge_base_id,
        "stream": False
    }
    response = requests.post(url, json=data, headers=headers)
    return response.json()

# 使用示例
answer = ask_question("什么是机器学习？", "kb_123")
print(answer["choices"][0]["message"]["content"])
```

---

## 配置说明（Docker Compose 示例）
```yaml
services:
  ollama:
    image: ollama/ollama:latest
    container_name: ollama
    ports:
      - "11434:11434"
    volumes:
      - ollama_data:/root/.ollama
      - ./models:/models
    environment:
      - OLLAMA_HOST=0.0.0.0
      - OLLAMA_NUM_PARALLEL=2
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: all
              capabilities: [gpu]

  ragflow:
    image: infiniflow/ragflow:latest
    container_name: ragflow
    ports:
      - "9380:9380"
    volumes:
      - ragflow_data:/app/ragflow/resource
      - ./knowledge_base:/app/ragflow/knowledge_base
    environment:
      - RAGFLOW_PORT=9380
      - RAGFLOW_WORKERS=4
    depends_on:
      - ollama
```

---

## 性能调优
- 内存优化：调整 Docker 内存限制、减少并发处理数量、使用量化模型版本
- 存储优化：使用 SSD 加速向量检索，定期清理临时文件，压缩历史对话

---

## 故障排除
### 常见问题
1. 容器启动失败
```bash
# 查看日志
docker-compose logs
# 重启
docker-compose down
docker-compose up -d
```
2. 模型加载缓慢
- 检查网络连接、确认模型文件完整性、调整 Ollama 并行数设置

3. 内存不足
```bash
docker stats
# 调整 docker-compose.yml 中资源限制
```
4. API 连接问题
```bash
curl http://localhost:11434/api/tags
curl http://localhost:9380/health
```

---

## 日志查看
```bash
# 查看所有服务日志
docker-compose logs -f

# 查看特定服务日志
docker-compose logs -f ollama
docker-compose logs -f ragflow

# 容器内日志
docker exec -it ollama journalctl -u ollama
```

---

### 数据安全
- ✅ 所有数据存储在本地
- ✅ 无外部网络传输敏感信息
- ✅ 支持数据加密存储
- ✅ 可配置访问权限控制


🔒 默认仅本地访问

🔒 可配置 HTTPS 加密

🔒 API 密钥认证

🔒 请求频率限制
