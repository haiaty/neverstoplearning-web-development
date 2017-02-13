


### Etags

An ETag is a validation token, or, simply put, a **sort of version number that is stuck on the file**. This is useful when a browser wants to check whether its cached copy of a file still is up to date (this check is called a “cache validation”). Instead of asking for the whole file again, it can just ask if the server still is serving the same file as last time. If so, there’s no need to redownload it.

Technically, this is done by giving each file a code. Received files will come with a header such as ETag: "564bb8e6-4d4b". This information is saved, and the next time we request the same file, the browser sends along the header If-None-Match: "564bb8e6-4d4b". The web server checks that this corresponds to the file it’s got. If we have a match, the server will respond with 304 Not Modified. Otherwise, it’ll send 200 OK along with the new version of the file (and its new ETag header).

ETags are an important cache mechanism, as they introduce big optimizations. A roundtrip to the server is still necessary, but it can save us the trouble **of redownloading a file we already have**. They come into play if Cache-Control has been set to no-cache or if the cached file is older than the specified max-age. If you are unfamiliar with web cache, this may still sound like some technical mumbo jumbo, so let’s clear it up.


# Resources

* [https://kjaer.io/quick-cache-this/](#https://kjaer.io/quick-cache-this/)
