---
title: "Topic Modeling Online Petitions"
excerpt: "Using NLP techniques to build a topic insight and recommendations dashboard for change.org petitions"
header:
  overlay_image: topic-modeling_splash.jpg
  overlay_filter: 0.5
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
categories:
  - Projects
tags:
  - natural language processing
  - matrix decomposition
  - topic modeling
  - collaborative filtering
  - content-based filtering
  - flask
  - plotly
modified: 2017-02-13
---

[change.org](http://change.org/), “The World’s Platform for Change”, is the largest host of online petitions on the web. For my fourth project at Metis, I decided to use natural language processing techniques to explore the treasure trove of petition text on change.org. I was curious to see which issues (perhaps legitimate, perhaps frivolous) people cared about, and wanted to develop a hypothesis around the broader world psyche around change. Disclaimer: This post is not affiliated with any official activities of change.org!

There are currently about 50 thousand active petitions on the site. As you may expect, an online forum like this has a lot of hits and misses - 17% of the ongoing petitions on the site have less than 10 supporters. However, there is a lot of good that the petitions on the site have facilitated, for example [President Obama signing a bill of rights for sexual assault survivors](https://www.change.org/p/urge-congress-to-support-rape-survivor-rights). There are also alot of [smaller community successes](https://www.change.org/p/st-helens-school-district-502-school-board-members-keep-mr-zmolek-as-a-full-time-choir-director-2). And then, there are those which [serve as a rallying point for action](https://www.change.org/m/demand-action-from-president-elect-donald-trump). 

I put myself in the shoes of a data scientist at [change.org](http://change.org/) and thought of what kind of data product or insight might be the result of this analysis. My primary two objectives were to: 

1. Perform topic modeling to improve intelligence around issues:
  * What topics do people care about?
  * Which topics are the most successful in producing change?
2. Develop functionality for users: 
  * Creating a petition dashboard that is an extension of current online services for petition starters and signers
    * For a given petition, provide topic breakdown
    * Provide petition recommendations based on topic similarity

Feel free to check out my code on [my github](https://github.com/neokt/topic-modeling-online-petitions)! 

**Data Collection and Storage**

There were over 50,000 open petitions on the change.org website and about 3,000 labeled as victorious. Due to the way the site used Javascript and interactivity, I found through trial and error that XPATH syntax worked best for assessing elements. I therefore chose to use Selenium to scrape the URLs for all petitions, and wrote Scrapy spiders to access petition text, comments and attributes. While Selenium gave me some headaches (mostly due to the slow speed of requiring a web driver to load the elements to be scraped), I really enjoyed using Scrapy’s extensibility, flexibility, speed and and the use of Scrapy Shell for testing. Here’s [one of the spiders I wrote](https://github.com/neokt/topic-modeling-online-petitions/blob/master/change_scraping/change_scraping/spiders/victories_spider.py) to crawl for victorious petition information.  

I found it best to store petition text and attributes as documents in mongoDB. For each petition I had collected:

* Petition ID
* Name of petitioner
* Primary decision maker
* Other decision makers
* Petitioner location
* Petitioner type (person or organization)
* Number of supporters
* Goal
* Tags
* Petition text

Due to time limitations I was only able to extract the top 10 comments associated with each petition; for each comment I obtained the following data points:

* Comment ID
* Commenter name
* Commenter location
* Commenter date
* Number of hearts ("likes")
* Comment text

**Exploring the data**

An initial exploration of the data showed trends in line with expectations. I imagined the number of petition supporters to be extremely right skewed, which was indeed the case. Victorious petitions had a median of 530 supporters while open petitions had a median of 41. Both had many outliers, and a wide spread of supporter counts.

![Exploratory analysis][chart1]

[chart1]: https://raw.githubusercontent.com/neokt/neokt.github.io/master/images/topic-modeling_chart1.png "Exploratory analysis"

**Topic Modeling**

A key stage of any natural language processing project is to tokenize and clean the text before it can be transformed into a word vector. This turned out to be an iterative process; I got all the way to the modeling stage a couple of times, only to notice that noise in the petition text was affecting my results. For example, I thought I had a website-related topic, until I realized that I hadn’t stripped URLs from my petition text and my modeling was capturing the youtube and article links in the petition body.

I wrote a custom tokenizer that would handle unicode characters, punctuation, URLs and stop words; I also removed non-english petitions from my petition set as I wanted to focus on the vast majority of english petitions. For other scenarios, a tokenizer from NLTK’s tokenizer module may have worked as well. 

I employed Scikit-learn’s TF-IDF vectorizer to build a word matrix, aggressively paring down which words I wanted to include, choosing a max_df of 60% and min_df of 10. This meant that the matrix would only capture words that appeared in less than 60% of the texts (to avoid commonly used words such as ‘and’, ‘the’) and appeared a minimum of 10 times. I also chose an ngram range of (1, 3), to be able to capture names and places if two words were commonly used together.

Non-negative matrix factorization (NMF) was applied to my word vector; this is a matrix decomposition technique that, when applied on a corpus of documents, allows one to extract additive models of the topic structure of the corpus. I chose an n of 30 - compared to 20, 40 or 50 topics, this appeared to reach the right balance of generalizability, specificity and interpretability.

Each topic was labeled after reviewing the top words for each group - while this had some subjectivity, these were fairly straightforward for most; for example:

* *Politics - Republican Party*: trump, donald, republican, mr, presidential, candidate, president, j, gop, party, candidates, hillary, clinton, nominee, returns, america, racist, muslims, campaign, debate
* *Community Issues*: community, public, board, support, new, residents, members, area, would, 1, property, local, development, plan, town, 2, building, program, current, service
* *Animal Cruelty*: animals, animal, dogs, dog, shelter, pet, cruelty, cats, korea, pets, meat, abuse, south, puppy, shelters, cat, owners, kill, breed, rescue

Comments on other methods not used:

* Attempted latent dirichlet allocation (LDA), but found that NMF appeared to approximate topics more, or at least produced top words that aligned with each other from a human perspective. 
* Applied K means clustering on the TF-IDF matrix; with more time, would have experimented with different values of K to maximize interpretability.

![conditional probability of success][chart2]

[chart2]: https://raw.githubusercontent.com/neokt/neokt.github.io/master/images/topic-modeling_chart2.png "conditional probability of success"

I calculated the probabilities of succeeding for each of the 30 topics I had identified, using the proportions of victorious petitions to all petitions. Unsurprisingly, petitions related to food, farming and livestock and animal cruelty tended to be the most successful; these are certainly uncontroversial topics that most people can get behind! 

It was also interesting that the topics revealed layered nuances - several topics appeared for government and education, but were separable by their content. For example, I labeled a topic “Education - Teaching” and another topic “Education - School Administration”; the former contained words such as “teacher” and “learning”, and the latter contained words such as “district” and “principal”.

Finally, there were some amusing and frivolous topics towards the bottom of the success chart, such as [Harambe](https://www.change.org/p/nintendo-make-harambe-a-pok%C3%A9mon) and [First world problems](https://www.change.org/p/elisha-compton-let-alaina-compton-dye-her-hair-blue), which as you can imagine are generally less likely to succeed.

In general, I found that the topics seemed to represent mostly American, rather than world interests. This seemed logical as change.org is a US-based site.

**Recommendations**

I used one of the outputs of my matrix factorization exercise (a petition vs. topic matrix) as the starting point to build a petition recommendation system using a content-based approach to filtering (using topic importances in place of where one might normally use user ratings). I used cosine similarity to compute the pairwise distances between all petitions in the matrix based on the topic importances. Due to hardware limitations, I was only able to calculate this for the first 10K of the petitions in my matrix - with more time I would have re-hosted the project on an AWS EC2 GPU instance and tried again with all the petitions in my dataset!

By ranking the resulting entries in the similarity matrix, I was able to produce the most similar petitions (by topic distribution). For example, a few of the most similar petitions for ‘Maximum Sentence for Darius Ewing for Setting Dog, Justice, on Fire' were: 

* Justice for Harper, Pit Bull Puppy Thrown Over 50-Foot Embankment in PA
* Honorable City Court Judge of Erie County Court : Justice for Burned Puppy "Phoenix"!
* Justice for Coco and puppies, dog stabbed for protecting her owner
* Brian A Clark: Drop Charges against Connersville Police Officer and his wife
* Justice for Taz, Puppy Stabbed and Dumped in Abilene, Texas

These petition subjects are pretty horrific (and worthy of rallying behind)! Based on a few test cases, I was pretty happy with how the recommendations turned out.

**Production**

Now that I had the topic landscape for the petitions and a recommender, I wanted to build a dashboard to productionalize these findings. A hypothetical dashboard would be integrated with the change.org website, and hence my prototype uses an internal reference - a unique petition ID - to identify petitions. I used Flask, Plotly and Cufflinks (a library that enables Plotly charting from a Pandas dataframe) to develop the interactive plots. 

My dashboard app is [live](http://bit.do/changeorg-petition-dashboard)! You can try it yourself with petition IDs 8287766 ([Stop abusing chickens](https://www.change.org/p/aramark-stop-abusing-chickens)) or 1508829 ([Urge MA legislators to support rape survivor rights](https://www.change.org/p/massachussets-legislators-support-rape-survivor-rights)). Tabled on the to-do list: Publish a list of compatible petition IDs, titles and links.

Here are a few screenshots of the app; the landing page contains an interactive bubble chart of petition topics representing their frequency and range of success. 
	
![landing page][screenshot1]

Here are the dashboard results for topic distributions and recommendations for the examples:

![first example][screenshot2]

![second example][screenshot3]

[screenshot1]: https://raw.githubusercontent.com/neokt/topic-modeling-online-petitions/master/screenshots/changeorg-petition-dashboard-screenshot01.png "landing page"
[screenshot2]: https://raw.githubusercontent.com/neokt/topic-modeling-online-petitions/master/screenshots/changeorg-petition-dashboard-screenshot02.png "first example"
[screenshot3]: https://raw.githubusercontent.com/neokt/topic-modeling-online-petitions/master/screenshots/changeorg-petition-dashboard-screenshot03.png "second example"

**Future work**

Currently, recommendations only work for the first 10K petitions in my data; an actual production pipeline would update the similarity matrix regularly with new petitions from change.org. 

With additional time, I would build out a classification pipeline to use petition metadata along with the petition text and comment language elements to predict the probability of petition success!

Questions about my analysis? Feel free to check out [my github](https://github.com/neokt/topic-modeling-online-petitions) or send me an e-mail at <neo.kaiting@gmail.com>.