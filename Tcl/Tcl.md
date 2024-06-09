# Introduction to Tcl

```Tcl

# initialize a variable
> set a 99
99

# command substitution
set b [expr $a*20]

# backslash substitution
set x \$a
$a

set newline \n



# print a string or variable value
> puts "Hello World!"
Hello World

> puts $a
99

# evaluate expressions
> expr 2 + 2
4

# writing a procedure
proc factorial {val} {
  set result 1
  while {$val>0} {
    set result [expr $result*$val]
    incr val -1
  }
  return $result
}

```
