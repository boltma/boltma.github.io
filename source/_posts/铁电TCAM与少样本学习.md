---
title: 铁电TCAM与少样本学习
date: 2019-12-09 22:30:36
tags:
---

本文是Nature Electronics上倪凯*Ferroelectric ternary content-addressable
memory for one-shot learning*[^1]一文的学习总结与分析，同时也补充了一些技术最近的动向与分析。

作为一篇Nature子刊的文章，这篇工作有两大非常重要的主要工作：阐述了将TCAM应用于少样本学习的方法，以及提出一种每单元两个铁电晶体管的TCAM结构。这篇文章除提出了可行性外，也留下了很多可以后续跟进的未解决的细节，非常值得深入学习。

可以参见Nature Electronics上的一篇评论[^2]简要了解这项工作。

# 关键技术

## TCAM

**三态内容寻址存储器（TCAM, ternary content-addressable memory）**是由**CAM**发展而来的一种存储器。它是一种很有前景的新型存储器，对于CAM，它除了有传统内存的功能（向指定地址写入数据，或从指定地址读出数据）外，还支持一种特殊的功能：输入待查数据，返回该数据被存储的地址。

这种功能非常强大，因为传统上要从内存中查找一项数据，不但需要一个好的查找算法，而且还会浪费大量时间将数据从内存搬运至CPU。而如果我们在电路层面支持在内存中查找数据，直接返回地址给CPU将非常有利。CAM与TCAM的区别在于：CAM只支持精确匹配，而TCAM则在比较数据时支持第三态（don't care），即对每一个比特，有0（mismatch）、1（match）和x（don't care）三态，因而支持模糊匹配。

## FeFET

**铁电晶体管（FeFET，ferroelectric field-effect transistor）**，重要的新型存储器件，不必多做介绍。基本原理在于：在传统MOSFET栅极的电容中加入一层铁电材料（如HfO~2~），使得可以通过铁电材料的正负极化存储数据。正极化时，晶体管阈值电压较低；负极化时，晶体管阈值电压较高。铁电晶体管的优点在于：
- 非易失存储；
- 无直流写功耗；
- 兼容现有的先进工艺；
- 可利用电流电压特性曲线中滞回特性的负电容效应进一步进行低功耗电路设计；
- 兼具非易失存储与晶体管的控制特性，电路设计灵活。

铁电晶体管的主要缺点是寿命较短，现有的HfO~2~铁电晶体管寿命约为10^5^周期[^3]，这主要是受铁电绝缘层的**电荷捕捉效应（charge trapping effect）**限制[^4]。

铁电晶体管的最近动向包括：

- 通过抑制电荷捕捉效应提高寿命[^5]，在参考文献的工作中可以达到10^12^周期；
- 利用电荷捕捉效应存储数据，实现铁电晶体管的另一种存储。这种存储是易失的，但寿命较长，相当于一种动态内存。可以利用两种存储的联合进行一些应用优化；
- 多层不同铁电材料的堆叠，每层存储一个比特，材料与厚度不同，通过设置参数以实现较好的**多层单元（MLC，multi-layer cell）**存储；
- 铁电二极管（Fe Diode）的应用。

## LSH

**局部敏感哈希（LSH，locality-sensitive hashing）**，是这篇工作中将神经网络与TCAM结合的关键技术。与传统哈希不同，局部敏感哈希希望数据相似时，哈希值相似；数据相距较远时，哈希值也相距较远。

# 主要工作

## 记忆增强神经网络

使用**记忆增强神经网络（MANN，memory augmented neural networks）**进行**元学习（meta-learning）**并不是什么新鲜的事。以分类问题为例，少样本的学习一直是很困难的问题。使用MANN我们可以不必在遇到新数据时重新训练学习参数，而是只需回写一个键值[^6]。对于MANN的具体理论我们不做分析，这里我们更关注如何利用TCAM提升它的效率。我们所针对的是MANN中的这样一个过程：将存储的参数从存储单元（内存或显存）搬运到计算单元（CPU或GPU），逐条比对距离后，找到最接近的类别。这个操作极其耗时，但利用TCAM我们可以大幅减低这个耗时，距离的比对将在存储单元中直接完成，并由存储单元返回类别。

![](铁电TCAM与少样本学习\1.webp)

上图显示了在TCAM上进行MANN操作的具体方式。以N-Way K-Shot分类问题（N个类别，每个类别K个样本，K很小）为例，我们从训练集中提取特征，然后存在内存中。对于查询请求，我们也提取其特征，与存储的特征比较余弦距离，然后得出分类，并将其特征以一定方式写回内存。这里具体的特征提取、写回内存的特征均不是我们关心的。传统方式下，如果我们有M个数据项要比对，每个是D维的，那么需要进行MD次乘法和M(D-1)次加法。

使用TCAM进行特征的比较时，我们最主要的问题在于，TCAM仅能进行二进制编码的匹配，而特征提取实际上是应该进行余弦距离比较的。为此，我们将特征提取的最后一层网络（通常应是softmax层）替换为LSH层，从而我们能转换为一个二值签名，只需比较汉明距离即可。在测评中可以看到，只要LSH算法较好时，网络的准确度不会下降太多。LSH层数是一个超参数，可以进行调参使得准确率不随之提高。

