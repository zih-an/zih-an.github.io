正所谓“工欲善其事，必先利其器”。对于尚处于入门阶段还一知半解的AI初学者来说，系统学习的第一步当属认识用于AI计算的硬件设备：从显卡型号和价格，到服务器的整体装配，再到其相应的实际应用范围。只有摸清楚硬件的基础条件和成本，才能进行“高性价比”的AI计算，钱要烧得合情合理。
> 硬件基础：请先学习 如何组装一台电脑


# 01 GPU型号及参数
## Nvidia显卡产品线概述
- GeForce：RTX 3090，4090，5090 系列
- Data Center：V100，A100，H100，H200，B100
	+ 开头字母为架构，Volta、Ampere、Hopper、Blackwell（2025）
	+ 对标架构：20系列（Turing）、30系列（Ampere）、40系列（Ada Lovelace）、50系列（Blackwell）
	+ 其中，V100为划时代的卡。a800、H800、H20都为中国特供卡。

> [2024年一文看懂B100、H200、L40S、A100、A800、H100、H800、V100](https://maimai.cn/article/detail?fid=1823470372&efid=Z6ZOjuBlCqNGI-LDHVafKg)


## 显卡的基本参数
> 显存低：模型就放不下，即便多卡、gradient accumulation模拟大batch，也无法塞下一个完整的大模型

- 架构
- 核心数（一般也和架构捆绑了）
	+ cuda cores / tensor cores
- 显存
- 算力指标（FLOPS）：位数越多，精度越高、占用显存越大。
	+ [FP64, FP32, FP16, BFLOAT16, TF32, and other members of the ZOO](https://moocaholic.medium.com/fp64-fp32-fp16-bfloat16-tf32-and-other-members-of-the-zoo-a1ca7897d407)
	+ FP64（双精度double）：科学计算、工程仿真等超算领域
	+ **FP32（单精度float）**：传统深度学习训练、图形渲染等。
		- AI还是主要看这个。pytorch default dtype=torch.float32
	+ FP16（半精度half）： 深度学习训练/推理。大模型一般用这个
		- 快、省显存，可能会溢出。需要配合 Loss Scaling、混合精度训练方法
	+ FP8：最新推理、训练（LLaMA3等）
	+ 其他：bfloat16等等，许多库（e.g. flash attn）会针对硬件的算力进行优化，许多模型（e.g. qwen）也会开源多个算力的版本。
- nvlink


[nvidia型号及重要参数: ](https://www.bilibili.com/video/BV1hf421D7vG/?spm_id_from=333.337.search-card.all.click&vd_source=332380f37aa700dd26b5052355228b7b)

<img src="./nvidia-param.png" width="900px" /> 



## 型号及适用情景表（待补充...
- 训练（Training）：A100、H100/H200
- 推理（Inference）：5090、4090
- 微调（Fine-tuning）
- 预训练（Pretraining）
- 蒸馏（Distillation）




# 02 认识算力（从个人工作站到服务器集群）
事实上，尽管单张卡的能力越来越强，但是集体的智慧还是大于个体的能力。nvlink/pcie 使得多张卡能够联系在一起，组成更加强劲的 集群/个人工作站。cpu的高性能计算（High Performance Computing，HPC）是成熟和常见的应用场景。而事实上，对于GPU的高性能计算而言，无论是个人工作站还是集群都早在图形渲染相关领域就有广泛的使用（参考Disney动画制作使用工作站/集群）。在一般的3D软件如blender中，仅需勾选多卡设备即可自动使用多卡并行渲染。

**AI三要素：数据、算法、算力。** 对于算力，它并非只是表面上花钱买“几百张、几千张”卡这么简单，而是会涉及更深层次硬件“体系结构”层面的拓扑设计，以保证高效并行计算。通常，按卡数多少/规模大小，有工作站（workstation）、服务器/单节点（node）、集群（cluster）三种基本配置。


> 先看最新消息：
> - [Elon Musk/如何打造十万卡集群](https://www.youtube.com/watch?v=aey0qZzJg-Q)
> - [字节：在超过 10,000 张 GPU 的大规模集群上高效、稳定地训练大语言模型](https://huggingface.co/papers/2402.15627)




## 多卡配置
### 个人工作站（单卡、双卡、四卡）
个人工作站可放置在使用者身边，当作普通的个人电脑使用。
- [b站up: 技数犬](https://space.bilibili.com/434570236?spm_id_from=333.337.0.0)


### 单机多卡服务器（8卡，server/node）
常见的如8卡4090、3090服务器


### 多机多卡集群（cluster）
大规模集群的硬件“3D并行”策略：
- 数据并行（最简单）：模型直接拷贝，不同的数据forward单独计算，backward汇总即可。
	+ 但是整个模型可能无法完整存在一块GPU上
	+ 不同的计算岛
- 张量并行：所有GPU协同工作，一起处理一个模型，像是组成了一个巨大的GPU。设备间会频繁通信。
	+ 同一个server节点中使用（比如8卡）
- 流水线并行：forward的每一步都看作流水线，交给不同的GPU，一步一步。
	+ 同一个计算岛内的多个server节点之间使用




## 算力参考（未完待续...）
- clay：
	+ 最小的模型：A single node - 8卡A800
	+ 最大的模型：cluster - 256 A800, for approximately 15 days, with progressive training
- meshanything v2:
	+ 32张a800
- get3D：
	+ 8张a100 2天
- pivotmesh：
	+ 8×A100-80GB 3天
- meshdiffusion
	+ 8 A100-80GB GPUs
- meshgpt：
	+ 4 * a100
- OctFusion/OctGpt（peng-shuai）
	+ 4 * 4090




# 03 GPU的使用及费用（租赁/购买）
> 基本使用略：e.g. linux命令、ssh、ftp等等。基本的连接软件，MobaXterm、Termius、vscode、pycharm等等。


租赁平台：
- AutoDL（最多租8卡，一般还租不到）
- Vast.ai（通常最多租8卡）
- aws 



# Conclusions
> 计算资源管理、训练稳定性

简而言之，对硬件设备的基本了解，对我们使用者（算法设计者）来说相当重要。作为使用者，与我们最相关的是 **计算精度 + 并行方式** 两个方面的控制。相应的对于pytorch，计算精度的管理常通过混合精度训练（AMP）和梯度缩放（Scaler）实现；并行方式则涵盖单机多卡和多机多卡的训练。当显存受限时，梯度累积（Gradient Accumulation）能有效模拟大批量训练。

为了防止训练过程中梯度爆炸，保证训练稳定，梯度裁剪（Gradient Clipping）是一个简单且有效的技巧，通常会配合混合精度训练、梯度累积等技巧一起用。

作为入门第一节，认识算力只是为了更好的驾驭算力。之后的几节，将会对“驾驭”算力进行展开。
另外，对于其他的一些和硬件无关的trick也将在之后的章节中穿插讲解，例如：
- warmup和Learning Rate Scheduling的配合
- 防止过拟合的dropout、data augmentation、early stop、正则化loss项
- checkpoint的存储和读取，以及断点继续训练

[链接：关于trick的blog](https://baidinghub.github.io/2020/04/03/AI%E5%B0%8F%E7%9F%A5%E8%AF%86%E7%B3%BB%E5%88%97(%E4%BA%8C)%20%E8%AE%AD%E7%BB%83%E8%BF%87%E7%A8%8BTrick%E5%90%88%E9%9B%86/#1-%E5%86%99%E4%BB%A3%E7%A0%81%E4%B9%8B%E5%89%8D%E8%A6%81%E5%81%9A%E7%9A%84%E4%BA%8B%E6%83%85)

