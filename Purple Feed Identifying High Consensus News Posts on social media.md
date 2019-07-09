#[Purple Feed: Identifying High Consensus News Posts on Social Media](http://www.aies-conference.com/2018/contents/papers/main/AIES_2018_paper_105.pdf)

## Key ideas

## Introduction
* Users play a role to select their sources for information as opposed to the past.
* Many users limit themselves to news stories that reinforce their views.
* This leads to a more politically fragmented, less cohesive society.
* There are systems to promote blue information for red users and viceversa but users do not like them.
* Purple news are news with high consensus of reader reactions. Hopefully these evoke
  a more unified response across society which might help to understand each other.

## Contributions

## Consensus definition and measurement
* "high consensus if there is a general agreement in readers' reaction despite readers' political leaning"
* consensus = abs((democrats disagree/number of democrats) - (republicans disagree/number of republicans))
* Mechanical turk used to measure consensus from WSJ blue/red feed and Twitter top 10 publishers.
* Confirmed that political extremes tend to pick lower consensus content.

## Study of consensus of news post

* Count Retweets to measure popularity. High consensus (158) and low consensus (177) on avg. Similar engagement.
* Do they cover similar or different topics? Consensus seems to cover non-US centric political topics.
* High cross-cutting exposure: # opposite leaning retweeters  > baseline of opposite leaning retwitters for a publisher
* Clearly high consensus correlates with high cross cutitng exposure

## Identifying high and low consensus news

* Features found
  - followers - on avg 67% are of the same political leaning
  - retweeters - active supporters, 78% of the same politicall leaning
  - repliers - 35% are of opposite political leaning

* Use Kulshrestha et al 2017 to measure political leaning in [-1, 1]

## Experimental evaluation

* Supervised learning using ground truth from the AMt dataset. Used SVM, Naive Bayes, Logistic Regression and Random forest.
* Pruned insignificant features and used 5-fold cross validation.
* Accuracies near 60/70% for predicting consensus.
* Publisher-based features are better than tweet-based features. Poor performance due to short size of tweet.
* 74% of high consensus posts identified, 70% of low consensus.

## Conclusion
* We propose a complementary approach to inject diversity in users news consumption.
* First attempt at operationalizing consensus of news posts on social media.
* Deployed 'purple feed', a system to highlight high consensus posts.
* Possibilities to study further evaluating the impact of showing high consensus posts on social media.
