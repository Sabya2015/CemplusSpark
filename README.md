# Spark Job TR logging integration

### This document gives a brief about how to get SPARK application load custom TR logging log4j configuration file bundled within application's fat JAR.

For issue description,root cause analysis and solution please refer below link.
https://thehub.thomsonreuters.com/docs/DOC-1678799

## Steps to execute the Spark-Job-TR-Logging application.

1) Install sbt</br>
   http://www.scala-sbt.org/download.html
  
2) Download and unzip ecp-log-test.zip</br>
   Update val eventSourceUUIDValue = "4800a410-81f9-476e-9dd0-ce8fd71ccd2a" in LogTest.scala with your registered UUID and update MDC      values as per your needs.</br>

   ecp-log-test\src\main\scala\com\thomsonreuters\scalaTest\LogTest.scala

3) Open command prompt and go to the folder path ecp-log-test</br>
   Run following commands </br>
```javascript 
sbt compile
sbt package 
```
4) After compilation and packaging tramslogtest-1.0.jar is generated</br>
   \ecp-log-test\target\tramslogtest-1.0.jar
   
5) Copy tramslogtest-1.0.jar,log4j.xml all the dependent jars from lib folder</br>
   \ecp-log-test\lib --all</br>
   \ecp-log-test\src\main\resources -- log4j.xml
   
6) Create a directory in filebrowser in Hue and put all the files mentioned above.</br>
   Can be done by drag and drop from the approprite folders.</br>
   For Ex:</br>
   /project/ecpdevreporting/TramsLogTest
   
   To get access to TR managed clusters.</br>
   Will need to create an M account if already not existing.Please use the below link to do so.</br>
   https://bigdataportal.int.thomsonreuters.com/mgmtAccountCheck
   
7) Using the edgenode name of the cluster you have access and where you have copied the files, login using putty.</br>
   Use M account credentials to login and execute below cmd to get all the files.
  
8) From the downloaded folde execute the below command after changing to your respective login id and local path.</br>

   
```javascript    
   spark-submit --class com.thomsonreuters.scalaTest.TramsLogTest --files /hadoop/user/m6022631/TramsLogTest/log4j.xml --driver-java-options "-Dlog4j.configuration=log4j.xml" --driver-class-path /hadoop/user/m6022631/TramsLogTest/slf4j-api-1.7.6.jar,/hadoop/user/m6022631/TramsLogTest/javax.json-1.0.4.jar,/hadoop/user/m6022631/TramsLogTest/kafka-log4j-appender-0.9.0.0.jar,/hadoop/user/m6022631/TramsLogTest/kafka-clients-0.8.2.1.jar,/hadoop/user/m6022631/TramsLogTest/loglayout-34.1.6.jar,/hadoop/user/m6022631/TramsLogTest/KafkaMessagingUtil-32.3.9.jar,/hadoop/user/m6022631/TramsLogTest/ServiceRegistry-34.0.4.jar,/hadoop/user/m6022631/TramsLogTest/config-1.3.0.jar,/hadoop/user/m6022631/TramsLogTest/scala-logging-slf4j_2.11-2.1.1.jar,/hadoop/user/m6022631/TramsLogTest/scala-logging_2.11-3.1.0.jar,/hadoop/user/m6022631/TramsLogTest/json-20070829.jar --jars /hadoop/user/m6022631/TramsLogTest/slf4j-api-1.7.6.jar,/hadoop/user/m6022631/TramsLogTest/javax.json-1.0.4.jar,/hadoop/user/m6022631/TramsLogTest/kafka-log4j-appender-0.9.0.0.jar,/hadoop/user/m6022631/TramsLogTest/kafka-clients-0.8.2.1.jar,/hadoop/user/m6022631/TramsLogTest/loglayout-34.1.6.jar,/hadoop/user/m6022631/TramsLogTest/KafkaMessagingUtil-32.3.9.jar,/hadoop/user/m6022631/TramsLogTest/ServiceRegistry-34.0.4.jar,/hadoop/user/m6022631/TramsLogTest/config-1.3.0.jar,/hadoop/user/m6022631/TramsLogTest/scala-logging-slf4j_2.11-2.1.1.jar,/hadoop/user/m6022631/TramsLogTest/scala-logging_2.11-3.1.0.jar,/hadoop/user/m6022631/TramsLogTest/json-20070829.jar --num-executors 2 --driver-memory 8g --executor-memory 20g --master yarn --deploy-mode client --queue root.ecpdevcemreporting /hadoop/user/m6022631/TramsLogTest/tramslogtest-1.0.jar
   hdfs dfs -get /project/ecpdevreporting/TramsLogTest
``` 
9) Check the index being created in preprod ELK</br>
   http://trams-logstash-eag-preprod.int.thomsonreuters.com:9200/_plugin/kopf/#!/rest

