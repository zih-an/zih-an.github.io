> AI=算力+算法+数据。在对算力和平台有基本的感性认识之后，下一步最需要了解的就是算法——使用算力的方法。在如此大规模算力的今天，如何“榨干”这些资源，其实也成为了一个难题。本自学系列，对于算法部分即按照算力资源进行划分：
> 1. 使用单GPU。以Andrej Karpathy的材料为准，配套使用google的tuning playbook，目的是掌握AI基础，熟悉基本的算力资源运用。
> 2. 使用多GPU。分布式计算，从最实用的DDP和cuda实践技能，到理论的各种数据并行、模型并行、流水线并行知识。如何利用大规模集群进行算力，可参考字节的信息。
> 对于数据部分，应该涉及数量、质量、使用手段，将放在后面进行掌握。主要是我也还不懂。


最近发现了前Tesla总监、前openai成员、李飞飞的高徒、我全新的AI学习对象 —— **Andrej Karpathy** 。他亲自准备了很多教学用代码，个人认为是非常好的零基础学习材料：
- [Neural Networks: Zero to Hero](https://karpathy.ai/zero-to-hero.html)
- [Andrej Karpathy youtube](https://www.youtube.com/@AndrejKarpathy)
- [nanoGPT](https://github.com/karpathy/nanoGPT/tree/master)
- 同时，openai 开源的[GPT2](https://github.com/openai/gpt-2)也是很好的入门学习材料


此外，对于LLM入门，还必须强烈推荐：
- GPT相关原文
	+ 2017 Transformer- Attention is all you need
		- 包含encoder + decoder 两部分
	+ 2018 GPT1- Improving Language Understanding by Generative Pre-Training
		- decoder only；pretrain + finetuning（transfer learning）
	+ 2019 BERT- BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding
		- encoder only + 大参数 scaling law；pretrain + finetuning
	+ GPT2- Language Models are Unsupervised Multitask Learners
		- decoder only + 大参数 scaling law；不再fintuning而使用prompt
	+ GPT3- Language Models are Few-Shot Learners
		- decoder only + 大参数！！！！大力出奇迹。
	+ 之后的GPT：再次引入fintuning，使用RLHF
- up主视频：
	+ [李沐的GPT系列论文精读](https://www.bilibili.com/video/BV1AF411b7xQ/?spm_id_from=333.337.search-card.all.click&vd_source=332380f37aa700dd26b5052355228b7b)
	+ [马少平-清华教授（非常中式教育，但是讲得很好）](https://space.bilibili.com/1000083494?spm_id_from=333.1387.follow.user_card.click)




# Neural Networks: Zero to Hero











