---
layout: post
title:  "NYT Articles and Comments Analysis with MongoDB and Docker"
date:   2022-04-17
categories: mongo mongodb big-data kaggle mongodb-atlas mongodb-charts analysis docker
---

Inspired by [Spiegel Mining lecture by Daniel Kriesel](https://youtu.be/-YpwsdRKt8Q) I decided to try some mining my own using this [New York Times dataset from Kaggle](https://www.kaggle.com/datasets/benjaminawd/new-york-times-articles-comments-2020).

In this example we will be concentrating on the articles dataset: `nyt-articles-2020.csv`. It contains more than *16K* articles with *11* features we can toy around with:

* newsdesk
* section
* subsection
* material
* headline
* abstract
* keywords
* word_count
* pub_date
* n_comments
* uniqueID

So here are a couple of questions we can ask ourselves:

* Which article has the largest/smallest word count?
* Which article has the most/littlest comments?
* What is the average word count per article for each newsdesk?
* How many articles are published on average per day?
* How many articles are published on average per weekday?
* On which day the most articles get published on average?
* ..

So let's get started by running the MongoDB Docker Container, setting up a database and importin the data. For this part please refer to the [documentation inside this repo](https://github.com/goseind/nyt-article-analysis#run-the-docker-container).

The repo also contains the file with all the [queries to answer the above questions](https://github.com/goseind/nyt-article-analysis/blob/main/nyt-mongosh-queries).

Additionaly you can watch [this YouTube video](https://youtu.be/2HfKDJpGgQ8) which explains the whole process step by step including data visualization with MongoDB Atlas and Charts.

<iframe width="560" height="315" src="https://www.youtube.com/embed/2HfKDJpGgQ8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>