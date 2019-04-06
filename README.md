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

可以看出训练过程大体上与deep voice 1很相似,但是加入了说话者的特征作为训练样本,文本经过发音字典或者字素到音素模型传唤为音素,语音通过频率提取得到基频,音素和音频在经过分段模块后得到分段后的语音,频率模块用音频,音素,频率特征和说话者特征来训练,持续时间模型用音素,分段语音和说话者特征来训练;语音模型用音频,音素,分段语音和说话者特征来训练;

**模型细节**

**1.分段模型**

deep voice 2的分段模型使用的依然是具有CTC loss的卷积-循环结构,但是在卷积层中增加了批量标准化和残差连接;

deep voice 1中的每层输出如下:

![](https://github.com/sysu16340234/deep_voice2/blob/master/img/1.png)

其中h(l)是第l层的输出,W(l)是卷积滤波器,b(l)是偏置向量

而deep voice 2的每层输出如下:

![](https://github.com/sysu16340234/deep_voice2/blob/master/img/2.png)

其中BN为批量标准化;

为了防止在解码静音时发生边界错误,在解码静音时采用启发式算法调整边界位置;
