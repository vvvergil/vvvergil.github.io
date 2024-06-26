---
title: 部署一个自己的gpt服务器
date: 2023-05-30
update: 2024-05-22
categories: 教程
tags: [ai]
---

# 前言

大家好，我是蒸锅。现在人工智能发展地越来越迅速，ai聊天、ai绘画、ai视频。ai正在不断突破我们的认知。日常工作里，能用到的地方也非常多，查概念、翻译、美化甚至生成文档。至少现在我用gpt的频率比浏览器还要多。

但是呢，国内访问gpt的官网是受到限制的，即使使用了一些“科学上网”的手段，也得忍受着很高的延迟，那些ai绘画、ai视频等服务更是很难享受到。那么何不自己建一个服务器，让自己在国内也能享受到流畅的ai服务。本篇教程是自己在不断实践，不断碰壁中总结出来的宝贵经验。那么咱们现在就开始。

# gpt是什么

<u>GPT是“Generative Pre-trained Transformer”的缩写，是一种基于Transformer架构的自然语言处理模型。GPT模型系列由OpenAI开发，旨在处理各种自然语言处理任务，如文本生成、问答、摘要等。</u>——gpt3.5turbo。

简而言之，gpt是OpenAI开发的ai模型，也是当前最强大的语言模型，OpenAI依靠他推出了两个盈利模式，第一种面向大部分普通人，通过官网直接与gpt3.5进行聊天。第二种面向开发人员，提供api调用接口，让开发人员可以开发自己的ai聊天界面。

## 1.面向普通人

这一节先讲一讲如何通过官网直接与gpt对话，想实践本节内容必须要知道如何“科学上网”，如果不能科学上网可以直接跳过本节。

虽然说是面向普通人，但这里的普通人更多地还是欧美等国家，国内用户。国内用户可能直接就卡在了注册用户上了。

OpenAI对国家政策的限制非常严格，但还是可以通过一些手段绕开他的限制。

