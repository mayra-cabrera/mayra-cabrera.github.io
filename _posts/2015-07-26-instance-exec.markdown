You probably had heard about `instance_eval` which evaluates a block in the context of an object:

{% highlight ruby %}
class MyHappyClass
  def initialize
    @a = 1
  end
end

obj = MyHappyClass.new
  obj.instance_eval do
   @a # => 1
  end
end  
{% endhighlight %}

The block is evaluated with the receiver as self, so it can access the receiver’s private methods and instance variables, such as @a. Well 
`instance_eval` has a slightly more flexible twin brother named `instance_exec` that allows us to pass arguments to the block:

{% highlight ruby %}
class A
  def initialize
    @b = 1
  end
end


class B
  def weird_behaviour
    @c = 1
    A.new.instance_eval{"@b: #{@b}, @c: #{@c}"}
  end
end  
{% endhighlight %}
    
What do you think is going to print `weird_behaviour`? You might think that the block in `B#weird_behaviour` can access both the `@b` instance variable from `A`class and the `@c`instance variable from B, but nope :(. 

{% highlight ruby %}
  B.new.weird_behaviour # => "@b: 1, @c: "
{% endhighlight %}

When it tries to print `@c` instance variable is going to print an empty space, why? because of this: since instance variables depend on self, when `instance_eval` switches `self` to the receiver, all the instances variables in the caller fall out of scopes, this means the code inside the block interprets `@c` as a instance variable of `A` (and because it wasn't initialized that means is nil and prints out an empty string!)

This is where `instance_exec` comes to the rescue! We can use it to merge `@b` and `@c` in the same scope like this: 

{% highlight ruby %}
class B
  def weird_behaviour
    @c = 1
    A.new.instance_exec(@c){|c| "@b: #{@b}, @c: #{@c}"}
 end
end

B.new.weird_behaviour # => "@b: 1, @c: 1" 
{% endhighlight %}

I don't know a real example of how use `instance_exec` can be useful, but hey doesn't hurt to know more huh? :)
