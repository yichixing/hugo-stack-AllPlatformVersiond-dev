+++
author = "taojl"
title = "用LMDeploy本地linux系统部署InternVL3"
collections = [
"用LMDeploy在本地linux系统部署InternVL3",
"LMDeploy部署的模型会启动模型API服务并且会开放API接口",
"LMDeploy部署后得到的API接口的URL地址可直接给openWebUI使用",
"三种模型部署工具的异同：都能开启模型API服务，但加载模型的方式不同，性能方面也不同"
]
date = "2025-01-28"
#description = "Sample article showcasing basic Markdown syntax and formatting for HTML elements."

categories = [
    "模型部署",
    "API服务",
"openWebUI"
]
tags = [
    "LMDeploy在linux系统部署模型",
    "",
    "",
    "",
]

series = ["AI模型部署","AI模型部署工具","API服务"]
#aliases = ["migrate-from-jekyl"]
#image = "pawel-czerwinski-8uZPynIu-rQ-unsplash.jpg"

+++
༼ 🚀挑战Gemini 2.5！最强开源企业级OCR大模型InternVL3！本地部署教程+实战测评全纪录，轻松搞定潦草手写汉字、模糊PDF扫描件、模糊复杂表格，效果炸裂超过人眼！支持Open WebUI༽
# part1: 环境准备与依赖安装

(01:30-01:53)
***1.1: 运行环境说明***
**作者提到的关键信息**: 演示将在**Ubuntu系统**上进行，使用**RTX A6000**显卡。
**作者建议**: Windows系统用户需要在Windows上**开启WSL (Windows Subsystem for Linux)** 才能进行后续操作。作者提供了微软官方关于如何安装WSL的文档链接。
**目的**: 明确演示所使用的硬件和操作系统环境，并为Windows用户提供前置条件说明。

(01:53-02:04)
***1.2: 安装Miniconda***
**关键操作**: 通过命令行执行一系列指令 (`wget`, `bash`, `eval`, `echo`, `source`) 下载并安装**Miniconda**。
**画面内容**:
```bash
# 安装Miniconda (如果尚未安装)
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh
bash ~/miniconda.sh -b -p $HOME/miniconda
eval "$($HOME/miniconda/bin/conda shell.bash hook)"
echo 'export PATH="$HOME/miniconda/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```
**目的**: **安装Conda环境管理器**，用于后续创建和管理Python虚拟环境。
**结果**: **Miniconda**成功安装并配置好环境变量。

(02:04-02:14)
***1.3: 创建并激活Conda虚拟环境***
**关键操作**: 使用`conda create`指令创建一个名为`lmdeploy`的**Python 3.11**虚拟环境，并通过`conda activate`指令激活该环境。
**画面内容**: `conda create -n lmdeploy python=3.11 -y && conda activate lmdeploy`
**目的**: 为模型部署创建一个**隔离的Python环境**，避免依赖冲突。
**结果**: 名为`lmdeploy`的Conda环境被成功创建并激活，终端提示符前缀变为`(lmdeploy)`。

(02:14-02:24)
***1.4: 安装LMDeploy***
**作者提到的关键信息**: 本次部署将使用**LMDeploy**工具包。
**关键操作**: 在已激活的`lmdeploy`环境中使用`pip install`指令安装**LMDeploy**。
**画面内容**: 命令行执行 `pip install lmdeploy` (演示脚本中为 `pip install lmdeploy>=0.7.3`)。
**目的**: 安装核心的模型部署工具**LMDeploy**。
**结果**: **LMDeploy**及其相关依赖包被成功下载并安装到`lmdeploy`环境中。

(02:25-02:30)
***1.5: 安装额外依赖包***
**关键操作**: 使用`pip install`指令安装`partial_json_parser`和`timm`这两个库。
**画面内容**:
```bash
pip install partial_json_parser
pip install timm
```
**目的**: 安装运行**InternVL3**模型或**LMDeploy**可能需要的**额外Python库**。
**结果**: `partial_json_parser`和`timm`被成功安装。

# part2: 使用LMDeploy部署模型API服务

(02:30-03:05)
***2.1: ♐启动模型API服务器***
**关键操作**: 执行`lmdeploy serve api_server`指令来启动模型服务。
**画面内容**:
```bash
lmdeploy serve api_server OpenGVLab/InternVL3-14B-Instruct --backend turbomind --server-port 23333 --tp 2 --chat-template internvl2_5
```
**作者提到的关键信息**:
*   指令指定了要部署的模型为 `OpenGVLab/InternVL3-14B-Instruct`。
*   使用了`turbomind`作为推理后端 (`--backend turbomind`)。
*   指定服务器端口为`23333` (`--server-port 23333`)。
*   设置**张量并行 (Tensor Parallelism)** 为2 (`--tp 2`)，表示使用2块GPU进行推理。
*   指定了聊天模板为`internvl2_5` (`--chat-template internvl2_5`)。
**作者建议**: 如果只有一块GPU，应将`--tp`参数设置为`1`。
**目的**: 将**InternVL3-14B-Instruct**模型加载到GPU，并通过**LMDeploy**启动一个**兼容OpenAI API**的HTTP服务，以便后续应用调用。
**结果**: **LMDeploy**开始下载模型权重文件（`.safetensors`），下载完成后加载模型并启动**Uvicorn Web服务器**。服务器成功运行在`http://0.0.0.0:23333`，并提示可以在浏览器中打开该地址查看API详情（Swagger UI）。

# part3: 安装并配置OpenWebUI

