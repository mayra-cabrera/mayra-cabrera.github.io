---
layout: post
title: Modules and Lookup path
description: Modules ancestors chain
modified: 2016-17-03
tags: [Ruby, Rails]
comments: true
---

*This is based on [The Well Grounded Rubyist](https://www.manning.com/books/the-well-grounded-rubyist) book, you should definitely buy it*

When you include a module in a class (or even in another module), Ruby inserts the module in the ancestors chain, right above the including class itself:

    module M1
      def my_awesome_method
        "M1#my_awesome_method"
      end
    end

    class C
      include M1
    end

    class D < C; end

    D.ancestors # => [D, C, M1, Object, Kernel, BasicObject]

Starting from Ruby 2.0 you also have a second way to insert a module in a class's chain of ancestors: the `prepend` method. It works like include but it inserts the module below the including class, rather than above it:

    module M2
      def my_awesome_method
        "M2#my_awesome_method"
      end
    end

    class C2
      prepend M2
    end

    class D2 < C2; end

    D2.ancestors # => [D, M2, C2, Object, Kernel, BasicObject]
