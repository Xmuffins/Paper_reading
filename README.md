# Paper_reading
这是一个论文阅读记录文库，主要做一些有意思的工作的个人阅读记录
评价为：得读论文啊

## Knowledge editing and explainable LLM generation
实际上这里得看下可控生成的论文
有作者管对transformer搞内部激活值变化对输出的分析，以理解transformer模型的推理或知识推断原理的这个方向叫Representation Engineering，应该不是新概念，蛮有趣的

1. Head-to-Tail: How Knowledgeable are Large Language Models (LLM)? A.K.A. Will LLMs Replace Knowledge Graphs?
文章用实验回答了三个问题
①大模型对事实性任务的回答效果如何？
    开源大模型普遍正确率和幻觉率在1：5以上，其余均为“不知道”
②大模型对常见事实和罕见事实的问题回答效果如何？
    罕见事实的回答效果更差一些
③Fine-tuning等常见模型调整策略是否有效帮助大模型提升事实性回答正确率？
    不能，fine-tuning倾向于提升模型回答“不知道”的频率，略降低模型回答正确率，幻觉率仍然很高
2. Locating and Editing Factual Associations in GPT
Neurips2022的文章，针对大模型出现幻觉的推理场景，记录模型中间各层激活值，对输入序列加噪声后用记录尝试恢复不同位置（层、序列位置、模块）的激活值，观察幻觉发生情况
   作者根据实验认为，attention倾向于序列信息复制融合，MLP是实际进行实体关系推断的模块
3. Representation Engineering: A Top-Down Approach to AI Transparency
设计多个prompt然后针对感兴趣的概念所在token位置抽取隐层表征，然后做PCA取主方向做所谓reading vector
留着细读，主要是针对transformer架构产生的幻觉可以进行span定位感觉很叼，给的结果示意图很让人惊讶

## Knowledge graph and LLMs
现有的KG配合LLM还是以prompt辅助查询居多，整体看还是LLM是一个模块，KG是一个模块。LLM与KG交互，生成结构化检索语句调取知识，然后基于拿到的知识三元组集合做推理。
实际上大模型实际上做没做逻辑推理还不太明晰，文本生成肯定是做了。

1. Chain-of-Knowledge: Grounding Large Language Models via Dynamic Knowledge Adapting over Heterogeneous Sources
生成Query语句从数据库检索答案，对prompt中的错误进行更改
3. MindMap: Knowledge Graph Prompting Sparks Graph of Thoughts in Large Language Models
先用大模型处理输入，找出具体要分析的实体串。
利用实体，生成SPARKQL查找语句，查找搜索两个东西：第一个是每个实体相关的子图，第二个是能串联所有实体的路径图。
用这俩拼出一个局部知识图，让大模型看这些三元组然后推断
4. Think-on-Graph: Deep and Responsible Reasoning of Large Language Model on Knowledge Graph
TOG
从某实体出发，每探索一次所有的关联边和实体之后进行一次剪枝和判断，如果没有能够回答问题的实体内容，则用LLM判断保留topK的路径。
如果LLM判断为能回答，则直接回答问题
TOG-R
每次不进行固定的剪枝，而是随机剪枝（不让LLM来负责剪枝）
