---
layout: post-no-feature
title: "Sifting and Sorting 560,000 Emails Without Excel"
description: "Taking Excel Spreadsheet Work To A Whole New Level"
tags: [ruby, coding at work, excel]

---

### The Problem Unveils Itself

It was the first time I have ever had to deal with something like this - which means I just gotta write about it. When I joined True&Co in 2013, we had about 200,000 emails. Even then it was the largest email list that I ever had to deal with and at the time the email marketing strategy was fairly standard. You sent one email out to everyone and then they ordered. At the beginning, True&Co relied heavily on a PR strategy and that tends to bring in more of an early-adopter, really engaged audience.

It's been over a year since then and the email base has multiplied many times over. Today the database handles over a million emails and when you have that large of an email base, it is not an appropriate email strategy to simply blast out one single email to your reachable base. There has to be some sort of segmentation and customization, especially when it comes to bras. Different people have different types of fits, and it is important that we customize the image assets and the message for that difference. The marketing team at True&Co continues to experiment with increasingly deeper segmentations and customizations. Such experiments mean we require more logic in email sorting than anything that has been previously built out for us. All great and good, but when you have a base population of over 1M emails then working with even a subset of that can become unwieldy for the tools you've used before. 

I am usually the person who is responsible for segmenting and providing the email list as according to the email marketing team's request. They would usually provide for me some sort of the logic: "Hey Jonathan, take all of the emails of people with these fit keys and stated sizes." Usually this can be done in a single SQL query from the database. However this new experiment had been something new. We wanted to bucket a certain population of 468,000 emails (based on an internal bra size measure, I think) but EXCLUDE any email from another population of 560,000 emails - selected based on some measure of engagement I believe. 

The twist is that the data on the 560,000 emails came from one source (our email provider) and the data on the 468,000 emails came from another source - our web app database. While hooks have been built to communicate between the two sources by our fine development team, it had not been built to accommodate the specific request coming from our email marketing team. 

So it looks something like this: 

Population 1: 560,000 emails (from email provider)

Population 2: 468,000 emails (from web app) (in the code below, it will be called 'marielle')

Find all from Population 2 that are not in Population 1.

This is a simple task. It is just not as simple when done on 468,000 emails. 

### Excel Fails To Do The Task

I export the relevant data files in CSV format from our two data sources. At this point the files are so large that my computer struggles to handle it. I work on a 2013 Retina Macbook Pro and I consider it fast. It does an admirable job downloading and manipulating 500-1,000 downloaded Facebook ads in Facebook Power Editor. But Excel on Mac is a pretty crappy piece of software and it takes 7 seconds just to open up 560,000 emails. 

The normal operating procedure goes something like this. Open up "File 1" and "File 2" side by side and then in one of the files create an Excel formula that looks something like this: 

= MATCH(a1, [file2]Sheet1!a3:a10, 0)

This Excel formula scans the value in Cell A1 and then checks for its presence in the specified row (here, A3 to A10) within Sheet 1 of File 2. Setting the tertiary value of 0 would ask Excel to do a complete match (kinda like dropping a FALSE at the end of a VLOOKUP formula would do). VLOOKUP by the way can do the same job but for the purposes of this task - simply saying TRUE or FALSE on whether or not A1's value exists in File 2 without saying where - MATCH does the job fine. 

Well, it would do the job fine except I knew from the very beginning this would be dog slow to process on Excel on Mac. From prior experience I knew that my computer could handle a task like this on 200,000 / 50,000 emails. Can it do 468,000 / 560,000?

Spoiler Alert: It couldn't. I was able to write the formula and put the two Excel files side by side. I was able to drag it down, but I was not able to access the finished product. Excel froze. I tried: 

* Combining File 1 and File 2 into a single XLSX
* Closing and quitting every other window on my computer
* Chunking files into 2/3/5 segments
* Reasoning
* Bargaining
* Crying

