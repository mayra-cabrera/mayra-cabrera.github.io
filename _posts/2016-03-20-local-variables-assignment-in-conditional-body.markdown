*This is based on [The Well Grounded Rubyist](https://www.manning.com/books/the-well-grounded-rubyist) book, you should definitely buy it*

Ruby doesn’t draw a clear line as compiled languages do between “compile time” and “runtime”, but the interpreter does parse your code before running it and certain decisions are made during that process. An important one is the recognition and allocation of local variables.

Consider this example:


{% highlight ruby %}
if false
  x = 1
end
p x # nil
p y # Fatal error: y is unknown
{% endhighlight %}

`p x` return nils, why? It's wrapped in a failing conditional, right? so is supposed to return an error just like `p y` did. Well not exactly...

You see when the Ruby parser sees an assignment (as in this `x = 1`) it allocates space for the local variable `x`, (and I mean for **the creation of the variable not the assignment of a value to it, but the internal creation of a variable**). This always takes place as a result of this kind of expression, even if the code isn’t executed! The result is that `x inhabits a strange kind of variable limbo.

None of this happens with class, instance, or global variables (@@x, @x, $x), all three of those variable types are recognizable by their appearance. But local variables look just like method calls so Ruby needs to apply some logic at parse time to figure out what’s what.
