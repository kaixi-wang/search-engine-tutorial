**Search Algorithm Comparison**

Considering the single query \'Lebron James\' in the screenshots above,
it appears that the default algorithm with results sorted in descending
order gives the most relevant results -- meaning pages where the main
subject/topic has something to do with \'Lebron James\'. The default
algorithm seems to favor article pages, while the Page Rank algorithm
tends to favor section pages. However, for both algorithms, it appears
that ascending order gives mostly irrelevant results, which makes sense.

In order to compare Solr\'s default search algorithm with the page
ranking algorithm more in depth, the results from a set of 8 unrelated
query terms were collected and analyzed.

The following pages contain tables of the results:

![](/Volumes/WDMyPasspor/kaixiwang_iMac_migration-from-MBP/localUSC/CSCI572/Homework/HW4/search-algorithm-comparison/media/image1.png){width="0.6in"
height="0.4in"}![](/Volumes/WDMyPasspor/kaixiwang_iMac_migration-from-MBP/localUSC/CSCI572/Homework/HW4/search-algorithm-comparison/media/image2.png){width="6.5in"
height="3.042361111111111in"}![](/Volumes/WDMyPasspor/kaixiwang_iMac_migration-from-MBP/localUSC/CSCI572/Homework/HW4/search-algorithm-comparison/media/image3.png){width="6.5in"
height="1.7590277777777779in"}

![](/Volumes/WDMyPasspor/kaixiwang_iMac_migration-from-MBP/localUSC/CSCI572/Homework/HW4/search-algorithm-comparison/media/image4.png){width="6.5in"
height="2.308333333333333in"}

![](/Volumes/WDMyPasspor/kaixiwang_iMac_migration-from-MBP/localUSC/CSCI572/Homework/HW4/search-algorithm-comparison/media/image5.png){width="6.5in"
height="2.5416666666666665in"}![](/Volumes/WDMyPasspor/kaixiwang_iMac_migration-from-MBP/localUSC/CSCI572/Homework/HW4/search-algorithm-comparison/media/image6.png){width="6.5in"
height="2.761111111111111in"}![](/Volumes/WDMyPasspor/kaixiwang_iMac_migration-from-MBP/localUSC/CSCI572/Homework/HW4/search-algorithm-comparison/media/image7.png){width="6.5in"
height="1.9368055555555554in"}

![](/Volumes/WDMyPasspor/kaixiwang_iMac_migration-from-MBP/localUSC/CSCI572/Homework/HW4/search-algorithm-comparison/media/image10.png){width="6.5in"
height="1.6527777777777777in"}![](/Volumes/WDMyPasspor/kaixiwang_iMac_migration-from-MBP/localUSC/CSCI572/Homework/HW4/search-algorithm-comparison/media/image12.png){width="6.5in"
height="2.025in"}

