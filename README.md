# PageRank-Project: Mining a Website to Provide Legal Analsis on US National Security Issues

In this project, I created create a simple search engine for the website https://www.lawfareblog.com .
This website provides legal analysis on US national security issues.

**Created by:** Reymon Pedroza

**Created on:** September 13, 2020


# Project Description

This project adapts the ideas and concepts presented from <a ref="https://galton.uchicago.edu/~lekheng/meetings/mathofranking/ref/langville.pdf">*Deeper Inside PageRank*</a> by Amy N. Langville and Carl D. Meyer. In their paper, they present a comprehensive survey of PageRank and provide several key concepts. Utilizing the equations presented in their paper, I created a data minning algorithm that capitilizes on the linking structure of URLs based on outgoing/incoming links and presents a sorted list of URLs based on "importance". 

# Task 1: The Power Method

To begin with, the power method implements Equation 5.1 and computes the pagerank vector. For each iteration, the power method only requires the storage of one vector, the current iteration, whereas other methods may require at least several vectors. In terms of runtime, since P is sparse, each vector-matrix multiplication required by the power method can be computed to be *nnz(P)*, the number of nonzeros in P. Thus, the runtime is estimated to be *O(n)*

To test that the function works properly, I use the same sample data set used within the paper,`small.csv.gz`.  


### Part 1: Running PageRank on Sample DataSet

Using the sample dataset from the *Deeper Inside Pagerank* paper, I obtained the following results: 

```
$ python3 pagerank.py --data=./small.csv.gz --verbose
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=6.0257e-01 url=4
INFO:root:rank=1 pagerank=4.6414e-01 url=6
INFO:root:rank=2 pagerank=3.4544e-01 url=5
INFO:root:rank=3 pagerank=1.9498e-01 url=2
INFO:root:rank=4 pagerank=9.9210e-02 url=3
INFO:root:rank=5 pagerank=8.9347e-02 url=1
```

## Part 2: Search Query

Part 2: The `pagerank.py` file has an option `--search-query`, which takes a string as a parameter and returns the websites related to the user-specified search term. If this argument is used, then program returns all urls that match the query string sorted according to their pagerank. Essentially, this gives us the most important pages on the blog related to our query. The following data uses the search terms 'corona', 'trump', and 'iran' to display the most important pages related to the query.


