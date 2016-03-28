So today at work we were wondering if ActiveRecord scopes behave like class methods. You may have heard that scopes are just syntactical sugar for class methods, in other words that this:

{% highlight ruby %}
class Cab
  scope :modern, -> { where('aged > ?', 2.years.ago) }
end
{% endhighlight %}

Could be written also as like this:


{% highlight ruby %}
class Cab
  def self.modern
    where('aged < ?', 2.years.ago) }
  end
end
{% endhighlight %}

And yes the behaviour will be the same....mostly! Why you might not know is that between scopes and class methods they are subtle differences.

Check the following class methods, one just returns modern cabs and another returns cabs driven by an specific person

{% highlight ruby %}
class Cab
  def self.modern
    where('aged < ?', 2.years.ago) }
  end

  def self.by_driver driver
    where(driver: driver)
  end
end
{% endhighlight %}

If we used them together `Cab.by_driver("Mayra").modern`, the following query will be generated:

{% highlight sql %}
SELECT "cabs".* FROM "cabs" WHERE "cabs"."driver" = "Mayra" AND (aged > "...")
{% endhighlight %}


Everything works as aspected right? Lets keep working and see what would happen if we continue to use class methods. Imagine that we use the former class methods in a controller, using `params`to set the driver name, and somehow a `nil` value is passed: 

{% highlight ruby %}
# params[:driver_name] => nil
Cab.by_driver(params[:author_name]).modern
{% endhighlight %}


Well this is where things get start to get messy, the following query will result like this:

{% highlight sql %}
SELECT "cabs".* FROM "cabs" WHERE "cabs"."driver" IS NULL AND (aged > '...')
{% endhighlight %}

This will return an array of cabs with **no driver** instead of cabs from **all drivers**. So whats the big deal you'll say? Just add a guard validation inside `by_driver` method

{% highlight ruby %}
def self.by_driver driver
  where(driver: driver) if driver.present? # Returns nil if no driver is present
end
{% endhighlight %}

Again: 

{% highlight ruby %}
# params[:author_name] #=> nil
Cab.by_driver(params[:author_name]).modern
{% endhighlight %}

And we get an error :( : 

{% highlight ruby %}
undefined method `modern` for NilClass
{% endhighlight %}


So what happened? Here is what: `by_driver` will return `nil` because `driver` is not present, so when we call `modern` on `nil` we get the error. So ok we can still fix this with a couple of extra code:

{% highlight ruby %}
def self.by_driver driver
  if driver.present?
    where(driver: driver)
  else
    all
  end
end
{% endhighlight %}


If driver is not present, we'll return the `all` chainable relation, and finally when we do this

{% highlight ruby %}
# params[:driver_name] => nil
Cab.by_driver(params[:author_name]).modern
{% endhighlight %}


We are going to get what we actually intended in the beginning:

{% highlight sql %}
SELECT "cabs".* FROM "cabs" WHERE (aged > '...')
{% endhighlight %}

Even if is working now, **all this could have actually been easily resolved with scopes**: 

{% highlight ruby %}
class Cab < ActiveRecord
  scope :by_driver, ->(driver) { where(driver: driver) }
  scope :modern, -> { where('aged > ?', 2.years.ago) }
end  
{% endhighlight %}

Because scopes always return chainables objects ;) 
