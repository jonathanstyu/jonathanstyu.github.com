---
layout: post-no-feature
title: "Machine Learning and Money Laundering"
description: ""
tags: [business, machine learning, money laundering]

---

Early this morning I came across an article that mixed two seemingly disparate parts of my lives: Machine learning and Money laundering - a [blog post](http://conf.startup.ml/blog/aml) prepared for a conference on "adversarial machine learning". 

The article discusses using network modeling between accounts and individuals within financial institutions to determine funny patterns in the flow of transactions between them. 

<figure>
	<img src='http://static1.squarespace.com/static/54063052e4b0a8b3682d6609/t/576fd9d5b3db2bd35f66cafe/1466948646795' style='width: 400px'>
</figure>

Viewed technically, I found it pretty interesting and the value of the analysis seemed clear to me so I shared it with a friend of mine with a background in anti-money laundering. To put it lightly, he did not enjoy it and briefly explained why: 

The techniques that the article is talking about speaks more towards discovering fraud than actual money laundering. Money laundering that is obvious enough to be detected by "machine learning" doesn't really need it in the first place. 

Actual money laundering is made up of totally legitimate transactions without fraud. For example, a terrorist organization is trying to get money into the US so that they can buy something. They set up a used auto lot website and get a bank account. A variety of "customers" start wiring $5,000 to $6,000 (below the reporting threshold for many financial organizations) to the bank account. Cars start going up on the site, appearing as they become available for sale and disappearing as they are "purchased". An algorithm is not going to help you as the investigator identify this as money laundering because an algorithm cannot go onto Google Maps and see that the physical address of the "lot" points to nothing or identify that the cars being "sold" are not commensurate with their market value. 

(I suggested that perhaps the cluster algorithm could help with identifying a foreign entity setting up new offshore accounts and wiring money in but he replied that while that works fine for identifying fraud it is not going to help unless the launderer is an idiot. And if you are looking for idiot launderers then a standard filter works just fine.)

Getting this sort of perspective was enriching. Machine learning has been getting a lot of attention in the news and it has been seeping into many other disciplines. The techniques might get a lot of press, but it is only as good as the data and humans are excellent at working outside of the data.

PS. I would not read [the comments to the HackerNews submission](https://news.ycombinator.com/item?id=11980846) for this unless you want to read a bunch of uneducated idiots deeming money laundering a "victimless crime". 