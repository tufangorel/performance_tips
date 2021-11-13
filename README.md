Performance Tips
=============================
Purpose : The purpose of this document is to provide a base check list for the code review activities.

## Contents

- [Java-SE](#java-se)

- [Collections](#collections)

- [Database Connection](#database-connection)

- [Threading](#threading)

- [GC Efficiency](#gc-efficiency)

- [Memory Space Efficiency](#memory-space-efficiency)

- [JVM Efficiency](#jvm-efficiency)

- [Log Generation](#log-generation)

- [Sorting](#sorting)

- [JVM Performance Analysis Monitoring Tools](#jvm-performance-analysis-monitoring-tools)


## Java SE

**-** Prefer java.util.concurrent.ThreadLocalRandom for muti-threaded environments instead of java.util.Random .

**-** Avoid JNI usage to call native C code from Java. Run Java code on JVM to get maximum performance benefits.

**-** Do not use Exception throwing mechanism for messaging purposes.

**-** Do not create String by concatenation instead use StringBuilder object for increased performance. Know that StringBuilder is not synchronized.

**-** Do not append to a String inside a for-while loop .

**-** Prefer StringBuffer in a multi-threaded environment and know that synchronization comes with additional performance cost.

**-** StringBuilder is faster than StringBuffer because StringBuilder is not thread-safe.

**-** Use lambda expressions instead of anonymous classes.

**-** Use parallel streams with caution because they do not always achieve better as expected.

**-** Use enumerated integers defined in a constant interface instead of constant String values for speed and memory benefits. Take performance advantage of comparing enumerated objects by identity comparison using "==" operator instead of equals() method.

**-** Prefer primitive data types instead of Wrappers as class members. For instance, use "int" for "Integer" or "double" for "Double" as a class member.

**-** If components of String concatenation is known at compile-time then prefer String concatenation "+" operator. However, if the String components can not be resolved at compile-time then use StringBuffer to produce the result String.

**-** Avoid casting explictly in a try block and throwing new exception in catch block, instead prefer instanceOf operator for type checking.

**-** When iterating over a collection do not use collection.size() as loop termination test in the for-while loop, instead assign the collection.size() to a variable outside the loop and use this variable as loop termination condition.

**-** Prefer int data type as loop index variable instead of other numeric data types for better performance.

**-** Prefer using interfaces and type overloading against runtime-reflection for performance gains.

**-** If it is not required then do not define new @annotations and process them at run-time.

**-** Do not serialize unnecessary fields and mark them with transient.

**-** Prefer java.lang.ref.Cleaner class instead of Finalizer to free memory.

**-** Do not forget to release resources that were opened/used inside a try block.

**-** Prefer try-with-resources in place of try-finally.

**-** Implement a better and concrete "hashCode" method to achieve higher performance.


## Collections

**-** Prefer ArrayList for accessing the nth element by index value to get benefit of constant access time cost.

**-** Prefer ArrayList for index based search operations over LinkedList which has a linear time cost.

**-** Prefer LinkedList for frequent insert and delete operations over ArrayList.

**- Not :** Insertion time for ArrayList: O(n). Insertion time for LinkedList: O(1).

**- Not :** Deletion time for ArrayList: Access time + O(n). Deletion time for LinkedList: Access time + O(1).

**- Not :** Access time for ArrayList: O(1). Access time for LinkedList: O(n).

**-** Use unsynchronized Collection types for better performance achievements in single-threaded environments.

**-** Hashtable is synchronized however HashMap is an unsynchronized version so performs better in single-threaded environments.

**-** Vector is synchronized however ArrayList is an unsynchronized version so performs better in single-threaded environments.

## Database Connection

**-** Use database connection pool to decrease the cost of creating new db connection objects for each connection.

**-** Tune the database connection pool min and max values to keep database performance efficient.

**-** Set transaction isolation level properly, if it is not required then do not use SERIALIZABLE isolation level because it is not performance efficient.

**-** Commit more frequently instead of keeping the locks for a longer time period.

**-** Make data load select operation with a fetch size and pagination instead of loading the whole data from the database at once for large data sets.

**-** Lazy load unnecessary data as much as possible.

## Threading

**-** Use thread pool to keep a number of worker threads for task processing.

**-** Set corePoolSize and maximumPoolSize values for ThreadPoolExecutor when initializing it with reasonable numbers to get maximum performance benefit.

**-** Prefer Atomic classes which utilize &quot;Compare and Swap&quot; instead of synchronized methods when appropriate.

**-** Prefer ConcurrentHashMap instead of Collections.synchronizedMap.

**-** Create or use Immutable thread-safe types in multi-threaded environments.

**-** Use jconsole utility to learn about the total number of currently running threads of a Java process.

**-** To take a full thread dump of a running Java process use jstack with a process-id.

> jstack 11256 </br>
2021-08-01 17:43:36 </br>
Full thread dump OpenJDK 64-Bit Server VM (11.0.10+9-LTS mixed mode): </br>
Threads class SMR info: </br>
_java_thread_list=0x00000180bae525e0, length=37, elements={ </br>
0x00000180b5b15000, 0x00000180b5b16000, 0x00000180b5b6b800, 0x00000180b5b6d000 </br>
} </br>
"Reference Handler" #2 daemon prio=10 os_prio=2 cpu=0.00ms elapsed=591.52s tid=0x00000180b5b15000 nid=0x4a4c waiting on condition  [0x000000655b8ff000] </br>
   java.lang.Thread.State: RUNNABLE </br>
        at java.lang.ref.Reference.waitForReferencePendingList(java.base@11.0.10/Native Method) </br>
        at java.lang.ref.Reference.processPendingReferences(java.base@11.0.10/Reference.java:241) </br>
        at java.lang.ref.Reference$ReferenceHandler.run(java.base@11.0.10/Reference.java:213) </br>
</br>

**-** Know that thread-safety comes with a performance cost.

## GC Efficiency

**-** Create smaller objects.

**-** Do not nest Java types.

**-** Prefer flat objects.

**-** Select appropriate GC algorithm.

**-** Use "jps -lv" from command line to get list of running Java processes.

**-** Monitor GC activity by using jstat utility coming inside JDK. jstat will print a new line to the standard output about current GC activity.

> jstat -gc -t 11008 1s </br>
Timestamp   S0C    S1C     S0U    S1U      EC       EU        OC         OU       MC        MU     CCSC   CCSU        YGC   YGCT    FGC    FGCT    CGC    CGCT     GCT </br>
26948,1     0,0   10240,0  0,0   10240,0 123904,0 112640,0  128000,0   96768,1   128616,0 116253,1 17560,0 14191,8     31    0,620   0      0,000   -      -      0,620 </br>
26949,1     0,0   10240,0  0,0   10240,0 123904,0 112640,0  128000,0   96768,1   128616,0 116253,1 17560,0 14191,8     31    0,620   0      0,000   -      -      0,620 </br>
26950,1     0,0   10240,0  0,0   10240,0 123904,0 112640,0  128000,0   96768,1   128616,0 116253,1 17560,0 14191,8     31    0,620   0      0,000   -      -      0,620 </br>

**-** Collect GC activities in a separate log file by setting JVM argument : -Xlog:gc*:gc.log:time,pid,tags,uptime,level </br>
>[2021-11-12T13:41:08.841+0300][2.220s][1980][info][gc,start     ] GC(3) Pause Young (Concurrent Start) (Metadata GC Threshold) </br>
[2021-11-12T13:41:08.841+0300][2.220s][1980][info][gc,task      ] GC(3) Using 6 workers of 8 for evacuation </br>
[2021-11-12T13:41:08.847+0300][2.226s][1980][info][gc,phases    ] GC(3)   Pre Evacuate Collection Set: 0.0ms </br>
[2021-11-12T13:41:08.847+0300][2.226s][1980][info][gc,phases    ] GC(3)   Evacuate Collection Set: 5.2ms </br>
[2021-11-12T13:41:08.847+0300][2.226s][1980][info][gc,phases    ] GC(3)   Post Evacuate Collection Set: 0.9ms </br>
[2021-11-12T13:41:08.847+0300][2.226s][1980][info][gc,phases    ] GC(3)   Other: 0.2ms </br>
[2021-11-12T13:41:08.847+0300][2.226s][1980][info][gc,heap      ] GC(3) Eden regions: 50->0(107) </br>
[2021-11-12T13:41:08.847+0300][2.226s][1980][info][gc,heap      ] GC(3) Survivor regions: 3->5(15) </br>
[2021-11-12T13:41:08.847+0300][2.226s][1980][info][gc,heap      ] GC(3) Old regions: 12->12 </br>
[2021-11-12T13:41:08.847+0300][2.226s][1980][info][gc,heap      ] GC(3) Humongous regions: 4->4 </br>
[2021-11-12T13:41:08.847+0300][2.226s][1980][info][gc,metaspace ] GC(3) Metaspace: 20598K->20598K(1069056K) </br>

**-** Display current active GC activity log on application console by setting "-verbose:gc" flag. </br>
> -verbose:gc </br>
[2.904s][info][gc] GC(5) Pause Young (Normal) (G1 Evacuation Pause) 130M->23M(254M) 7.720ms </br>
[3.125s][info][gc] GC(6) Pause Young (Normal) (G1 Evacuation Pause) 167M->23M(254M) 6.691ms </br>
[3.872s][info][gc] GC(7) Pause Young (Concurrent Start) (Metadata GC Threshold) 138M->25M(305M) 12.619ms </br>
[3.873s][info][gc] GC(8) Concurrent Cycle </br>
[3.883s][info][gc] GC(8) Pause Remark 25M->25M(305M) 3.584ms </br>
[3.885s][info][gc] GC(8) Pause Cleanup 25M->25M(305M) 0.089ms </br>


## Memory Space Efficiency

**-** Null instance variables consume memory, if null instance variables are not used then remove them from the enclosing class definition.

## JVM Efficiency

**-** Set min and max heap sizes for your application with JVM flags Xms and Xmx.

**-** Turn -XX:+HeapDumpOnOutOfMemoryError JVM flag on to produce automatic heap dump when a memory problem occurs.

**-** Set -XX:+HeapDumpOnOutOfMemoryError and -XX:HeapDumpPath JVM options to generate a heap dump for extra heap consumption cases.

**-** Use command line tools to analyze heap and thread dumps.

**-** Use "jps -lv" from command line to get list of running Java processes.

**-** Generate heap histogram and analyze number of instances for each Java object by using jcmd.

> jcmd 17472 GC.class_histogram </br>
17472: </br>
 num     #instances         #bytes  class name (module)  </br>
-------------------------------------------------------  </br>
   1:         73517        8531872  [B (java.base@11.0.10)  </br>
   2:         22482        1978416  java.lang.reflect.Method (java.base@11.0.10)  </br>
   3:         69712        1673088  java.lang.String (java.base@11.0.10)  </br>
</br>

**-** To run a full GC before generating the heap histogram, excute jmap with -histo:live parameter.

> jmap -histo:live 17472 </br>
 num     #instances         #bytes  class name (module) </br>
------------------------------------------------------- </br>
   1:         73680        8554400  [B (java.base@11.0.10) </br>
   2:         22482        1978416  java.lang.reflect.Method (java.base@11.0.10) </br>
   3:         69875        1677000  java.lang.String (java.base@11.0.10) </br>
</br>

**-** Generate heap dump file by using jmap and running Java process id.

> jmap -dump:live,file=heap_dump.hprof 17472 </br>
Heap dump file created </br>

**-** Use eclipse MAT to analyze generated heap dump file.


## Log Generation

**-** Use a centralized logging mechanism.

**-** Do not log unnecessary information.

**-** Do not use VERBOSE, DEBUG, INFO levels for production environment.

**-** Do not use System.out for logging to the console instead select a logging framework with a customizable log level feature.

## Sorting

**-** java.util.Arrays.sort method is optimized for item count therefore use Arrays.sort for better performance achievement. Know that java.util.Arrays.sort is optimized for a list of items containing less than 47 elements to use insertion sort instead of quicksort.

## JVM Performance Analysis Monitoring Tools

**-** [VisualVM](https://visualvm.github.io/)

**-** [GCViewer](https://github.com/chewiebug/GCViewer)

**-** [The Eclipse Memory Analyzer](https://www.eclipse.org/mat/)

**-** [jconsole](https://openjdk.java.net/tools/svc/jconsole/)