After about an hour and a half of wrestling with Excel it appeared to me this was not tenable. The email test was going out in 3 hours and we needed to get these populations created. My pride was on the line! That is when it occurred to me that we might be able to do this programmatically. 

### Programmatical Attempt 1

During my time at App Academy we had to do little exercises in Ruby creating and opening files. That experience had been stashed away in the back of my mind until now. But now it was finally coming to be important! JY is finding Excel to be insufficient for my data sorting and segmentation needs. Can a Ruby script be the answer?

Very quickly I Googled the File module in Ruby to get a refresher and opened TextMate. I quickly wrote the below code. 

    a = File.read('/Users/jonathanyu/Desktop/population.txt')
    b = File.read('/Users/jonathanyu/Desktop/marielle.csv')

    population = []
    a.split(/\n/).each do |email|
      population << email
    end


    File.open('/Users/jonathanyu/Desktop/marielle_minus_population.txt', 'w') do |csv_object|
      b.split(/\n/).each do |email|
        if population.include?(email) == false
          csv_object << email
          csv_object << "\n"
        end
      end
    end


It is not exactly world-changing code. My thinking behind what I wrote was simple. Throw all 560,000 emails of Population 1 into an Array, iterate through each of the 468,000 emails in Marielle and compare them against the Population 1 Array. 

I sat back and ran the Ruby script in my terminal. The computer shuddered and paused. Nothing happened for a while. Then for a while longer. I started to get a little nervous. This is taking an awful long time. Did something break? I threw in pry and looked through the code. I had it output to screen the result of the numerous calculations. The code is working so why on earth is it taking so long?  

### Programmatical Attempt 2

It was then that something my former coworker Paul once said a while ago suddenly occurred to me. I remember that back when I had thought that I was going to be a Rails developer, Paul was walking me through a coding challenges when he said that you should probably try a Hash. For some reason, throwing an include? on an Array takes time while simply going to the key of a Hash is instanteous. I honestly don't know why it is such but I couldn't care less. I needed my problem solved. 

I slightly modified the code below. 


    a = File.read('/Users/jonathanyu/Desktop/population.txt')
    b = File.read('/Users/jonathanyu/Desktop/marielle.csv')

    population = {}
    a.split(/\n/).each do |email|
      population[email] = true
    end


    File.open('/Users/jonathanyu/Desktop/marielle_minus_population.txt', 'w') do |csv_object|
      b.split(/\n/).each do |email|
        if population.has_key?(email) == false
          csv_object << email
          csv_object << "\n"
        end
      end
    end
		

The big difference is that instead of having an Array, we have a Hash. As each email is processed from the population.txt file, it is added as a key in the Hash. When we need to do the cross checking, we call the has_key? function on our Hash. This is the part that was making my previous function dog slow. 

Amazingly enough, this shit worked and in like seconds. It literally took 2 seconds and the majority of it was sorting through the thousands of emails. Within seconds, I had a file "marielle_minus_population.txt" sitting on my desktop with slightly over 250,000 emails. My coworkers - befuddled as to why I had been angrily yelling at my computer for the past 1-2 hours - were pleased to find the CSVs, unaware of the pains I went through to create them! 

### Conclusion 

This is no great technical shakes if you actually develop for a living but for a marketer and former finance dropout like myself, it was pretty cool. We basically did a manipulation of a CSV file that I would not have been able to do with Excel. Excel is probably the best piece of software Microsoft has ever put together and I love it, but when I think about it some of the calculations that people do in it can be better done in code. When you open up Excel just to do a sorting operation you are probably using it wrong. 

And this experience has kind of reiterated to me the feeling that if you know how to use Excel well then you are not that far off from being able to write code because an Excel spreadsheet is itself a piece of software. You write functions and stuff that help process inputs and generate outputs. The only difference is that in a spreadsheet you see everything as it happens in front of you. That is not the case with code. 

If this email test is successful then we will probably have the developers write this functionality into the site's hooks to our email provider so this is just a one time thing. But I really enjoyed this particular challenge and hope to come across similar ones in the future. 