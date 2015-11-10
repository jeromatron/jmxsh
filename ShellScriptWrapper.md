I've written and uploaded a shell script wrapper.  To use it, just make it executable and edit it to point to your copy of jmxsh.jar.  I haven't figured out yet how to make it usable in the #! line of an executable text file, sorry about that.

It's not much, but it allows you to type things like:

```
$ jmxsh
$ jmxsh -h youhost.yourdomain.com -p 9004
$ jmxsh yourscript.jmxsh
```

and create scripts like:

```
# yourscript.jmxsh
# Sample script for connecting to multiple hosts.

set hosts [list]
lappend hosts {restaurant1.abcb.com:9004}
lappend hosts {restaurant2.abcb.com:9004}
lappend hosts {restaurant3.abcb.com:9004}

foreach {host} $hosts {
  set parts [split $host ":"]
  set hostname [lindex $parts 0]
  set port [lindex $parts 1]

  # for each jmx host...

  # Connect to it.
  jmx_connect -h $hostname -p $port

  # Assume getWaitresses returns a java String array
  set java_waitresses [jmx_invoke -n -m com.abcb.Management:type=Staff getWaitresses]
  # Need to use Java/tcl to convert to Tcl list of strings
  set tcl_waitresses [$java_waitresses getrange]

  # List the Waitresses
  puts "Waitresses for $hostname:"
  foreach {waitress} $tcl_waitresses {
    puts "$waitress"
    # Slap Kyousuke
    if {$waitress == "kyousuke"} {
      slap $waitress
    }
  }

  # Do other stuff with this JMX server.
  do_other_stuff_with_this_JMX_server

  # Close this connection
  jmx_close
}
```

Here's the code for the wrapper itself.  Comments welcome!

```
#!/bin/sh
#
# jmxsh
#
# A shell wrapper for jmxsh.
#
# Assumes java is in the PATH.  If not, will need to edit this script.
#
# You'll need to modify this when you find a place to jmxsh.
#
JMXSH_JARFILE=./jmxsh.jar

exec java -jar $JMXSH_JARFILE "$@"
```