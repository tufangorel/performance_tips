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

## Collections

**-** Prefer ArrayList for accessing the nth element by index value to get benefit of constant access time cost.

**-** Prefer ArrayList for index based search operations over LinkedList which has a linear time cost.

**-** Prefer LinkedList for frequent insert and delete operations over ArrayList.

**- Not :** Insertion time for ArrayList: O(n). Insertion time for LinkedList: O(1).

**- Not :** Deletion time for ArrayList: Access time + O(n). Deletion time for LinkedList: Access time + O(1).

**- Not :** Access time for ArrayList: O(1). Access time for LinkedList: O(n).

**-** Use unsynchronized Collection types for better performance achievements in single threaded environments.

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

## Log Generation

**-** Use a centralized logging mechanism.

**-** Do not log unnecessary information.

**-** Do not use VERBOSE, DEBUG, INFO levels for production environment.

## Sorting

**-** java.util.Arrays.sort method is optimized for item count therefore use Arrays.sort for better performance achievement. Know that java.util.Arrays.sort is optimized for a list of items containing less than 47 elements to use insertion sort instead of quicksort.
