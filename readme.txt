Spark2.1.1��װ������ԣ�ʹ��spark-submit�ύjob��ʹ��spark-shell���н���ʽ�ύ


һ������Spark2.1.1
wget https://archive.apache.org/dist/spark/spark-2.1.1/spark-2.1.1-bin-hadoop2.7.tgz

������ѹ��spark�Ƶ�/usr/localĿ¼��
tar -zxvf spark-2.1.1-bin-hadoop2.7.tgz
mv spark-2.1.1-bin-hadoop2.7 spark
mv spark /usr/local

��������confĿ¼
cp spark-env.sh.template spark-env.sh
����export JAVA_HOME=/opt/app/jdk

�ġ�����
./sbin/start-all.sh

�塢ʹ��spark-submit�ύjob
./bin/spark-submit --class org.apache.spark.examples.SparkPi ./examples/jars/spark-examples_2.11-2.1.1.jar 10000

ʹ��http://10.5.2.241:4040/������job���м��
JPS�鿴���̣����һ��SparkSubmit����

����ʹ��spark-shell���н���ʽ�ύ
����root�µ��ı��ļ�hello.txt
./bin/spark-shell
�ٴ�����һ��terminal����jps�۲���̣��ῴ��spark-submit����
sc
sc.textFile("/root/hello.txt")
val lineRDD = sc.textFile("/root/hello.txt")
lineRDD.foreach(println)
�۲���ҳ�����
val wordRDD = lineRDD.flatMap(line => line.split(" "))
wordRDD.collect
val wordCountRDD = wordRDD.map(word => (word,1))
wordCountRDD.collect
val resultRDD = wordCountRDD.reduceByKey((x,y)=>x+y)
resultRDD.collect
val orderedRDD = resultRDD.sortByKey(false)
orderedRDD.collect
orderedRDD.saveAsTextFile("/root/result")
�۲���
���д����sc.textFile("/root/hello.txt").flatMap(_.split(" ")).map((_,1)).reduceByKey(_+_).sortByKey().collect