```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --search_query='corona'
INFO:root:rank=0 pagerank=4.5861e-03 url=www.lawfareblog.com/lawfare-podcast-united-nations-and-coronavirus-crisis
INFO:root:rank=1 pagerank=4.0460e-03 url=www.lawfareblog.com/house-oversight-committee-holds-day-two-hearing-government-coronavirus-response
INFO:root:rank=2 pagerank=2.6116e-03 url=www.lawfareblog.com/britains-coronavirus-response
INFO:root:rank=3 pagerank=2.5390e-03 url=www.lawfareblog.com/prosecuting-purposeful-coronavirus-exposure-terrorism
INFO:root:rank=4 pagerank=2.3557e-03 url=www.lawfareblog.com/israeli-emergency-regulations-location-tracking-coronavirus-carriers
INFO:root:rank=5 pagerank=2.2895e-03 url=www.lawfareblog.com/why-congress-conducting-business-usual-face-coronavirus
INFO:root:rank=6 pagerank=2.2727e-03 url=www.lawfareblog.com/livestream-house-oversight-committee-holds-hearing-government-coronavirus-response
INFO:root:rank=7 pagerank=2.2520e-03 url=www.lawfareblog.com/congressional-homeland-security-committees-seek-ways-support-state-federal-responses-coronavirus
INFO:root:rank=8 pagerank=2.1878e-03 url=www.lawfareblog.com/paper-hearing-experts-debate-digital-contact-tracing-and-coronavirus-privacy-concerns
INFO:root:rank=9 pagerank=2.0339e-03 url=www.lawfareblog.com/cyberlaw-podcast-how-israel-fighting-coronavirus

$ python3 pagerank.py --data=./lawfareblog.csv.gz --search_query='trump'
INFO:root:rank=0 pagerank=6.6243e-02 url=www.lawfareblog.com/donald-trump-and-politically-weaponized-executive-branch
INFO:root:rank=1 pagerank=6.0194e-02 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=2 pagerank=3.4969e-02 url=www.lawfareblog.com/trump-administrations-worrying-new-policy-israeli-settlements
INFO:root:rank=3 pagerank=3.2193e-02 url=www.lawfareblog.com/document-trump-revokes-obama-executive-order-counterterrorism-strike-casualty-reporting
INFO:root:rank=4 pagerank=3.0971e-02 url=www.lawfareblog.com/dc-circuit-overrules-district-courts-due-process-ruling-qasim-v-trump
INFO:root:rank=5 pagerank=2.8460e-02 url=www.lawfareblog.com/how-trumps-approach-middle-east-ignores-past-future-and-human-condition
INFO:root:rank=6 pagerank=2.5252e-02 url=www.lawfareblog.com/why-trump-cant-buy-greenland
INFO:root:rank=7 pagerank=2.2457e-02 url=www.lawfareblog.com/oral-argument-summary-qassim-v-trump
INFO:root:rank=8 pagerank=2.1462e-02 url=www.lawfareblog.com/dc-circuit-court-denies-trump-rehearing-mazars-case
INFO:root:rank=9 pagerank=2.1103e-02 url=www.lawfareblog.com/second-circuit-rules-mazars-must-hand-over-trump-tax-returns-new-york-prosecutors

$ python3 pagerank.py --data=./lawfareblog.csv.gz --search_query='iran'
INFO:root:rank=0 pagerank=6.6131e-02 url=www.lawfareblog.com/praise-presidents-iran-tweets
INFO:root:rank=1 pagerank=2.9199e-02 url=www.lawfareblog.com/how-us-iran-tensions-could-disrupt-iraqs-fragile-peace
INFO:root:rank=2 pagerank=1.7709e-02 url=www.lawfareblog.com/cyber-command-operational-update-clarifying-june-2019-iran-operation
INFO:root:rank=3 pagerank=1.4604e-02 url=www.lawfareblog.com/aborted-iran-strike-fine-line-between-necessity-and-revenge
INFO:root:rank=4 pagerank=8.4512e-03 url=www.lawfareblog.com/iranian-hostage-crisis-and-its-effect-american-politics
INFO:root:rank=5 pagerank=8.3989e-03 url=www.lawfareblog.com/parsing-state-departments-letter-use-force-against-iran
INFO:root:rank=6 pagerank=8.2581e-03 url=www.lawfareblog.com/announcing-united-states-and-use-force-against-iran-new-lawfare-e-book
INFO:root:rank=7 pagerank=8.0561e-03 url=www.lawfareblog.com/trump-moves-cut-irans-oil-revenues-whats-his-endgame
INFO:root:rank=8 pagerank=7.1939e-03 url=www.lawfareblog.com/us-names-iranian-revolutionary-guard-terrorist-organization-and-sanctions-international-criminal
INFO:root:rank=9 pagerank=5.9405e-03 url=www.lawfareblog.com/iran-shoots-down-us-drone-domestic-and-international-legal-implications
```

## Part 3: Filtering Pages

The webgraph of lawfareblog.com (the P matrix) naturally contains a lot of structure. For example, essentially all pages on the domain have links to the root page https://lawfareblog.com/ and similarly broad pages like https://www.lawfareblog.com/topics and https://www.lawfareblog.com/subscribe-lawfare. These pages therefore have a large pagerank. We can get a list of the pages with the largest pagerank by running

```
$ python3 pagerank.py --data=./lawfareblog.csv.gz
INFO:root:rank=0 pagerank=8.4156e+00 url=www.lawfareblog.com/litigation-documents-related-appointment-matthew-whitaker-acting-attorney-general
INFO:root:rank=1 pagerank=8.4156e+00 url=www.lawfareblog.com/lawfare-job-board
INFO:root:rank=2 pagerank=8.4156e+00 url=www.lawfareblog.com/documents-related-mueller-investigation
INFO:root:rank=3 pagerank=8.4156e+00 url=www.lawfareblog.com/litigation-documents-resources-related-travel-ban
INFO:root:rank=4 pagerank=8.4156e+00 url=www.lawfareblog.com/subscribe-lawfare
INFO:root:rank=5 pagerank=8.4156e+00 url=www.lawfareblog.com/masthead
INFO:root:rank=6 pagerank=8.4156e+00 url=www.lawfareblog.com/topics
INFO:root:rank=7 pagerank=8.4156e+00 url=www.lawfareblog.com/our-comments-policy
INFO:root:rank=8 pagerank=8.4156e+00 url=www.lawfareblog.com/upcoming-events
INFO:root:rank=9 pagerank=8.4156e+00 url=www.lawfareblog.com/about-lawfare-brief-history-term-and-site
```
These pages are not very interesting, however, because they are not articles. How can we find the most important articles? The answer is to modify the P matrix by removing all links to non-article pages.

How do we know if a link is a non-article page? This is a hard question to answer with 100% accuracy, but there are many methods that get us most of the way there. An easy method is to remove all pages that have "too many" links. The `--filter_ratio` argument removes all pages that have more links than the specified fraction. 

