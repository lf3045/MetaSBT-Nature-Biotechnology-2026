# Genome Biology：15.6万份原核基因组横评四大注释工具——高质量细菌优先Bakta，MAG和古菌更适合PGAP

做细菌或古菌基因组分析，组装完成以后几乎都会遇到同一个问题：

> **到底用 Prokka、Bakta、EggNOG-mapper，还是 NCBI PGAP？**

这些工具都很常见，但它们解决的问题并不完全一样。有人更擅长找基因，有人更擅长给基因“起名字”，有人对碎片化基因组更稳，还有的工具计算资源很省，却因为数据库更新较慢而牺牲了注释深度。

2026年9月，Jundzill等人在 *Genome Biology* 发表了一项大规模benchmark，系统比较 Prokka、Bakta、EggNOG-mapper 和 PGAP。作者总共处理 **156,033个基因组数据集**，覆盖高质量 *E. coli*、近3万种细菌和古菌代表基因组、3,137个细菌MAG，以及98,742个加入不同程度移码错误的模拟基因组。

最后得到的结论很实用：

- **高质量细菌分离株**：如果重点是尽可能多地获得有名称、有描述的蛋白，Bakta表现最好；
- **古菌、MAG、碎片化或错误较多的基因组**：PGAP整体更稳；
- **想让更多基因至少获得一个GO功能标签**：跨物种数据中PGAP覆盖更广；
- **想让已经注释到的基因获得更丰富的GO术语**：EggNOG-mapper更强；
- **机器和硬盘资源非常有限**：Prokka仍然轻量，但论文并不建议把它继续当作默认的全功能注释方案。

这篇文章更重要的价值，是把“哪个软件最好”改成了一个更具体的问题：

> **你的基因组是什么类型？质量怎么样？最后到底想得到什么信息？**

---

## 一、原核基因组“注释”其实包含两件不同的事

很多初学者会把注释理解成一个步骤：输入FASTA，得到一个GFF文件。

实际至少可以拆成两层。

第一层是**结构注释**：

- 哪一段是蛋白编码基因；
- 起始和终止位置在哪里；
- 有没有rRNA、tRNA、tmRNA；
- 是否存在CRISPR、ncRNA等其他元件。

第二层是**功能注释**：

- 这个蛋白叫什么；
- 属于哪个蛋白家族；
- 有没有KEGG、COG、PFAM或GO信息；
- 是否能和UniProt、RefSeq等数据库中的已知蛋白对应。

一个工具可能很会“找到基因”，但不一定能给这些基因提供丰富功能；另一个工具可能擅长功能转移，却并不负责完整的RNA和结构特征预测。

因此，直接拿“预测了多少个gene”评价软件，很容易得出错误结论。

---

## 二、作者一次性跑了156,033个基因组数据集

这次benchmark的规模远大于常见的几十个模式菌比较。

主要数据可以分成四部分：

| 数据类型 | 数量 | 用途 |
|---|---:|---|
| *E. coli* 基因组 | 24,393 | 建立低分类差异、研究充分的基线 |
| 细菌和古菌代表基因组 | 29,761 | 测试跨物种、跨分类群表现 |
| 细菌MAG | 3,137 | 测试宏基因组组装基因组 |
| 人工加入移码的基因组 | 98,742 | 压力测试不同工具对序列错误的稳定性 |

代表基因组主要来自 **GTDB release 207.0**，组装序列从NCBI获取。作者还专门选择了大量不同细菌和古菌门，避免benchmark只围绕少数模式菌展开。

