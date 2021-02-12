---
layout: post
title:      "CLI Data Gem Project"
date:       2021-02-12 19:24:26 +0000
permalink:  cli_data_gem_project
---


WELL THIS WAS A WEIRD ONE.

So the objective of this project was to create a Ruby Gem that used data scraped with Nokogiri from a live website. Hearing this, I immediately thought of this one idea my boyfriend came up with about 6 years ago: an app that helps you figure out where to eat by randomly choosing a restaurant close to you. You essentially spin a wheel and bam!, that's where you're going. Although moderately outdated as an app idea, I thought it would be funny for him to see this unexpectedly materialize so many years later.

So first I had to check to see if there was a website which listed restaurants by area that could be scraped with Nokogiri.  I learned that if a website was Javascript-heavy, Nokogiri wouldn't exactly work, as Nokogiri goes in quick and comes out quick; it essentially wouldn't have time to scrape with all of the Javascript. My first obvious options were third-party delivery service sites. Unfortunately, those proved to be too Javascript-heavy and could not be scraped. I then tried Yelp, Yellow Pages, Trip Advisor, a tourism site specific to my own city that listed restaurants, and a few others I can't remember but none of them worked. 

Beginning to think I was just dumb and doing it wrong, I thankfully stumbled upon Open Table, which is a website that allows you to search for restaurants in your area, see their menu, ratings, and cuisine category, and book a reservation online. After I determined that this could be scraped, I was met with my first major hurdle: how do I effectively scrape these web pages based on location? I discovered that the “Near Me” search option shared similar urls between all locations, the only difference being the actual location name in the url. So in my program I prompted the User to enter a location name and interpolated that into the url. The problem, though, is that Open Table is not available in every city, so I knew that I would have to find a way to check the validity of the location provided by the user. This was probably the most difficult problem to solve in my code.

Searching online for how I might be able to do this, I came across some ways to check the error code of a webpage. None of these seemed to really work for me for some reason, possibly due to the url I was using being https instead of http, so I analyzed how to raise and rescue an error and constructed my own way. This is an incredibly abridged documentation of my process, as it took quite a few days to figure this problem out. Buzzing with this achievement, I was ready for the next issue!

I realized after running my code a few times that only the same 10 or so restaurants would come up. On the site page, the list of restaurants extends to multiple pages, obviously depending on how many restaurants there are in the city. How was I going to scrape the restaurant data from the other pages? Well! I discovered that the url used when you click on another page was similar as well! All I needed to do was interpolate the page number. So I determined that I needed to be able to scrape the amount of pages that the location has in total and use a counter and loop to have Nokogiri go through each page. I could not include the very last page, as when you clicked on that, the site broadened your location to state-wide or something and added hundreds more restaurants. So I kept the last page off. To break it down, in my code I would check if the number that the counter was at was less than the total number of pages, interpolated that number into the url, then scraped that page. With the scraped data array, I iterated through them down to the level where I could extrapolate the restaurant name, rating, cuisine type, and link to make a reservation (the restaurant page url). To store the data for each restaurant, I would create a new Restaurant object and assign its attributes to the appropriate scraped data.

The cuisine type was a bit difficult to do, as on the site the text for it included non-word characters ("•"), some had more than one type, and some included a more refined location. Although a bit clunky and literal, I decided to just break the string up into an array and pop off anything after the second "•", and then those non-word characters themselves, leaving the cuisine type, be it one or multiple. 

So all of that information was shoveled into @scraped_restaurants_array so that when that method of the Scraper class was called, it would be what was returned. That method is called in the Restaurant class, which is then called on in the CLI and then sampled there in the sampled_restaurant method, so that a location only needs scraped once. 

The options in the CLI, after you give your location, the data is scraped for you, and a restaurant is picked at random, are to get the link for that restaurant in order to make a reservation, "spin" the wheel again (have the sample method called again on the array of restaurants to give you another randomly chosen restaurant), or exit. 

And that is it! 

Creating this program over the holidays, during a pandemic, and working a full-time job have been a bit of a hassle but all's well that ends well! If I can do it, so can you!

And ya, so, I just googled and saw that this has already TOTALLY BEEN DONE BUT OH WELL. 

link to my project repository:
[https://github.com/OliviaJoyHanson/IndecisiveDiner.git](http://)