Using the option of `--filter_ratio=0.2`, we can estimate the most important articles on the domain with the following command:

```
python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2
INFO:root:rank=0 pagerank=4.2773e+00 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=1 pagerank=2.7717e+00 url=www.lawfareblog.com/livestream-nov-21-impeachment-hearings-0
INFO:root:rank=2 pagerank=2.7533e+00 url=www.lawfareblog.com/opening-statement-david-holmes
INFO:root:rank=3 pagerank=1.8720e+00 url=www.lawfareblog.com/senate-examines-threats-homeland
INFO:root:rank=4 pagerank=1.7417e+00 url=www.lawfareblog.com/what-make-first-day-impeachment-hearings
INFO:root:rank=5 pagerank=1.7411e+00 url=www.lawfareblog.com/livestream-house-armed-services-committee-hearing-f-35-program
INFO:root:rank=6 pagerank=1.7347e+00 url=www.lawfareblog.com/whats-house-resolution-impeachment
INFO:root:rank=7 pagerank=1.6384e+00 url=www.lawfareblog.com/congress-us-policy-toward-syria-and-turkey-overview-recent-hearings
INFO:root:rank=8 pagerank=1.5597e+00 url=www.lawfareblog.com/summary-david-holmess-deposition-testimony
INFO:root:rank=9 pagerank=9.1265e-01 url=www.lawfareblog.com/events
```
When Google calculates their P matrix for the web, they use a similar process to modify the P matrix in order to reduce spam results. The exact formula they use, however, is a jealously guarded secret.

In the case above, notice that we have removed the blog's most popular article (www.lawfareblog.com/snowden-revelations). The blog editors believe that Snowden's revelations about NSA spying are so important that they link to the article on every single webpage in the domain, and out "anti-spam" `--filter-ratio` argument removed this article from the list. In general, it is a challenging open problem to remove spam from pagerank results, and all current solutions rely on careful human tuning and still have lots of false positives and false negatives.

## Part 4: Utilizing the Alpha Parameter

The runtime of pagerank depends heavily on the eigengap of the $\bar\bar P$ matrix, and that this eigengap is bounded by the alpha parameter. By utilizing the alpha parameter we can yield shorter runtimes.

For example, in the following commands, the first three take around 10 iterations while the last command takes around 700 iterations to converge.
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose
$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --alpha=0.99999
$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2
$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2 --alpha=0.99999
```

This begs the question does the second command (with the --alpha option but without the `--filter_ratio)` option not take a long time to run? The answer is that the P graph for www.lawfareblog.com naturally has a large eigengap and so is fast to compute for all alpha values, but the modified graph does not have a large eigengap and so requires a small alpha for fast convergence.

As such, changing the values of $\alpha$ give us very different page ranks:
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=4.2773e+00 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=1 pagerank=2.7717e+00 url=www.lawfareblog.com/livestream-nov-21-impeachment-hearings-0
INFO:root:rank=2 pagerank=2.7533e+00 url=www.lawfareblog.com/opening-statement-david-holmes
INFO:root:rank=3 pagerank=1.8720e+00 url=www.lawfareblog.com/senate-examines-threats-homeland
INFO:root:rank=4 pagerank=1.7417e+00 url=www.lawfareblog.com/what-make-first-day-impeachment-hearings
INFO:root:rank=5 pagerank=1.7411e+00 url=www.lawfareblog.com/livestream-house-armed-services-committee-hearing-f-35-program
INFO:root:rank=6 pagerank=1.7347e+00 url=www.lawfareblog.com/whats-house-resolution-impeachment
INFO:root:rank=7 pagerank=1.6384e+00 url=www.lawfareblog.com/congress-us-policy-toward-syria-and-turkey-overview-recent-hearings
INFO:root:rank=8 pagerank=1.5597e+00 url=www.lawfareblog.com/summary-david-holmess-deposition-testimony
INFO:root:rank=9 pagerank=9.1265e-01 url=www.lawfareblog.com/events

$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2 --alpha=0.99999
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=4.7947e+01 url=www.lawfareblog.com/covid-19-speech-and-surveillance-response
INFO:root:rank=1 pagerank=4.7947e+01 url=www.lawfareblog.com/lawfare-live-covid-19-speech-and-surveillance
INFO:root:rank=2 pagerank=7.2709e+00 url=www.lawfareblog.com/cost-using-zero-days
INFO:root:rank=3 pagerank=2.1691e+00 url=www.lawfareblog.com/lawfare-podcast-former-congressman-brian-baird-and-daniel-schuman-how-congress-can-continue-function
INFO:root:rank=4 pagerank=1.4214e+00 url=www.lawfareblog.com/events
INFO:root:rank=5 pagerank=1.0863e+00 url=www.lawfareblog.com/water-wars-increased-us-focus-indo-pacific
INFO:root:rank=6 pagerank=1.0863e+00 url=www.lawfareblog.com/water-wars-drill-maybe-drill
INFO:root:rank=7 pagerank=1.0863e+00 url=www.lawfareblog.com/water-wars-disjointed-operations-south-china-sea
INFO:root:rank=8 pagerank=1.0863e+00 url=www.lawfareblog.com/water-wars-us-china-divide-shangri-la
INFO:root:rank=9 pagerank=1.0863e+00 url=www.lawfareblog.com/water-wars-sinking-feeling-philippine-china-relations
```

