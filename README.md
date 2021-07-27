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


## Java SE

**-** Prefer java.util.concurrent.ThreadLocalRandom for muti-threaded environments instead of java.util.Random .

**-** Avoid JNI usage to call native C code from Java. Run Java code on JVM to get maximum performance benefits.

**-** Do not use Exception throwing mechanism for messaging purposes.

**-** Do not create String by concatenation instead use StringBuilder object for increased performance. Know that StringBuilder is not synchronized.

**-** Do not append to a String inside a for-while loop .

**-** Prefer StringBuffer in a multi-threaded environment and know that synchronization comes with additional performance cost.

**-** StringBuilder is faster than StringBuffer because StringBuilder is not thread-safe.

**-** Prefer lambda expressions over anonymous classes.

**-** Use enumerated integers defined in a constant interface instead of constant String values for speed and memory benefits. Take performance advantage of comparing enumerated objects by identity comparison using "==" operator instead of equals() method.

**-** Prefer primitive data types instead of Wrappers as class members. For instance, use "int" for "Integer" or "double" for "Double" as a class member.

**-** If components of String concatenation is known at compile-time then prefer String concatenation "+" operator. However, if the String components can not be resolved at compile-time then use StringBuffer to produce the result String.

**-** Avoid casting explictly in a try block and throwing new exception in catch block, instead prefer instanceOf operator for type checking.

**-** When iterating over a collection do not use collection.size() as loop termination test in the for-while loop, instead assign the collection.size() to a variable outside the loop and use this variable as loop termination condition.

**-** Prefer int data type as loop index variable instead of other numeric data types for better performance.

**-** Prefer using interfaces and type overloading against runtime-reflection for performance gains.

**-** If it is not required then do not define new @annotations and process them at run-time.

**-** Do not serialize unnecessary fields and mark them with transient.


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

**-** Prefer Atomic classes which utilize &quot;Compare and Swap&quot; instead of synchronized methods when appropriate.

**-** Know that thread-safety comes with a performance cost.

## GC Efficiency

**-** Create smaller objects.

**-** Do not nest Java types.

**-** Prefer flat objects.

**-** Select appropriate GC algorithm.

## Memory Space Efficiency

**-** Null instance variables consume memory, if null instance variables are not used then remove them from the enclosing class definition.

## JVM Efficiency

**-** Set min and max heap sizes for your application with JVM flags Xms and Xmx.

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

**-** Turn -XX:+HeapDumpOnOutOfMemoryError JVM flag on to produce automatic heap dump when a memory problem occurs.



## Log Generation

**-** Use a centralized logging mechanism.

**-** Do not log unnecessary information.

**-** Do not use VERBOSE, DEBUG, INFO levels for production environment.

**-** Do not use System.out for logging to the console instead select a logging framework with a customizable log level feature.

## Sorting

**-** java.util.Arrays.sort method is optimized for item count therefore use Arrays.sort for better performance achievement. Know that java.util.Arrays.sort is optimized for a list of items containing less than 47 elements to use insertion sort instead of quicksort.
