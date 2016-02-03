#Apache Spark介绍及案例展示

2013年年底，我第一次接触到Spark，当时我对Spark所使用的Scala语言产生了较大的兴趣。一段时间后，我做了一个预测泰坦尼克号船上人员生存概率的数据科学项目。事实证明这是一个更深入了解Spark概念和编程框架的绝佳途径。我强烈建议任何希望学习Spark的开发者都寻找一个项目入手。
如今，诸如亚马逊、eBay和雅虎等公司都开始采用Spark技术。许多机构将Spark部署在上千个节点的集群中。据Spark FAQ中记录，已知的最大集群节点个数已超过8000。事实上，Spark是一项非常值得学习的技术。
本文主要介绍了Spark概念及一些实例。这些信息主要从Apache Spark网站和相关书籍中获取。

##1.什么是Apache Spark?

正如广告所提到的，Spark是一个运算速度快如闪电的Apache项目。它有一个逐渐壮大的开源社区，同时它还是现今最热门的Apache项目。
Spark提供了一个运算速度快的一般化数据处理平台。Spark可以让你的程序提高100倍的内存计算速度，或者10倍的磁盘计算速度(Hadoop)。去年的Daytona GraySort比赛中，Spark只用了Hadoop十分之一数量的机器就实现了其三倍多的速度。Spark已经成了处理PB级别数据运算速度最快的开源工具。
Spark还可以更加便捷地完成你的项目，为了更好地说明这个问题，我们首先看下如何实现大数据中的“Hello World!”案例。用Java语言编写的MapReduce过程需要大约50行的代码，而利用Spark你只需要以下几行代码：

```
sparkContext.textFile("hdfs://...")
    .flatMap(line => line.split(" "))
    map(word => (word, 1)).reduceByKey(_+_)
    .saveAsTextFile("hdfs://...")
```

学习如何使用Apache Spark时，另一个重要的东西是其提供了脱机的交互式shell(REPL)。利用REPL，我们可以逐行检测代码是否有误。考虑到Spark较为简短的代码风格，这使得即时数据分析任务成了容易实现的事情。
此外，Spark还提供了其他的一些特性：

当前Spark提供了Scala、Java、Python和R的API接口
较好地整合了Hadoop生态系统和数据储存系统(HDFS, Amazon S3, HIVE, HBase, Cassandra等)
既可以在Hadoop YARN或者Apache Mesos等集群上运行，也可以单机运行。

Spark核心组件可以和其他一些高效的软件库无缝连接使用。这些软件库包括SparkSQL, Spark Streming, MLlib(机器学习专用)和GraphX，下文将详细介绍这些组件。其他一些软件库和扩展功能目前正处于开发过程中。


##2.Spark核心组件

Spark是处理大规模数据的并行分布式基础引擎。它主要负责以下几个功能：

内存管理和故障恢复
制定并管理集群中的任务
和数据储存系统交互

Spark引入了RDD(Resilient Distributed Dataset)的概念，RDD是一个抽象的数据集，它提供对数据并行和容错的处理。我们可以通过加载外部数据集或从驱动程序集中切分得到一个可以包含任意类型项目的RDD。
RDD支持两种类型的运算：

数据转换(数据映射、过滤、合并等)在一个RDD上执行，而其结果被储存到另外一个RDD中。
数据运算(降维、计数等)则是通过在RDD中计算后才返回相应的结果。

Spark的数据转换过程并不是实时返回运算结果。实际上，该过程知识记录下需要执行的操作过程和相应的数据集。只有当执行数据运算过程且结果已经返回到驱动程序中时，Spark才执行数据转换进程。该设计使得Spark可以更高效地执行任务。例如，如果一个大型数据集被转换成许多子集并被传输到第一步的数据运算过程中，那么此时Spark只能处理并返回第一步的运算结果，并无法处理整个数据集的运算过程。
默认设定下，任何一个处理数据转换过程的RDD将会在每次处理完数据运算后被还原。然而，你也可以使用高速缓存的方法将RDD保存下来，此时Spark会将内容储存在集群中以便于下次更快捷地调用。

##3.SparkSQL

SparkSQL是Spark的一个组件，它可以利用SQL或者Hive查询语法来查询数据。它起先被视为MapReduce的替代方案，现今SparkSQL已被整合到Spark堆栈中。为了提供对更多数据类型的支持，它将SQL语句纳入系统中，这使其成为一个非常强大的工具。以下是Hive兼容查询语句的实例：

```
// sc is an existing SparkContext
val sqlContext = new org.apache.spark.sql.hive.HiveCONTEXT(sc)

sqlContext.sql("CREATE TABLE IF NOT EXISTS src (key INT, value STRING)")
sqlContext.sql("LOAD DATA LOCAL INPATH 'examoles/src/main/resources/kvl.txt' INTO TABLE src")

// Queries are expressed in HiveQL
sqlContext.sql("FROM src SELECT key, value").collect().foreach(println)
Spark Streaming
```

