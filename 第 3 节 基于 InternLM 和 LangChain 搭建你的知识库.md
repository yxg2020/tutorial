1.大模型开发范式

	1.LLM的局限性
		1.知识时效性受限：如何让LLM能够获取最新的知识
		2.专业能力有限：如何打造垂域大模型
		3.定制化成本高：如何打造个人专属的LLM应用
	2.RAG VS Finetune
		1.RAG:低成本；可实时更新；受基座模型影响大；单次回答知识有限。
		2.Finetune:可个性化微调；知识覆盖面广；成本高昂；无法实时更新。
	3.RAG：检索增强生成
		 Sentence Transformer--文本向量化；
		 Chroma向量数据库--匹配相似文本段 ；
		用户输入--文本向量化--匹配相似文本段--Prompt；
		用户输入----------------------------Prompt；
		Prompt--InternLM--最终输出；
		
2.LangChain简介

	1.LangChain 框架是一个开源工具，通过为各种LLM提供通用接口来简化应用程序的开
	发流程，帮助开发者自有构建LLM应用。
	2.LangChain核心组成模块：
		1.链（Chains）：将组建组合实现端到端应用，通过一个对象封装实现一系列
		LLM操作；
		2.Eg.检索问答链，覆盖实现RAG（检索增强生成）的全部流程。
	3.基于LangChain搭建RAG应用
3.构建向量数据库

	1.加载源文件
		-确定源文件类型，针对不同类型源文件选用不同的加载器
			-核心在于将带格式文本转化为无格式字符串
	2.文档分块
		-由于单个文档往往超过模型上下文上限，我们需要对加载的文档进行切分
			-一般按字符串长度进行分割
			-可以手动控制分割快的长度和重叠区间长度
	3.文档向量化
		-使用向量数据库来支持语义检索，需要将文档向量化存入向量数据库
			-可以使用任意一种Embedding模型进行向量化
			-可以使用多种支持语义检索的向量数据库，一般使用轻量级的Chroma
4.搭建知识库助手

	1.将InternLM接入LangChain
		-LangChain支持自定义LLM，可以直接接入到框架中
		-我们只需要将InternLM部署在本地，并封装一个自定义LLM类，调用本地
		InternLM即可
	2.构建检索问答链
		-LangChain提供了检索问答链模板，可以自动实现知识检索，Prompt嵌入，LLM
		问答的全部流程
		-将基于InternLM的自定义LLM和已构建的向量数据库接入到检索问答链的上游
		-调用检索问答链，即可实现知识库助手的核心功能
	3.RAG方案优化建议
		-基于RAG的问答系统性能核心受限于：
			-检索精度
			-Prompt性能
		-一些可能的优化点
			-检索方面：
				-基于语义进行分割，保证每一个Chunk的语义完整
				-给每一个Chunk生成概括性索引，检索时匹配索引
			-Prompt方面：
				-迭代优化Prompt策略
5.Web Demo部署

	1.支持建议Web部署的框架，如Gradio,Streamlit等
6.作业

	1，基础作业：
	复现课程知识库助手搭建过程 (截图)
		1.安装相关依赖和相关资源
		
		![[Pasted image 20240214130153.png]]
		
		2.安装语料库和模型
		
		![[Pasted image 20240214130300.png]]
		
		3.部署web-demo，及本地映射
		
		![[Pasted image 20240214131042.png]]
		4.web界面
		
		![[Pasted image 20240214125222.png]]
	
	2.进阶作业：
	选择一个垂直领域，收集该领域的专业资料构建专业知识库，并搭建专业问答助手，并
	在 [OpenXLab](https://openxlab.org.cn/apps) 上成功部署（截图，并提供应用
	地址）