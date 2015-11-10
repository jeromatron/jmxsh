# Printing a Java String Array #

Q. Suppose the following JMX command returns a Java array of Strings:

`jmx_invoke -m com.my.stuff:type=MyBean getStringList`

Only it looks something like: "[Ljava.lang.String;@1c9b9ca"

How can you get at the real strings?

A. Use the Java/Tcl interface to Java objects.

```
set java_obj [jmx_invoke -n -m com.my.stuff:type=MyBean getStringList]
# now java_obj is a Java/Tcl "object"
set string_list [$java_obj getrange]
# now string_list is a simple Tcl list of strings that you can 
# do whatever you want with
```

Note that you have to pass "-n" to the jmx\_invoke command.  This tells it to give you the Java/Tcl object back, and not just the result of an Object.toString() call.