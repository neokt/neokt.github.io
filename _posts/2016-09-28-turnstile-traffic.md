---
title: "Turnstile Traffic"
excerpt: "Exploring MTA turnstile data for a cause"
header:
  overlay_image: mta1.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
categories:
  - Projects
tags:
  - numpy
  - pandas
  - matplotlib
  - seaborn
  - exploratory data analysis
modified: 2016-09-28
---

{% include base_path %}

A fictional non-profit organization, Women Tech Women Yes (WTWY), plans to deploy street teams to collect e-mail addresses to raise awareness and increase attendance at their upcoming gala. They have requested an analysis of New York subway turnstile data to optimize the placement of their teams and maximize the number of e-mail addresses collected.

In my opinion, the [two hardest questions](http://www.datascienceweekly.org/articles/data-science-project-checklist-to-use-before-you-start-a-project-to-convey-you-can-actually-get-work-done) to answer when starting on any data science project are:

- What is the question you want to answer (and the business case for this question)?
- What data do you plan on using and how will you obtain it?

Thankfully, the project parameters addressed both of those questions - but I imagine that a lot of time will be spent here when embarking on more open ended projects in the future.

## Project Scoping
We divided the problem space into two distinct areas for exploration:

- **Volume** - Maximizing exposure to subway commuters
- **Conversion** - Optimizing the likelihood of collecting signatures from desirable gala attendees and donors

We decided to focus exclusively on volume, but given more time would have incorporated additional datasets to address conversion.

## Tools
It's worth describing some of the fundamental tools to perform data analysis and visualization using Python (potential for expansion in an upcoming post - Essential Python Libraries for Data Science). While many are likely familiar with these libraries, I will describe my impressions of them here as I intend to reference them frequently in my posts.

- **Data Analysis**
    - *NumPy* - NumPy is a linear algebra library for Python. NumPy can be used to perform linear algebra (matrix) operations and many useful math operations.
    - *Pandas* - Pandas is an open source library built on top of NumPy. It is primarily used for data cleaning and preparation, allows fast analysis, and can work with data from a wide variety of sources. The core of Pandas is the Dataframe, an object similar to an Excel table that allows one to filter, subset and perform operations on data.
- **Data Visualization**
    - *Matplotlib* - Matplotlib is the most popular plotting library for Python. It gives enormous control over every aspect of a plot but is dexterous enough to produce a quick visualization with just a few lines of code.
    - *Seaborn* - Seaborn is the aesthetically pleasing cousin to Matplotlib - it is built off Matplotlib and is designed to work well with Pandas.

## Approach

1. **Acquiring Data** - Weekly subway turnstile data since May 2010 is available on the [MTA's website](http://web.mta.info/developers/turnstile.html). Acquiring the data was a simple matter of pulling several week's worth of data from the site. We decided to use May 2016 data to approximate traffic during the anticipated deployment time period of May 2017.
2. **Understanding the Data** - Turnstile counts were tracked as cumulative entries and exits, for several measures of time during the day, for an individual turnstile. To see patterns in the data by station, it became apparent that we would have to aggregate the data across individual time intervals for each station. To make things a little more complicated, the time measures were not uniform and there were occasions where turnstile counts were reset or irregularly audited.
3. **Cleaning and Prepping the Data** - This was primarily done using Pandas and included:
    - Removing trailing whitespace
    - Parsing date time values
    - Calculating incremental entries/exits
    - Removing outliers
4. **Exploring the Data** - To investigate our project objectives, we performed the following aggregations to understand which stations had the highest volume and how turnstile traffic varied by time:
    - Time of day
    - Day of week
    - Station
5. **Creating Meaningful Visualizations** - We wanted to share our findings with others, and a picture speaks a thousand words. Each graph we plotted illustrated a key point. We used Matplotlib to create the first 3 graphs and Seaborn for the heatmap:
    - A histogram to illustrate that a small number of stations account for a disproportionately large share of the traffic
    - A stacked bar graph to illustrate the total volume of entries/exits for the highest volume stations
    - A line graph to illustrate how volumes vary by day of the week for each of the highest volume stations
    - A heatmap to illustrate how volumes vary by time of the day for each of the highest volume stations

## Recommendations
To provide actionable recommendations, we would need to consider the limited resources that a non-profit may have. Our data led us to very specific recommendations on shift prioritization purely based on the volumes we were seeing. In order of magnitude, these were:

1. 42nd Street - Grand Central Station from 4PM-8PM, Mon-Fri
2. 34th Street - Herald Square from 4PM-8PM, Mon-Fri
3. 42nd Street - Grand Central Station from 8AM-12PM, Mon-Fri
4. 34th Street - Herald Square from 8AM-12PM, Mon-Fri
5. 42nd Street - Grand Central Station from 12PM-4PM, Mon-Fri
6. 34th Street - Herald Square from 12PM-4PM, Mon-Fri
7. Remaining top 10 stations from 4PM-8PM, Mon-Fri

Unsurprisingly, large commuter hubs and the weekday rush hour drove the bulk of the traffic. Without any further data on conversion, we recommended targeting these stations/time slots based on volume alone.

## Thoughts on Improvement
There were two notable areas in which we could have improved our analysis:

- **Refining deployment locations** - We had a logical inclination that people who are static are more likely to pay attention to solicitors, so we thought about how we would use additional data to refine our placement around [coffee shops](https://data.ny.gov/Economic-Development/Retail-Food-Stores-Map/p2dn-xhaw), [station platforms](https://osc.state.ny.us/audits/allaudits/093016/14s23.pdf), and even smoking stops.
- **Incorporating Conversion** - While we didn't have time to incorporate additional datasets to support hypotheses on conversion, we could have inserted relative assumptions. These could include hypotheses such as 'people may be more likely to stop on the weekends', or that 'people who work at (and are proximate to) tech companies may be more sympathetic to the cause'. Additional data that we would have incorporated would involve concentrations of desirable sociodemographics, professions, residents (vs. tourists vs. transit commuters) and political donations.

## Takeaways
- **Insights from a single source of data are less meaningful than from joining multiple datasets** - The real value in exploratory data analysis is being able to join multiple datasets to extract new patterns and insights. While our volume-based analysis was telling, the ability to cross-reference datasets for conversion would have given the analysis more impact.
- **Data science = data storytelling** - Even the most brilliant analysis can be useless if it is not structured and communicated well. Storyboarding our presentation up front helped us to streamline our investigation and give our analysis and visualizations focus.

## Presentation
<iframe src="https://docs.google.com/presentation/d/1FDhCiV-IVSWog1hbzznK-8J90VM-eBV9k0KKPSI3oXk/embed?start=false&loop=true&delayms=3000" frameborder="0" width="600" height="468" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true" align="center"></iframe>
