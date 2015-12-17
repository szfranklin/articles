#Spark入门 （针对Python）
##概述

经过多年来开拓性的工作，UC Berkeley AMP Lab开发了Spark。它使用分布式内存数据结构，提高了数据处理的速度，在大多数工作上优于Haddop。本文用一个真实的数据集，展示Spark的结构，以及基本的转换（transformations）与行动（actions）。如果你想尝试编写和运行自己的Spark代码，可以到Dataquest试试本教程的（英文）互动版本。

弹性分布式数据集（RDD）

Spark的核心结构是RDD，全称“弹性分布式数据集”（resilient distributed dataset）。从名字即可看出，RDD是Spark里的数据集，分布于RAM或内存，或许多机器中。 一个RDD对象本质是多个元素的组合。它可以是包含多个元素（元组、列表、字典等）的列表。你可以把数据集载入为RDD，然后运行此RDD对象可用的方法（methods），就像Pandas里的数据框（DataFrames）。

##PySpark

Spark是用Scala语言写成的，Scala把要编译的东西编译为Java虚拟机（JVM）的字节码（bytecode）。Spark的开源社区开发了一个叫PySpark的工具库。它允许使用者用Python处理RDD。这多亏了一个叫Py4J的库，它让Python可以使用JVM的对象（比如这里的RDD）。
开始操作之前，先把一个包含《每日秀》（the Daily Show）所有来宾的数据集载入为RDD。这里用的数据集是FiveThirtyEight’s dataset的tsv版本。TSV文件是由 “\t” 分隔的数据文件，不同于像CSV文件用逗号 “,” 分隔。

```
raw_data = sc.textFile("daily_show.tsv")

raw_data.take(5)

['YEAR\tGoogleKnowlege_Occupation\tShow\tGroup\tRaw_Guest_List',

 '1999\tactor\t1/11/99\tActing\tMichael J. Fox',

 '1999\tComedian\t1/12/99\tComedy\tSandra Bernhard',

 '1999\ttelevision actress\t1/13/99\tActing\tTracey Ullman',

 '1999\tfilm actress\t1/14/99\tActing\tGillian Anderson']
SparkContext
```
SparkContext 是管理Spark里的集群（cluster）和协调集群运行进程的对象。SparkContext与集群的manager相连。Manager负责管理运行具体运算的执行者。下面一幅图来自Spark官方文档，能更好地展示这个结构：

SparkContext对象通常以变量sc的形式被引用。运行：
```

raw_data = sc.textFile("daily_show.tsv")

```
把TSV数据集载入为 RDD对象raw_data 。这个RDD对象类似一个包含许多字符串对象（string objects）的列表，数据集中每一行是一个字符串对象。之后，使用 take() 方法打印出前五个元素：
```
raw_data.take(5)
```
take(n) 返回RDD的前n个元素。想了解更多RDD可用的方法，可查阅PySpark的官方文档。

##惰性计算（Lazy Evaluation）

你可能会问：如果RDD与Python列表相似，那为什么不使用括号直接获取RDD里的元素？
这是因为RDD对象分布于很多个部分，我们无法对其进行列表的标准操作，而且RDD本身就是为了处理分布式数据开发的。RDD抽象方式的优势是可以让Spark在本地计算机运行。在本地运行时，Spark把本地计算机的内存划分为很多部分，以模拟在许多机器上进行计算的情境，所以在本地运行时也无需改动代码。
Spark RDD的另一个优点是代码的惰性计算（lazily evaluate）。Spark把一个计算拖延到不得不运行的时候。在上面的代码中，直到运行 raw_data.take(5) ，Spark才把TSV文件载入RDD。当raw_data = sc.textFile(“dail_show.tsv”) 被调用时，创建了一个指向此文件的指针。但只有当raw_data.take(5) 需要此文件时，文件才真正被读取进raw_data。本教程以及未来的讲解中会出现更多惰性计算的例子。

##流水线（Pipelines）

