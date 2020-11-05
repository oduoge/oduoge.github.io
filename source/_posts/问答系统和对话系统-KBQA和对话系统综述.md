---
title: 问答系统和对话系统-KBQA和对话系统综述
date: 2020-11-05 15:21:12
tags:
- 待整理
categories:
- 问答系统
---

https://zhuanlan.zhihu.com/p/93023782

问答系统和对话系统的综述
主要对问答系统和对话系统进行技术综述，包括落地场景及开源框架选择
问答系统
综述问答系统相关技术细节及开源框架
问答系统的发展
问答系统的分类
知识领域分类：面向限定领域的问答系统；面向开放领域的问答系统；面向常用问题集（ Frequently Asked Questions, FAQ ）的问答系统
答案来源：基于结构化的问答系统，例如KBQA系统；基于文本的问答系统，如SQuAD阅读理解问答、Facebook DrQA、CoQA；基于问答对的问答系统，如FAQ问答
答案反馈机制：基于检索式的问答系统；基于生成式的问答系统
问答系统关键技术
FQA
简介： FAQ检索型问答是根据用户的新Query去FAQ知识库找到最合适的答案并反馈给用户
传统文本匹配方法：主要解决字面相似度问题
信息检索BM25
向量空间模型VSM
深度学习文本匹配方法：解决相似度匹配问题；问题答案对匹配.
Represention-based Method
Interaction-based Method
KBQA( 知识图谱-基于ES(ElasticSearch)和gAnswer构建智能问答系统可以查看 )
语义解析
信息抽取
向量建模
基于阅读理解的问答
传统的机器阅读理解问答：一般是先对文本进行实体和属性的解析和提取（类似于实体和关系抽取），构建结构化的知识图谱，在知识图谱上进行问答（类似KBQA系统）
实体识别与检测：对文本中提及的实体，进行分类(实体文本/文本分类)
实体链接：识别的实体需要与外部知识库进行链接匹配
属性填充：从文本检测出来的属性，补充到外部知识库中
知识检索：根据实体和属性进行相关问答答案的问答（这个部分与KBQA系统中的问题-答案映射算法类似）
端对端的机器阅读的问答(在SQuAD数据集上)
微软的R-net
Match-LSTM with Answer Pointer
BiDAF模型
QANet(NarrativeQA数据集)
BiDAF+Self-att+ELMO（SQuAD2.0）
BERT(HotpotQA)
DCN模型
数据集：SQuAD，完形填空数据集 CNN/Daily News 和 Children‘s Book Test (CBT)

VOA(视觉问答)
问答系统开源平台及实现方式
百度 AnyQ
AnyQ 系统采用配置化、插件化框架设计，搭载由百度自主研发、业界领先的 SimNet 语义匹配技术，针对 FAQ 问答的种种技术难题给出了高效的解决方案
IBM QuestionAnsweringSystem
QuestionAnsweringSystem是一个Java实现的人机问答系统，能够自动分析问题并给出候选答案。IBM人工智能计算机系统"沃森"（Watson）在2011年2月美国热门的电视智力问答节目"危险边缘"（Jeopardy！）中战胜了两位人类冠军选手，QuestionAnsweringSystem就是IBM Watson的Java开源实现。
gAnswer
gAnswer 能够将自然语言问题转化成包含语义信息的查询图，然后，将查询图转化成标准的SPARQL查询，并将这些查询在图数据库中执行，最终得到用户的答案。
ADAM
ADAM 一个问题回答系统，从维基百科中提取答案，然后用自然语言提出问题 ,基于工业自然语言框架spaCy流水线
OpenEphyra
OpenEphyra 是一个使用 Java 开发的模块化、可扩展的问答系统、安装简单
DeepQA
DeepQA是Google基于知识库的问答系统.
其他开源对话系统对比

没有对上述系统进行详细的测试，还以其他的社区问答开源平台

