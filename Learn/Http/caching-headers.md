

### Cache Control

Cache-Control is an HTTP response header that defines the caching rules for a file. It can be set to a comma-separated selection of the following values:

public or private define who should be able to cache the file.
public: The file is cacheable by the browser and by all intermediaries (such as CDNs).
private: The file is only cacheable by the browser.
no-cache or no-store define stricter caching rules
no-cache: Cache the file, but don’t use it without checking with the server that it hasn’t changed.
no-store: Nobody should cache anything.
max-age: The number of seconds for which we can use the cached file. After this, we will have to check for it again (although we may not have to redownload it; see cache validation).
Combining these settings can be useful in a number of configurations:

public and a given max-age is useful for purely static content.
private and a given max-age is for information that isn’t necessarily personal, but is user-specific.
no-cache is great when it’s critical to have the latest version of a file (for live feeds, or content based on random numbers, for instance).
no-store is mainly for private, personal data that shouldn’t be cached (for security reasons).
Once again, there is another header with overlapping functionality: Expires. While Cache-Control: max-age=600 will set a 10 minute timeout, Expires: Fri, 20 Nov 2015 23:56:13 GMT will just time out at the set date. When using Expires, we also add a Date header so we know what time the server thinks it currently is.

Also good to know: max-age overrides Expires.


### Etags

An ETag is a validation token, or, simply put, a **sort of version number that is stuck on the file**. This is useful when a browser wants to check whether its cached copy of a file still is up to date (this check is called a “cache validation”). Instead of asking for the whole file again, it can just ask if the server still is serving the same file as last time. If so, there’s no need to redownload it.

Technically, this is done by giving each file a code. Received files will come with a header such as ETag: "564bb8e6-4d4b". This information is saved, and the next time we request the same file, the browser sends along the header If-None-Match: "564bb8e6-4d4b". The web server checks that this corresponds to the file it’s got. If we have a match, the server will respond with 304 Not Modified. Otherwise, it’ll send 200 OK along with the new version of the file (and its new ETag header).

ETags are an important cache mechanism, as they introduce big optimizations. A roundtrip to the server is still necessary, but it can save us the trouble **of redownloading a file we already have**. They come into play if Cache-Control has been set to no-cache or if the cached file is older than the specified max-age. If you are unfamiliar with web cache, this may still sound like some technical mumbo jumbo, so let’s clear it up.


# Resources

* [https://kjaer.io/quick-cache-this/](#https://kjaer.io/quick-cache-this/)
