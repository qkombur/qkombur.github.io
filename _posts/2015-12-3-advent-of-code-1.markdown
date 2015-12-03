---
layout: post
title:  "Advent of Code - Day 1"
date:   2015-12-03
categories: puzzle
---

# [Advent of Code Day 1](http://adventofcode.com/day/1)

Advent of code is like a festive project eular. As such, I suggest you drink eggnog while coding solutions.

>Here's an easy puzzle to warm you up.

>Santa is trying to deliver presents in a large apartment building, but he can't find the >right floor - the directions he got are a little confusing. He starts on the ground floor (floor 0) and then follows the instructions one character at a time.

>An opening parenthesis, (, means he should go up one floor, and a closing parenthesis, ), means he should go down one floor.

>The apartment building is very tall, and the basement is very deep; he will never find the top or bottom floors.

>For example:

    (()) and ()() both result in floor 0.
    ((( and (()(()( both result in floor 3.
    ))((((( also results in floor 3.
    ()) and ))( both result in floor -1 (the first basement level).
    ))) and )())()) both result in floor -3.

>To what floor do the instructions take Santa?

Now I'm a JavaScript guy myself so I used JS. Even though anything else is faster. 

{% highlight javascript %}
var string = very long //You get your own string.
var floor = 0; // Start at floor zero. 
for (var i = 0, len = string.length; i < len; i++) {
  if  (string[i] == "("){
    // If "(" go up a floor.
    floor = floor + 1;
  }
    // If ")" go down a floor.
  if (string[i]== ")") {
    floor = floor - 1;
     
  }  
}
//Print out santa's floor.
console.log(floor);

{% endhighlight %}

Pretty simple and really rough way to handle the problem. After debugging missing semicolons and giving the website a correct answer I was greeted with this.

>Now, given the same instructions, find the position of the first character that causes him to enter the basement (floor -1). The first character in the instructions has position 1, the second character has position 2, and so on.

>For example:

    ) causes him to enter the basement at character position 1.
    ()()) causes him to enter the basement at character position 5.

>What is the position of the character that causes Santa to first enter the basement?

So this is pretty easy all you gotta do is add a check to see what floor you are on, and then print out the counter "i" and add 1 to the result.

{% highlight javascript %}

 if (floor == -1) {
    console.log(i+1);
    break;
  }

{% endhighlight %}

Pretty fun! If your reading this for a solution I hope I helped!