1. 首先访问OpenAI的[chatgpt官网](https://chatgpt.com/) 。在这一步可能会遇到的问题是，OpenAI会限制你访问，提示你关闭梯子。解决方法是更换你的梯子节点，最好是欧美印度那边，多试几个。

2. 点击注册，会提示你输入邮箱，这里邮箱不要输国内邮箱，可以注册一个gmail。

3. 手机验证码，到这一步先暂停，打开wildcard的[官网](https://wildcard.com.cn/) ，这是一家主营ai服务的网站。在wildcard注册成功后你可以免费申请3次国外手机号半个小时的使用权，申请后，会告诉你手机号，以及国家地区代码。返回chatgpt页面，选择和你申请的手机号相同的国家地区，输入刚刚申请手机号，获取验证码。此时返回wildcard，等待验证码，根据wildcard的描述，手机号并不是百分之百成功的，可能会被其他人占用，可以多试几次。三次免费机会用完还可以去找一下国外专门提供虚拟手机号服务的网站。

4. 邮箱验证，输入刚才注册时填入的邮箱，在邮箱中点击验证按钮，验证成功后就可以正常登录OpenAI的官网了。

5. 此时就可以与chatgpt对话了，基础需求基本都能满足。也可以花钱升级更高的套餐，不过这套餐金额就有那么点贵了。

![升级价目表](/image/gpt/price.png)

## 2.面向开发者

这一节来讲一下作为一个开发者如何使用Openai提供的api接口开发自己的ai聊天软件。本节内容依然需要“科学上网”，且需要已经按照上节步骤注册了Openai的账号。

    1. 首先访问Openai-API的[官网](https://platform.openai.com/) 。同样的，登不进去就换几个节点。

    2. 如果你是第一次注册账号的话，会有几十美元的免费额度。下一步点击API keys选项，获取key。你会看到如下界面。

![](/image/gpt/apiKeys.png)

   3. 这里先解释一下Openai-api的收费模式。api采用按量收费的模式，即统计在与模型的对话中使用的token数来计费，token数可以简单理解为字数，它通常还会包含一个对话的上下文信息，越好的模型收费越贵，但就我个人的直观感受来说价格是完全可以接受的，纯个人用3.5turbo模型100人民币可以供我用好几个月。下图是gpt3.5turbo模型的收费说明。Openai区分不同的项目来方便开发者管理，每个项目都有一个api-key，这是软件调用Openai模型的凭证，api-key非常重要！！切记不要将其暴露出去，Openai根据api-key确定身份，也根据它来计费。

![](/image/gpt/gpt3.5.png)

    4. 下面来调用api，以node.js为例。首先安装Openai的官方库`

```js
npm install --save openai`
```

其次，在你项目的环境变量中设置OPENAI_API_KEY为你刚刚获取到的api-key。

最后使用如下方式调用api

```js
import OpenAI from "openai";

const openai = new OpenAI();

async function main() {
  const completion = await openai.chat.completions.create({
    messages: [{ role: "system", content: "You are a helpful assistant." }],
    model: "gpt-3.5-turbo",
  });

  console.log(completion.choices[0]);
}

main();
```

更加详细的方法可以查阅Open-api的官方文档。

# 部署自己的gpt服务器

为了不使用“科学上网”也能使用到ai的便利。可以按照后面的步骤自己部署一个gpt服务器。

## 获取api-key

第一步仍然是获取一个api-key。对于能够科学上网的人来说可以按照上一章的内容自己去官网申请一个。不过，为了能一直使用，还必须绑定一个能够交易外币的信用卡（我尝试过使用虚拟信用卡，不过失败了，Openai的限制太严格了）。对于无法“科学上网”的人来说，也可以去中间商那里获取api-key。有很多公司专门负责这种业务，即他们对api-key作了一层代理。让你使用他们的key也能正常调用gpt模型。区别如下：

|          | 官网key             | 代理商key                       |
|:--------:|:-----------------:|:----------------------------:|
| 价格       | 便宜                | 贵，一般是官网价格的1.1倍到1.5倍          |
| 国内能否正常访问 | 不能直接访问官网          | 国内能直接访问官网(毕竟主营目标就是被限制的国家和地区) |
| 支付方式     | 能进行外币交易的信用卡       | 微信、支付宝、银联等                   |
| 国内能否正常调用 | 不能，会检查调用api的服务器地区 | 基本都能                         |
| 服务完整性    | 最新的(毕竟就是人家的东西)    | 一些新出的服务无法立刻使用                |

两种方式获得key的方式各有千秋，可以结合自己的喜好自由选择。我这里提供两个提供api-key代理服务的公司:[openai-hk](https://www.openai-hk.com/) ，[wildcard](https://wildcard.com.cn/) 供参考。

## 购买服务器

这里又分两种情况，如果是官网获取的key的话，国内的服务器是不行的(香港的是否可以不确定)，要么购买海外的服务器，要么使用代理的方式将请求和响应转发一次(网上有很多教程)。而使用代理商key的话，国内的服务器就可以，当然香港的服务器更保险一点。服务器买轻量应用服务器，配置调到最低即可，可以去腾讯云、阿里云、百度云等大公司提供的服务买，但这样会贵很多，也可以去一些小公司，比如香草云这种，会便宜很多(大概十几块一个月吧)。下面以香草云为例来说明一下：

![](/image/gpt/xiangcaoyun1.png)

点进去就可以看到几个很便宜的香港服务器。选择最便宜的即可，配置可以选低一些，系统选择Centos，其他的都按默认的来。

购买完成后，在控制台界面就能看到自己购买的服务器了，点击管理，可以看到服务器的ip地址、用户名、密码等信息。

## 暴露端口号

在服务器安全组选项卡下，还需要暴露gpt服务的端口号，以便能够访问该服务。

点击添加规则，协议选择tcp，目标端口选择2024即可

## 连接服务器

下载XShell或者finalShell，下好后打开，新建一个SSH连接，服务器的ip地址，用户名，密码等信息。连接成功后，准备下载docker，首先下载docker.sh脚本

```
curl -fsSL https://get.docker.com -o get-docker.sh
```

然后安装docker

```
sh get-docker.sh
```

随后启动docker，设置docker开机自启

```
systemctl start docker
systemctl enable docker
```

检查docker是否启动成功

```
systemctl status docker
```

如果出现了 active(running)，就说明docker安装成功

## 选择gpt项目

现在有很多开源的gpt项目，可以根据自己的喜好进行选择，各个项目也都有各自详细的配置说明。这里以chatgpt-next-web为例说明。

```
docker run  --name chatgpt-next-web   -d -p 2024:3000 --restart=always \
-e OPENAI_API_KEY=换成你的中转key \
-e CODE=页面访问密码 \
-e HIDE_USER_API_KEY=1 \
-e BASE_URL=https://api.openai-hk.com   yidadaa/chatgpt-next-web
```

复制上述内容到终端，同时将你从官方或者代理商那里拿到的api-key替换上述中文部分，也可以设置一个你喜欢的页面访问密码，只有输入正确的访问密码才能获取gpt服务。如果不想设置，希望任何访问你服务的人都能获得gpt服务，也可以把`-e CODE=页面访问密码 \`这行删去。

输入`docker ps`可以检查项目是否运行成功。

至此，我们个人的gpt服务器已经部署完成。

## 访问

还记得你的服务器ip地址吗，在浏览器中输入 ip:2024 即可访问你的gpt服务
