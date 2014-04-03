---
layout: post
title:  "Why do we have different models to read and update data? aka CQRS"
date:   2013-12-22 10:20:50
categories: availability cqrs reporting
---

In a few projects, we have kept the data update mechanism different from the data read mechanism. 

Some Examples :
1. In FreshersWorld, data is read/queried from a SOLR database, but is primarily updated in MySQL.
2. In Comcast/Lithium, data is inserted using Django ORM models. But data reads are done using SQL and a smallish reporting framework we built internally, instead of using django models directly.

The simple reason is that each user interface needs a different view of the same underlying data. This is actually a design pattern for this, and its called Command Query Responsibility Segregation, or CQRS. 

Yes, it has its drawbacks - keeping data consistent is a real problem, and the complexity isn't always worth it. As always, the hardest part is deciding if this pattern really applies to your problem.