## [《KG-BERT: BERT for Knowledge Graph Completion》](https://arxiv.org/pdf/1909.03193.pdf)
- 目标：做KG-completion
- 思路：基于预训练的BERT，做SPO三元组的embedding， 不依赖原句
- 训练
    - 将SPO三元组拼接成[CLS]S[SEP]P[SEP]O[SEP]的形式， 其中S和O用同样的segment embedding
    - S和O是entity的name或者description
    - 用二分类判断三元组是否正确， 或者用多分类，给定S\O 判断relation的类型
- 预测
    - 给定SPO，判断是否正确
    - 给定SO，判断relation在schema中的哪一个
    - 给定 SP，判断O是哪个（将所有可能的O列举，拼接成三元组之后预测），按照得分排序取第一个
- 启发：
    - 不依赖上下文，只依赖三元组的文字表达，利用BERT的大量预训练，做分类任务。听起来不太靠谱但是可以尝试
    - 对于训练预料的采集要求降低，只需要知道三元组的列表，不需要关联到相应的文本中去