Spark大量借用了Hadoop的Map-Reduce模式，但许多地方与Hadoop不同。如果你有使用Hadoop和传统Map-Reduce的经验，Cloudera有一篇很棒的文章探讨这些差异。如果你从没使用过Map-Reduce 或 Hadoop也不用担心，本教程会介绍需要了解的概念。
使用Spark时，需要理解的核心概念是数据流水线（data pipelining）。Spark里的每个运算/计算本质是都是一系列步骤（step）。这些步骤能被连在一起，按顺序运行，形成一个流水线。流水线中的每个步骤返回一个Python值（例如Integer），一个Python数据结构（例如字典），或者一个RDD对象。首先，我们来看map() 函数。
Map()
map(f) 把函数f应用于RDD的每个元素。因为RDD是可迭代的（像多数Python对象一样），Spark每次迭代都运行f，之后返回一个新RDD。
为了便于理解，这里示范一个使用map 的例子。如果你观察仔细，就会发现 raw_data 目前的格式并不利于后续工作。现在每个元素都是一个字符串。为了便于管理，我们要把每个元素都转换成一个列表。Python的传统做法是：

使用for循环在集合中迭代

把每个字符串根据分隔符断开

把结果储存为一个列表

下面展示在Spark中使用map实现这个任务的方法。
在稍后的一段代码中，我们要：

调用RDD的map（）函数，把括号里的内容应用于数据集的每一行。

写一个lambda函数，根据分隔符”\t”把字符串分开，把结果存储为叫做daily_show的RDD。

在daily_show 上，调用RDD的take()函数，显示前五个元素（行）。

map(f) 函数是一个用于转换的步骤。需要提供给它一个命名过的函数或lambda函数。代码及输出如下：
```

daily_show = raw_data.map(lambda line: line.split('\t'))

daily_show.take(5)

[['YEAR', 'GoogleKnowlege_Occupation', 'Show', 'Group', 'Raw_Guest_List'],

 ['1999', 'actor', '1/11/99', 'Acting', 'Michael J. Fox'],

 ['1999', 'Comedian', '1/12/99', 'Comedy', 'Sandra Bernhard'],

 ['1999', 'television actress', '1/13/99', 'Acting', 'Tracey Ullman'],

 ['1999', 'film actress', '1/14/99', 'Acting', 'Gillian Anderson']]
 ```
 
##Python与Scala，永远的好朋友

我们习惯了用Python写出任务的逻辑。PySpark众多的优点之一，是可以把逻辑和具体的数据转换分开。在上面的代码中，我们用Python写了一个lambda函数：

```
raw_data.map(lambda: line(line.split('\t')))
```
而当这段代码运行于RDD时，又利用了Scala的优势。这就是PySpark的力量。尽管没有学习任何关于Scala的知识，我们还是利用了Spark的Scala结构在数据处理上的优异表现。更棒的是，当我们运行：
```
daily_show.take(5)
```
返回的结果还是对Python友好的格式。
转换与行动
Spark里有两类方法：

1.转换（Transformations） - map(), reduceByKey()

2.行动（Actions） - take(), reduce(), saveAsTextFile(), collect()

转换是惰性运算，总是返回对一个RDD对象的引用。不过，直到某个行动需要使用转换过的RDD，转换才会运行。任何返回RDD的函数都是转换，任何返回某个值的函数都是行动。在你实现本教程并尝试写PySpark代码的过程中，这些概念会变得更加清晰。
不可变
你可能会觉得奇怪：为什么不直接拆分每个字符串，而是要新建一个叫做daily_show的新对象？在Python中，可以直接逐个对集合里的元素进行修改，而不必返回或指派新对象。
RDD对象是不可变的。一旦对象被创建，它们的值就无法再变化。在Python里，列表和字典是可变的，这意味着我们可以改变这些对象的值，而元组是不可变的。在Python中修改一个元组对象，唯一方法就是创造一个包含所需改动的新元组。Spark利用RDD不可变的性质来提升速度，具体的原理超出本教程的讨论范围。

ReduceByKey()

我们想要对《每日秀》每年的来宾数目进行统计。在Python中，如果daily_show 是一个列表，其中包含多个列表，下面的一段代码可以实现我们的目的：
```

tally = dict()

for line in daily_show:

  year = line[0]

  if year in tally.keys():

    tally[year] = tally[year] + 1

  else:

    tally[year] = 1
```
tally 的每个键（key）会是唯一的，而值（value）是数据集中包含key的行数。
如果想用Spark获得相同结果，需要使用Map 步骤，接ReduceByKey步骤。
```

tally = daily_show.map(lambda x: (x[0], 1)).reduceByKey(lambda x,y: x+y)

print(tally)

PythonRDD[156] at RDD at PythonRDD.scala:43
```

##解释

