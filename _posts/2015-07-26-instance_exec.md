---
layout: post
title: Instance exec - The forgotten twin brother of instance_eval
description: Instance eval and another way to mix code
modified: 2015-07-26
tags: [blocks, ruby, instance_eval]
comments: true
---
You probably had heard about `instance_eval` which evaluates a block in the context of an object:

{% gist mayra-cabrera/4ff5daae60b483d3bd8b %}

The block is evaluated with the receiver as self, so it can access the receiver’s private methods and instance variables, such as @a. Well 
`instance_eval` has a slightly more flexible twin brother named `instance_exec` that allows us to pass arguments to the block:


{% gist mayra-cabrera/47fe62080edd477a5d13 %}

    
What do you think is going to print `weird_behaviour`? You might think that the block in `B#weird_behaviour` can access both the `@b` instance variable from `A`class and the `@c`instance variable from B, but nope :(. 

    B.new.weird_behaviour # => "@b: 1, @c: "

When it tries to print `@c` instance variable is going to print an empty space, why? because of this: since instance variables depend on self, when `instance_eval` switches `self` to the receiver, all the instances variables in the caller fall out of scopes, this means the code inside the block interprets `@c` as a instance variable of `A` (and because it wasn't initialized that means is nil and prints out an empty string!)

This is where `instance_exec` comes to the rescue! We can use it to merge `@b` and `@c` in the same scope like this: 

{% gist mayra-cabrera/fd8e0e87132eb4e44d13 %}

I don't know a real example of how use `instance_exec` can be useful, but hey doesn't hurt to know more huh? :)