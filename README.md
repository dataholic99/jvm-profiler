Introduction
============
Uber JVM Profiler provides a Java Agent to collect various metrics and stacktraces for JVM processes. It is initially
created to profile Spark applications which usually have dozens of processes for a single application. However
Uber JVM Profiler is a generic Java Agent and could be used for any JVM process as well.

How to Build
============

1. Make sure JDK 8+ and maven is installed on your machine.
2. Run: mvn clean package

Example to Run
==============

Following command will start the example application with the profiler agent attached, which will report metrics to the console output:
```
java -javaagent:target/jvm-profiler-0.0.3.jar=reporter=com.uber.profiling.reporters.ConsoleOutputReporter,tag=mytag,metricInterval=5000,durationProfiling=com.uber.profiling.examples.HelloWorldApplication.publicSleepMethod,argumentProfiling=com.uber.profiling.examples.HelloWorldApplication.publicSleepMethod.1,sampleInterval=100 -cp target/jvm-profiler-0.0.3.jar com.uber.profiling.examples.HelloWorldApplication
```

Feature List
==============
Uber JVM Profiler supports following features:

1. Debug memory usage for all your spark application executors, including java heap memory, non-heap memory, native memory (VmRSS, VmHWM), memory pool, and buffer pool (directed/mapped buffer).

2. Debug CPU usage, Garbage Collection time for all spark executors.

3. Debug arbitrary java class methods (how many times they run, how much duration they spend). We call it Duration Profiling.

4. Debug arbitrary java class method call and trace it argument value. We call it Argument Profiling.

5. Do Stacktrack Profiling and generate flamegraph to visualize CPU time spent for the spark application.

6. Debug IO metrics (disk read/write bytes for the application, CPU iowait for the machine).

Parameter List
================

The java agent supports following parameters:

- reporter: class name for the reporter, e.g. com.uber.profiling.reporters.ConsoleOutputReporter, or com.uber.profiling.reporters.KafkaOutputReporter.

- tag: plain text string which will be reported together with the metrics.

- metricInterval: how frequent to collect and report the metrics, in milliseconds.

- durationProfiling: configure to profile specific class and method, e.g. com.uber.profiling.examples.HelloWorldApplication.publicSleepMethod. It also support wildcard (*) for method name, e.g. com.uber.profiling.examples.HelloWorldApplication.*.

- argumentProfiling: configure to profile specific method argument, e.g. com.uber.profiling.examples.HelloWorldApplication.publicSleepMethod.1 (".1" means getting value for the first argument and sending out in the reporter).

- sampleInterval: frequency (milliseconds) to do stacktrace sampling, if this value is not set or zero, the profiler will not do stacktrace sampling.

- ioProfiling: whether to profile IO metrics, could be true or false.

- brokerList: broker list if using com.uber.profiling.reporters.KafkaOutputReporter.

- topicPrefix: topic prefix if using com.uber.profiling.reporters.KafkaOutputReporter. KafkaOutputReporter will send metrics to multiple topics with this value as the prefix for topic names.



