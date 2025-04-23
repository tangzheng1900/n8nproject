为了演示如何将文档从原始文件转换为向量，并使用嵌入模型与Milvus数据库进行交互，我们将遵循以下步骤：

1. **加载文档**：从原始文件中读取文本。
2. **使用嵌入模型**：将文本转换为向量。
3. **连接Milvus数据库**：将生成的向量存储在Milvus中。

在这个示例中，我们将使用`sentence-transformers`库(用于生成嵌入向量)和`pymilvus`库(用于Milvus数据库的操作)。

### Python 代码示例

```python
import os
import numpy as np
from sentence_transformers import SentenceTransformer
from pymilvus import Collection, CollectionSchema, FieldSchema, DataType, connections

# 步骤1：从目录加载文档
def load_documents_from_directory(directory):
    documents = []
    for filename in os.listdir(directory):
        if filename.endswith('.txt'):
            with open(os.path.join(directory, filename), 'r', encoding='utf-8') as file:
                documents.append(file.read().strip())
    return documents

# 步骤2：使用嵌入模型将文档转换为向量
def documents_to_embeddings(documents):
    model = SentenceTransformer('all-MiniLM-L6-v2')  # 使用一个轻量嵌入模型
    embeddings = model.encode(documents, convert_to_numpy=True)  # 转换为numpy数组
    return embeddings

# 步骤3：连接到Milvus数据库并创建集合
def connect_to_milvus(collection_name):
    connections.connect("default", host='127.0.0.1', port='19530')  # 连接到Milvus
    # 定义集合模式
    id_field = FieldSchema(name="id", dtype=DataType.INT64, is_primary=True, auto_id=True)
    embedding_field = FieldSchema(name="embedding", dtype=DataType.FLOAT_VECTOR, dim=384)  # 设为嵌入维度
    schema = CollectionSchema(fields=[id_field, embedding_field], description="Document Embeddings")
    
    # 创建集合
    collection = Collection(name=collection_name, schema=schema)
    return collection

# 步骤4：将嵌入存储到Milvus中
def insert_embeddings_into_milvus(collection, embeddings):
    ids = [i for i in range(len(embeddings))]  # 生成ID
    collection.insert([ids, embeddings.tolist()])  # 插入数据
    print(f'插入了 {len(embeddings)} 个向量到 Milvus.')

# 示例用法
if __name__ == '__main__':
    directory = 'path/to/documents'  # 替换为您的文档目录
    documents = load_documents_from_directory(directory)
    embeddings = documents_to_embeddings(documents)
    
    collection_name = 'document_embeddings'
    collection = connect_to_milvus(collection_name)
    insert_embeddings_into_milvus(collection, embeddings)
```

### 代码说明

1. **加载文档**：
   - `load_documents_from_directory`函数从指定目录读取所有`.txt`文件，并将文本收集在列表中。

2. **生成嵌入向量**：
   - `documents_to_embeddings`函数使用`sentence-transformers`库中的模型将文本转换为向量。

3. **连接至Milvus**：
   - `connect_to_milvus`函数连接到Milvus数据库，并（如果尚未存在）创建一个新的集合来存储向量。

4. **插入向量**：
   - `insert_embeddings_into_milvus`函数将生成的嵌入向量插入到Milvus集合中。

### 依赖安装

请确保您安装了以下依赖：

```bash
pip install sentence-transformers pymilvus
```

### 注意事项

- 请确保在运行代码之前将`'path/to/documents'`替换为实际文档所在的目录路径。
- 确保Milvus数据库正在运行，并且与代码中的连接设置一致。