对话系统
对话系统的发展
对话系统关键技术
管道式
端对端
对话系统开发辅助语言
AIMI
AIMI是由Richard Wallace开发的。他制造了一个名为A.L.I.C.E.(人工语言学互联网计算机实体)的机器人，该机器人赢得了几项人工智能大奖。有趣的是，寻找人工智能的图灵测试之一是让人类通过文本界面与机器人交谈几分钟，看看他们是否认为它是人类。AIML是一种XML格式，它定义了匹配模式和确定响应的规则
Chatscript
Chatscript
对话系统开源平台
Rasa
Rasa是一款搭建智能助手及机器人的开源系统平台，主要由两个部分组成：rasa NLU(主要用于理解用户消息，包括意图识别、实体识别、填充槽位值等，把用户的自然语言输入转换为结构化数据 )；rasa Core 对话管理平台，类似于对话系统中DPL(对话策略学习)、DST(对话策略跟踪)
Botfront 是一个建立在Rasa库之上的免费开源聊天机器人平台。类似于Rasa-X，但是Rasa-X不开放源码，使用这个前端框架需要Docker/Docker-Compose来同时运行多个容器，比如web-botfront


Uber Plato
Plato是一个用于构建、训练和部署会话 AI 智能体的平台
FacebookParAl
ParAI 一个用于共享、培训和测试对话模型的python框架，从开放域的聊天到VQA(可视化的问题回答)
ConvLab
ConvLab由清华与微软联合开发多领域的端对端对话系统平是DSTC8 track1的官方平台，基于SLM-Lab（强化学习平台）实现
DeepPavlov
DeepPavlov是一款开源的对话AI架构 （Context QA SQuAD)对话式文档问答，百度UNIT3.0创新技术的范畴。可以与Rasa接口对接
PyDial
PyDial是一个开源的端到端的统计对话系统工具包，它为所有的对话系统模块提供了统计方法的实现。此外，它还被扩展为提供多域会话功能。它提供了各自对话系统模块的简单配置、简单的可扩展性和独立于域的实现。
Virtual Human Toolkit
VHT 旨在帮助和支持研究人员和开发人员创建虚拟人对话系统。
百度UNIT
UNIT搭载业界领先的需求理解、对话控制及底层的自然语言处理、知识挖掘等核心技术让您的产品快速拥有对话交互能力
腾讯TBP
腾讯智能对话平台（Tencent Bot Platform，TBP）专注于“对话即服务”的愿景，全面开放腾讯对话系统核心技术，为大型企业客户、开发者和生态合作伙伴提供开发平台和机器人中间件能力，实现便捷、低成本构建人机对话体验和高效、多样化赋能行业
科大讯飞AIUI
AIUI 是科大讯飞2015年推出的一套以语音为核心的人机交互解决方案，意在使应用和设备能够快速具备能听会说，能理解会思考的能力。接入了 AIUI 的应用和设备 可以轻松实现查询天气、播放音视频资源、设置闹钟以及控制智能家居等能力
Opendial
Opendial 一个基于java、领域独立的开发语音对话系统的工具包。OpenDial的主要重点是健壮和自适应的对话管理，但OpenDial也可以用来建立成熟的、端到端的对话系统，集成语音识别、语言理解、生成和语音合成
京东Wukong-robot
Wukong-robot 是一个简单、灵活、优雅的中文语音对话机器人/智能音箱项目，目的是让中国的 Maker 和 Haker 们也能快速打造个性化的智能音箱
晓多客户机器人
晓多客服人平台 成都晓多科技有限公司（简称晓多）核心团队由来自百度NLP和京东JIMI机器人团队的人工智能专家组成，毕业于北京大学、复旦大学、斯坦福大学等世界知名高校 。客服机器人构建平台，通过自主配置知识库(内置知识问答库、自定义问答库、动态问答、多轮会话和知识图谱)，添加知识库后启动训练，提高机器人领域识别能力。
支持FQA一问一答任务，也支持KBQA和多轮问答任务，但是需要收费，主要应用无客服机器人，试用版不能够进行语料的训练，平台不怎么开放。

