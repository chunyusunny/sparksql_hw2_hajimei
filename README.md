# sparksql_hw2_hajimei 苦逼肝作业中。。
作业一： 如何避免小文件问题？给出2～3种解决方案<br/>
->产生小文件的原因：<br/>
1. RDD partition数量过多->与任务并发数有关<br/>解决思路：<br/>
  1.1 适当配置spark.sql.shuffle.partitions(但会影响并发度)<br/>
  1.2 动态调整计划->AQE
2. 表分区数量过多


作业二：实现Compact table command<br/>
• 要求：<br/>
  添加compact table命令，用于合并小文件，例如表test1总共有50000个文件，<br/>
  每个1MB，通过该命令，合成为500个文件，每个约100MB。<br/>
• 语法：<br/>
  COMPACT TABLE table_identify [partitionSpec] [INTO fileNum FILES];<br/>
• 说明：<br/>
  1.如果添加partitionSpec，则只合并指定的partition目录的文件。<br/>
  2.如果不加into fileNum files，则把表中的文件合并成128MB大小。<br/>
  3.以上两个算附加要求，基本要求只需要完成以下功能：<br/>
  COMPACT TABLE test1 INTO 500 FILES;<br/>
