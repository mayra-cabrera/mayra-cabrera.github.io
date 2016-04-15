In Ruby, blocks have direct access to variables that already exist. However, block parameters (the variable names between the pipes) behave differently from non-parameter variables. If you have a variable of a given name in scope and also use that name as one of your block parameters, **then the two variables are not the same as each other**.

Check this example: 

{% highlight ruby %}
def block_local_parameter
  x = 100
  [1,2,3].each do |x|
    puts "Parameter x is #{x}"
    x = x + 10
    puts "Reassigned to x in block; it's now #{x}"
  end
  puts "Outer x is still #{x}"
end
{% endhighlight %}

The output from a call to this method is

{% highlight ruby %}
    Parameter x is 1
    Reassigned to x in block; it's now 11
    Parameter x is 2
    Reassigned to x in block; it's now 12
    Parameter x is 3
    Reassigned to x in block; it's now 13
    Outer x is still 100
{% endhighlight %}
The x inside the block isn’t the same as the `x` outside the block, because `x`  is used as a block parameter. Even reassigning to `x` inside the block doesn’t overwrite the “outer” `x`. This behavior enables you to use any variable name you want for your block parameters without having to worry about whether a variable of the same name is already in scope.

### Ruby special notation 

Ruby provides a special notation to indicate that you want one or more variables to be local to the block, **even if variables with the same name already exist**: a semicolon parameter list

{% highlight ruby %}
def block_local_variable
  x = "Original x!"
  3.times do |i;x|
    x = i
    puts "x in the block is now #{x}"
  end
  puts "x after the block ended is #{x}"
end
{% endhighlight %}


The semicolon, followed by `x`, indicates that the block needs its own `x`, **unrelated to any x that may have been created already in the scope outside the block**. In the example, we assign to x inside the block, but these assignments don’t affect the `x` that
existed already. The output shows that the original `x` survives:

{% highlight ruby %}
    x in the block is now 0
    x in the block is now 1
    x in the block is now 2
    x after the block ended is Original x!
{% endhighlight %}

