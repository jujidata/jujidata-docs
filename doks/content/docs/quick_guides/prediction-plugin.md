---
title: "快速上手：标注插件开发"
linkTitle: "标注插件开发"
weight: 50
date: 2026-06-17T09:06:20Z

params:
  seo:
    title: "标注插件开发：数据标注加速引擎"
    description: "学习如何开发预标注插件，通过本地代码和模型加速数据标注流程"
    canonical: ""
    robots: "index,follow"
---

# 标注平台预标注插件开发说明

[据吉桌面版](https://jujidata.com/downloadApp)支持接入本地的[预标注插件](/docs/concepts/pre-annotation/)，用户可通过预标注插件调用本地代码或模型对标注任务进行自动化预标注。

## 插件项目结构

一个典型的使用 [uv](https://docs.astral.sh/uv/) 管理的 Python 预标注插件通常包含以下文件：

```
.
├── config.py
├── infer.bat
├── infer.py
├── infer.sh
├── main.py
├── output_formatter.py
└── pyproject.toml
```

### 通信协议

标注平台与插件通过标准输入输出进行数据交互：

- **输入**：单个任务的数据以 JSON 字符串形式通过 `stdin` 传入
- **输出**：标注结果必须为符合标注格式要求的 JSON 字符串，通过 `stdout` 输出

## 代码样例

### config.py

该文件用于配置日志系统和读取环境变量。日志文件存储在 `./logs/` 目录，方便调试和故障排查。

以下是一个调用 LLM API 的插件配置示例：

```python
# config.py
from pathlib import Path
from dataclasses import dataclass
import os, logging

def setup_logging():
    """初始化日志系统，将日志输出到文件"""
    log_dir = Path(__file__).parent / "logs"
    log_file = log_dir / "app.log"
    os.makedirs(log_dir, exist_ok=True)
    
    logging.basicConfig(
        format="%(asctime)s - %(name)s - %(levelname)s - %(message)s",
        datefmt="%Y-%m-%d %H:%M:%S",
        level=logging.INFO,
        handlers=[
            logging.FileHandler(
                log_file.resolve(), 
                encoding="utf-8"
            )
        ]
    )

@dataclass
class LLMConfig:
    """LLM 配置数据类"""
    access_key: str
    endpoint: str
    model: str

    @classmethod
    def from_env(cls) -> "LLMConfig":
        """
        从环境变量解析 LLM 配置
        
        期望的环境变量：
        - LLM_ACCESS_KEY: LLM 服务的 API 密钥
        - LLM_ENDPOINT: API 端点 URL
        - LLM_MODEL: 使用的模型名称
        
        返回值：
            LLMConfig: 配置对象
            
        抛出异常：
            ValueError: 任何必需的环境变量缺失
        """
        access_key = os.getenv('LLM_ACCESS_KEY')
        endpoint = os.getenv('LLM_ENDPOINT')
        model = os.getenv('LLM_MODEL')
        
        if not access_key:
            raise ValueError("LLM_ACCESS_KEY 环境变量必须设置")
        if not endpoint:
            raise ValueError("LLM_ENDPOINT 环境变量必须设置")
        if not model:
            raise ValueError("LLM_MODEL 环境变量必须设置")
        
        return cls(access_key=access_key, endpoint=endpoint, model=model)
```

### infer.bat

Windows 环境下的插件入口。在 Windows 版本应用中添加插件时，选择此文件作为入口点。

示例如下（调用 LLM API 的插件）：

```powershell
@echo off
setlocal enabledelayedexpansion

REM 获取脚本目录
set SCRIPT_DIR=%~dp0

REM 检查必需的环境变量
if not defined LLM_ACCESS_KEY (
	echo ERROR: LLM_ACCESS_KEY 环境变量未设置
	exit /b 1
)
if not defined LLM_ENDPOINT (
	echo ERROR: LLM_ENDPOINT 环境变量未设置
	exit /b 1
)
if not defined LLM_MODEL (
	echo ERROR: LLM_MODEL 环境变量未设置
	exit /b 1
)

if not exist "!SCRIPT_DIR!logs" (
    mkdir "!SCRIPT_DIR!logs"
)

uv run --python "!SCRIPT_DIR!.venv\Scripts\python.exe" "!SCRIPT_DIR!infer.py" 2>>"!SCRIPT_DIR!logs\error.log"
	set PY_EXIT=!ERRORLEVEL!
	exit /b !PY_EXIT!
```

> **注意**：环境变量 `LLM_ACCESS_KEY`、`LLM_ENDPOINT`、`LLM_MODEL` 应根据实际配置调整，也可以删除环境变量检查。

### infer.sh

macOS 和 Linux 环境下的插件入口。在 macOS 或 Linux 版本应用中添加插件时，选择此文件作为入口点。

示例如下（调用 LLM API 的插件）：

```bash
#!/bin/bash
# 获取脚本所在目录
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

# 切换到脚本目录
cd "$SCRIPT_DIR"

# 运行 infer.py，传递所有参数
uv run python infer.py "$@"
```

### infer.py

该文件负责从标注平台接收 JSON 字符串输入，调用 `main.py` 的 `process()` 函数进行处理，并输出结果。

```python
import logging
import sys
import json
import os
import re

# 配置 stdout/stderr 编码为 UTF-8
if hasattr(sys.stdout, "reconfigure"):
    sys.stdout.reconfigure(encoding="utf-8", errors="replace")
    sys.stderr.reconfigure(encoding="utf-8", errors="replace")

# 将项目路径添加到 Python 路径
project_path = os.path.dirname(os.path.abspath(__file__))
sys.path.append(project_path)

from config import setup_logging

setup_logging()
logger = logging.getLogger(__name__)

from main import process


def fix_bad_json_backslashes(s: str) -> str:
    """
    修复 JSON 字符串中无效的反斜杠转义
    
    处理无效的 JSON 转义序列如 \p, \m, \T 等，转换为 \\p, \\m, \\T
    """
    # 移除 invalid JSON escapes
    s = s.encode("utf-8", errors="replace").decode("utf-8")
    s = re.sub(r'\\(?!["\\/bfnrtu]|u[0-9a-fA-F]{4})', r'\\\\', s)

    # 修复 \u 后面没有跟 4 个十六进制数字的情况
    s = re.sub(
        r'\\u(?![0-9a-fA-F]{4})',
        r'\\u'.replace('\\', '\\\\'),
        s
    )

    # 修复其他无效的转义
    s = re.sub(
        r'\\(?!["\\/bfnrtu])',
        r'\\\\',
        s
    )
    return s

if __name__ == "__main__":
    logger.info("开始执行 infer.py...")
    
    # 获取输入 JSON 数据（三种来源优先级）
    if len(sys.argv) > 1:
        logger.info("从命令行参数读取 JSON 数据")
        json_param = sys.argv[1]
    elif 'SCRIPT_PARAM_0' in os.environ and os.environ['SCRIPT_PARAM_0']:
        logger.info("从环境变量 'SCRIPT_PARAM_0' 读取 JSON 数据")
        json_param = os.environ['SCRIPT_PARAM_0']
    else:
        logger.info("从 STDIN 读取 JSON 数据")
        raw = sys.stdin.buffer.read()
        json_param = raw.decode("utf-8", errors="replace") 
    
    if not json_param:
        raise ValueError("输入 JSON 数据不能为空")
    
    logger.info(f"收到输入数据（最后 1000 字符）: {json_param[-1000:]}...")

    try:
        json_param = fix_bad_json_backslashes(json_param)
        input_data = json.loads(json_param)
    except json.JSONDecodeError as e:
        raise ValueError(f"输入 JSON 格式错误: {e}")
    
    # 通过 main 函数处理输入数据
    result = process(input_data)
    
    # 输出结果为 JSON
    print(json.dumps(result, ensure_ascii=False))
    
    logger.info("infer.py 执行完成！")
```

**说明**：

- 输入 JSON 数据有三个获取来源，优先级为：命令行参数 → 环境变量 → 标准输入。v1.1.5 版本后，默认使用标准输入方式。
- `fix_bad_json_backslashes()` 函数用于处理文本数据中的无效反斜杠。**建议在上传数据前先清理这类问题数据**。

### main.py

该文件为插件的主体逻辑，包含推理处理代码（如调用大模型或本地模型：YOLO、Kokoro 等）。

```python
# main.py

import logging
from config import LLMConfig
from output_formatter import format_inference_output

logger = logging.getLogger(__name__)

def process(data: dict) -> list:
  """
  推理处理主函数

  参数：
      data (dict): 标注任务的输入数据，可从插件调试界面获取输入样例。
      
  返回值：
      list: 推理结果，包含标注对象的列表
  """
  # 第 1 步：从环境变量解析 LLM 配置
  config = LLMConfig.from_env()
  logger.info(f"LLMConfig 已加载: {config}")
  
  # 第 2 步：执行推理
  model_results = ... # TODO: 实现具体的推理逻辑
  
  # 第 3 步：转换模型输出到标注结果格式
  result = format_inference_output(data, model_results)
  
  # 返回结果
  return result
```

### output_formatter.py

该文件负责将模型推理结果转换为标注平台要求的输出格式（对象数组）。

```python
# output_formatter.py

import logging

logger = logging.getLogger(__name__)

def format_inference_output(data: dict, model_results: dict) -> list:
  """
  将模型推理结果转换为标注格式
  
  参数：
      data (dict): 输入的标注任务数据
      model_results (dict): 模型的推理结果
      
  返回值：
      list: 符合标注格式要求的结果对象列表
  """
  pass
```

## 标注结果格式说明

### 格式要求

1. **标注界面的 XML 配置**决定了输出结果的格式。不同的标注类型（Tag）有不同的输出要求。
2. **所有标注对象**必须包含 `id` 和 `type` 字段，其中 `id` 可以随机生成。
3. 以下是常见标注类型的输出格式示例（后续将继续补充）。
4. **嵌套的标注**需要使用相同的 `id`。例如：用户在文本中选定内容（`Labels/Label`）然后回答问题（`Textarea`），对应的两个标注对象需使用相同的 `id`。
5. 推理结果对某个Tag为空时，建议不输出该object。

> **提示**：完成手动标注任务后，观察学习标注结果的实际格式，与 `format_inference_output()` 的输出进行对比和调整。

### 常见标注类型

#### Labels / Label

用于文本标签标注：

```json
{
    "id": "随机生成的唯一标识符",
    "type": "labels",
    "value": {
        "text": "被标注的文本内容", 
        "labels": ["标签值"],
        "start": 173, 
        "end": 251
    },
    "origin": "prediction",
    "to_name": "text",
    "from_name": "Labels 组件的名称"
}
```

**字段说明**：
- `text`: 用户选定的文本内容
- `labels`: 标签值数组
- `start`: 选定文本在原文本中的起始索引（0-based）
- `end`: 选定文本在原文本中的结束索引（不包含）

#### Textarea 

用于自由文本输入标注：

```json
{
    "id": "随机生成的唯一标识符",
    "type": "textarea",
    "value": {"text": ["生成或输入的文本内容"]},
    "origin": "prediction",
    "to_name": "text",
    "from_name": "Textarea 组件的名称"
}
```

#### Choices / Choice

用于单选或多选标注：

```json
{
    "id": "随机生成的唯一标识符",
    "type": "choices",
    "value": {"choices": ["选择的选项名称"]},
    "origin": "prediction",
    "to_name": "text",
    "from_name": "Choices 组件的名称"
}
```

#### PolygonLabels / Label

用于图像多边形标注（如描边、区域分割）：

```json
{
  "id": "随机生成的唯一标识符",
  "type": "polygonlabels",
  "value": {
    "closed": true,
    "points": [
      [73.29921332636744, 74.30249632892804],
      [80.30757455554766, 76.3582966226138],
      ...
    ],
    "polygonlabels": ["标签值"]
  },
  "origin": "prediction",
  "to_name": "Image 组件的名称",
  "from_name": "PolygonLabels 组件的名称",
  "image_rotation": 0,
  "original_width": 2732,
  "original_height": 1534
}
```

**字段说明**：
- `closed`: 是否为封闭多边形
- `points`: 多边形顶点坐标数组（相对坐标，范围 0-100）
- `polygonlabels`: 标签值数组
- `original_width`/`original_height`: 原始图片的宽度和高度（像素）
```