Which of these rankings is better is entirely subjective, and the only way to know if you have the "best" alpha for your application is to try several variations and see what is best. If large alphas are good for your application, you can see that there is a tradeoff between quality answers and algorithmic runtime. 

# Task 2: The Personalization Vector

## Part 1: Comparing Personalization Vector to Queried Results

This function enables the `--personalization_vector_query` command line argument, which provides an alternative method for searching by doing the filtering on the personalization vector.

Which results are better? That depends on what you mean by "better." With the `--personalization_vector_query` option, a webpage is important only if other coronavirus webpages also think it's important; with the `--search_query` option, a webpage is important if any other webpage thinks it's important. As such, utilizing either or both options will result in different webpages being returned while removing or adding specific queries. You'll notice that in the later example, many of the webpages are about Congressional proceedings related to the coronavirus. From a strictly coronavirus perspective, these are not very important webpages. But in the broader context of national security, these are very important webpages.

Google engineers spend TONs of time fine-tuning their pagerank personalization vectors to remove spam webpages. Exactly how they do this is another one of their secrets that they don't publicly talk about.

## Using Personalization Vector
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query='corona'
INFO:root:rank=0 pagerank=8.8870e-01 url=www.lawfareblog.com/covid-19-speech-and-surveillance-response
INFO:root:rank=1 pagerank=8.8867e-01 url=www.lawfareblog.com/lawfare-live-covid-19-speech-and-surveillance
INFO:root:rank=2 pagerank=1.8256e-01 url=www.lawfareblog.com/chinatalk-how-party-takes-its-propaganda-global
INFO:root:rank=3 pagerank=1.4907e-01 url=www.lawfareblog.com/brexit-not-immune-coronavirus
INFO:root:rank=4 pagerank=1.4907e-01 url=www.lawfareblog.com/rational-security-my-corona-edition
INFO:root:rank=5 pagerank=1.0729e-01 url=www.lawfareblog.com/trump-cant-reopen-country-over-state-objections
INFO:root:rank=6 pagerank=1.0199e-01 url=www.lawfareblog.com/britains-coronavirus-response
INFO:root:rank=7 pagerank=1.0199e-01 url=www.lawfareblog.com/prosecuting-purposeful-coronavirus-exposure-terrorism
INFO:root:rank=8 pagerank=9.4298e-02 url=www.lawfareblog.com/lawfare-podcast-mom-and-dad-talk-clinical-trials-pandemic
INFO:root:rank=9 pagerank=8.7207e-02 url=www.lawfareblog.com/house-oversight-committee-holds-day-two-hearing-government-coronavirus-response
```
## Using Query
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --search_query='corona'
INFO:root:rank=0 pagerank=1.1602e-01 url=www.lawfareblog.com/congress-needs-coronavirus-failsafe-its-too-late
INFO:root:rank=1 pagerank=5.6374e-02 url=www.lawfareblog.com/house-oversight-committee-holds-day-two-hearing-government-coronavirus-response
INFO:root:rank=2 pagerank=5.0830e-02 url=www.lawfareblog.com/britains-coronavirus-response
INFO:root:rank=3 pagerank=5.0481e-02 url=www.lawfareblog.com/prosecuting-purposeful-coronavirus-exposure-terrorism
INFO:root:rank=4 pagerank=4.8031e-02 url=www.lawfareblog.com/livestream-house-oversight-committee-holds-hearing-government-coronavirus-response
INFO:root:rank=5 pagerank=4.7743e-02 url=www.lawfareblog.com/paper-hearing-experts-debate-digital-contact-tracing-and-coronavirus-privacy-concerns
INFO:root:rank=6 pagerank=4.3727e-02 url=www.lawfareblog.com/why-congress-conducting-business-usual-face-coronavirus
INFO:root:rank=7 pagerank=2.5817e-02 url=www.lawfareblog.com/israeli-emergency-regulations-location-tracking-coronavirus-carriers
INFO:root:rank=8 pagerank=2.5463e-02 url=www.lawfareblog.com/lawfare-podcast-united-nations-and-coronavirus-crisis
INFO:root:rank=9 pagerank=1.9066e-02 url=www.lawfareblog.com/congressional-homeland-security-committees-seek-ways-support-state-federal-responses-coronavirus
```

