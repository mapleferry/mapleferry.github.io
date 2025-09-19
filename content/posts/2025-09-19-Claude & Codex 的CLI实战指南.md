---
title: "Claude & Codex 的CLI实战指南"
date:  2025-09-19T11:45:00+08:00
categories: ['AI']
series: ['AI']
ShowToc: true
#TocOpen: true
tags: ['AI','CLI','Claude','Codex','实战']

---

今天我们聊个实在的。Web UI确实漂亮，但对于我们这些恨不得在Vim里收发邮件的开发者来说，每切一次窗口都是对生产力的一次“打断施法”。

如果能直接在命令行里和代码助手对话，那该多爽？比如，用管道符 `|` 把一段代码直接传给它重构，或者用一个 `alias` 把它变成你自己的CLI工具。

这篇博客，就是为你准备的“懒人包”。我们将一步步搞定 Anthropic Claude 和 OpenAI Codex（我们会通过官方的OpenAI CLI来调用它）的命令行配置和使用。

Let's do this.

---

### 第一站：驯服 Claude (通过 Python SDK)

Anthropic 官方没有提供一个独立的CLI，但这根本不是问题。我们是开发者，我们自己造！最简单直接的方式就是用它的Python库写一个薄薄的封装。

#### 1. 准备工作

- **Python 和 pip**：确保你电脑里有这俩兄弟。没有的话，官网下一下。
- **Anthropic API Key**：去 [Anthropic官网](https://console.anthropic.com/) 登录，在后台找到你的API密钥。

#### 2. 安装

打开你的终端，一行命令搞定：

Bash

```
pip install anthropic
```

#### 3. 配置

我们要做一个有素养的开发者，绝不把密钥硬编码在代码里。把它设置成环境变量：

Bash

```
# 把 'sk-...' 替换成你自己的API Key
export ANTHROPIC_API_KEY='sk-...'

# 为了让它永久生效，可以把这行命令加到你的 .bashrc, .zshrc 或其他shell配置文件里
echo "export ANTHROPIC_API_KEY='sk-...'" >> ~/.zshrc
```

#### 4. 创建我们的 “CLI”

在你的工作目录下，创建一个名为 `claude.py` 的文件，然后把下面的代码复制进去。

Python

```
import sys
import anthropic

# 检查是否有命令行参数作为输入
if len(sys.argv) < 2:
    print("用法: python claude.py \"你的问题\"")
    sys.exit(1)

# 从命令行参数获取完整的prompt
prompt = " ".join(sys.argv[1:])

try:
    # 初始化客户端 (它会自动从环境变量读取API Key)
    client = anthropic.Anthropic()

    # 发送请求
    message = client.messages.create(
        model="claude-3-sonnet-20240229", # 你也可以换成 claude-3-opus-20240229 等其他模型
        max_tokens=2048,
        messages=[
            {"role": "user", "content": prompt}
        ]
    )

    # 打印结果
    print(message.content[0].text)

except Exception as e:
    print(f"出错了: {e}")

```

#### 5. 实战使用

现在，你的 `claude.py` 就是一个功能强大的CLI工具了！

**试试看：**

Bash

```
# 提问一个算法问题
python claude.py "用python写一个快速排序算法，并加上详细的注释"

# 让它帮你写Shell命令
python claude.py "写一个bash命令，查找当前目录下所有.log文件里包含'ERROR'的行"
```

终端会立刻返回你想要的代码或命令。是不是很酷？你甚至可以给它加个 `alias` 让使用更方便：`alias claude='python /path/to/your/claude.py'`。

---

### 第二站：指挥 Codex (通过 OpenAI CLI)

OpenAI 官方提供了一个非常方便的CLI工具，让调用Codex或GPT模型变得超级简单。

#### 1. 准备工作

- **Python 和 pip**
- **OpenAI API Key**：去 [OpenAI Platform](https://www.google.com/search?q=https://platform.openai.com/api-keys) 获取。

#### 2. 安装

同样是一行命令：

Bash

```
pip install openai
```

#### 3. 配置

和Claude一样，我们使用环境变量。OpenAI的CLI默认会读取 `OPENAI_API_KEY`。

Bash

```
# 把 'sk-...' 替换成你自己的API Key
export OPENAI_API_KEY='sk-...'

# 同样，建议加到你的 .zshrc 或 .bashrc 里
echo "export OPENAI_API_KEY='sk-...'" >> ~/.zshrc
```

#### 4. 实战使用

OpenAI的CLI用法非常直接，基本就是`openai api <endpoint>.<action>`的格式。对于代码生成，我们主要用 `completions`。

**试试看：**

我们要调用一个擅长代码的模型，比如 `gpt-3.5-turbo-instruct`（它是`code-davinci-002`的后继者，效果更好）。

Bash

```
# 给出一个函数头，让它补全函数体
openai api completions.create \
  -m gpt-3.5-turbo-instruct \
  -p "def create_s3_bucket(bucket_name):" \
  --max-tokens 150 \
  --temperature 0.2
```

**参数解释：**

- `-m` / `--model`: 指定你要调用的模型。
- `-p` / `--prompt`: 你的输入提示。
- `--max-tokens`: 控制输出的最大长度，避免刷屏。
- `--temperature`: 控制创意的程度，对于写代码，我们通常用一个较低的值（比如0.2）来保证结果的确定性。

**另一个例子，生成一个Dockerfile：**

Bash

```
openai api completions.create \
  -m gpt-3.5-turbo-instruct \
  -p "# Create a Dockerfile for a basic Python Flask app" \
  --max-tokens 250
```

你会看到一个完整的、可用的Dockerfile直接打印在你的终端里。