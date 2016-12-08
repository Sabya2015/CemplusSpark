# Spark Job TR logging integration

### This document gives a brief about how to get SPARK application load custom TR logging log4j configuration file bundled within application's fat JAR.

For issue description,root cause analysis and solution please refer below link.
https://thehub.thomsonreuters.com/docs/DOC-1678799

## Steps to execute the Spark-Job-TR-Logging application.

1) Install sbt</br>
   http://www.scala-sbt.org/download.html
  
2) Download and unzip ecp-log-test.zip

3) Open command prompt and go to the folder path ecp-log-test
   Run following commands 
```javascript 
sbt compile
sbt package 
```
4) After compilation and packaging tramslogtest-1.0.jar is generated
   .\ecp-log-test\target\tramslogtest-1.0.jar
   
5)
