---
layout: page
status: publish
published: true
author:
  display_name: admin
  login: admin
  email: dhrumil.mehta@gmail.com
  url: ''
author_login: admin
author_email: dhrumil.mehta@gmail.com
wordpress_id: 45
wordpress_url: http://www.politicalframing.com/?page_id=45
date: '2013-07-17 06:35:15 -0500'
date_gmt: '2013-07-17 06:35:15 -0500'
categories: []
tags: []
comments: []
---
#Motivation
In their seminal work,&nbsp;<span style="text-decoration: underline;">Metaphors we Live By</span>, George Lakoff and Mark Johnson argue that certain metaphorical structures are deeply embedded in our use of language and reflect &ldquo;linguistic frames&rdquo; through which we see the world. A linguistic frame is a conceptual domain through which we can understand an idea. For example, when talking about a sports match we might say "Team B demolished team A" or "Team A gained ground". In both cases we would be understanding sports through the frame of a battle. There are many other metaphors we use in our day to day life such as the "time as money (time as a fungible commodity)" metaphor, in which we will often talk about "saving/spending/wasting" time, although one cannot literally do these things with time; these metaphors are deeply embedded in our thought and manifested in our language. In <a href="http://www.amazon.com/The-Political-Mind-Cognitive-Scientists/dp/0143115685/ref=sr_1_1?ie=UTF8&amp;qid=1376053454&amp;sr=8-1&amp;keywords=political+mind">Political Mind</a> Lakoff&nbsp;conjectures that Democrats and Republicans in American politics use theses frames in different contexts. The goal of this project is to use computational methods to analyze framing in political speeches.</p>
#Overview
In this project, we have mined the past 15 years worth of speeches from US Senate and House of Representatives and are studying its rhetoric by creating a model of rhetorical framing. First, the speeches are partitioned by topic and the classifier is trained to classify a novel speech into its appropriate speech category. Having ascertained that the program can do this with a fair amount of accuracy, we can then feed the classifier a frame rather than a speech. A "frame" is modeled as a "bag-of-words" that contains words that are highly associated with a particular term. Example frames can be found in "/data/frames". The program will put the log-likelihoods table in the "/results/" directory where they can be extracted and viewed. We can extrapolate further infromation from these results.</p>
#Data
*Please note that data is not is included in the source-control, actual dataset is 2GB and growing*</p>
<ul>
<li>10,000 speeches were mined for each of the following topics: "Afghanistan, defecit, education,foreign+policy, health+care, immigration, Iraq, marriage, social+security, welfare". Speeches were mined from the "capitol words api" - http://capitolwords.org/api</li>
<li>Frames were built using my "framemaker.py" and Princeton's Wordnet database.</li><br />
</ul></p>
#Methodology
###(1) Modeling Frames:
Frames are modeled as bags of words approximately 500 words in size. The user enters a "seed" word to query Princeton's WordNet database, for example "crime". The program will then find definitions of "crime" to which the user will answer yes or no in order to disambiguate the proper sense of the word. After that, the program will mine wordnet for a few words in the semantic vecinity of "crime" and ask the user if those definitions are valid. After what is generally 15 or so questions, the program will output a 500 word "frame" to track the concept that the user is trying to follow. An example of the "crime frame" that I built is found below:<br />

