# deep voice2 学习

## 模型结构

deep voice2有两种结构,分别是单speaker和多speaker的模型

### 单speaker deep voice 2
**整体结构**

单speaker deep voice 2 保留了deep voice 1的主体结构,其推理部分结构如下:

![](https://github.com/sysu16340234/deep_voice2/blob/master/img/fig1.png)

**推理过程**大致如下:文本通过发音字典转换为音素,然后将音素输入音素持续时间模块进行持续时间预测,得到持续时间后和音素一起进行上采样,得到采样结果输入频率预测模块得到预测基频,与deep voice1不同的是,deep voice 2的持续时间和频率是分开预测而不是进行联合预测,最后将预测的基频和经过上采样的音素输入语音模块生成语音音频;

**训练过程**的图例如下:

![](https://github.com/sysu16340234/deep_voice2/blob/master/img/trainng.png)
