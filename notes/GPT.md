## [《Improving Language Understanding by Generative Pre-Training》](https://s3-us-west-2.amazonaws.com/openai-assets/research-covers/language-unsupervised/language_understanding_paper.pdf)

- 目标：用与训练的LM模型提升NLU任务的效果
- 基于大量未标注语料训练的两个问题
    - 如何设置合理的训练目标？LM/NMT/discourse coherence? 这也是GPT和bert的区别之一
    - 如何将预训练的模型得到的表征应用到下游任务中去？
- 模型结构
    - 一个没有encoder-attention的transformer decoder
    - 给定一个窗口的输入(最后n个token)，预测下一个单词
    - 模型返回的是最后一个token对应的embedding，而不是真个窗口sequence的embedding
- fine-tune
    - 将结构化问题的输入变成一个sequence。不同的field的输入在sequence里面用delimiter区分开来
    - 句子的最后加上[Extract] token。 用来训练成为整个句子的embedding。 这点和bert的[CLS]类似
    - decoder是只能attention到前面的token的，所以输入句子的顺序很有影响（对比bert，用的是LML，不收影响）因此做两个句子的相似度检测，需要交换两个句子的顺序，分别输入模型
    - 可以把LM和目标任务联合训练。 将两者的loss加权求和
- 实验
    - 随着transformer的层数增加，模型fine-tune之后的效果越来越好，每层都能捕捉到新的信息
    - 做zero-shot预测，随着pre-train的step变多，效果越来越好。说明预训练是能捕捉到一些通用的语义的东西的
    - fine-tune的时候加上LM损失在大数据集有帮助，小数据集上表现不如不加
- GPT 和BERT区别
    - GPT是LM预训练，BERT是MLM+NSP预训练
    - GPT的self attention只能attention到前面的token， bert是双向的
    - GPT做下游任务fine-tune的时候，多个输入的顺序是有影响的。比如text entailment任务，premise要在hypothiesis之前,bert没这要求（原因见上一条）
    - GPT可以直接做生成任务，而BERT不能做生成任务