另外LSH可以使用精确的，从而TCAM作为CAM使用；LSH也可以是模糊的**（ternary LSH）**，从而利用TCAM的第三态特性。

使用TCAM，效率是很高的，也节省了很多功耗。这一点毋庸置疑，具体分析可以详见文献。而且准确率的下降在可接受范围内。文献中指出，在Omniglot数据集上，一次搜索能量降低到1/60，延时降低到1/2700。

![](铁电TCAM与少样本学习\2.webp)

## 2FeFET/Cell TCAM

![](铁电TCAM与少样本学习\3.png)

这个TCAM单元结构如图。两个FeFET分别存储互补的数据，即存储逻辑1时，左管负极化（高阈值电压），右管正极化（低阈值电压）； 存储逻辑0时，左管正极化，右管负极化。查找时，先给ML（match line）预充电到高电平，放开，然后通过SL（search line）给两个铁电管栅极施加互补的电压：查找逻辑1，左管高电平，右管低电平；查找逻辑0，左管低电平，右管高电平。如果失配，则正极化的铁电管SL上是高电平，则打开，ML接地将ML电放去。如果匹配，则正极化铁电管SL是低电平，截止，负极化铁电管也截止，那么ML维持高电平。对于x状态（don't care）有两种方式实现：可通过使两个铁电管均负极化存储x；或查找时两根SL均通低电平，两种方式下ML均维持高电平。

具体对一个阵列而言，判定匹配程度是通过ML放电速度判断的。这个放电过程是简单的寄生电容一阶放电过程，符合指数衰减1-exp(-t/τ)，其中τ与失配单元数量成正比，如下图所示（这也很容易理解）。图示为6个TCAM单元的情形，实际上补充材料显示了更多数量并联的可能性（可以继续设计外围电路，这并非重点）。我们需要注意的是：我们并不需要区分具体有多少个单元失配，我们可能只需了解哪些行是最匹配的。因而放电速度最快的几行可以直接忽略，可以等待一定时间再查看电平（这个时间损耗很小）。

![](铁电TCAM与少样本学习\4.webp)

与传统的其它TCAM结构比较，这个结构具有相当多的优势。与CMOS实现的TCAM相比，CMOS需要16个晶体管，非常浪费空间，而且不具有非易失特性。与阻变器件实现的TCAM相比，铁电晶体管不必有直流电流流经阻性器件，大大节约功耗，同时阻变器件要在集成电路的**后道工序（BEOL，Back end of line）**加工，使得寄生效应明显，更加剧了漏电，并且阻变器件的高阻和低阻之比较小，从而可探测范围较小 ，需要更精细的外围放大感测电路。下表显示了现有的TCAM单元的实现方式的比较。

![](铁电TCAM与少样本学习\5.png)

# 后续跟进

我认为值得跟进的点有以下内容：

- TCAM外围感知电路的设计
- 电路层更大规模TCAM阵列的可行性（注意写干扰问题）
- 其它AI应用上的使用前景（本文只是demo）

参考文献中值得注意的TCAM相关，后续需要阅读的有：

- Karam, Robert, et al. "Emerging trends in design and applications of memory-based computing and content-addressable memories." *Proceedings of the IEEE* 103.8 (2015): 1311-1330.、
- Laguna, Ann Franchesca, Michael Niemier, and X. Sharon Hu. "Design of hardware-friendly memory enhanced neural networks." *2019 Design, Automation & Test in Europe Conference & Exhibition (DATE)*. IEEE, 2019.
- Ni, Kai, et al. "Write disturb in ferroelectric FETs and its implication for 1T-FeFET and memory arrays." *IEEE Electron Device Letters* 39.11 (2018): 1656-1659.

总而言之，这篇文章信息量实在太大，值得日后再次仔细阅读。

# 参考文献

[^1]: Ni, Kai, et al. "Ferroelectric ternary content-addressable memory for one-shot learning." *Nature Electronics* 2.11 (2019): 521-529.
[^2]: Huang, Peng, Runze Han, and Jinfeng Kang. "AI learns how to learn with TCAMs." *Nature Electronics* 2.11 (2019): 493-494.
[^3]: Müller, J., et al. "Ferroelectricity in HfO 2 enables nonvolatile data storage in 28 nm HKMG." *2012 Symposium on VLSI Technology (VLSIT)*. IEEE, 2012.
[^4]: Yurchuk, Ekaterina, et al. "Charge-trapping phenomena in HfO 2-based FeFET-type nonvolatile memories." *IEEE Transactions on Electron Devices* 63.9 (2016): 3501-3507.
[^5]: Ni, Kai, et al. "Critical role of interlayer in Hf 0.5 Zr 0.5 O 2 ferroelectric FET nonvolatile memory performance." *IEEE Transactions on Electron Devices* 65.6 (2018): 2461-2469.
[^6]: Santoro, Adam, et al. "Meta-learning with memory-augmented neural networks." *International conference on machine learning*. 2016.