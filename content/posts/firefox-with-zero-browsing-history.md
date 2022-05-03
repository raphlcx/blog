---
title: "Firefox with zero browsing history"
date: 2022-05-03T11:26:24+08:00
---
I have been using Firefox as my default browser for the past couple of years.

Firefox has several built-in features to customise how you want it to remember your browsing histories. You can configure it to remember everything (the default), remember none (acting like private browsing 24/7), or have fine-grain customisation to store site data (logged in websites) but not browsing histories.

With the majority of my time spent remote working and sharing screens, I wouldn't want my colleagues to know where I was browsing on my device. Browser highlights visited hyperlinks; typing a search query on the address bar shows the list of matching sites. All of these give away my privacy.

So, I decided to experiment with a zero-browsing history setup on my Firefox but not to the extent of it acting like private browsing all the time. I still want it to store login information on websites that I have logged in to so I wouldn't have to log in to all the sites every time I restart the browser.

I'm using Firefox 99.0.1 on macOS Monterey 12.3.1. The history settings:

{{< figure src="/ff-history-setting.png" caption="Firefox history setting">}}

That sets things up according to what I want - preserve site logins but do not remember any histories.

For frequently visited sites, I bookmark them. When typing search queries on the address bar, Firefox will search on the bookmark instead, allowing easier access to those bookmarked sites:

{{< figure src="/ff-address-bar-search-setting.png" caption="Firefox address bar search setting">}}

One tricky thing with bookmark is that Firefox will only select the matching result if the search query matches from the beginning of the bookmarked URL. For instance, for the bookmark named "AWS" with URL `https://us-east-1.console.aws.amazon.com/console/home?region=us-east-1#`.

{{< figure src="/ff-search-bookmark-by-name.png" caption="Searching with \"AWS\" does not select the bookmark">}}
{{< figure src="/ff-search-bookmark-by-url.png" caption="But searching with \"us\" selects the bookmark">}}

If it is unconventional for you to type "us" to search for the AWS bookmark, you can set a keyword on the bookmark. Right-click on the bookmark and select "Edit bookmark...". Here, we will use the keyword "a".

{{< figure src="/ff-bookmark-keyword.png" caption="Setting a keyword on a bookmark">}}

When searching bookmark on the address bar, you can type "a", and Firefox will select the AWS bookmark even when its URL starts with "us".

{{< figure src="/ff-search-bookmark-by-keyword.png" caption="Searching with \"a\" selects the bookmark!">}}
