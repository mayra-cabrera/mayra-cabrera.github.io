---
layout: post
title: Scopes and class methods
description: ActiveRecord scopes vs Class Methods (They are not the same!)
modified: 2015-08-04
tags: [ActiveRecord, Ruby, Rails]
comments: true
---

So today at work we were wondering if ActiveRecord scopes behave like class methods. You may have heard that scopes are just syntactical sugar for class methods, in other words that this:

{% gist mayra-cabrera/5ed21f4bf2864398e095 %}

Could be written also as like this:

{% gist mayra-cabrera/499dbce3ec52a3ed4619 %}

And yes the behaviour will be the same....mostly! Why you might not know is that between scopes and class methods they are subtle differences.

Check the following class methods, one just returns modern cabs and another returns cabs driven by an specific person

{% gist mayra-cabrera/936a91e45a251b4cc344 %}

If we used them together `Cab.by_driver("Mayra").modern`, the following query will be generated:


    SELECT "cabs".* FROM "cabs" WHERE "cabs"."driver" = 'Mayra' AND (aged > '...')

Everything works as aspected right? Lets keep working and see what would happen if we continue to use class methods. Imagine that we use the former class methods in a controller, using `params`to set the driver name, and somehow a `nil` value is passed: 
 

    # params[:driver_name] => nil
    Cab.by_driver(params[:author_name]).modern


Well this is where things get start to get messy, the following query will result like this:

    SELECT "cabs".* FROM "cabs" WHERE "cabs"."driver" IS NULL AND (aged > '...')


This will return an array of cabs with **no driver** instead of cabs from **all drivers**. So whats the big deal you'll say? Just add a guard validation inside `by_driver` method


{% gist mayra-cabrera/2adde38eb97df60d7fb9 %}

Again: 

    # params[:author_name] #=> nil
    Cab.by_driver(params[:author_name]).modern


And we get an error :(: 

     undefined method `modern` for NilClass

What happened? Here is what: `by_driver` will return `nil` because `driver` is not present, so when we call `modern` on `nil` we get the error. So ok we can still fix this with a couple of extra code:

{% gist mayra-cabrera/f707955ca1e4daf3d4ab %}


If driver is not present, we'll return the `all` chainable relation, and finally when we do this



    # params[:driver_name] => nil
    Cab.by_driver(params[:author_name]).modern


We are going to get what we actually intended in the beginning:

    SELECT "cabs".* FROM "cabs" WHERE (aged > '...')

Even if is working now, **all this could have actually been easily resolved with scopes**: 


{% gist mayra-cabrera/adb9a7876ff174d91281 %}

Because scopes always return a chainable object ;) 
