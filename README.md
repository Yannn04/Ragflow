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
具体的网上有部署教程https://ragflow.com.cn/docs/deploy_local_llm
```

2. 下载模型（可选）
<img width="1911" height="913" alt="image" src="https://github.com/user-attachments/assets/97e8da0d-a8d9-46a0-850f-d3121105b5c4" />

```bash
下载需要的模型
将模型文件放置在 models/ 目录下（例如 Qwen3-VL:4B, DeepSeek-R1:8B）
我的仓库有给出这两个模型，也可以自己下载
```

3. 配置环境打开ragflow
```bash
运行docker
```
<img width="1919" height="1022" alt="33db130cc80ddb8509ac3989253d7658" src="https://github.com/user-attachments/assets/b0373b03-2aea-4bc1-ac33-a641e653a29e" />

```bash
注册登录
```

<img width="1919" height="961" alt="a23c62438a6491abe720619e9186471f" src="https://github.com/user-attachments/assets/35652de2-9e17-4f74-b4d7-3907737e4850" />

---

## 模型管理
### 模型配置chat与embedding
```bash
注意这里的Url是本地ip地址：11434
还得去本机控制页面设置一下环境变量
```
<img width="1834" height="892" alt="692ec04354d88c9311706009943b24b7" src="https://github.com/user-attachments/assets/dd3a48d0-9eb4-42ee-bc17-8699f0cdb931" />

```bash
在 RAGflow 界面：进入设置 → 模型配置 → 从下拉菜单选择目标模型。
```
<img width="1848" height="953" alt="27db303f5e686af3076f906c368be2b2" src="https://github.com/user-attachments/assets/198e413e-110c-4c9c-8447-8102026bea7f" />

---

## 知识库管理
1. 创建知识库（RAGflow Web UI）
- 点击“新建知识库”，填写名称和描述，选择嵌入模型与检索策略。
<img width="1919" height="963" alt="580925cc16a9965fa13ae7173625da2b" src="https://github.com/user-attachments/assets/1b7c4cbe-76ef-435b-812b-f15b0b898740" />

2. 上传文档（支持）
- PDF、Word (.doc/.docx)、Excel、PowerPoint、纯文本 (.txt)、网页 (.html)、电子书 (.epub)
- <img width="1919" height="920" alt="6970e5f48f4d5f016a0d6aca745e7772" src="https://github.com/user-attachments/assets/c6fc1cd3-fcfd-4edb-8bbf-851352510cac" />



  


3. 文档处理（RAGflow 自动）
- 解析文档 → 文本分割 → 生成嵌入 → 建立向量索引
- <img width="1919" height="918" alt="b750475ff09740e37c8f81d83002ef41" src="https://github.com/user-attachments/assets/45542006-74f8-4c69-aa25-f48190961f88" />


---

## 使用指南
### 基础问答
- 访问 RAGflow Web UI，选择目标知识库，输入问题，系统将检索并生成答案。
- <img width="1919" height="917" alt="80ee5992529d8eeb4b5ccbb1aaa01d3f" src="https://github.com/user-attachments/assets/ac6adcd7-6223-4af5-830f-5242d650c704" />


### 多模态对话（Qwen3-VL）
- 上传图片或含图文档，输入与视觉内容相关的问题，模型将结合图像与文本回答。
- <img width="1919" height="955" alt="808ad581a1792c446cbae5ac9c8fd6c1" src="https://github.com/user-attachments/assets/4031967b-ffe6-496f-914e-b86d5eda5fc8" />


---

## API 使用示例
```python
'''
所需配置：
1.将BASE_URL替换为自己部署ragflow服务器（或者自己电脑localhost）的ip:port
2.将API_KEY的api key替换为自己在ragflow中生成的api key
3.run it
'''
import json
import requests
BASE_URL ='http://192.168.0.109/v1/'#http://localhost,/v1/api/
API_KEY = 'ragflow-I1NmRiYThjZmU0ZjExZjA5MGI0MDI0Mm'

def start_conversation():
    url = BASE_URL + 'api/new_conversation'
    headers = {"Content-Type": "application/json",
                    'Authorization': 'Bearer ' + API_KEY}
    response = requests.get(url, headers=headers)
    conversation_id = None
    msg = None
    if response.status_code == 200:
        content = response.json()
        conversation_id = content['data']['id']
        msg = content['data']['message']  # content role
        print("start_conversation:", conversation_id, msg)
    else:
        print(f"Request failed with status code: {response.status_code}")
    return response.status_code, conversation_id, msg


def get_answer(conversation_id, msg, quote=False, stream=True):
    url = BASE_URL + 'api/completion'
    params = {
        "conversation_id": conversation_id,  # 替代为你自己的对话ID
        "messages": [{"role": "user", "content": msg}],  # 替代为你的消息内容
        "quote": False,
        "stream": False,
    }
    print(params)
    headers_json = {"Content-Type": "application/json",
                    'Authorization': 'Bearer ' + API_KEY}

    try:
        response = requests.post(url=url, headers=headers_json, data=json.dumps(params))
        print(response.json())
        response.raise_for_status()  # Raises an HTTPError for bad responses (4xx and 5xx)
    except requests.exceptions.HTTPError as http_err:
        print(f"HTTP error occurred: {http_err}")
        return response.status_code, None, None
    except Exception as err:
        print(f"An error occurred: {err}")
        return None, None, None

    try:
        content = response.json()
        data = content.get('data', None)
        retcode = content.get('retcode', None)
        retmsg = content.get('retmsg', None)
        if data:
            answer = data.get('answer', None)
        else:
            answer = None
    except ValueError:
        print("Failed to parse JSON response")
        return response.status_code, None, None

    return response.status_code, answer, retmsg


def chat():
    status_code, conversation_id, msg = start_conversation()
    # conversation_id = input("Enter conversation ID: ")
    print("Chatbot initialized. Type 'exit' to end the conversation.")

    while True:
        user_message = input("You: ")
        if user_message.lower() == 'exit':
            print("Ending the conversation.")
            break
        status_code, answer, retmsg = get_answer(conversation_id, user_message)
        if status_code is not None and status_code == 200 and answer:
            print(f"Chatbot: {answer}")
        elif retmsg:
            print(f"Failed to get a response from the chatbot. Error message: {retmsg}")
        else:
            print("Failed to get a response from the chatbot.")


if __name__ == "__main__":
    chat()
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
## UI界面
1. 创建html文件

2. 将iframe嵌入网站处于所需位置
<img width="1919" height="923" alt="image" src="https://github.com/user-attachments/assets/d040ea9e-8a9d-4e0f-a786-81d1df65ed86" />

3. 运行html文件
<img width="1917" height="922" alt="823f9bc1e276c50ae22ae854c3971704" src="https://github.com/user-attachments/assets/51eb75ea-2703-4325-a684-423290cac02b" />


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
