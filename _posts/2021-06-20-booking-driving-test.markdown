---
layout: post
title:  "Booking a Driving Test with Python"
date:   2021-06-20 00:15:08 +1000
categories: jekyll update
---
It's often not clear whether it's faster to do it the old fashined way or write a script.
A couple of weeks ago I decided it was time to free up some space on my laptop. Looking through old files, I stumbled across a folder from a couple of years ago. Reading through it, I thought it could make a good first post for my blog.

At the time I'd just finished a the last semester of my degree, in which I'd decided to take an elective in programming.
One of my main projects for that summer holiday was to finally get my manual driving license. I already had an automatic licence, but was looking at working in agriculture, where a manual licence was a near-necessity.

Booking a driving test in Perth was difficult at the best of times, but near impossible over the busy holiday period. Test centres would often book out for months in advance. Booking a driving test meant repeatedly checking the DOTWA website until a slot opened. In fact, earlier in the holidays my sister had booked a driving test in Albany owing to the lack of availability in Perth.

There were better ways to use my time than checking a website several times an hour. Surely there was some way the process could be automated? Maybe I could even use my new skills in Python?

Here's what I came up with! You'll have to excuse the poor quality, I apparently hadn't learned about screen recording at the time. The video shows the script in action, with a little detail about what it does. You can access the script itself at ...

INSERT VIDEO HERE

It's a little hard to see from the screen, but the main idea behind the script is to use a webdriver called Selenium to access the site exactly the way a human would. There's no web API or endpoints I can access, so the script has to do all the clicking and typing for you. Writing this up now, I would love to be able to run the script again while I write this up. Unfortunately, unless you've paid for a test, you can only access the first page:

![Landing page](/images/PDA_booker/landing_page.png){:class="img-responsive"}

The script begins with importing all the necessary libraries and defining variables for things like the dates, times and locations I would like to book a test. I've blanked out confidential details since this is on the world wide web for all to see. I seem I didn't know about config files at the time, so all of the variable values are hard coded into the script.

  test_centre_text = 'West Perth'
  earliest_time = "08:00"
  latest_time = "17:00"
  earliest_dfn = 9
  latest_dfn = 15

  rootdir = Path('C:/Users/allan/Documents/pythonStuff/PDAbooker')
  booked_path = rootdir / "booked_bool.txt"

  booked_bool_file = open(str(booked_path), "r")
  booked_bool = booked_bool_file.read()

  if booked_bool == "TRUE":
      print("Already booked!")
      sys.exit(0)



You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated. FUCK!

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
