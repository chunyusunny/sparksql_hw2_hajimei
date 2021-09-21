# sparksql_hw2_hajimei 苦逼肝作业中。。

作业一： 如何避免小文件问题？给出 2 ～ 3 种解决方案<br/>
->产生小文件的原因：<br/>

1. RDD partition 数量过多->与任务并发数有关<br/>解决思路：<br/>
   1.1 适当配置 spark.sql.shuffle.partitions(但会影响并发度)<br/>
   1.2 动态调整执行计划->AQE framework(Adaptive Query Execution)

   - 动态合并 shuffle partitions<br/>
   - 动态调整 join 策略<br/>
   - 动态优化倾斜的 join(skew joins)<br/>
     （spark.sql.adaptive.enabled=true 开启该功能, spark.sql.adaptive.shuffle.targetPostShuffleInputSize 设置 reduce 任务处理文件的上限)<br/>

2. 表分区数量过多(源数据本身就是大量的小文件)<br/>
   2.1 将原始数据进行按照分区字段进行 shuffle 后再进行处理(可能会引起数据倾斜->知道倾斜键的情况下，可以将原始数据分成几个部分处理，不倾斜的按照分区键 shuffle，倾斜部分可以按照 rand 函数来 shuffle)<br/>
   2.2 Coalesce and Repartition-> 通过对数据集再分区控制输出分区数<br/>
   refer to: https://issues.apache.org/jira/browse/SPARK-24940<br/>
   2.3 AQE<br/>

作业二：实现 Compact table command<br/>
• 要求：<br/>
添加 compact table 命令，用于合并小文件，例如表 test1 总共有 50000 个文件，<br/>
每个 1MB，通过该命令，合成为 500 个文件，每个约 100MB。<br/>
• 语法：<br/>
COMPACT TABLE table_identify [partitionSpec] [INTO fileNum FILES];<br/>
• 说明：<br/> 1.如果添加 partitionSpec，则只合并指定的 partition 目录的文件。<br/> 2.如果不加 into fileNum files，则把表中的文件合并成 128MB 大小。<br/> 3.以上两个算附加要求，基本要求只需要完成以下功能：<br/>
COMPACT TABLE test1 INTO 500 FILES;<br/>
