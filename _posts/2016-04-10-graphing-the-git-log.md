The default git log is useful but not necessarily optimized for day-to-day reviewing of history. If you need a subsef ot the information displayed in each commit you can use `git log --oneline`, this will only display abrreviated commit hash and commit message subject (that are the first 50 characters of your commit message). 

But lets get interesting, the following will proved a nicely organized and concise view of the log:

{% highlight bash %}
$ git log --oneline --decorate --graph --all -30
{% endhighlight %}

Let's break this down:

* `online`: This display abbreviate commit hash & message
* `decorate`: Display the local and remote branches along with the commit hash
* `graph`: Draw the commits with ASCII art lines to identify branching
* `all`: Show the history of all branches, **not just the current branch**
* < number >: Show only the specified number of commits, rather than paging through all commits

Of course all of this is a bit of handful to type out, `git alias`to the rescue for sure: 

{% highlight bash %}
$ git config --global alias.sla 'log --oneline --decorate --graph --all'
{% endhighlight %}

For now on we can just use `git sla` which wraps all of those options. 


