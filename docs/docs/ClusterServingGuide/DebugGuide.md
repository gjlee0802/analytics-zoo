# Cluster Serving Debug Guide

This guide provides step-by-step debug to help you if you are stuck when using Cluster Serving
### Check if Cluster Serving environment is ready
Run following commands in terminal
```
echo $FLINK_HOME
echo $REDIS_HOME
```
the output directory
```
/path/to/flink-version
/path/to/redis-version
``` 
 
should be displayed, otherwise, go to [Programming Guide](ProgrammingGuide.md) **Installation** section.

### Check if Flink Cluster is working
Run following commands in terminal
```
netstat -tnlp
```
output like following should be displayed, `6123,8081` is Flink default port usage.
```
tcp6       0      0 :::6123                 :::*                    LISTEN      xxxxx/java
tcp6       0      0 :::8081                 :::*                    LISTEN      xxxxx/java
```
if not, run `$FLINK_HOME/bin/start-cluster.sh` to start Flink cluster.
### Check if Cluster Serving is running
```
$FLINK_HOME/bin/flink list
```
output of Cluster Serving job information should be displayed, if not, go to [Programming Guide](ProgrammingGuide.md) **Launching Service** section to make sure you call `cluster-serving-start` correctly.

### Still, I get no result
Getting no result means after you call 1.`InputQueue.enqueue` and use `OutputQueue.query`, or 2. just use `InputQueue.predict`  to get result, you get `[]` or `NaN`

If you get empty result, aka `[]`, means your input get no return, you could go to `$FLINK_HOME/log/flink-taskexecutor-...log` to check your output. If you are sure you have done above check and still get empty result, raise issue [here](https://github.com/intel-analytics/analytics-zoo/issues) and post this log.

If you get invalid result, aka `NaN`, means your input does not match your model, please check your data shape. e.g. if you use Tensorflow Keras Model, you can use `model.predict(data)` locally to check if it works. This test also applies to other deep learning frameworks.