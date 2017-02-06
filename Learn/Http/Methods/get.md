You should use the GET method when you are getting something off the server and not actually changing anything.  For example, the form for a search engine should use GET, since searching a Web site should not be changing anything that the client might care about, and bookmarking or caching the results of a search-engine query is just as useful as bookmarking or caching a static HTML page.

Also, don't ever use GET method in a form that capture passwords and other things that are meant to be hidden.

GET is good if you want the request to be cacheable and/or bookmarkable. 
