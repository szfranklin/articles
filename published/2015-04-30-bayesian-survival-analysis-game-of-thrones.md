# 基于贝叶斯生存分析的的《冰与火之歌》人物死亡率分析

《冰与火之歌》书迷遍布全球。该小说凭借其丰富的人物设置受到广大书迷青睐。然而，在马丁( Martin )笔下，无论好人、坏人，主角、配角都难逃命运的捉弄。除不计其数的无名小卒外，马丁的世界里有916位有名字的角色，其中三分之一都已以各种方式结束了自己在小说中的生命。本文中，我们将进一步探究小说人物的死亡模式，建立贝叶斯生存模型来预测各角色的死亡概率。

本文数据来自冰与火之歌维基( A Wiki of Ice and Fire )。依据该数据我们创建了截至目前书中出现的916名角色的数据集。用人物首次出现的章节，性别，是否为贵族，所属势力，死亡的章节(若已故)作为解释变量来预测这些角色在未来两本书中的存活情况。

## 方法论

采用Weibull 分布外推在7本书中的各个角色的生存概率。Weibull分布提供了一种建立危险函数( hazard function )模型的方法。而危险函数主要测量人物在特定“书龄”上的死亡概率。Weibull分布主要依赖与两个参数，k和lambda，这两个参数决定了Weibull分布的形状。

在参数估计之前我们选取均匀分布作为先验概率。对于尚存角色，分析k和lambda如何描述人物的存活状况；对于已故人物，分析参数如何预测人物死亡时间。

对守夜人( Night’s Watch )，生存概率的后验分布如图1。

![图1](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWuia8ictvkvfNDAickWOlyiaMQCiaX67aGnXYVNVEYsva46DfmJicB6boMJMPEsHSnMGuwwyEwmiaK6VgrpA/640?wxfmt=png&tp=webp&wxfrom=5)

图1：lambda的分布比较紧密(在0.27附近)，K的分布比较宽松。
   
接下来，本文通过生存曲线分析人物的生存情况。为与生存曲线相联系，计算k和lambda的均值以及90%的置信区间。进一步，绘制原始数据和基于后验均值的生存曲线以及置信区间。

## 个人分析：雪诺( Jon Snow )
采用贝叶斯生存分析方法可以预测个性化人物(例如雪诺)的生存情况。在卷五：魔龙的狂舞( A Dancewith Dragons )结尾，守夜人生存的置信区间为0.36到0.56。Jon能活下来的估计并不乐观。即使Jon可以顺利活过第5本书，他在接下来的两本书中存活的概率将降到0.3到0.51。

![图2](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWuia8ictvkvfNDAickWOlyiaMQCwW2QwCTsKs1ztUAWN723AEyBNa6zp07EqiaXIoiae0QtWUicFzftOx7icA/640?wxfmt=png&tp=webp&wxfrom=5)

图2：置信区间紧紧围绕在真实数据周围，均值为合理预测。

值得注意的的是Jon并非守夜人的普通一员。他受过良好的教育，拥有精良的武器和战斗技能。接下来，将样本选为守夜人中家族地位显赫，教育良好的贵族。守夜人中只有11人为贵族。所以置信区间(如图3所示)非常分散，最优近似( Best Estimate )显示贵族背景并不能提高守夜人的生存率。

![图3](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWuia8ictvkvfNDAickWOlyiaMQC1FpiauR4Z79fvl41kUT3muAgYyqqZC1ygHYia5jhboDto1ia6bNn3CvZw/640?wxfmt=png&tp=webp&wxfrom=5)

图3：当只有贵族角色时，生存曲线的置信区间显著加宽，概率置信区间下限非常接近0。

##家族因素

接下来，我们分家族研究人物的生存情况。这里包括9个主要家族，守夜人，野人( the Wildlings )，和其他( a "None" category，指无法归入某类势力的人物)。

![图4](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWuia8ictvkvfNDAickWOlyiaMQCHmFYzLt0pI94jSopiabjyKC9G4qhfKxUA2bfcBM6X0ibO38JiaDO6bVPw/640?wxfmt=png&tp=webp&wxfrom=5)

图4 ：Arryn (蓝)，Lannister (金)，None (绿)及Stark (灰)的生存概率。

![图5](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWuia8ictvkvfNDAickWOlyiaMQCbvhFfDMT052JoUuGjHR0UNhT6c2cMoKOyiavaQW2WIF8dCbcNrxDdMA/640?wxfmt=png&tp=webp&wxfrom=5)

图5：Tyrell (绿)，Tully (蓝)，Baratheon (橘)及Night’sWatch (灰)生存概率。

![图6](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWuia8ictvkvfNDAickWOlyiaMQCiceCf0OukbNASP6I5wx8nnJYSScDrkpSLicF6uBAiaPC13GN1og8Xaribg/640?wxfmt=png&tp=webp&wxfrom=5)

图6：Martell (橘)，Targaryen (栗色)，Greyjoy (黄)及Wildling (紫色)生存概率。

图4、5、6的置信区间表明艾琳家族( Houses Arryn )、提利尔家族( Houses Tyrell )以及马泰尔家族( Houses Martell )有较高的生存率。主要原因是其远离书中主要冲突，不过这也意味着这些家族信息较少，我们只有至多5个死亡成员样本，所以生存曲线并没有包含足够的样本点。信息量的稀疏体现为较宽的置信区间。相反，北境诸侯( in the north )、史塔克家族( the Starks )、守夜人和野人这些家族(或势力)有较低的生存曲线和较窄的置信区间。他们在情节主线中占据主要篇幅，许多重要人物都是他们的一员。

