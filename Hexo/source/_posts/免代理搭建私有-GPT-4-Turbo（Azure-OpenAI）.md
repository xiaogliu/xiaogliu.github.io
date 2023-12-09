---
title: 免代理搭建私有 GPT-4 Turbo（Azure OpenAI）
date: 2023-12-09 15:41:27
tags: [ChatGPT]
categories: AI
e_title: deploy-gpt4-turbo-without-vpn
---

# 一. 申请 Azure OpenAI API 使用资格

## 1.1 重点

- 需要使用企业邮箱，个人邮箱会被拒绝。这里的企业邮箱可以是个人域名下的邮箱，只要不是 @[gmail.com](http://gmail.com/), @[yahoo.com](http://yahoo.com/), @[hotmail.com](http://hotmail.com/) 这些就好。
- 另外，原则上申请使用资格是代表**你自己的公司**，所以最好不要使用你所在打工的公司邮箱，至于会有怎么的后果我并不清楚，申请时请注意。
	![代表自己公司](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/Azure%20Open%20AI%20deployment/iShot_2023-12-09_10.42.07.png)

## 1.2 前置工作

除了准备企业邮箱，还需要准备好订阅 ID。

登陆 [Azure](https://portal.azure.com/)，然后到搜索 ”订阅“ 即可看到订阅 ID，如果没有 “添加” 即可。

> 如果不是新用户，需要绑定信用卡。

> 如果是新用户，会默认分配一个 ”Free Trial“ 的订阅 ID，有效期为一个月。如果你申请的 Azure Open AI API 挂在试用订阅 ID 下面，记得升级试用订阅 ID 为正式订阅 ID，不然一个月到期后，试用帐号被删除，这个帐号下面的资源也会同步删除。

### 1.2.1 订阅入口

![订阅 ID 入口](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/Azure%20Open%20AI%20deployment/image1.png)

### 1.2.2在订阅列表中选择要部署的 ID

![订阅 ID](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/Azure%20Open%20AI%20deployment/iShot_2023-12-09_10.32.57.png)

## 1.3 填写申请表

点击 [Request Access to Azure OpenAI Service](https://go.microsoft.com/fwlink/?linkid=2222006&clcid=0x409&culture=en-us&country=us) 进去申请页面，一般 24 小时会出结果。

> 但也有可能遇到进一步审核的情况，会让你提交更多企业资料。这种情况一般是因为你填的公司微软审核无法验证。

# 二. 部署 Azure Open AI

使用资格申请下来后就可以部署 Azure Open AI 了。

## 2.1 创建 Azure OpenAI 实例

搜索 ”Azure OpenAI“ 进入创建流程。

### 2.1.1 Azure OpenAI 入口

![Azure OpenAI 入口](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/Azure%20Open%20AI%20deployment/iShot_2023-12-09_10.54.54.png)

### 2.1.2 创建新实例

![创建新实例](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/Azure%20Open%20AI%20deployment/iShot_2023-12-09_10.55.10.png)

### 2.1.3 填写实例信息

**实例信息中唯一需要关注的是实例区域的选择，因为不同区域 Open AI 的模型不一样。** 比如现在（2023-12-09）如果你想在一个实例中同时部署 `gpt-4-1106-preview` 和`gpt-35-turbo-1106` 美国地区只有 West US 满足。点击 [Azure OpenAI Service models](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models) 查看哪些区域有哪些模型。

![填写实例信息](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/Azure%20Open%20AI%20deployment/iShot_2023-12-09_15.04.24.png)
> `gpt-4-1106-preview` 即 GPT-4 Turbo，比 GPT-4 更强大并且价格更低。所以[模型代码及价格](https://openai.com/pricing)
## 2.2 部署 Azure OpenAI 模型

创建 Azure Open AI 实例成功后，便可部署模型。

### 2.2.1 进入刚刚创建的实例

点击 “Create new deployment”

![Deployments](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/Azure%20Open%20AI%20deployment/iShot_2023-12-09_11.07.29.png)

### 2.2.2 填写模型信息

填写模型信息后点击 "Create" 就可以创建模型了。

![填写模型信息](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/Azure%20Open%20AI%20deployment/iShot_2023-12-09_11.09.24.png)

## 2.3 获取 Azure OpenAI API 信息

推荐从 Chat --> View Code 中获取这些信息

### 2.3.1 点击 ”View Code”

![view code](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/Azure%20Open%20AI%20deployment/iShot_2023-12-09_11.13.44.png)

### 2.3.2 复制 Endpoint 和 Key，后面部署客户端时需要

![sample code](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/Azure%20Open%20AI%20deployment/iShot_2023-12-09_11.14.52.png)

# 三. 部署客户端

## 3.1 开源客户端选择

目前有很多开源的 ChatGPT 客户端，我用的是 [ChatGPT-Next-Web](https://github.com/Yidadaa/ChatGPT-Next-Web)。

它支持多种部署方式，下面是以部署到私人服务器为例，介绍怎么通过 [ChatGPT-Next-Web](https://github.com/Yidadaa/ChatGPT-Next-Web) 部署 Azure OpenAI。

## 3.2 部署到私人服务器

[ChatGPT-Next-Web](https://github.com/Yidadaa/ChatGPT-Next-Web) 最开始只支持 OpenAI 的 API，从 `2.9.11` 版本开始，已支持 Azure OpenAI 的部署，不同 API 只需要配置对应的环境变量即可。

### 3.2.1 获取部署 Azure OpenAI 必要环境变量

- **AZURE_URL (required)**: `https://{azure-resource-url}/openai/deployments/{deploy-name}` 
- **AZURE_API_VERSION (required)**: Azure API Version

上面两个变量都可以在上文提到的 Endpoint 中获取：`https://{azure-resource-name}.openai.azure.com/openai/deployments/{deploy-name}/chat/completions?api-version={api-version})` 

> 不要直接把 endpoint 放到 AZURE_URL 中，AZURE_URL 只需要 endpoint 的一部分！

- **AZURE_API_KEY (required)**: Azure API Key

### 3.2.2 非必要环境变量但最好设置

- **CODE**：在部署 Azure OpenAI 实例时我选择了外网均可访问，如果把 ChatGPT 客户端部署到服务器上，默认外网都可访问，设置 **CODE** 就给自己客户端加了一层密码，只有输入密码后才能使用。如果只在本地使用，该变量可忽略。

> 如果你部署的客户端外网可访问，这个密码务必设置为强密码，不然可能被[爆破](https://github.com/Yidadaa/ChatGPT-Next-Web/blob/main/docs/faq-cn.md#%E4%B8%BA%E4%BB%80%E4%B9%88%E6%88%91%E7%9A%84-token-%E6%B6%88%E8%80%97%E5%BE%97%E8%BF%99%E4%B9%88%E5%BF%AB)!

- **HIDE_USER_API_KEY**：这个参数决定用户是否禁止用户自己的 API key，对于 Azure OpenAI，用户数据它的 API key 也没用，设置为 `1` 。

### 3.2.3 部署客户端

以 Linux 服务器为例，可使用 [ChatGPT next web Docker 镜像](https://hub.docker.com/r/yidadaa/chatgpt-next-web/tags) 实现快速部署：

```bash
docker run -d \
  --name chatgpt-web \
  -p 9000:3000 \
  -e AZURE_URL=https://{azure-resource-url}/openai/deployments/{deploy-name} \
  -e AZURE_API_KEY={azure-api-key} \
  -e AZURE_API_VERSION=2023-07-01-preview \
  -e CODE={app-custom-password} \
  -e HIDE_USER_API_KEY=1 \
  --restart=always \
  yidadaa/chatgpt-next-web:latest
```

上面命令中使用的是服务器 `9000` 接口，可根据你自己的配置灵活调整。

# 四. 客户端配置

## 4.1 配置 Auth

如果第三步部署成功，访问你的域名应该可以看到如下界面

![ChatGPT next web](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/Azure%20Open%20AI%20deployment/iShot_2023-12-09_15.34.08.png)

点击 auth，输入配置的密码（**CODE**）后就可以正常访问了。

## 4.2 聊天记录同步

聊天记录默认存储在本地，目前（2023-12-09）ChatGPT next web 支持 WebDAV 和 [UpStash](https://upstash.com/) 两种同步方法：
- WebDAV：如果你有外网可访问的 WebDAV，推荐这种同步方法；
- UpStash：如果你没有外网可访问的 WebDAV，可以直接使用 UpStash，免费版本就够用了。UpStash 的同步配置过程请看 ChatGPT next web 文档 [synchronise-chat-logs-cn.md](https://github.com/Yidadaa/ChatGPT-Next-Web/blob/main/docs/synchronise-chat-logs-cn.md)
