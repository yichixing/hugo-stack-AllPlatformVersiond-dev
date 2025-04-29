+++
author = "taojl"
title = "用VLLM本地linux系统部署Qwen3"
collections = [
"直接用Vllm命令就能在linux系统下将模型加载并启动成一个模型的API服务了",
"openWebUI通过vllm暴露的API接口的URL连接模型的API服务",
"",
""
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

series = ["AI模型部署","AI模型部署工具"]
#aliases = ["migrate-from-jekyl"]
#image = "pawel-czerwinski-8uZPynIu-rQ-unsplash.jpg"

+++
༼ 企业级最强开源大模型Qwen3震撼发布！本地部署+全面客观测评！Qwen3-235B-A22B+Qwen3-32B+Qwen3-14B谁是最强王者？ollama+LM Studio+vLLM本地部署I༽
# Part 1: 准备vLLM部署环境

(01:57-02:06) ***1.1: 介绍vLLM及部署平台***
*   **作者提到的关键信息**：如果需要进行**企业项目**部署，可以使用**vLLM**。作者将在**Ubuntu系统**上演示使用**vLLM**进行部署。

(02:06-02:10) ***1.2: 展示部署环境***
*   **关键操作**：作者执行`nvidia-smi`命令查看服务器的显卡信息。
*   **目的**：确认部署环境的硬件配置。
*   **画面内容**：显示服务器使用的是**NVIDIA RTX A6000**显卡，显存为**48G**，CUDA版本为12.7。

(02:10-02:19) ***1.3: 安装Miniconda***
*   **目的**：安装**Miniconda**用于管理Python环境和依赖包。
*   **关键操作**：作者展示并执行了一系列命令来下载、安装**Miniconda**，并配置相关的环境变量。
*   **画面内容**:
    ```bash
    # 安装Miniconda (如果尚未安装)
    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh
    bash ~/miniconda.sh -b -p $HOME/miniconda
    eval "$($HOME/miniconda/bin/conda shell.bash hook)"
    echo 'export PATH="$HOME/miniconda/bin:$PATH"' >> ~/.bashrc
    source ~/.bashrc
    ```
*   **结果**：**Miniconda**环境安装并配置完成。

(02:19-02:29) ***1.4: 创建并激活Conda环境***
*   **目的**：创建一个独立的Python环境（`qwen3_env`）用于**vLLM**部署，指定**Python版本为3.10**。
*   **关键操作**：执行`conda create`命令创建环境，然后执行`conda activate`命令激活该环境。
*   **画面内容**:
    ```bash
    # 创建conda环境
    conda create -n qwen3_env python=3.10 -y
    conda activate qwen3_env
    ```
*   **结果**：成功创建并进入了名为`qwen3_env`的**Conda**环境。

(02:29-02:35) ***1.5: 安装vLLM及依赖***
*   **目的**：安装**vLLM**库及其运行所需的依赖库（**ray**, **transformers**, **accelerate**）。
*   **关键操作**：在已激活的**Conda**环境（`qwen3_env`）中，使用`pip install`命令安装**vLLM**及相关依赖。
*   **画面内容**:
    ```bash
    pip install vllm ray transformers accelerate
    ```
*   **结果**：**vLLM**及其依赖库安装完成。

# Part 2: 使用vLLM启动Qwen3 14B模型服务

(02:35-02:46) ***2.1: 启动vLLM服务***
*   **目的**：使用**vLLM**将**Qwen3 14B**模型加载并启动为一个API服务。
*   **关键操作**：执行`vllm serve`命令，指定模型为**Qwen/Qwen3-14B**，并添加了参数`--enable-reasoning`和`--reasoning-parser deepseek_r1`。
*   **画面内容**:
    ```bash
    vllm serve Qwen/Qwen3-14B --enable-reasoning --reasoning-parser deepseek_r1
    ```
*   **结果**：**vLLM**服务成功启动，API服务器监听在`http://0.0.0.0:8000`。

# Part 3: 通过Open WebUI调用vLLM模型

(02:46-03:38) ***3.1: 安装、启动并配置Open WebUI连接vLLM***
*   **目的**：配置一个Web UI（**Open WebUI**）来方便地与通过**vLLM**部署的**Qwen3 14B**模型进行交互。
*   **关键操作**:
    1.  在本地电脑终端安装**Open WebUI**：`pip install open-webui`。
    2.  启动**Open WebUI**服务：`open-webui serve`。
    3.  在浏览器访问`http://localhost:8080`。
    4.  进入**Open WebUI**设置 -> 管理员设置 -> 外部连接。
    5.  在**OpenAI API连接**设置中，将**API请求地址**设置为**vLLM**服务器的**IP地址+端口号+/v1**（例如：`http://192.168.1.105:8000/v1`）。
    6.  **API Key**处随意输入几个字母。
    7.  点击保存。
*   **结果**：**Open WebUI**配置完成，可以连接到**vLLM**服务。

(03:38-03:52) ***3.2: 在Open WebUI中选择并使用模型***
*   **目的**：通过配置好的**Open WebUI**界面与**Qwen3 14B**模型进行对话测试。
*   **关键操作**：在**Open WebUI**中，点击**新建对话**，然后在模型选择列表中选择刚配置好的**Qwen: Qwen3 14B**模型。
*   **结果**：用户现在可以通过**Open WebUI**与本地通过**vLLM**部署的**Qwen3 14B**模型进行交互。

# Part 4: 补充信息

(15:39-15:43) ***4.1: 获取代码和指令***
*   **作者建议**：视频中使用的代码和指令会放在视频下方的描述栏或者评论区。

(15:43-15:49) ***4.2: 通过博客获取笔记***
*   **作者建议**：如果在视频下方无法找到代码和指令，可以访问作者的博客（**www.aivi.fyi**）查找本期视频对应的笔记。