

NAME

    jmxstat - Poll JMX attributes and more

SYNOPSIS

    java -jar jmxstat.jar host:port [mbean-attributes...] [--contention] [--disable-contention] [--performGC] [interval [count]]

DESCRIPTION

    jmxstat connects to a remote JMX server via RMI and reads MBean attributes
    at a regular interval. Authentication is not supported.
    
    jmxstat supports these arguments:
    
    host:port            Host and port of the JMX enabled process to connect to
    
    mbean-attributes     Space separated list of mbean names and attributes to
                         query in the following format:
    
                            mbeanDomain:mbeanKey=mbeanValues,...[mbeanAttribute1[.field],...]

    --contention         Render blockedCount and blockedTime for all threads.
                         blockedCount is the total number of times threads
			 blocked to enter or reenter a monitor.
			 blockedTime is the teim elapsed in milliseconds for
			 all threads blocked to enter or reenter a monitor.

    --disable-contention Disable the blocked time monitoring activated by the 
                         --contention options

    --performGC          Perform a full GC

    interval             Pause _interval_ seconds between each query

    count                Select count records at interval second intervals

EXAMPLES

    **Precondition**. These examples assume a JMX enabled process is listening
    on localhost:9999. To use jmxstat itself as the monitored process:
    
        java -Dcom.sun.management.jmxremote.port=9999 -Dcom.sun.management.jmxremote.authentice=false -Dcom.sun.management.jmxremote.ssl=false -jar jmxstat.jar localhost:9999

    Display the number of loaded classes (every 5 seconds, by default):
    
        java -jar jmxstat.jar localhost:9999 java.lang:type=ClassLoading[LoadedClassCount]
    
    Display heap usage and thread count every 2 seconds 3 times:
    
        java -jar jmxstat.jar localhost:9999 java.lang:type=Memory[HeapMemoryUsage.max,HeapMemoryUsage.committed,HeapMemoryUsage.used] java.lang:type=Threading[ThreadCount] 2 3

    Display thread count and contention information

        java -jar jmxstat.jar localhost:9999 --contention java.lang:type=Threading[ThreadCount] 2 3
 
    Perform a Full garbage collector

        java -jar jmxstat.jar localhost:9999 --performGC
