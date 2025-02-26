### 1. information not found in metastore
#### 발생일시
* 250226
#### 원인
* init-schema가 정상적으로 동작하지 않은것 같음(추정)
#### Log
```shell
Exception in thread "main" MetaException(message:Version information not found in metastore.)
        at org.apache.hadoop.hive.metastore.RetryingHMSHandler.<init>(RetryingHMSHandler.java:62)
        at java.base/jdk.internal.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
        at java.base/jdk.internal.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
        at java.base/jdk.internal.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
        at java.base/java.lang.reflect.Constructor.newInstance(Constructor.java:490)
        at org.apache.hadoop.hive.metastore.utils.JavaUtils.newInstance(JavaUtils.java:87)
        at org.apache.hadoop.hive.metastore.HMSHandlerProxyFactory.getProxy(HMSHandlerProxyFactory.java:43)
        at org.apache.hadoop.hive.metastore.HiveMetaStore.startBinaryMetastore(HiveMetaStore.java:554)
        at org.apache.hadoop.hive.metastore.HiveMetaStore.startMetaStore(HiveMetaStore.java:704)
        at org.apache.hadoop.hive.metastore.HiveMetaStore.main(HiveMetaStore.java:343)
        at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.base/java.lang.reflect.Method.invoke(Method.java:566)
        at org.apache.hadoop.util.RunJar.run(RunJar.java:328)
        at org.apache.hadoop.util.RunJar.main(RunJar.java:241)
Caused by: MetaException(message:Version information not found in metastore.)

```