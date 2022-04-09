---
layout: post
title:  "Booking a Driving Test with Python"
date:   2021-06-20 00:15:08 +1000
categories: jekyll update
---
It's often not clear whether it's faster to do it manually or write a script. Whether or not I save any time, I often find myself scripting a task if I have to do it more than a few times. Even a few 'wasted' hours usually result in some new skills.

A couple of years ago I'd just finished a the last semester of my degree, in which I'd decided to take an elective in programming. One of my main projects for that summer holiday was to upgrade my automatic driving license to a manual license.

Booking a driving test in Perth was difficult at the best of times, but near impossible over the busy holiday period. Test centres would often book out for months in advance. Snagging a booking meant repeatedly checking the DOTWA website until a slot opened.

There were better ways to use my time than checking a website several times an hour. Surely there was some way the process could be automated? I decided it was an opportunity to try something new with Python.

The attached video gives a rundown of what I ended up putting together. The video shows the script in action, with a little detail about what it does (apparently I hadn't discovered screen recording at the time). You can access the script itself at ...

INSERT VIDEO HERE

The main idea is to use a webdriver called Selenium to access the site exactly the way a human would. There's no endpoints I can access, so the script essentially clicks and types its way through the pages.

The first thing I'll do is set up Selenium. First I'll import the webdriver module and create an instance of the webdriver.Chrome() class:

{% highlight ruby %}
from selenium import webdriver
{% endhighlight %}
{% highlight ruby %}
driver = webdriver.Chrome(r'C:\Users\allan\AppData\Local\Programs\chromedriver\chromedriver.exe')
{% endhighlight %}

The webdriver.Chrome() class contains methods for interacting with the webpage. The instance is initialised to a path containing the ChromeDriver executable, which I've already downloaded. Now I can use 'get()' to navigate to the PDA booking webpage:

{% highlight ruby %}
driver.get('https://online.transport.wa.gov.au/pdabooking/manage/?0')
{% endhighlight %}

I'd like to run the script again while I write this up. Unfortunately, unless you've paid for a test, you can only access the first page:

![Landing page](/images/PDA_booker/landing_page.png){:class="img-responsive"}
&nbsp;  
&nbsp;  

Interacting with this page means identifying individual HTML elements. Take entering a licence number as an example. To enter a number in the "Driver's License/Permit Number", I need to access the right HTML element. I can find it using inspect element:


![Driver's License HTML](/images/PDA_booker/drivers_license.png){:class="img-responsive"}
&nbsp;  
&nbsp;  

One of the ways Selenium can select an element is using its Xpath. An XPath is like a file path for all the elements in an XML document. However, because XML and HTML are very similar, they can also be used to navigate HTML documents like a webpage. Inspect element provides an easy way to find an XPath:


![Driver's License HTML](/images/PDA_booker/xpath.png){:class="img-responsive"}
&nbsp;  
&nbsp;   


The XPath for the element is "//*[@id='id8']/ol/li[5]/div/input". The webdriver can be used to create an 'element' instance,
which has its own methods for interacting with the webpage. In this case I'll create an instance called "lic_num" and then use its send_keys() method to send it a license number:

{% highlight ruby %}
lic_num.send_keys('1234567')
{% endhighlight ruby %}

Selenium provides a few ways to interact with a webpage. For example, you could use click() to click 'continue':

{% highlight ruby %}
cont_button = driver.find_element_by_id('id5')
cont_button.click()
{% endhighlight ruby %}

For this project, I didn't need to use Selenium for anything more complicated than entering text, clicking buttons, and selecting dropdown items. Most of the script is logic used to book the right test. Settings at the top of the script specify a date and time range:

{% highlight ruby %}
test_centre_text = 'West Perth'
earliest_time = "08:00"
latest_time = "17:00"
earliest_dfn = 9
latest_dfn = 15
{% endhighlight ruby %}

In this case I'm trying to book any test from 8am to 5pm, between 9 and 15 days from now ('dfn'). Over the peak summer period, this was very soon to book a test! Once Selenium has navigated to the right page, a function called 'get_test_time_date()'
is used to strip out date and time information from the text options on the page:

{% highlight ruby %}
def get_test_time_date(input_list):
    print("GETTING TEST TIME DATE")
    i = 0
    global dates
    global times
    global dates_dt
    global times_dt
    dates = []
    times = []
    dates_dt = []
    times_dt = []
    date_pat = r'\d+/\d+/\d+'
    time_pat = r'\d+:\d+\s+\w+'
    for entry in input_list:
        date_gr = re.search(date_pat, entry)
        time_gr = re.search(time_pat, entry)
        #print(date_gr)
        #print(time_gr)
        date = date_gr.group()
        time12 = time_gr.group()
        if len(time12)<8:
            time12 = '0' + time12
        time24 = convert24(time12)
        dt_string = date + ' ' + time24
        print(dt_string)
        date_dt = datetime.strptime(date, '%d/%m/%Y').date()
        time_dt = datetime.strptime(time24, '%H:%M').time()

        print(date)
        #print(time12)
        print(time24)
        print("date_dt: ", date_dt)
        print("time_dt: ", time_dt)
        print("\n")

        dates.append(date)
        times.append(time24)

        dates_dt.append(date_dt)
        times_dt.append(time_dt)        

        i = i + 1
{% endhighlight ruby %}

The get_test_time_date() function creates several lists, each containing dates or times representing the booking slots on the webpage. I've used global variables here, which I realise now is a progamming sin. Really these variables would be better off as attributes of a class. Creating a class would also give me somewhere to bundle up all my functions.

From here the, the get_suitability() function performs some comparisons and returns a boolean list, describing whether each booking option meets the date and time criteria. The first suitable test slot is then booked, or else the script closes the webdriver and exits. A simple batch script is used to repeatedly call my python script:

{% highlight ruby %}
echo off
title My Test Batch File
:: See the title at the top. And this comment will not appear in the command prompt.

echo 'False' >C:\Users\allan\Documents\pythonStuff\PDAbooker\booked_bool.txt

:while
start "my_booking_script" python.exe C:\Users\allan\Documents\pythonStuff\PDAbooker\PDAbooker2.py
timeout /t 60
taskkill /f /FI "WINDOWTITLE eq my_booking_script"

timeout /t 240
goto :while
{% endhighlight ruby %}

The script runs about every 5 minutes by default, which I thought would give me a good chance of snagging a short-lived test slot. While reducing this period might have improved my chances, I didn't want to create unnecessary traffic on the DOTWA website (or potentially get my IP banned). Once a booking has been made, a text file called 'booked_bool.txt' is edited, which then prevents the webdriver from running. All test slots are recorded in a .csv file, which allows me to see how long it took to book a test. In my case it ran for about 3 full days before finding a suitable booking! It certainly seems this project saved me some time - and thanks to a little Python I now have a manual license.

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
