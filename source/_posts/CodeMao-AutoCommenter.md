---
title: "CodeMao-AutoCommenter使用文档"
tags: [Auto,CodeMao]
index_img: /img/article/article_CodeMao_AutoCommer.jpg
date: 2024-1-15 14:00:00
---

![signs](/img/article/article_CodeMao_AutoCommer_concent.jpg)

# 目录

- [项目简介](#项目简介)

- [功能列表](#功能列表)

- [环境要求](#环境要求)

- [功能列表](#功能列表)

- [文件结构](#文件结构)

- [使用方法](#使用方法)

- [功能详解](#功能详解)

- [项目总结](#项目总结)

- [下载地址](#下载地址)

## 项目简介

猫毡（雾）最强爬虫）

这个 Python 项目可以自动在编程猫网站上对作品进行**点赞**,**评论**以及**回复**,实现自动化模拟用户行为,以提高作品曝光率。✨

## 作品说明

原作:

- [python自动评论](https://shequ.codemao.cn/community/429585 )
- [Python评论自动回复](https://shequ.codemao.cn/community/430412)

**by** [伴雪纷飞](https://shequ.codemao.cn/user/2856172)

现作者账号转移至 [伴只狗头](https://shequ.codemao.cn/user/1888155714)

参考:

- 账号密码登录代码 **by** [PXstate](https://shequ.codemao.cn/user/1258391425) 
- 屏蔽词转换代码 **by** [伴雪纷飞](https://shequ.codemao.cn/user/2856172)

感谢《**A Byte of Python**》(《简明Python教程》)Swaroop C H撰 漠伦译 
- [说明文档](http://zhuanlan.zhihu.com/p/24672770)

## 功能列表

- [x] 模拟登录
  - [x] 支持账号密码登录 🔐
  - [x] 支持直接 Cookie 登录 🍪
- [x] 获取最新作品
  - [x] 从网站接口获取最新上传作品列表 🆕
- [x] 获取最新回复
  - [x] 获取新的评论与回复以及点赞信息😉
- [x] 自动点赞
  - [x] 调用接口对作品点赞 👍
  - [x] 统计点赞次数 🔢
- [x] 自动评论
  - [x] 随机获取评论内容 💬
  - [x] 随机选择表情 😃
  - [x] 统计评论次数 📊
- [x] 自动回复
  - [x] 多种回复模式📃
- [x] 数据过滤
  - [x] 跳过已评论过的作品 ❌
  - [x] 跳过点赞数过多的作品 🙅‍♂️
- [x] 结果记录
  - [x] 记录已评论过的作品,避免重复 🙅‍
  - [x] 支持自定义保存位置和文件名
- [x] 随机间隔
  - [x] 每次操作后随机等待一定时间间隔 ⏳
- [x] 配置文件
  - [x] 支持修改评论回复内容✍
- [x] 侦测评论
  - [x] 侦测作品前300个评论,避免因为配置文件丢失而重复评论🎫
- [x] 速度至上
  - [x] 1.10版本全面重写🔍
  - [x] 优秀的代码逻辑💎
- [x] 生成文件内容优化🚀

## 环境要求

### 使用的库

- os
- requests
- bs4 
- random
- json
- time

### 需要安装的库

- requests
- beautifulsoup4  

### 库的安装方法

可以使用 pip 直接安装:

```python
pip install requests
pip install beautifulsoup4
```

如果使用pip安装速度慢,可以先配置pip国内镜像源加速安装:

```python
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple # 永久设置
```
```python
  pip install example -i https://pypi.tuna.tsinghua.edu.cn/simple # 暂时设置 
```

配置完成后,再使用pip安装库。

还有其他国内源

- [清华](https://pypi.tuna.tsinghua.edu.cn/simple)
- [阿里云](http://mirrors.aliyun.com/pypi/simple/)
- [中国科技大学](https://pypi.mirrors.ustc.edu.cn/simple/)
- [华中理工大学](http://pypi.hustunique.com/)
- [山东理工大学](http://pypi.sdutlinux.org/)
- [豆瓣](http://pypi.douban.com/simple/)

## 文件结构

```
├── Automatic-comments-v1.x.x.py    主程序文件 💻
├── config.json  					信息配置文件 📄
└── example.txt     				结果记录文件 📄
```

## 使用方法

1. 配置 Python 环境,安装需要的库 👩‍💻
2. 运行 Automatic-comments-v1.x.x.py ▶️
3. 根据提示选择登录方式
   - 选择账号密码登录,输入编程猫账号和密码 👤
   - 选择 Cookie 登录,输入获取的 Cookie 值 🍪
4. 输入结果记录文件保存路径等选项 📁
5. 选择仅**点赞**或**点赞+评论**或**点赞+评论+回复**✅
6. 程序将自动运行,打开作品列表并处理 🤖

## 功能详解

### 初始化账户信息
```python
Account = {
    "phonenum": " ", "password": " ", "filepath": " ",
    "userid": " ", "nickname": " ",
}
CONFIG_FILE_PATH = path.join(getcwd(), "config.json")
HEADERS = {...}
```
🔑 初始化账户信息，包括电话号码、密码、文件路径、用户ID和昵称。还定义了配置文件的路径和HTTP请求头。
### 预设数据类 `Pre_Data`
```python
class Pre_Data:
    def __init__(self):
        self.proxies_list = [{"http": "http://114.114.114.114:2333"}]
        self.contents = ["666＃°Д°", "加油！:O", ...]
        self.emojis = ["编程猫_666", "编程猫_棒", ...]
        self.answers = ["这是{}的自动回复...", "{}的自动回复来喽", ...]
```
📌 存储了预设的代理列表、评论内容、表情和自动回复模板。这些数据用于构建社区互动。
### 实际使用的数据类 `Data`
```python
class Data:
    def __init__(self):
        self.proxies_list = ([" "],)
        self.UNDO_LIST = [""]
        self.contents = ([" "],)
        self.emojis = ([" "],)
        self.answers = [" "]
```
📊 此类用于实际运行时存储和更新数据，如代理列表、评论内容等。
### 发送请求的函数 `send_request`
```python
def send_request(url, method, data=None, headers=HEADERS, direct=None):
    ...
```
🌐 此函数负责向指定的URL发送GET或POST请求。它还处理代理选择和异常情况。
### 检查配置文件是否存在的函数 `has_config_file`
```python
def has_config_file():
    ...
```
📁 检查配置文件是否存在，以确定是否需要加载或创建新的配置。
### 保存和加载账户信息的函数
```python
def save_account(path):
    ...
def load_account():
    ...
```
💾 这两个函数用于在配置文件中保存和加载账户信息及预设数据。
### 为账户信息提供交互式输入的函数 `input_account`
```python
def input_account():
    ...
```
👤 提供交互式界面以输入账户信息，如电话号码和密码。
### 登录函数 `login`
```python
def login():
    ...
```
🔐 处理用户登录逻辑，包括从网页解析所需的PID、发送登录请求、处理登录响应和更新账户信息。
### 检查评论的函数 `checkcomments`
```python
def checkcomments(work_id, special_id):
    ...
```
🔍 此函数用于检查特定作品的评论是否已存在。它通过发送请求到编程猫社区API，获取特定作品的评论数据，并遍历这些评论以检查是否包含特定用户ID的评论。
### 将文本写入指定文件的函数 `write`
```python
def write(text, name):
    ...
```
✍️ 用于将文本内容写入指定的文件。这主要用于记录已经进行过互动的作品ID，以避免重复互动。
### 排序文件中的数字的函数 `sort_numbers_in_file`
```python
def sort_numbers_in_file(input_file_path):
    ...
```
🔢 用于对文件中的数字进行排序。这通常用于优化生成的文件内容，例如将作品ID按照数值顺序排列。
### 点赞函数 `like_work`
```python
def like_work(cookie, work_id):
    ...
```
👍 对指定作品进行点赞。该函数发送POST请求到编程猫社区API，完成点赞操作，并处理响应以确认操作是否成功。
### 评论函数 `comment_work`
```python
def comment_work(cookie, work_id):
    ...
```
💬 对指定作品进行评论。它随机选择预设的评论内容和表情，然后发送POST请求进行评论。
### 回复函数 `reply_work`
```python
def reply_work():
    ...
```
🔄 处理接收到的回复消息。该函数首先检查是否有新的回复消息，然后根据消息类型（如评论回复或作品回复）发送适当的回复内容。
### 主函数 `main`
```python
def main():
    ...
```
🚀 定义了脚本的主要运行逻辑。它包括登录逻辑、用户交互以选择操作模式（如仅点赞、仅评论等），并循环执行所选操作。

## 项目总结

这个项目主要运用了 requests、BeautifulSoup 等库模拟登录并解析页面,通过调用官方 API 实现自动化点赞和评论。✨

程序加入了一定的过滤机制,间隔设置以及代理来规避风控。🛡️

可以作为 Python 网络爬虫和接口调用的练手项目,与此同时需要注意合理使用,遵守网站规则。❗️

**如果有任何问题欢迎反馈,本项目仅用于技术学习交流。💬**

## 下载地址

- [GitHub访问加速](https://cloud.tsinghua.edu.cn/d/df482a15afb64dfeaff8/)
- [GitHub下载加速](https://ghproxy.com/) 
- [123云盘](https://www.123pan.com/s/oRHZVv-PBKtv.html)    提取码:*ygyc* 
- [蓝奏云网盘](https://zybqw.lanzout.com/b04e5pmcd)    密码:*ygyc* 
- **解压密码**:***zybqw***

## 联系我

如果您对此项目有任何问题或建议，欢迎随时联系我。😊

- 博客：zybqw.github.io
- 邮箱：zybqw@qq.com 📧
- 猫站主页：https://shequ.codemao.cn/user/12770114 🌐
- QQ: 3611198191
- 微信: Aurorzex

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=zybqw/CodeMao-AutoCommenter&type=Date)](https://star-history.com/#zybqw/CodeMao-AutoCommenter&Date)


## 感谢您的阅读！😉
