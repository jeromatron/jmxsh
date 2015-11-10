# Introduction #

This is a short script to invoke garbage collection on a list of remote servers.


# Details #

```
#
# Script for remotely invoking garbage collection on a set of servers.
#

set hosts [list]
lappend hosts {localhost:8082}
#lappend hosts {some.other.host.com:####}
# ...add as many as you want...

foreach {host} $hosts {
  set parts [split $host ":"]
  set hostname [lindex $parts 0]
  set port [lindex $parts 1]

  # for each host...

  # Connect to it.
  jmx_connect -h $hostname -p $port

  # Invoke the garbage collector.
  jmx_invoke -n -m java.lang:type=Memory gc

  # Close this connection
  jmx_close
}
```

If the above were saved as "invoke\_gc.jmxsh", then it may be invoked with:

```
$ java -jar jmxsh.jar invoke_gc.jmxsh
```