![](https://raw.githubusercontent.com/PoliticalFraming/politicalframing.github.io/master/blog/methodology/crimeframe.jpg)

###(2) Building Classifiers
<strong>Classifier A:</strong> A multinomial naive bayes classifier trained to classify a novel speech into one of the topics mentioned in the "data" section above. The classifier can classify a novel speech with approximately 70% accuracy on 7 topics (chance would be ~14% so it works pretty well).</p>
<p><strong>Classifier B: </strong> A binomial naive bayes classifier is trained to classify a novel speech as Democrat or Republican. The classifier is trained on all of the speeches only within a certain category.</p>
<p>*Note - 20 fold cross validation was used to determine the accuracy of each of the classifiers.</p>
###(3) Observing Framing
<strong>Classifier A:</strong> After training the classifier on all of the the speeches, rather than feeding it a novel speech to classify, we then feed it a frame (bag of 500 words) and output the log likelihoods of where the classifier tries to put each frame. Analyzing these log likelihoods gives us an idea of how the frame is used in each topic area with relation to the other topic areas.</p>
<p><strong>Classifier B:</strong> After training the classifier on all of the speeches within a particular topic area, we feed it frames instead of a novel speech. The classifier outputs log-likelihoods of each frame being either Democrat or Republican. We can observe these numbers and the ratio of D:R to understand how polarized a frame is within a particular topic area (for example, on immigration speeches, the crime frame is patently republican). We can further plot these numbers over time to understand trends in framing analysis.</p>
#Results
The full results are provided in the files at the end of this section. The pictures below show how to interpret the results. For this, we do a case study following the "crime" frame in speeches related to immigration.</p>
<p>Results for Classifier A (from Version 0.0):</p>

----------------- crimeframe image --------------------
------------ caption: Here we can see a strong affiliation for example, between the crime frame and the immigration topic."

<p>One Result from Classifier B (immigration topic only):</p>
![](https://raw.githubusercontent.com/PoliticalFraming/politicalframing.github.io/master/blog/methodology/FramingData2.png)
------------ caption: Looking further at the speeches only within the immigration topic, we can see that the high prevalence of the crime frame is attributed to Republicans moreso than Democrats.

<p>Changes over time in Classifier B (Version 1.0 Results):</p>
![](https://raw.githubusercontent.com/PoliticalFraming/politicalframing.github.io/master/blog/methodology/immigration-crime-moving.png)
------------ caption: Changes in use of crime frame over time. Plot of ratio of log likelihoods. (not shown on log scale)

<p>&nbsp;</p>
<blockquote><p><strong>Remaining Results from Version 0.0 are found in this file: <a href="http://www.politicalframing.com/wp-content/uploads/2013/07/DRSpeechData.xlsx">DRSpeechData</a></strong></p>
<p><strong>Results from version 1.0 (temporal analysis) are found here: <a href="http://www.politicalframing.com/wp-content/uploads/2013/07/ImmigrationPPT.pptx">ImmigrationPPT</a></strong></blockquote></p>
<h1 id="conclusions">Conclusions</h1><br />
This project is currently on version 1.0 - and is a preliminary pass at attempting to understand framing of political rhetoric computationally. I intend to make the methods more rigorous and the outputs more human-readable. The hope is that this software will one day analyze every member of congress individually, always seeking rhetorical shifts and anomalies to inform journalists of where new stories are. Furthermore, it aspires to also be a helpful tool to allow researchers to study the changes in rhetoric in any deliberative body that releases its records for public view over time. We intend to add many other types of document analysis focusing on computational rhetorical analysis.</p>
<h1 id="references">References</h1></p>
<p>[1] Lakoff, George&nbsp;and Mark Johnson. Metaphors We Live By.&nbsp;Chicago: University of Chicago, 1980. <em>Print</em>.</p>
<p>[2] Lakoff, George. The Political Mind: Why You Can't&nbsp;Understand 21st-century&nbsp;Politics with an 18th-century Brain.&nbsp;New York: Viking, 2008. <em>Print</em>.</p>
<p>[3]&nbsp;&nbsp;Rennie, J.D.M., Shih, L., Teevan, J. and Karger, D.R.&nbsp;Tackling the Poor Assumptions of Naive Bayes Text&nbsp;Classifiers. Proceedings of the Twentieth International&nbsp;Conference on Machine Learning (ICML). 2003.</p>

**Please send questions or comments to:**
	
* al [dot] johri [at] gmail [dot] com
* dhrumil [dot] mehta [at] gmail [dot] com
