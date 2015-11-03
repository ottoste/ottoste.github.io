---
layout: post
title:  Word Finder 
date:   2015-10-30 
author: Otto Täht
categories: elixir 
---
This exercise required me to write a program, that finds how many times the word, we are looking for, occurs in some text. So it takes in two arguments:

1. Text
2. Target word

And gives us output saying, how many times that word appears in the text.

#### 1.Making list of text.

To begin with, I realised that the program should convert the text into a list containing words. So I started writing tests:

This one simply tests, if the function is splitting the string into a list, which has words as elements.
{% highlight elixir %}
test "makes list of first string" do
  assert WordFinder.make_list_of_strings("hello world") == ["hello", "world"]
end
{% endhighlight %}

In our case, "Hello" and "hello" are the same thing, regardless the upper/lowercase letters. So let's convert all letters to downcase. Note: we have to do the same thing for user-input. 
{% highlight elixir %}
test "need to remove uppercase letters from string" do
  assert WordFinder.make_list_of_strings("Hello") == ["hello"]
end
{% endhighlight %}

Since we are splitting the string at whitespaces, punctuation gets included in words also. That means we need to remove punctuation before splitting.
{% highlight elixir %}
test "need to remove punctuation from string" do
  assert WordFinder.make_list_of_strings("Hello, world!") == ["hello","world"]
end
{% endhighlight %}

The function I ended up is below. I've written the result of each line after "->". 
{% highlight elixir %}
# Lets say contents = "Hello, world!"
def make_list_of_strings(contents) do
  String.downcase(contents) -> "hello, world!"
  |> String.codepoints  -> ["h","e","l","l","o",","," ","w","o","r","l","d","!"]
  |> Enum.filter(fn c -> c =~ ~r/[a-z]/ or c =~ ~r/[ ]/ end)  -> ["h","e","l","l","o"," ","w","o","r","l","d",]
  |> List.to_string   -> "hello world"
  |> String.split(" ")  -> ["hello", "world"]
end
{% endhighlight %}

#### 2.Counting 
Next we need to find out how many times our target word occurs in the list. I wrote following tests to check this function:

{% highlight elixir %}
 test "second word appears in first string once" do
  assert WordFinder.get_count_of_word_appearance(["hello", "world"], "world") == 1
end

test "second word appears in first string more than once" do
  assert WordFinder.get_count_of_word_appearance(["hello","world","world","world",], "world") == 3
end

test "second word appears in first string, but is a part of different word" do
  assert WordFinder.get_count_of_word_appearance(["hand"], "and") == 0
end
{% endhighlight %}


Luckily, there is a built in function in Elixir, which we can use:
  
  <strong>Enum.count(collection, function)</strong> - counts the elements in collection, for which the function returns true. 

{% highlight elixir %}
def get_count_of_word_appearance(list_of_first_string, target_word) do
  Enum.count(list_of_first_string, fn(x) -> x == (target_word) end)
end
{% endhighlight %}

#### 3.Build output message

{% highlight elixir %}
def print_the_word_count(count) do
  IO.puts("The word has been found #{count} times")
end
{% endhighlight %}

#### 4.Combining all previous functions

{% highlight elixir %}
def display_word_count(text, word) do
  make_list_of_strings(text)
  |> get_count_of_word_appearance(word)
  |> print_the_word_count
end
{% endhighlight %}


