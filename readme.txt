Spark2.1.1安装部署测试，使用spark-submit提交job，使用spark-shell进行交互式提交


一、下载Spark2.1.1
wget https://archive.apache.org/dist/spark/spark-2.1.1/spark-2.1.1-bin-hadoop2.7.tgz

二、解压，spark移到/usr/local目录下
tar -zxvf spark-2.1.1-bin-hadoop2.7.tgz
mv spark-2.1.1-bin-hadoop2.7 spark
mv spark /usr/local

三、进入conf目录
cp spark-env.sh.template spark-env.sh
增加export JAVA_HOME=/opt/app/jdk

四、启动
./sbin/start-all.sh

五、使用spark-submit提交job
./bin/spark-submit --class org.apache.spark.examples.SparkPi ./examples/jars/spark-examples_2.11-2.1.1.jar 10000

使用http://10.5.2.241:4040/，进行job运行监控
JPS查看进程，会多一个SparkSubmit进程

六、使用spark-shell进行交互式提交
创建root下的文本文件hello.txt
./bin/spark-shell
再次连接一个terminal，用jps观察进程，会看到spark-submit进程
sc
sc.textFile("/root/hello.txt")
val lineRDD = sc.textFile("/root/hello.txt")
lineRDD.foreach(println)
观察网页端情况
val wordRDD = lineRDD.flatMap(line => line.split(" "))
wordRDD.collect
val wordCountRDD = wordRDD.map(word => (word,1))
wordCountRDD.collect
val resultRDD = wordCountRDD.reduceByKey((x,y)=>x+y)
resultRDD.collect
val orderedRDD = resultRDD.sortByKey(false)
orderedRDD.collect
orderedRDD.saveAsTextFile("/root/result")
观察结果
简便写法：sc.textFile("/root/hello.txt").flatMap(_.split(" ")).map((_,1)).reduceByKey(_+_).sortByKey().collect


