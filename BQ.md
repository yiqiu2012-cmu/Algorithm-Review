# Job Recommendation
**Content-based**: Given item profiles (category, price, etc.) of your favorite, recommend items that are similar to what you liked before. 

##### Java Servlet and Tomcat:
I used Java to build a few servlets. The first one is a SearchItems API that provides the functionality to search around, The second servlet that allows a user to set or unset their favorite records. The last one that recommends similar items to the user. And I deploy these servlets on tomcat.
Java servlet: java class to interact with remote server to handle reponse and request preocudure calls
Tocat: environment to run web service and implement servlet

##### What’s the RESTful API and what’s the benefit of REST?
(REST) is an architectural approach to designing web services. 

**Benefits of REST**
Operations are directly based on HTTP methods, so that server doesn’t need to parse extra thing
URL clearly indicates which resources a client wants, easy for clients to understand.
The server is running in stateless mode, improving scalability. (stateless means server does not need to retain session information. Those information are sent to the server by the clients with every packet of information. Reduce server load caused by retention of session information)

 Implemented **Java servlets** to handle post and get reqeust given parameters (lat, lon, keyword), and make requests to search from a 3rd party service GitHub via **GitHubAPI**, result return as json array, use **Jackson** to parse jason and store in **model entity class** Item. Item implemented using **Bilder pattern** to aoivd errors when dealing with many parameters. Github only returns a description of a job position but we need to generate a tag or a keyword. We then use keywords to improve recommendation results. We **extract keywords/categories** from the job description for recommendation by using **MonkeyLearn API**, which implemnts **TF-IDF**, which is one of the very common algorithms to extract keywords from text.

**Why choose Monkey Learn API**
One of the few keyword extraction APIs that are free
Very detailed documentation and provide many client libraries
However, it has data throttling so don’t try to do a load test on it or play it too frequently. If your account got blocked, apply for a new one. 

**TF-IDF**
TF-IDF algorithm tells you the ordered keywords in a text document (keyword extraction). TF (Term frequency) is the frequency of a word in a document. However, ‘a’, ‘the’, ‘is’ are common in every document and their TF values are high in almost every document which will overwhelm some real keywords.  IDF (Inverse Document Frequency) is the importance of a word in general. IDF values are very low for these meaningless words. Final score is 
Final score = TF * IDF

Once a user favorite an item, we wiil store the item into MySQL database. MySQL is an open-source relational database management system (RDBMS).

**How we filter out recommended result**
Given a user, get all the events (ids) this user has favorited.
```
history: history_id, user_id, item_id, last_favor_time.
Set<String> itemIds = connection.getFavoriteItemIds(userId);
```
Given all these events, get their keywords and sort by count.
```
keywords: item_id, keyword.
Set<String> keywords = connection.getKeywords(itemId);
```
Given these categories, use Github Job API with keyword, then filter out user favorited events.

# Spring Shopping cart
Use Spring framework to initialize all Java classes(annotation-based) becasue Spring helps reduce dependency and increase code modularity




