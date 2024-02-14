1.什么是大模型？

	1.人工智能领域中参数量巨大，拥有庞大计算能力和参数规模的模型；

2.特点及应用

	1.利用大量数据进行训练
	2.拥有数十亿甚至千亿个参数
	3.模型在各种任务中展现出惊人的性能

3.InterLM模型全链条开源

	1.InternLM是一个开源的轻量级训练框架，旨在支持大模型训练而无需大量的依
	赖。基于InternLM训练框架，上海人工智能实验室已经发布了两个开源的预训练模
	型：InternLM-7B和InternLM-20B。
	2.Lagent是一个轻量级、开源的基于大语言模型的智能体（agent）框架，支持用户
	快	速地将一个大语言模型转变为多种类型的智能体，并提供了一些典型工具为大语言
	模型赋能。通过Lagent框架可以更好的发挥InternLM的全部性能。
	3.浦语·灵笔是基于书生·浦语大语言模型研发的视觉-语言大模型，有着出色的图文理
	解和创作能力，使用浦语·灵笔大模型可以轻松的创作一篇图文推文
	
	![[Lagent.png]]

4.模型介绍

	1.通过单一的代码库，它支持在拥有数千个GPU的大型集群上进行预训练，并在单
	个GPU上进行微调，同时实现了卓越的性能优化。在1024个GPU上训练时，InternLM可
	以实现近90%的加速效率。
	2.InternLM包含了一个拥有70亿参数的基础模型和一个为实际场景量身定制的对话模
	型，该模型具有一下特点：
		1.利用数万亿的高质量token进行训练，建立了一个强大的知识库；
		2.支持8K token的上下文窗口长度，使得输入序列更长并增强了推理能力。

5.浦语·灵笔图文理解创作 
	1. InternLM-Xcomposer-7B介绍
		
	1. 浦语·灵笔是基于书生·浦语大语言模型研发的视觉·语言大模型，提供出色的
	图文理解和创作能力，具有多项优势：
		1.为用户打造图文并茂的专属文章。
		2.设计了高效的训练策略，为模型注入海量的多模态概念和知识数据，赋予其强
		大的图文理解和对话能力。

6.实验部分通用配置

	1.pip、conda 换源
		1.pip 换源
		-临时使用镜像源安装，如下所示：some-package为你需要安装的包名
		pip install -i https://mirrors.cernet.edu.cn/pypi/web/simple 
		some-package
		-设置pip默认镜像源，升级 pip 到最新的版本 (>=10.0.0) 后进行配置，如下
		所示：
		python -m pip install --upgrade pip pip config set global.index-
		url https://mirrors.cernet.edu.cn/pypi/web/simple
		-如果您的 pip 默认源的网络连接较差，临时使用镜像源升级 pip：
		python -m pip install -i 
		https://mirrors.cernet.edu.cn/pypi/web/simple --upgrade pip
	2.conda 换源
		1.镜像站提供了 Anaconda 仓库与第三方源（conda-forge、msys2、pytorch 
		等），各系统都可以通过修改用户目录下的 .condarc文件来使用镜像站。
		不同系统下的 .condarc目录如下：
			- Linux: ${HOME}/.condarc
			- macOS: ${HOME}/.condarc
			- Windows: C:\Users\<YourUserName>\.condarc
		注意：
			- Windows用户无法直接创建名为 .condarc 的文件，可先执行 conda 
			- config --set show_channel_urls yes 生成该文件之后再修改。
		快速配置
			cat <<'EOF' > ~/.condarc
			channels:
			  - defaults
			show_channel_urls: true
			default_channels:
			  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
			  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
			  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
			custom_channels:
			   conda-forge: 
				https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
			   pytorch: 
			   https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
			EOF
			
7.模型下载

	1.Hugging Face
	-使用 Hugging Face 官方提供的huggingface-cli命令行工具。安装依赖:
	pip install -U huggingface_hub
	-然后新建 python 文件，填入以下代码，运行即可。
	    - resume-download：断点续下
		- local-dir：本地存储路径。（linux 环境下需要填写绝对路径）
	import os
	# 下载模型
	os.system('huggingface-cli download --resume-download 
	internlm/internlm-chat-7b --local-dir your_path')
	-以下内容将展示使用 `huggingface_hub` 下载模型中的部分文件
	import os 
	from huggingface_hub import hf_hub_download  # Load model directly 
	hf_hub_download(repo_id="internlm/internlm-7b", 
	filename="config.json")
	2.ModelScope
	-使用modelscope中的snapshot_download函数下载模型，第一个参数为模型名称，
	参数cache_dir为模型的下载路径。
	-注意：cache_dir最好为绝对路径。
	-安装依赖：
		pip install modelscope==1.9.5
		pip install transformers==4.35.2
	-在当前目录下新建 python 文件，填入以下代码，运行即可。
		import torch
		from modelscope import snapshot_download, AutoModel,
		 AutoTokenizer
		import os
		model_dir = snapshot_download('Shanghai_AI_Laboratory/internlm-
		chat-7b', cache_dir='your path', revision='master')
	3.OpenXLab
	-OpenXLab 可以通过指定模型仓库的地址，以及需要下载的文件的名称，文件所需下
	载的位置等，直接下载模型权重文件。
	-使用python脚本下载模型首先要安装依赖，安装代码如下：pip install -U 
	openxlab安装完成后使用 download 函数导入模型中心的模型。
	from openxlab.model import download
	download(model_repo='OpenLMLab/InternLM-7b', model_name='InternLM-
	7b', output='your local path')
	
8.课后作业

	1.基础作业：
		1.使用 InternLM-Chat-7B 模型生成 300 字的小故事（需截图）。
		
		![[3E0B3B8D-DE80-4351-A723-C10618305BD4.png]]
		![[Pasted image 20240212223511.png]]	
		2.熟悉 hugging face 下载功能，使用 huggingface_hub python 包，下载 InternLM-20B 的 config.json 文件到本地（需截图下载过程）。
		![[Pasted image 20240213001953.png]]
	2.进阶作业：
		1.完成浦语·灵笔的图文理解及创作部署（需截图）
		![[Pasted image 20240213001152.png]]
		
		2.完成 Lagent 工具调用 Demo 创作部署（需截图）
		
		![[Pasted image 20240212230110.png]]
	3.整体实训营项目：
		时间周期：即日起致课程结束
		即日开始可以在班级群中随机组队完成一个大作业项目，一些可提供的选题如
		下：
		- 人情世故大模型：一个帮助用户撰写新年祝福文案的人情事故大模型
		- 中小学数学大模型：一个拥有一定数学解题能力的大模型
		- 心理大模型：一个治愈的心理大模型
		- 工具调用类项目：结合 Lagent 构建数据集训练 InternLM 模型，支持对 
		 MMYOLO 等工具的调用其他基于书生·浦语工具链的小项目都在范围内，欢迎大
		家充分发挥想象力。