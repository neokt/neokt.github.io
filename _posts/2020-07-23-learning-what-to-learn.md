---
title: "Learning what to learn"
excerpt: "Thoughts on full stack data science and a collation of resources on machine learning systems design"
header:
  overlay_image: learning_splash.jpg
  overlay_filter: 0.5
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
categories:
  - Career
tags:
  - career
  - learning
  - full stack data science
  - machine learning systems design
modified: 2020-07-20
---

I wanted to take some time to self-reflect on my journey as a data scientist, and restart blogging as a declaration of intent to learn. 
This declaration of intent will hopefully generate future blog posts, so - stay tuned!

I've found that thinking about what to learn next to advance in the field of machine learning creates a lot of paralysis. I'm interested in learning more about 
deep learning, reinforcement learning, and Bayesian methods. At the same time, very few problems at work require trying these methods, so it's been difficult to keep up with the latest.

In the time that I've had to think as a result of the Coronavirus shutdown, I've realized a few things about myself - 
I really enjoy building data products that people use, and I consider myself a full stack data scientist, or a generalist. 
There's been a lot written about [full stack data science](https://multithreaded.stitchfix.com/blog/2019/03/11/FullStackDS-Generalists/), 
but the gist is that we work on all aspects of data collection, data processing, feature engineering, reporting, 
training, deployment, visualization, monitoring, testing... and are able to wrap that all into production-ready pipelines, as well as 
have the business acumen to approach whitespace problems, determine whether machine learning is an appropriate solution, and share results in a productive way.

That being said, many companies out there are going in the direction of hiring data science specialists 
(e.g., product analysts who do A/B testing, research scientists who only work on NLP, or machine learning engineers who optimize production code). 
There is no right answer to which is a better career path. I do believe that generally speaking, in the machine learning world, often the solution 
that delivers the most value is not the one that uses the most specific algorithmic technique,
but rather the one that can take an inefficient decision-making process and build an system - through heuristics, machine learning, or otherwise - to address it. 

Coming to the above conclusion has helped me to narrow the field of what I want to learn going forward - to be a better full stack 
data scientist, I'd like to focus my learning on the field of machine learning systems design!

The diagram below (from the paper [Hidden Technical Debt in Machine Learning Systems](https://papers.nips.cc/paper/5656-hidden-technical-debt-in-machine-learning-systems.pdf), 
but found via this [excellent post by Luigi of MLinProduction](https://mlinproduction.com/why-i-started-mlinproduction/)) 
explains it best - learning algorithms ("ML Code") are only a small component of a production ML system. There is so much about deploying models that isn't covered 
in most beginner data science learning resources, and the knowledge of how to build a sustainable ML system is often learned
by experience. 

![](https://i2.wp.com/mlinproduction.com/wp-content/uploads/2020/05/ml-components.png?w=1680&ssl=1)

Through paying attention to [TWiMLAI](https://twimlai.com/), 
[r/datascience](https://www.reddit.com/r/datascience/) and [r/MachineLearning](https://www.reddit.com/r/MachineLearning/), 
I've collated several resources that I plan to review over the next few months: 

ML-specific resources for systems design:
* [ML in Production](https://mlinproduction.com/) - Luigi Patruno's blog on building and operating machine learning systems in production
* [Machine Learning Systems Design](https://github.com/chiphuyen/machine-learning-systems-design) - Chip Huyen's book on machine learning systems design
* [Mercari Machine Learning System Design Pattern](https://mercari.github.io/ml-system-design-pattern/) - Yusuke Shibui's system design patterns for training, serving and operation of machine learning systems in production
* [Building Machine Learning Powered Applications: Going from Idea to Product](https://www.amazon.com/Building-Machine-Learning-Powered-Applications/dp/149204511X) - Emmanuel Ameisen's book on building an end-to-end data product
* [MLOps](https://ml-ops.org/) - Framework for an end-to-end machine learning development process to design, build and manage reproducible, testable, and evolvable ML-powered software

General systems design: 
* [Grokking the systems design interview](https://www.educative.io/courses/grokking-the-system-design-interview) - The primary interview resource anyone recommends when preparing for systems design interviews
* [Systems design primer](https://github.com/donnemartin/system-design-primer/blob/master/README.md#system-design-topics-start-here) - An organized collection of resources to learn how to build systems at scale

I'm excited to go down this learning path in my quest to become a better data scientist and machine learning engineer! In addition, I'm planning to kickstart my writing on a few other topics, specifically:
 1. **Back to Basics**: A series where I explain foundational machine learning or statistics concepts for myself, so I can better explain them to others
 2. **Mentorship**: I've had an opportunity to participate in several mentorship panels for women and high school students who are interested in data science careers, and would like to share my thoughts here
  
Do you know of any more machine learning systems design resources? Feel free to send me an e-mail at <neo.kaiting@gmail.com>.
