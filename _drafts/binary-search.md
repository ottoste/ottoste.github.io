---
layout: post
title:  Binary Search Algorithm
date:   2015-11-03 12:48:25
author: Otto Täht
categories: elixir 
---
#### Description

Binary search is an efficient algorithm for finding an item from an ordered list of items. It works by repeatedly dividing in half the portion of the list that could contain the item, until you've narrowed down the possible locations to just one. 

#### Example
Let’s say we have a sorted list of numbers from 1 to 300.

List = [1…300]

And we want to find out on which index we have number 272.

Target = 272

Using linear search, in other words going through the list one-by-one, we would end up with 272 guesses, before we reach our goal.

So to get started, lets define a function and see step-by-step, how it reaches our goal. Our function takes two arguments, a list and a target number. Based on the list it basically calculates two extra arguments. 

List = range of numbers

Target = value we are looking for

Min = fist position of list = 0

Max = last position of list = length of list -1

{% highlight elixir %}
def search(list, target, max, min) do
    mid = div(min + max, 2)
    guess = Enum.at(list, mid)
    cond do
      target == guess -> mid
      target <  guess -> search(list, target, min, mid - 1)
      target >  guess -> search(list, target, mid + 1, max)
    end
  end
{% endhighlight %}

1. Now lets start off by trying to find our target from list. We have following function:

Bsearch([1…300], 272, 0, 299)

Now we need to find the mid-point of the range:

mid = div((0+299), 2) = 149

And our guess will be the value at index 149:

Guess = Value.at(149)    ->150

Since we know that 272 is bigger than 150 we can carry on with a list [151…300]

2.This time we have function with following arguments:

Bsearch([1…300], 272, 150, 299)

Mid = div((150+299), 2) = 224

Guess = 225

Since we know that 272 is bigger than 225 we can carry on with a list [226…300]

3.

Bsearch([1…300], 272, 225, 299)

Mid = div((225+299), 2) = 262

Guess = 263

Since we know that 272 is bigger than 263 we can carry on with a list [264…300]

4.

Bsearch([1…300], 272, 263, 299)

Mid = div((263+299), 2) = 281

Guess = 282

Since we know that 272 is smaller than 282 we can carry on with a list [264…281]

5.

Bsearch([1…300], 272, 263, 280)

Mid = div((263+299), 2) = 271

Guess =2 72

Guess == Target!

It took us only 5 guesses instead of 272. Not bad.

#### Run-Time

What would be the maximum number of guesses for a list containing 300 elements?

9

But for a list with 2 million elements?

21

That’s because maximum number of binary search guesses equals base-2 logarithm of the number of elements in the list 

1  element  = 0 guesses

2  elements = 1 guess

4  elements = 2 guesses

8  elements = 3 guesses

16 elements = 4 quesses

32 elements = 8 guesses

 












[docs]:      			http://elixir-lang.org/docs.html