Notice that the \'og\_url\' for all the results returned based on the
Page Rank algorithm are all \[\'http://www.latimes.com\'\]. Looking at
the \'id\' of these pages indicates that these pages are not necessarily
duplicates -- they mostly refer to different sections or topic homepages
present in the navigation bar that link to and describe many articles
featured under that particular section or topic. Since we are only
showing the top ten results, this overlap in og\_url is expected due to
the way page ranks are calculated.

To expand on what was described in Part II, a higher PR score indicates
that a page has more in-links (other pages linking to the particular
page) from higher scoring pages. This is because in the calculations for
the Page Rank, the score for any individual page is a sum function of
the PR scores of the pages that link to it. Each page essentially
equally distributes its own PR score among the pages it links out to,
and so a higher score can be obtained from having many pages linking to
it (sum of many small values), having fewer pages with higher scores
themselves linking to it (sum of a few larger value), or both. Regarding
the results obtained for this assignment, pages linked to in the
navigation bar (main section pages with
og\_url=\'http://www.latimes.com\') thus get the highest PR scores
because all, or nearly all, other pages contain a link to them.
Accordingly, single article pages have lower scores because fewer other
pages link to them. Moreover, if an article is featured on the main page
of the LAtimes or the main page of a section, it will also have a higher
score because it receives a portion of the relatively large score of the
main page while articles not appearing in a feature main page will not
have this contribution. This means that headline news or current
articles on the most salient, attention grabbing topics (as determined
by the writers and editors of the news site) will have relatively high
scores compared to that of older, perhaps outdated, archived articles or
articles on more obscure subject matters.

Another way to understand PR is as a probability distribution where the
average sum of all page scores upon convergence is 1. Any single PR
score represents that probability that a random walk of the web will
land on that page. It this case, it is likely that a person randomly
clicking links from page to page will land on a main section page since
almost -- if not all -- pages on the news site have a link to it in the
navigation bar, or, although slightly less likely, on a feature article
since the main page will have a link to it.

Knowing just that PR scores are derived from linkage structure, one
might expect the set of results from using the PR algorithm to be the
same for every query since the linkages don\'t change in response to a
query -- however, this is not the case. Although we can probably assume
every page on the LAtimes domain to have a link back to the homepage,
only 7 out of the 8 PR results contained the homepage as the top ranked
results. The query \"Hurricane Florence\" actually returned a section
page as the top result. Additionally, one might assume that every result
will contain a match for the query terms on the page, which also isn\'t
true. The query for \'Star Wars\' using the PR algorithm ranks the
homepage as first, but the homepage does not mention \'Star Wars.\' This
behavior can be attributed to the fact that we have only added Page Rank
scores as a fieldType that affects ranking. The search engine still runs
on Lucene and therefore uses Lucene indexing and scoring. When
performing a search using the PR scores, we aren\'t always returned the
same set of the highest scoring main pages in the same order because
queries are processed using the Lucene scoring system that implements PR
scores as a query class. PR scores and term matching on a page are not
the sole determinants of the result set.

One way to compare the performance of the two algorithms is to count the
overlaps. The following two bar charts show the number of similar
results in each set of results by query. The top graph shows overlaps
based on og\_url. The bottom graph shows exact duplicate pages based
![](/Volumes/WDMyPasspor/kaixiwang_iMac_migration-from-MBP/localUSC/CSCI572/Homework/HW4/search-algorithm-comparison/media/image13.png){width="5.261805555555555in"
height="2.185416666666667in"}on id.

(\*) Note: Since all og\_url values for results found using PageRank
were \[\'http://www.latimes.com\'\], the number of overlaps represents
the count of \[\'http://www.latimes.com\'\] in the results obtained from
using the default algorithm

![](/Volumes/WDMyPasspor/kaixiwang_iMac_migration-from-MBP/localUSC/CSCI572/Homework/HW4/search-algorithm-comparison/media/image14.tiff){width="5.760416666666667in"
height="2.248611111111111in"}

Additionally, on the following page is a comparison of the top 30 PR
scores of a union of all the 8 results (in descending order by score)
obtained from using the page rank algorithm (left) and the default
algorithm (right):

![](/Volumes/WDMyPasspor/kaixiwang_iMac_migration-from-MBP/localUSC/CSCI572/Homework/HW4/search-algorithm-comparison/media/image15.png){width="1.7673611111111112in"
height="4.504166666666666in"}![](/Volumes/WDMyPasspor/kaixiwang_iMac_migration-from-MBP/localUSC/CSCI572/Homework/HW4/search-algorithm-comparison/media/image16.png){width="1.7083333333333333in"
height="4.55625in"}As is expected, for the PR results, the results with
the higher rank tend to have the greatest PR scores. For the default
results, there is no such relationship between the rank and the score.
This, along with the few overlapping results, suggests that the two
ranking algorithms perform significantly differently.

CONCLUSION

In most cases, using Page Rank alone to sort results from a search
engine will not suffice in most circumstances. The behavior observed
indicates that the PR algorithm is best suited for underspecified
queries and finding reliable resources, but less suited for searches for
specific answers to questions. However, it appears that the algorithm
would provide some added value when used in conjunction with other
sorting algorithms in biasing results towards more \'important\' or
\'reliable\' results on popular or trending topics.
