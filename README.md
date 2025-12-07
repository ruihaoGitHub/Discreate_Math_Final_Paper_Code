# 深圳地铁智能助手 (Shenzhen Metro AI Assistant)

*一个基于知识图谱与大语言模型的离散数学课程期末项目*

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Framework](https://img.shields.io/badge/Framework-NetworkX-orange)](https://networkx.org/)

本项目以“深圳地铁路线查询”为应用场景，实现了一个经典的**图增强检索（Graph-RAG）** 架构，解决了大模型在处理精确、结构化任务时可能“一本正经地胡说八道”的问题。

## 核心架构：图增强检索 (Graph-RAG)

本项目的核心思想是“分工合作”：**让专业的工具做专业的事**。我们不让大模型直接回答路线问题，而是将其作为一个“翻译官”和“组织者”。

系统的完整工作流程如下：

1.  **用户输入**：用户用自然语言提问（例如：“我想从粤海门去红树湾南”）。
2.  **意图理解 (LLM)**：大语言模型（通过魔搭社区API调用）负责“听懂人话”。它将用户的自然语言解析为结构化的指令，即提取出起点和终点 `{'start': '粤海门', 'end': '红树湾南'}`。
3.  **图谱检索 (Knowledge Graph)**：系统使用 `NetworkX` 库在预先构建好的深圳地铁知识图谱上，运行**确定性的最短路径算法**（如 Dijkstra 或 BFS），找到 100% 准确的路线。
4.  **答案生成 (LLM)**：将图算法计算出的精确路线列表，再次提交给大语言模型，让它将这些结构化的数据“翻译”成一段通顺、礼貌、人性化的导航回复。

通过这种方式，我们既利用了 LLM 的灵活性，又保证了核心答案的绝对准确性。

## 主要功能

-   **自然语言理解**：能听懂日常的问路方式。
-   **精确路径规划**：基于图算法提供最短换乘或最少站点的路线。
-   **人性化回复**：生成的导航文本清晰、友好，包含线路和方向信息。
-   **高可靠性**：杜绝了模型在关键路径信息上产生幻觉的可能。

## 快速开始

请按照以下步骤在你的本地环境中运行本项目。

### 1. 环境配置

首先，克隆本仓库到你的本地：
```bash
git clone https://github.com/ruihaoGitHub/Discreate_math_final_paper_code.git
cd Discreate_math_final_paper_code
```

然后，安装所需的 Python 依赖库：
```bash
pip install networkx openai
```
*(建议创建一个独立的 Python 虚拟环境)*

### 2. 配置 API 密钥

> **⚠️ 重要提示：配置 API 密钥**
>
> 本项目需要调用**魔搭社区（ModelScope）** 提供的大模型 API 服务来驱动语言理解和生成模块。该服务对个人开发者**免费**，但需要你进行注册并获取 `Access Token` (即 API Key)。

请按照以下步骤获取并配置你的密钥：

1.  访问 [ModelScope 个人访问令牌页面](https://modelscope.cn/my/myaccesstoken)。
2.  使用你的淘宝/阿里云账号登录或注册一个新账号。
3.  **【关键】** 根据页面提示，**确保你的魔搭账号已经成功绑定了阿里云账号**（通常需要进行实名认证），否则 API 调用会失败。
4.  复制页面上生成的 `Access Token`。
5.  打开项目中的 `discreate_math.py` 文件，找到以下代码行：

    ```python
    # 请在这里填入你的魔搭 SDK Token
    api_key='你的_MODELSCOPE_API_KEY_在这里', 
    ```
6.  将 `'你的_MODELSCOPE_API_KEY_在这里'` 替换为你刚刚复制的 `Access Token`。

### 3. 运行程序

一切准备就绪后，在你的终端中运行主程序：

```bash
python discreate_math.py
```

## 使用示例

程序启动后，你就可以开始向它提问了：

```console
(venv) D:\> python discreate_math.py
正在构建地铁网络...
--- 深圳地铁智能助手 ---

请问您从哪去哪儿 (输入 q 退出): 从粤海门去红树湾南怎么走？
用户提问: 从粤海门去红树湾南怎么走？

>>>> 回答:
您好！从粤海门到红树湾南，推荐您按以下路线乘坐：

1.  在 **粤海门站** 乘坐 **9号线** (往文锦方向)。
2.  经过2站，在 **红树湾南站** 下车即可。

希望这段信息对您有帮助！