(03:05-03:32)
***3.1: 安装并启动OpenWebUI***
**作者提到的关键信息**: 使用**OpenWebUI**作为与模型API交互的前端界面。
**关键操作**: 使用`pip install open-webui`指令安装**OpenWebUI**。
**画面内容**: `pip install open-webui`
**目的**: 安装Web用户界面。
**结果**: **OpenWebUI**安装完成。
**关键操作**: 使用`open-webui serve`指令启动**OpenWebUI**服务。
**画面内容**: `open-webui serve`
**目的**: 运行Web界面服务器。
**结果**: **OpenWebUI**服务启动，可以通过浏览器访问`localhost:8080`。

(03:32-04:06)
***3.2: 配置OpenWebUI连接模型API***
**关键操作**: 在浏览器中打开`localhost:8080`访问**OpenWebUI**。
**关键操作**: 点击左下角头像 -> 设置 -> 管理员设置 -> 外部连接。
**关键操作**: 确认“**OpenAI API**”开关已打开。
**关键操作**: 在“管理OpenAI API连接”的输入框中，填入由**LMDeploy**启动的API服务器地址。
**作者提到的关键信息**: API地址应为`http://[部署服务器的IP地址]:23333/v1`。视频中示例为`http://192.168.1.105:23333/v1`。
**关键操作**: 点击右下角的“保存”按钮。
**关键操作**: 返回主界面，点击左上角“新建对话”。
**关键操作**: 在模型选择下拉菜单中，选择刚刚部署的模型 `OpenGVLab/InternVL3-14B-Instruct`。
**目的**: 将**OpenWebUI**前端指向本地运行的**InternVL3模型API**，使其能够调用该模型进行交互。
**结果**: **OpenWebUI**成功连接到本地部署的模型API，可以在聊天界面中选择并使用`OpenGVLab/InternVL3-14B-Instruct`模型。



1.  **FastAPI:**
    *   **定位:** FastAPI 是一个**通用的、高性能的Python Web框架**，用于构建API（应用程序接口）。
    *   **作用:** 它的核心功能是**创建Web服务和API端点**。在模型部署的场景中，你可以使用FastAPI来编写代码，定义模型的输入/输出接口，接收用户请求，调用模型进行推理，然后将结果返回给用户。♈它本身**不包含**模型加载、优化或推理执行的功能，♐你需要自己集成这些逻辑（例如使用`transformers`库加载模型，用PyTorch/TensorFlow进行推理）。
    *   **类比:** 它更像是建造房屋的**“脚手架”或“工具箱”**，提供了构建API的基础结构，但具体的模型推理逻辑需要你自己实现。

2.  **Ollama:**
    *   **定位:** Ollama 是一个**简化在本地运行大型语言模型（LLM）的工具**。
    *   **作用:** 它**打包了**运行多种开源LLM所需的环境、模型权重下载和管理，并**内置了一个API服务器**（通常兼容OpenAI API格式）。用户可以通过简单的命令 (`ollama run model_name`) 快速启动并与模型交互，或者通过其提供的API进行调用。它的**重点在于易用性和本地快速体验**。
    *   **类比:** 它更像是一个**“开箱即用的LLM运行器”**，帮你处理了很多底层的配置和模型管理，让你能快速用上各种模型。

3.  **LMDeploy:**
    *   **定位:** LMDeploy 是一个专注于**大型语言模型（LLM）的高效部署工具包**，由InternLM团队开发。
    *   **作用:** 它提供了**模型转换、推理引擎（如TurboMind）优化、量化**以及**API服务部署**等一系列功能。它的**核心优势在于推理性能优化**，可以显著提升LLM的推理速度并降低显存占用。它也提供了一个**开箱即用的API服务器**（如视频中展示的`lmdeploy serve api_server`），并且这个服务器也**兼容OpenAI API格式**。
    *   **类比:** 它更像是一个**“高性能LLM部署与服务引擎”**，不仅帮你部署模型并提供API，还着重于优化模型的运行效率。

**总结与比较：**

*   **相似之处:** LMDeploy 和 Ollama 都提供了**相对完整的、用于运行LLM并提供API服务**的功能，都致力于简化模型部署的过程，并且都提供了兼容OpenAI的API接口，方便与其他应用集成。从这个角度看，LMDeploy和Ollama更像是同类工具，都是**LLM Serving（服务）框架/工具**。
*   **不同之处:**
    *   **LMDeploy vs Ollama:** LMDeploy 更侧重于**推理性能优化**（特别是对于InternLM系列模型，利用TurboMind等引擎），提供更细致的部署配置选项（如张量并行）；而Ollama更侧重于**跨模型兼容性和本地运行的便捷性**。
    *   **LMDeploy/Ollama vs FastAPI:** FastAPI 是一个**更底层的Web框架**，用于构建各种API，包括模型API。而LMDeploy和Ollama是**更高层、更专注于LLM部署的解决方案**，它们内部可能使用了类似FastAPI/Uvicorn这样的Web框架来构建其API服务部分，但它们还封装了模型加载、优化、推理和管理等更多功能。

**简单来说：**

*   ♈如果你想**快速在本地跑各种开源LLM**，并且不太关心极致的性能优化，**Ollama** 是个好选择。
*   ♈如果你想**高效地部署特定（尤其是InternLM系列）的LLM**，追求**推理速度和低资源占用**，并需要一个兼容OpenAI的API服务，**LMDeploy** 是一个强大的工具。
*   如果你想**从头开始构建一个定制化的模型API服务**，需要完全控制API的逻辑和行为，或者部署非LLM模型，那么你会**使用FastAPI**这样的Web框架作为基础。

因此，LMDeploy 和 Ollama 可以被认为是同类（LLM Serving工具），而 FastAPI 是构建这类工具（或其他Web API）的基础框架。