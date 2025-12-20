“工欲善其事，必先利其器”。在简单了解“算力”硬件设备之后，在进入真正的开发之前，依然还需要先了解许多业内常用的社区平台和工具。


# 01 模型/数据集/代码共享平台


## Huggingface
大模型开源社区

### Model
以 [Qwen3](https://huggingface.co/collections/Qwen/qwen3-67dd247413f0e2e4f653967f) 为例，一般存在多个版本：
1. 不同参数量
	- 235B-A22B，32B-A3B，30B，14B，8B，4B，1.7B，0.6B
2. 不同算力精度
	- 默认FP16，还有FP8、int4等等版本，详见模型网页。
3. 是否微调（或者叫post-training）
	- Base：只有stage1，被喂了大量的语料库，但是不具备任何的问答等能力
	- Instruct：可以进行问答，经过了第二阶段的post-training（参考GPT）
	- Thinking：具有推理和思考能力

可以使用huggingface的 `pip install transformers` 进行调用，也可以在space进行尝试。配合langchain，可以与其他的大模型串联成大的ai app。同时，对开源了代码+权重的模型，都可以用LoRA或者QLoRA方法进行微调训练。


### Space
提供了直接运行模型的环境，都是用 gradio 快速搭建的ui，能直接展示/预览模型的功能和效果。许多大模型都有页面的部署。



### Dataset
顾名思义。挺多的，shapenet、objaverse都在上面。一个有意思的数据集叫 弱智吧。


## GitHub / GitLab
开源代码。

## Google drive / Dropbox + pickle
依然有不少人选择将数据集和checkpoints放在网盘自行下载。


# 02 资源监控类
## wandb / tensorboard

## gpustat / nvidia-smi

## htop / top




# 03 命令行与远程开发工具
## tmux / screen 

## MobaXterm / Termius



# 04 开发环境

## conda

## docker 



