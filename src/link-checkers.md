# Link Checkers

Many web based documents are likely to include references to external web resources via web links.

Various errors can arise when using web links in a published document, particularly a long-lived document:

- the initial web link / uniform resource locator (URL) may contain errors and return a page not found error;
- over time, the linked to resource may disappear from the web or get redirected to another location;
- over time, the content of a page may change.

In the first two cases, it is easy enough to create a simple automation script that extract all the known links in a page and then test that a request made to that location returns an "OK" response (rather than a "page not found" error or "page permanently relocated" warning, for example).

In the third case, we might want to create an automation script that submits the web location to a web page archiving service so that we can preserve a copy of the page at the time we first referenced it in our documents. If nothing else, this allows us to review a page we linked to so that we can see what it contained if it disappears or is edited at a later date.

As well as links to remote pages, links are often used in a web document to specify the location of an asset that is to be embedded in a web page, such as image, audio or video files. Once again, a link checker can be used to resolve the location of these assets to check that an asset of the desired type does still exist at that location. (Further checks might create a reference signature or "hash" value of the asset that can be used to identify whether or not the asset has changed over time.)