你可能注意到了，打印tally 并没有像我们希望的那样返回统计数值。这是由于惰性计算的缘故。PySpark推迟map 和reduceByKey 的执行，直到需要使用它们（结果）的时候。在使用take() 预览tally 的前几个元素之前，先来过一遍上面的代码：

```
daily_show.map(lambda x: (x[0], 1)).reduceByKey(lambda x, y: x+y)
```
在map 步骤，我们使用了一个lambda函数，用来创建一个元组其中包含：

键: x[0], 列表的第一个值
值: 1, 整数

这里的策略是创建一个元组，其中包含年份作键，取值为1。在运行map 之后，Spark 会在内存中保留一个类似下列形式的，由多个元组构成的列表：

```
('YEAR', 1)

('1991', 1)

('1991', 1)

('1991', 1)

('1991', 1)

...
```
而我们想把这些化简为：
```
('YEAR', 1)

('1991', 4)

...
```
reduceByKey(f) 允许我们用函数f，将键相同的元组合并。
使用take 命令查看上面两个步骤的结果。take的作用是强迫惰性代码立即执行。由于 tally 是RDD，我们无法使用Python的len 函数来计算数目，而是要用RDD的 count() 函数。

```
tally.take(tally.count())

[('YEAR', 1),

 ('2013', 166),

 ('2001', 157),

 ('2004', 164),

 ('2000', 169),

 ('2015', 100),

 ('2010', 165),

 ('2006', 161),

 ('2014', 163),

 ('2003', 166),

 ('2002', 159),

 ('2011', 163),

 ('2012', 164),

 ('2008', 164),

 ('2007', 141),

 ('2005', 162),

 ('1999', 166),

 ('2009', 163)]
Filter
```
与Pandas不同的是，Spark无法识别首行是标题，也没有拿掉这些标题。我们需要想个办法从集合中去掉这个元素：

```
('YEAR', 1)
```
你可能会试着从RDD里直接去掉这个元素，但请注意RDD是不可变的对象，一旦被创建就无法更改。去掉这个元组的唯一方法，就是创建一个不包含此元组的RDD对象。
Spark有一个filter(f) 函数。此函数允许我们根据现存的RDD创建一个新的RDD，新RDD中只包含符合要求的元素。定义一个只返回二元值True 或 False函数 f 。只有True 对应的项目会出现在最终的RDD中。更多有关filter函数的内容可见Spark官方文档。

```
def filter_year(line):

    if line[0] == 'YEAR':

        return False

    else:

        return Truefiltered_daily_show = daily_show.filter(lambda line: filter_year(line))

```

##大家一起来

为了展示Spark的强大，这一节示范如何把一系列数据转换连成一个流水线，并观察Spark在后台处理一切。Spark在编写时就意图为这个目的服务，而且为处理连续任务进行了高度优化。以前用 Hadoop连续处理大量任务非常耗时。这是因为实时产生的结果都需要被写入硬盘，而且Hadoop 根本没意识到完整流水线的重要性（如果你对此感到好奇，可以从这个网址了解更多http://qr.ae/RHWrT2）。
感谢Spark嚣张的内存使用方式（只把硬盘用作备份和特殊任务）以及建构合理的内核。与Hadoop相比，Spark可以大大节省周转时间。
在下面一段代码中，我们进行一系列操作：筛掉没有职业的来宾，把每个职业名称变为小写，统计各个职业，并输出统计结果的前五项。
```

filtered_daily_show.filter(lambda line: line[1] != '') \

                   .map(lambda line: (line[1].lower(), 1)) \

                   .reduceByKey(lambda x,y: x+y) \

                   .take(5)

[('radio personality', 3),

 ('television writer', 1),

 ('american political figure', 2),

 ('former united states secretary of state', 6),

 ('mathematician', 1)]

```
##后续

希望本教程激发了你对Spark的兴趣，并掌握了如何用PySpark编写我们熟悉的Python代码，同时利用分布式处理的优势。在涉及更大数据集的工作中，PySpark会大放光芒，因为它模糊了数据科学在“本地计算机”与“大型在线分布式计算（也被称作云）”中的界限。
如果你喜欢本教程，可以去Dataquest 阅读下一章节（英文版），下一章会进一步讲解Spark的转换与行动。
原作者：Srini Kadamati
翻译：王鹏宇
原文链接：https://www.dataquest.io/blog/spark-intro/

​