ChatterBot
ChatterBot是用Python构建的基于机器学习的对话对话引擎，它可以根据已知对话的集合生成响应。ChatterBot的语言独立设计允许它被训练成任何语言 。
Wit-ai
Wit-ai
Snips
Snips平台是一个软件解决方案，为私人语音助手提供支持。无论是企业还是个人，任何人都可以在单板电脑上设置Snips平台(例如:a Raspberry Pi 3, a i。MX8M板，Android或iOS设备)，并安装语音助手。国外语音助手开发框架，类似国内腾讯的Wu-kong
提供Snips NLU模块,NLP模块是基于spaCy
KITT.AI（百度收购）
snowboy 是一个开源的、轻量级语音唤醒引擎，可以通过它很轻松地创建属于自己的类似“hey, Siri” 的唤醒词。它的主要特性如下：
高度可定制性。可自由创建和训练属于自己的唤醒词
始终倾听。可离线使用，无需联网，保护隐私。精确度高，低延迟
轻量可嵌入。耗费资源非常低（单核 700MHz 树莓派只占用 10% CPU）
开源跨平台。开放源代码，支持多种操作系统和硬件平台，可绑定多种编程语言
Kore AI
KoreAI是业界第一个也是唯一一个企业级、端到端的对话型人工智能机器人平台，用于设计、创建、培训、测试和托管人工智能和nlp型聊天机器人，用于最受欢迎的消费者和商业通信渠道。也 是一个基于web的平台，它提供了满足消费者和企业bot用例所需的所有技术工具、流程和业务组件。
模块非常丰富，支持技能打断/恢复,如下图所示，会话流程设计(非常重要的环节)，两个意图（查询航班和查询天气）(后面重点来介绍Kore.ai的相关技术细节，主要三个模块(Fundamental Meaning（Chatscript编写） Custom Machine Learning (ML)、Ontology-based Knowledge Graph Engine (KG))



SAP Conversational AI(前身是Recast.AI)
SAP Conversational AI傻瓜式的对话机器人构建平台，界面交互比较友好，主要是面向企业级的用户，SAP公司主要是为企业提供软件管理服务，收购Recast.ai后与其他产品进行绑定。中文支持不好，NLU模块只支持意图识别和实体识别(需要自己构建训练)

Watson Assistant
Watson Assistant
Activechat.ai
activechat流程式的对话系统平台，NLU模块调用Dialog Flow的NLU模块API进行意图识别、词槽和实体识别
Flow.ai
flow.ai 流程式构建对话流程，NLU模块需要训练（自己构建好意图语料库），对于中文，官方语言中没有官方语言的选择，试用了一下，是可以支持的，比BotStart纯流程构建要好很多，支持语音输入模块,如下操作


BotStar
BotStar流程式的对话系统平台，没有使用到NLU中意图识别和词槽填充，主要应用于机器导引的流程式场景，使用BotStar简单做了一个流程的机器人，如下图所示


BotPress
botpress 是一个开源的一体化机器人创建平台，提供了构建、调试和部署基于人工智能的对话助手所需的所有工具。
相对流程式的对话管理，botpress有更好的扩展性，支持FQA，对话引擎、NLU，同时支持主要NLU第三方平台，比如Google Dialogflow、Rasa NLU等，简答实践效果如下图所示


BotKit
Botkit 一个开放源码的开发工具，用于为主要的消息传递平台构建聊天机器人、应用程序和自定义集成
参考文献
Relation Extraction Survey

DeepPavlov：一个面向端到端对话系统和聊天机器人的开源库

NLP 笔记 - Question Answering System

自动问答系统开源轮子简介

NLP（二）, Storm, Pulsar

阿里云小蜜对话机器人背后的核心算法

基于知识图谱的问答在美团智能交互场景中的应用和演进

人机对话系统调研

开源问答系统开源软件

百度 UNIT 技术负责人揭秘：如何让你的对话系统更智能

详解ParlAI：为让AI更会聊天，Facebook开源了这个“一站式对话研究工具

Uber 宣布开源 Plato 人工智能对话平台

DeepPavlov：一个面向端到端对话系统和聊天机器人的开源库

微软和清华开源ConvLab: 多领域端到端对话系统平台

《从零开始学习自然语言处理(NLP)》-DeepPavlov框架解析（4）

2019-06-17问答系统项目落地调研

Watson之心：DeepQA

十个Chatbot框架介绍

任务型对话管理的产品实践（第三篇）- 业界解决方案一览

微软小冰论文——第六代小冰架构解析

大厂对话系统实践文章链接

基于深度学习的FAQ问答系统