## Part 2: Filtering to Find Coronavirus Webpages That Don't Specifically Mention Coronavirus

Another use of the `--personalization_vector_query` option is that we can find out what webpages are related to the coronavirus but don't directly mention the coronavirus. This can be used to map out what types of topics are similar to the coronavirus.

For example, the following query ranks all webpages by their `corona` importance, but removes webpages mentioning `corona` from the results. You can see that there are many urls about concepts that are obviously related like "covid", "clinical trials", and "quarantine", but this algorithm also finds articles about Chinese propaganda and Trump's policy decisions. Both of these articles are highly relevant to coronavirus discussions, but a simple keyword search for corona or related terms would not find these articles.

```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query='corona' --search_query='-corona'
INFO:root:rank=0 pagerank=8.8870e-01 url=www.lawfareblog.com/covid-19-speech-and-surveillance-response
INFO:root:rank=1 pagerank=8.8867e-01 url=www.lawfareblog.com/lawfare-live-covid-19-speech-and-surveillance
INFO:root:rank=2 pagerank=1.8256e-01 url=www.lawfareblog.com/chinatalk-how-party-takes-its-propaganda-global
INFO:root:rank=3 pagerank=1.0729e-01 url=www.lawfareblog.com/trump-cant-reopen-country-over-state-objections
INFO:root:rank=4 pagerank=9.4298e-02 url=www.lawfareblog.com/lawfare-podcast-mom-and-dad-talk-clinical-trials-pandemic
INFO:root:rank=5 pagerank=7.9633e-02 url=www.lawfareblog.com/fault-lines-foreign-policy-quarantined
INFO:root:rank=6 pagerank=7.5307e-02 url=www.lawfareblog.com/limits-world-health-organization
INFO:root:rank=7 pagerank=6.8115e-02 url=www.lawfareblog.com/chinatalk-dispatches-shanghai-beijing-and-hong-kong
INFO:root:rank=8 pagerank=6.4847e-02 url=www.lawfareblog.com/us-moves-dismiss-case-against-company-linked-ira-troll-farm
INFO:root:rank=9 pagerank=6.4847e-02 url=www.lawfareblog.com/livestream-house-foreign-affairs-committee-holds-hearing-crisis-idlib
```

## Part 3: National Security Topic: Mexico
By using `personalization_vector_query='mexico' --search_query='-mexico'`, this allows us to see all the top list of webpages that are highly related to Mexico but do not specifically mention the word Mexico. By doing so, we are able to see some specific topics that could be interpreted to reference Mexico appear as well as seeing some articles containing important topics relating the US government and foreign nations in general. Likewise, we see that web pages relating to the President of the United States, such as tax returns and impeachment, have numerous amounts of incoming and outgoing connections.


```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query='mexico' --search_query='-mexico'
INFO:root:rank=0 pagerank=2.4554e-01 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=1 pagerank=2.1185e-01 url=www.lawfareblog.com/clearing-misconceptions-cross-border-migrant-smugglers-interview-gabriella-sanchez
INFO:root:rank=2 pagerank=2.0264e-01 url=www.lawfareblog.com/increasingly-difficult-migration-climate
INFO:root:rank=3 pagerank=1.4642e-01 url=www.lawfareblog.com/livestream-nov-21-impeachment-hearings-0
INFO:root:rank=4 pagerank=1.4642e-01 url=www.lawfareblog.com/opening-statement-david-holmes
INFO:root:rank=5 pagerank=1.0809e-01 url=www.lawfareblog.com/senate-examines-threats-homeland
INFO:root:rank=6 pagerank=1.0644e-01 url=www.lawfareblog.com/whats-house-resolution-impeachment
INFO:root:rank=7 pagerank=1.0644e-01 url=www.lawfareblog.com/what-make-first-day-impeachment-hearings
INFO:root:rank=8 pagerank=1.0644e-01 url=www.lawfareblog.com/livestream-house-armed-services-committee-hearing-f-35-program
INFO:root:rank=9 pagerank=9.4453e-02 url=www.lawfareblog.com/congress-us-policy-toward-syria-and-turkey-overview-recent-hearings
```