##男女(性别因素)

书中塑造了丰富的女性角色，但依旧以男性人物为主(男女比例为769:157)。女性生存概率的置信区间较宽，但是其生存状态显著好于男性。如图7。

![图7](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWuia8ictvkvfNDAickWOlyiaMQCiclFTXPnVq50FVdF4eR2iaC4TlnNe7ia0InCYZiay1zeqZHWHM8NVqjcFA/640?wxfmt=png&tp=webp&wxfrom=5)

图7：维斯特洛(Westeros)中女性存活概率高于男性。

##地位(阶层)

小说中贵族和贫民人数差距很大，其生存曲线也展现出不同态势。如图8所示，平民倾向于在出场阶段迅速死亡，若能安全度过“介绍期”则将存活较长时间，甚至生存概率会高于部分贵族。

![图8](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWuia8ictvkvfNDAickWOlyiaMQCS4V1x8rMWXHw3XNibKkUJKXhHTmWib2uvtrk97xJUevbK66gEeBqFgyw/640?wxfmt=png&tp=webp&wxfrom=5)

图8：贵族在介绍期存活的概率较大，但生存概率的下降速度要大于平民。

##个性角色分析

利用本文提及的方法，可以结合性别，家族，地位等复合因素提供针对个体角色的粗糙预测模型。在书中给一个非常受欢迎的角色是艾莉亚( Arya )，许多读者关心她在书中的命运。史塔克家族的贵族女性中还包括一些值得注意的角色如珊莎( Sansa )和布雷妮( Brienne，宣誓效忠于史塔克家族，虽然她后来才被介绍)。另外，皇后瑟曦( Cersei )和可怜的弥赛拉( Myrcella )也十分令人着迷。为了得到生存曲线的准确区间估计，我们将贵族女子和平民女子的数据加以综合。

![图9](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWuia8ictvkvfNDAickWOlyiaMQCTW40LekqcICeVex2D2uy9KWEMZHeX1ZgJywA1naMeLMpmLq5Xwbdjw/640?wxfmt=png&tp=webp&wxfrom=5)

图9：各组置信区间都比较宽松。与史塔克家族相比，兰尼斯特( Lannister )家族的贵族女性死亡可能性更高。虽然信息不明确，但艾莉亚会比瑟曦活得久一些。

此外我们还关心两个小角色，野人公主瓦迩（ Val ）和神秘的魁蜥( Quaithe )。她们并不是故事一开始就出现，所以分析相对比较复杂。瓦迩在章节2.1中被引入，她在整个时序中存活的概率在0.1到0.53之间。魁蜥在章节1.2中首次出现，她的生存概率为0.58到0.85，明显高于瓦迩。

![图10](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWuia8ictvkvfNDAickWOlyiaMQCd0uDEAYmhjpS1DYynNWj1EKG3llJXxGV4Ric2GSWicubZcrAaDlTqic9w/640?wxfmt=png&tp=webp&wxfrom=5)

图10：代表一些小角色的生存曲线，魁蜥和瓦迩有不同的生存曲线。

有足够的数据能够区分大多数男性角色的家族、性别和地位，以绘制他们的生存曲线。图11显示，兰尼斯特兄弟的生存曲线居中，在第七本书的生存概率为0.35到0.79。达里奥( Daario )生存曲线的置信区间较宽，但考虑到他是在章节2.5中才出现，所以存活概率较大。曼斯( Mance )的存活概率最不容乐观。曼斯在章节2.2故事中登场，他的存活概率为0.19到0.56。

![图11](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWuia8ictvkvfNDAickWOlyiaMQC7COxIWwWBQh7m9gVAk1AHYCxrebaE3qVWcVHFcVdcn1khGAaLvnFWQ/640?wxfmt=png&tp=webp&wxfrom=5)

图11：不同地位、联盟的男性角色的生存曲线。

有一些角色，我们期望看到他们一命呜呼，但是图12显示他们还要活很久。希恩( Theon)似乎会痛苦的活着直到到结局。瓦尔德·弗雷( Walder Frey )在章节0.4中初次登场，存活的可能性为0.44到0.72之间。目前为止，霍斯特·徒利( Hoster Tully )可能是唯一一个死于衰老的人，所以弗雷将有可能活到结局。

![图12](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWuia8ictvkvfNDAickWOlyiaMQClYqDHHXWdPpKRicJ2e9cdHZ6NA17H9ywgibPb6sh1kbeiaXG2Piajg0Phw/640?wxfmt=png&tp=webp&wxfrom=5)

图12：不同地位、联盟的男性的生存曲线。

##总结

孰生孰死在故事中充满变数，但从现有数据中，我们可以观察到不同组别下人物生死的模式。对于一些特定角色，尤其是男性角色，我们可以对他们在未来的故事中的遭遇做简单预测。但对于数据较少的、非主要家族的女性来说，预测的准确性则有待商榷。


本文内容翻译并编辑自 Bayesian Survival Analysis in A Song of Ice and Fire，by Erin Pierce and Ben Kahle.
原文链接：http://www.reddit.com/r/statistics/comments/31oz8n/bayesian_survival_analysis_in_a_song_of_ice_and/.compact
翻译：新妍
校对：Jude