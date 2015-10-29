---
layout: post
title:  Anagram Checker 
date:   2015-10-28 
author: Otto Täht
categories: elixir 57exercises
---

This is the 24th exercise out of 57 from Brian B. Hogans book [Exercises for Programmers][1]. I try to guide you through the process how I ended up with the solution. "I" is rather egoistic phrase to use here since most of the exercise was done while pairing with Mateu. 

But for starters the exercise itself:

####Anagram Checker

Create a program that compares two strings and determines if the two strings are anagrams. The program should prompt for both input strings and display the output as shown in the example below.

####Example Output

Enter the first string: note
Enter the second string: tone
"note" and "tone" are anagrams.

####Constraints

1.Implement the program using a function called isAnagram, which takes in two words as its arguments and returns true or false. Invoke this function from your main program.

2.Check that both words are the same length.

### Solution

We decided not to worry about prompting user and giving output in the beginning and only focus on the function is_anagram, which is described above. It takes in two words and returns true or false.

We started off by trying to put on paper all possible cases, that we could think of. And started writing tests to cover all cases. 

Some trivial cases:

1. "a", "a" -> true

2. "a", "b" -> false

{% highlight elixir %}
test "two words of one character being the same" do
  assert AnagramChecker.is_anagram("a", "a") == true
end

test "two words of one character being different" do
  assert AnagramChecker.is_anagram("a", "b") == false
end
{% endhighlight %}

Some cases that need already a little more complicated code to pass:

1. "ab", "ba" -> true

2. "ab", "cb" -> false

{% highlight elixir %}
test "two words of two characters in different order" do
  assert AnagramChecker.is_anagram("ab", "ba") == true
end

test "two words of two characters being different" do
  assert AnagramChecker.is_anagram("ab", "cb") == false
end
{% endhighlight %}

And finally tests that in our opinion cover all possible cases:

1. "aba", "bab" -> false

2. "note", "noted" -> false

{% highlight elixir %}
test "two words of three characters having different character count" do
  assert AnagramChecker.is_anagram("aba", "bab") == false
end

test "two words with different length" do
	assert AnagramChecker.is_anagram("note", "noted") == false
end
{% endhighlight %}

The first test of the last two was neccessary, because until that point we could get all tests green by simply checking if the first string contains characters from the second string. So we had to come up with something, which would work for words with any length and basically check if the counts of each character in both strings are equal. We decided to convert both strings to lists, sort the lists and compare them to each other. So whenever you take two anagrams and sort them alphabetically you'll end up with two identical strings or in that case lists. Genius.

So here's the function is_anagram we came up in the end:
{% highlight elixir %}
def is_anagram(first_word, second_word) do
	first_word_as_list = String.codepoints(first_word) 
  second_word_as_list = String.codepoints(second_word) 
		
  first_word_sorted = Enum.sort(first_word_as_list)
  second_word_sorted = Enum.sort(second_word_as_list)
		
	first_word_sorted == second_word_sorted
{% endhighlight %}

Now I had to prompt the user for two input words. I pipe the user input to String.strip to get rid of "/n" in the end of the string.
{% highlight elixir %}
def prompt_for_first_word do
  IO.gets("Enter the first word:") |>String.strip
end

def prompt_for_second_word do
  IO.gets("Enter the second word:") |>String.strip
end
{% endhighlight %}

Next I defined function display(not a good name for that one probably), which constructs two different output messages depending on the result of is_anagram function.
{% highlight elixir %}
def display(first_word,second_word) do
  if is_anagram(first_word, second_word) == true do
    IO.puts("\"#{first_word}\" and \"#{second_word}\" are anagrams.")
  else
    IO.puts("\"#{first_word}\" and \"#{second_word}\" are not anagrams.")
  end
end
{% endhighlight %}

And to tie it all together defined a function called combine, which passes the words inserted by user to the display function.
{% highlight elixir %}
def combine do
  display(prompt_for_first_word, prompt_for_second_word)
end
{% endhighlight %}


[1]:  https://pragprog.com/book/bhwb/exercises-for-programmers