Spark Streaming支持实时流式数据处理，比如Web服务器日志文件、Twitter等社交网络数据和类似Kafka的信息数据。Spark中，Spark Streaming接收输入流数据并将其划分成小子集。接下来，如下图所示，这些数据被Spark引擎所处理并被整合成最终的结果。

Spark Streaming的API接口和Spark核心组件非常匹配，因此所有的编程人员可以轻易地处理流式数据。


##4.MLlib

MLlib是一个机器学习库，它提供了为大规模集群计算所设计的分类、回归、聚类和协同过滤等机器学习算法。其中一部分算法也适用于处理流式数据，比如普通线性二乘回归估计和k均值聚类算法。值得注意的是，Apache Mahout(Hadoop的机器学习算法软件库)已经脱离MapReduce阵营转而投向Spark MLlib中。


##5.GraphX


GraphX是用于绘图和执行绘图并行计算的软件库，它为ETL(探索性分析和反复的绘图计算)提供了一套统一的工具。除了绘图操作技巧，它还提供了类似于PageRank的一般性绘图算法。


##6.如何使用Apache Spark?

现在我们已经回答了“什么是Apache Spark?”这个问题，接下来让我们思考下Spark可以用来处理啥样的问题呢？
最近，我偶然看到一篇文章。文章中提到利用Twitter流式数据来检测地震。有趣的是，实验表明该技术可以比日本气象局更快地告知百姓地震的情况。即使他们在文章使用了不同的方法，但我认为这是一个很好的例子，它可以用来检验我们如何利用简化的片段代码而不使用粘接代码将Spark付诸实践。
首先，我们必须将与“地震”或者“抖动”有关的推文过滤出来。我们可以非常轻易地利用Spark Streaming来实现该目标：
```
TwitterUtils.createStream(...)
    .filter(_.getText.contains("earthquake") || _.getText.contains("shaking"))
```

接下来我们可以对处理完的推文数据做一些语义分析，并判断是否能反映出当前的地震情况。比如，类似于“地震”或者“现在正在晃动！”的推文将被视为具有正效应，而类似于“参加地震会议。”或者“昨天的地震真恐怖。”则被视为无影响效应。文章作者利用支持向量机模型来实现该目标，我们将在 此基础上利用流式数据版本的模型来实现。下文是利用MLlib生成的代码示例：
```
// We would prepare some earthquake tweet data and load it in LIBSVM format.
val data = MLUtils.loadLibSVMFile(sc, "sample_earthquate_tweets.txt")

// Split data into training (60%) and test (40%).
val splits = data.randomSplit(Array(0.6,0.4),seed = 11L)
val training = split(0).cache()
val test = splits(1)

// Run training algorithm to build the model
val numIterations = 100
val model = SVMWithSGD.train(training, numIterations)

// Clear the default threshold.
model.clearThreshold()

// Compute raw scores on the test set.
val scoreAndLabels = test.map{point => 
    val score = model.predict(point.features)
    (score, point.label)}

// Get evaluation metrics.
val metric = new BinaryClassificationMetrics(scoreAndLabels)
val auROC = metrics.areaUnderROC()

println("Area under ROC =" + auROC)
```

如果我们关注模型的预测准确率，那么我们可以进一步对检测到地震做出反应。需要注意的是，对于包含地理信息的推文，我们还可以获取震源位置。利用这个信息，我们可以通过SparkSQL从现有的Hive table(储存需要接收地震提醒的用户信息)中提取出他们的邮箱地址并发送一封私人电子邮件：
```
//sc is an existing SparkContext.
val sqlContext = new org.apache.spark.sql.hive.HiveContext(sc)
// sendEmail is a custom function
sqlContext.sql("FROM earthquake_warning_users SELECT firstName, lastName, city, email")
    .collect().foreach(sendEmail)
```

##Apache Spark的其他应用

除检测地震情况外，Spark还有许多潜在的应用。
以下是Spark在大数据中的部分应用：
1.在游戏领域中，从实时的潜在游戏事件中迅速地挖掘出有价值的模式可以创造出巨大的商业利益，比如用户返回率情况、如何制定定向广告以及如何自动调整游戏的复杂度等。
2.在电子商务领域中，实时交易数据将被传递到k均值算法或者ALS等协同过滤流算法中。这些运算结果将和顾客评论等非结构化数据结合起来，用于不断改进交易模式以适应新趋势的发展。
3.在金融或证券领域中，Spark堆栈技术可以被应用到信用诈骗和风险管控系统中。通过获取大量的历史数据和其他一些外泄数据以及一些连接/请求信息(IP地理信息或时间信息)，我们可以取得非常好的模型结果。

##结论

总而言之，Spark帮助人们简化了处理大规模数据的步骤流程。不管是处理结构化还是非结构化数据，Spark将许多复杂的功能(比如机器学习算法和图算法)无缝地结合起来。Spark使得大量的从业者都可以进行大数据分析，让我们一探究竟吧！
原文作者：RADEK OSTROWSKI
原文链接：http://www.toptal.com/spark/introduction-to-apache-spark
译者：Fibears