![大规模benchmark整体设计](https://raw.githubusercontent.com/lf3045/MetaSBT-Nature-Biotechnology-2026/main/articles/GenomeBiology-Prokaryotic-Annotation-Benchmark-2026/images/fig01_benchmark_overview.png)

*图1｜研究整体设计。GTDB代表基因组、E. coli、MAG以及加入0.5%、1%和2%随机缺失的模拟基因组分别进入Prokka、Bakta、EggNOG-mapper和PGAP。来源：Jundzill et al., Genome Biology 2026，CC BY 4.0。*

---

## 三、为什么作者没有简单比较“预测了多少个基因”？

不同软件处理断裂基因、重叠基因和假基因的方式不同。

例如同一段DNA：

- 软件A可能认为它是一个完整基因；
- 软件B可能把它拆成两个片段；
- 软件C可能完全不报。

如果只比较gene count，拆得越碎的软件反而可能看起来“预测得更多”。

所以作者重点使用了**coding density，编码区域占整个基因组的比例**。

同时又把编码区域进一步分成：

- **described coding density**：已经有明确功能描述的区域；
- **undescribed coding density**：主要是 hypothetical protein 或没有功能名称的区域；
- **non-coding density**：没有检测到特征的区域。

这个指标仍然不能直接代表“正确率”，但比单纯比较基因数量更不容易被基因拆分方式干扰。

对于GO功能注释，作者另外用了两个概念：

**GO coverage**：有多少基因至少获得了一个GO术语。

**GO richness**：一个已经被注释的基因平均能获得多少个GO术语。

这两个指标回答的是不同问题。

---

## 四、先看24,393个E. coli：Bakta给出的“已知功能区域”最多

在研究最充分的 *E. coli* 中，四个工具已经出现明显差异。

PGAP得到的**总编码区域**最大，平均约4.71 Mb；Bakta和EggNOG-mapper约4.50 Mb，Prokka约4.46 Mb。

但如果只看已经拥有明确功能描述的区域，Bakta最高：

- Bakta：约 **4.49 Mb**；
- PGAP：约 **4.43 Mb**；
- EggNOG-mapper：约 **4.35 Mb**；
- Prokka：约 **3.72 Mb**。

差距最明显的是“没有描述”的部分。

Bakta平均只有约 **12 kb** 未描述编码区域；Prokka达到约 **741 kb**。

这也是为什么作者把Bakta作为高质量细菌分离株的主要推荐之一。

![E. coli编码区域和基因长度比较](https://raw.githubusercontent.com/lf3045/MetaSBT-Nature-Biotechnology-2026/main/articles/GenomeBiology-Prokaryotic-Annotation-Benchmark-2026/images/fig02_ecoli_coding_density.png)

*图2｜24,393个E. coli基因组中四个工具的编码区域和feature长度分布。黄色为有描述区域，红色为未描述区域。来源：原论文，CC BY 4.0。*

---

## 五、Prokka的问题很大一部分来自数据库年代

Prokka仍然很流行，原因很简单：

- 安装容易；
- 运行快；
- 输出格式熟悉；
- 数据库只有约 **0.6 GB**。

但论文指出，它所捆绑的数据库明显老于其他工具，核心数据库更新停留在较早时期。

这会产生一个直接后果：

> 同一个真实存在的蛋白，数据库里没有足够新的同源信息时，就更容易被写成 hypothetical protein。

所以Prokka的结果不一定是“找不到基因”，很多时候是**找到以后无法给出足够好的描述**。

对于只需要快速粗略结构注释的小项目，它仍然有价值；如果后面要做深入的功能解释，单独依赖Prokka就越来越吃亏。

---

## 六、rRNA一碎，所有工具都会变差，但PGAP更稳

完整的 *E. coli* 理论上有22个rRNA基因。

在contig数量少于10的基因组中，Prokka、Bakta和PGAP都能平均找到接近22个rRNA。

但当组装变得碎片化以后，结果明显下降。

在contig数超过10的基因组中：

- PGAP平均找到约 **12.96** 个rRNA；
- Prokka约 **8.99**；
- Bakta约 **8.17**。

这说明rRNA统计不仅受生物学影响，也很容易受到组装连续性的影响。

如果你的项目要比较“某些菌有没有完整rRNA operon”，应该先看assembly质量，不能把注释软件输出的rRNA数直接当成生物学差异。

---

## 七、GO结果最容易被误读：覆盖率和丰富度不是一回事

在 *E. coli* 这个研究非常充分的物种中，EggNOG-mapper表现很漂亮。

GO coverage平均约：

- EggNOG-mapper：**66.07%**；
- Bakta：**63.88%**；
- PGAP：**36.12%**。

而GO richness差距更大：

- EggNOG-mapper：平均约 **37个GO term/基因**；
- Bakta：约4个；
- PGAP：约2个。

这看起来像EggNOG-mapper全面获胜。

但换成上万种不同细菌以后，结论发生了很大变化。

因此，功能注释工具在模式菌上的成绩，不一定能代表它在环境新菌或远缘物种中的表现。

---

## 八、扩展到26,970种细菌：Bakta在“有名称的编码区域”上优势非常明显

在26,970个细菌代表基因组中，作者逐个比较哪个工具拥有最大的 described coding density。

结果：

- Bakta在 **23,121个** 物种中排名第一；
- PGAP：3,251个；
- EggNOG-mapper：596个；
- Prokka：只有2个。

如果研究目标是给一个质量较好的细菌分离株尽量补齐蛋白名称和功能描述，这个结果对Bakta非常有利。

总编码区域则没有这么统一：PGAP在9,870个细菌物种中领先，Bakta在9,852个中领先，两者几乎打平。

所以更准确的说法是：

> **Bakta更擅长把细菌编码区域变成“有描述的注释”，PGAP则经常检测到更多总编码区域。**

![跨细菌和古菌的工具表现](https://raw.githubusercontent.com/lf3045/MetaSBT-Nature-Biotechnology-2026/main/articles/GenomeBiology-Prokaryotic-Annotation-Benchmark-2026/images/fig03_taxonomy_performance.png)

*图3｜26,970个细菌和2,791个古菌基因组中的跨分类群表现。图中比较总编码区域、有描述区域、RNA特征和GO覆盖。来源：原论文，CC BY 4.0。*

---

## 九、古菌的答案更明确：PGAP优势最大

作者分析了 **2,791个古菌基因组**。

在总编码区域上：

- PGAP在1,858个基因组中排名第一；
- EggNOG-mapper为847个；
- Bakta为72个；
- Prokka为8个。

如果看 described coding density，差距更明显：

- PGAP：**2,549个**；
- EggNOG-mapper：242个；
- Bakta和Prokka没有领先案例。

因此论文给出的古菌推荐非常直接：

> **优先PGAP。**

不过这里需要补一个重要背景：**Bakta官方并不把古菌作为正式支持对象。**

所以这部分结果不能简单解读成“Bakta算法不行”，更准确地说，它本来就不是为这个使用场景设计的。

---

## 十、最有意思的反转：跨物种以后，PGAP的GO覆盖率反而最高

在大规模细菌数据中，GO coverage变成：

- PGAP：平均 **31.20%**；
- EggNOG-mapper：11.44%；
- Bakta：2.40%。

古菌中：

- PGAP：**20.03%**；
- EggNOG-mapper：5.80%；
- Bakta：0.13%。

也就是说，当物种从 *E. coli* 扩展到大量远缘细菌和古菌后，PGAP能让更多基因至少得到一个GO标签。

但EggNOG-mapper仍然保持极高的GO richness：

- 细菌约35.4个GO term/基因；
- 古菌约38.7个。

所以这两个工具更像在回答不同问题。

如果你更关心：

> “尽量多的基因有没有一个可用的功能标签？”

PGAP更有优势。

如果你更关心：

> “已经匹配成功的基因能不能拿到更丰富的功能术语？”

EggNOG-mapper更合适。

很多项目完全可以把二者组合使用。

---

## 十一、但GO比较并不是完全公平的“同规则比赛”

这篇论文自己指出一个很关键的方法学限制。

EggNOG-mapper按照作者推荐的默认设置运行时，只输出经过筛选的 **non-electronic GO terms**。

这会主动压低GO coverage。

PGAP和Bakta并没有完全受到同样的限制。

因此这里的结果更应该理解为：

> **在论文所采用的默认工作流条件下，各工具最终能给用户什么。**

它并不能严格证明某个工具的底层功能数据库在所有设置下都比另一个工具更完整。

这也是benchmark论文最容易忽略的一点：

**参数本身就是工具的一部分。**

---

## 十二、3,137个MAG里，PGAP明显更稳定

作者又单独分析了 **3,137个细菌MAG**。

这些MAG平均完整度约 **85.22%**，平均污染约 **1.67%**。

和高质量 *E. coli* 相比，所有工具都出现了从“有描述区域”向“未知区域”的明显转移。

其中Bakta下降尤其明显，而PGAP在MAG中反而表现出最稳定的结果：

- 平均 described coding density最高；
- undescribed coding density最低；
- rRNA检测数量最高；
- GO coverage约 **31.54%**，明显高于EggNOG-mapper的6.95%和Bakta的0.18%。

这让PGAP成为论文对MAG的主要推荐。

![MAG和移码错误对注释的影响](https://raw.githubusercontent.com/lf3045/MetaSBT-Nature-Biotechnology-2026/main/articles/GenomeBiology-Prokaryotic-Annotation-Benchmark-2026/images/fig04_mag_frameshift.png)

*图4｜MAG以及0.5%、1%、2%随机缺失造成的移码错误会怎样改变四个工具的编码区域和gene count。来源：原论文，CC BY 4.0。*

---

## 十三、作者还做了一个很“暴力”的移码压力测试

为了模拟序列错误，作者从32,914个基因组出发，分别随机删除：

- 0.5%的碱基；
- 1%的碱基；
- 2%的碱基。

最终得到 **98,742个错误基因组**。

以论文数据中平均约4.37 Mb的细菌基因组计算，这相当于平均大约每：

- 199 bp；
- 99 bp；
- 49 bp

就出现一次删除事件。

这是一个非常激进的stress test。

Prokka、Bakta和EggNOG-mapper在0.5%错误时，预测feature数量大约会迅速翻倍，因为原本完整的基因被切成多个片段；错误继续增加以后，编码区域逐渐下降，说明越来越多片段已经无法识别。

PGAP受到的影响明显更小。

作者认为主要原因是PGAP内部结合了 **ORFfinder和ProSplign**，能够显式处理一部分frameshift，而Prokka和Bakta主要依赖Prodigal。

---

## 十四、这里不能把0.5%—2%随机缺失直接等同于真实测序错误率

这一部分很容易被公众号标题写成：

> “PGAP抗测序错误能力碾压其他工具。”

这个说法太强。

论文模拟的是全基因组随机删除，而且0.5%—2%的删除比例非常高。它更像是在测试：

> **当开放阅读框被大量破坏时，各工具会怎样失败。**

真实项目中的问题可能来自：

- contig断裂；
- indel；
- 长读长残余错误；
- strain mixture；
- chimeric contig；
- contamination；
- 错误的遗传密码表。

这些错误并不等价。

因此Fig.4提供的是很有价值的鲁棒性压力测试，但不能替代你自己数据上的质量控制。

---

## 十五、数据库版本甚至可以改变“一个基因组有多少feature”

作者还比较了较新的PGAP重新注释和GTDB中保存的旧注释。

结果新版本平均多检测到：

> **139.8个feature。**

这个结果非常值得写进自己的分析流程。

很多论文Methods会写：

> “使用EggNOG进行功能注释。”

但没有记录：

- 软件版本；
- 数据库版本；
- 下载日期；
- 参数。

几年以后重新跑一次，结果可能已经明显不同。

所以对于可重复性，数据库版本和软件版本应该和测序平台一样被认真记录。

---

## 十六、这篇2026论文还有一个很重要的现实限制：benchmark本身用的是旧版本快照

论文比较的是一套固定时间点的软件环境，包括：

- Prokka；
- Bakta 1.7，database 5.0；
- EggNOG-mapper 2.1.9，使用2022年的数据库快照；
- PGAP 2022-10-03.build6384，数据库访问时间为2023年2月。

论文Methods里Prokka版本还出现了一个小的不一致：工具说明段写的是1.15.6，而实际workflow execution列出的Nanozoo容器标签为1.14.6。

这说明我们不应该把论文中的数字理解成：

> “2026年最新Bakta永远比最新PGAP强多少。”

更合适的用途是理解四类工具的**设计特点和失败模式**。

随着数据库和算法升级，具体百分比会继续变化。

---

## 十七、计算资源差异同样很现实

数据库大小差别非常明显：

| 工具 | 论文中的数据库/资源特点 |
|---|---|
| Prokka | 约0.6 GB，最轻量 |
| Bakta | 约60 GB |
| EggNOG-mapper | 约51 GB；dbmem模式需要约44–48 GB RAM |
| PGAP | 约30.4 GB，CPU开销最高 |

论文benchmark在Google Cloud Batch上通过Nextflow和Docker执行。

分配资源大致为：

- Prokka：4 CPU / 2 GB RAM；
- Bakta：8 CPU / 14 GB RAM；
- EggNOG-mapper：6 CPU / 48 GB RAM；
- PGAP：8 CPU / 8 GB RAM。

因此工具选择还要考虑实验室服务器条件。

一个只有8 GB内存的普通工作站，很难舒服地运行EggNOG-mapper的高内存模式。

---

## 十八、如果只想得到一个实用选择表，可以这样用

| 你的数据/目标 | 更值得优先考虑 |
|---|---|
| 高质量细菌分离株，希望尽量减少hypothetical protein | **Bakta** |
| 古菌 | **PGAP** |
| 细菌MAG | **PGAP** |
| contig很多、组装碎片化 | **PGAP** |
| 怀疑存在较多frameshift/序列错误 | **PGAP** |
| 希望更多基因至少得到一个GO | **PGAP**，必要时再加EggNOG-mapper |
| 希望已注释基因获得大量GO/KEGG/COG等功能术语 | **EggNOG-mapper** |
| 机器资源非常有限，只需要快速基础注释 | **Prokka** 或 Bakta db-light |
| 希望做发表级、信息比较完整的细菌注释 | Bakta + EggNOG-mapper，或PGAP + EggNOG-mapper按目标组合 |

![原核基因组注释工具选择决策树](https://raw.githubusercontent.com/lf3045/MetaSBT-Nature-Biotechnology-2026/main/articles/GenomeBiology-Prokaryotic-Annotation-Benchmark-2026/images/fig05_decision_tree.png)

*图5｜作者给出的简化工具选择决策树。论文总体不推荐把Prokka继续作为通用默认方案，但在资源非常有限时仍保留使用价值。来源：原论文，CC BY 4.0。*

---

## 十九、我更建议把“结构注释”和“功能注释”拆成两步思考

对于普通细菌分离株，一条比较稳妥的路线可以是：

**组装与QC**

→ **Bakta或PGAP做结构和基础功能注释**

→ **EggNOG-mapper补充正交关系、GO、KEGG、COG、PFAM等功能信息**

→ **针对研究问题再跑专用数据库**

例如：

- AMR：AMRFinderPlus / CARD；
- 毒力：VFDB；
- BGC：antiSMASH；
- CAZyme：dbCAN；
- CRISPR：专门CRISPR工具；
- 分泌系统：对应专用预测工具。

一个“通用注释软件”很难在所有专门问题上同时做到最好。

---

## 二十、对于MAG，先提高基因组质量通常比纠结注释软件更重要

这篇论文显示PGAP在MAG上更稳，但不意味着换成PGAP就能修复低质量组装。

MAG本身可能存在：

- completeness不足；
- contamination；
- strain heterogeneity；
- chimeric contig；
- 基因被contig边界截断。

这些问题会直接影响后面的基因预测。

所以更合理的顺序是：

**先做MAG质量控制和去污染**

→ **再做结构注释**

→ **再做功能注释**

而不是看到hypothetical protein多，就不断换注释软件。

---

## 二十一、这项benchmark最大的限制：没有真正的“全物种金标准”

作者自己也明确承认，大规模跨物种原核基因组注释目前缺少一个真正的gold standard。

因此这篇论文大量使用：

- coding density；
- hypothetical protein比例；
- 数据库accession覆盖；
- rRNA/tRNA/tmRNA的生物学预期；
- GO coverage和richness

作为间接指标。

这些指标都很有价值，但也都有边界。

例如：

> 编码区域更大，不代表一定更准确。

一个软件如果过度预测，也可能得到更高coding density。

同样：

> hypothetical protein更少，也不代表所有功能名称都正确。

功能转移本身可能出现错误。

所以这篇论文更适合评价**工具行为和实用覆盖能力**，不能把所有指标理解成真实注释准确率。

---

## 二十二、还有一个隐藏变量：数据库比算法本身更重要

Prokka、Bakta、EggNOG-mapper和PGAP之间的差异，并不全来自“谁的算法更聪明”。

很多差异实际上来自：

- 用了什么参考数据库；
- 数据库多久更新一次；
- 是否使用taxonomy约束；
- 是否允许电子GO证据；
- 是否主动过滤可疑ORF；
- 是否对frameshift做专门修复。

所以未来如果做自己的benchmark，最好把“软件”和“数据库”当成两个变量分别记录。

论文里同一个PGAP随着数据库时间变化就能多出约140个feature，这已经说明问题。

---

## 二十三、未来的原核注释工具可能会越来越像“多模型集成”

作者最后提出几个很值得关注的方向。

第一，未来工具可能会把多个注释器的结果放到统一框架中，而不是要求用户只能选一个。

第二，当传统序列同源搜索无法解释未知蛋白时，AlphaFold这类结构预测可以提供新的功能线索。

第三，原核注释应该继续从CDS扩展到完整调控结构，包括：

- promoter；
- ribosome-binding site；
- terminator；
- transcriptional unit。

第四，结果需要更标准化、可查询，而不是继续依赖大量彼此不兼容的GFF和TSV文件。

这也是未来宏基因组和泛基因组分析里很实际的需求。

---

# 结语：以后别再问“Prokka还是Bakta”，先问你的基因组是什么

这项 *Genome Biology* benchmark一次性比较156,033个原核基因组数据集，给出了一个很清楚的现实结论：

**不存在一款软件在所有场景都最好。**

高质量细菌分离株中，Bakta在给编码区域提供明确描述方面表现突出；古菌、MAG、碎片化基因组和移码压力测试中，PGAP整体更稳定；功能注释方面，PGAP更偏向覆盖更多基因，EggNOG-mapper则能给已经匹配到的基因提供更丰富的GO术语。

Prokka依然拥有一个非常明确的优势：轻量、快速、简单。但在数据库更新和功能覆盖已经明显落后的情况下，把它机械地作为所有细菌基因组的默认注释工具，已经很难得到最完整的信息。

对普通生信研究来说，这篇论文最有价值的使用方式可以压缩成四个问题：

1. **这是细菌还是古菌？**
2. **是高质量分离株，还是MAG/碎片化组装？**
3. **我要的是找基因，还是给基因补功能？**
4. **我的服务器能承担多大的数据库和计算量？**

把这四个问题回答清楚，工具选择通常就不会太困难。

而且无论最终使用哪一个软件，Methods里都应该完整记录：

> **软件版本 + 数据库版本 + 数据库日期 + 参数 + taxonomy来源。**

因为这篇论文已经用十几万份基因组证明了一件很实际的事：

**注释结果会随着工具、样本质量、物种背景和数据库时间一起变化。**

---

## 论文信息

Jundzill M, Hölzer M, Mangul S, et al. **Large-scale benchmarking of prokaryotic annotation tools across thousands of species.** *Genome Biology*. 2026;27:284.

DOI: 10.1186/s13059-026-04262-0

Published: 4 September 2026.

本文主图依据论文 **Creative Commons Attribution 4.0 International (CC BY 4.0)** 许可转载。原论文Fig.1标注使用BioRender制作；如后续单独拆图或二次改图，建议继续保留原作者、论文来源和许可信息。
