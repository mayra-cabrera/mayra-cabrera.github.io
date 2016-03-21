---
layout: post
title: Local variables assignment in conditional body
modified: 2016-20-03
tags: [Ruby, Rails]
comments: true
---


Ruby doesn’t draw a clear line as compiled languages do between “compile time” and “runtime”, but the interpreter does parse your code before running it and certain decisions are made during that process. An important one is the recognition and allocation of local variables.

Consider this example:


```ruby
  if false
	  x = 1
  end
  p x # nil
  p y # Fatal error: y is unknown
```

`p x` return nils, why? Is never assigned because it's wrapped in a failing conditional, right? so is supposed to return an error just like `p y` did, well not exactly. When the Ruby parser sees an assignment as in this `x = 1` it allocates space for the local variable `x`, the creation of the variable —not the assignment of a value to it, but the internal creation of a variable— always takes place as a result of this kind of expression, even if the code isn’t executed! The result is that `x inhabits a strange kind of variable limbo.

None of this happens with class, instance, or global variables. All three of those
variable types are recognizable by their appearance (@@x, @x, $x). But local variables look just like method calls. Ruby needs to apply some logic at parse time to figure out what’s what, to as great an extent as it can.
