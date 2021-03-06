---
date: 2013-03-29 10:26:00
layout: post
slug: generating-a-sitemap-in-jekyll-without-a-plugin
title: Generating a Sitemap in Jekyll without a Plugin
description: A guide to generating a sitemap.xml in Jekyll without using a plugin.
category: Development
tags: [Jekyll, Sitemap, XML]
suggested_tweet:
  url: 'http://davidensinger.com/2013/03/generating-a-sitemap-in-jekyll-without-a-plugin/'
  text: 'Generating a Sitemap in Jekyll without a Plugin by @DavidEnsinger'
  hashtags: ['Jekyll', 'jekyllrb']
  related: ['jekyllrb', 'havvg']
---

During my switch from **WordPress** to **Jekyll**, I decided to simplify the scope of my site. One of the changes I made was to remove my portfolio, as I’d like to put more of an emphasis on my writing than on my past projects. As a result, I culled many pages that were indexed by search engines. This meant that I needed a proper **sitemap.xml**, principally for use with Google’s [Webmaster Tools](www.google.com/webmasters/tools).

My initial thought was to find a plugin, but I then realized that wasn’t possible. I host the site on **GitHub Pages**, which runs Jekyll in `--safe` mode. This means that I can’t use any custom plugins and that precludes the implementation of the most popular plugin, which is the [Sitemap.xml Generator](http://www.kinnetica.com/projects/jekyll-sitemap-generator/).

One way around this limitation would be to generate the site locally with a source branch and then compile the pages on a master branch to push to GitHub. There are clear benefits to doing it this way, but for the time being I’d prefer to keep my site simple and let GitHub do the compiling.

My cursory search for a plugin-less solution led to this [sitemap.xml](https://github.com/havvg/havvg.github.com/blob/master/sitemap.xml) by [havvg](https://github.com/havvg).

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9 http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd" xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
{% raw %}
  {% for post in site.posts %}
  <url>
    <loc>{{ site.baseurl }}{{ post.url }}</loc>
    {% if post.lastmod == null %}
    <lastmod>{{ post.date | date_to_xmlschema }}</lastmod>
    {% else %}
    <lastmod>{{ post.lastmod | date_to_xmlschema }}</lastmod>
    {% endif %}
    <changefreq>weekly</changefreq>
    <priority>1.0</priority>
  </url>
  {% endfor %}
  {% for page in site.pages %}
  {% if page.sitemap != null and page.sitemap != empty %}
  <url>
    <loc>{{ site.baseurl }}{{ page.url }}</loc>
    <lastmod>{{ page.sitemap.lastmod | date_to_xmlschema }}</lastmod>
    <changefreq>{{ page.sitemap.changefreq }}</changefreq>
    <priority>{{ page.sitemap.priority }}</priority>
  </url>
  {% endif %}
  {% endfor %}
{% endraw %}
</urlset>
{% endhighlight %}

The {% raw %}`{% for post in site.posts %}`{% endraw %} loops through the posts and finds the pertinent metadata for the sitemap. The same looping method is used for pages, but with a conditional statement to include only the pages with the `sitemap` variable (and its corresponding values) present.

    ---
    layout: default
    sitemap:
        priority: 0.7
        changefreq: monthly
        lastmod: 2013-03-29T12:49:30-05:00
    ---

The `lastmod` variable uses the [ISO format](http://wwp.greenwichmeantime.com/info/iso.htm), which allows for YYYY-MM-DD. It’s bit simpler than including the notation for hours, minutes, seconds, and time zone.

In summary, it’s easy to generate a sitemap.xml for use with Jekyll on GitHub Pages. Thanks again to [havvg](https://github.com/havvg) for making available such a great example [sitemap.xml](https://github.com/havvg/havvg.github.com/blob/master/sitemap.xml).

<div class="gray-box">
  <p><strong>More Info:</strong> Visit the official <a href="http://www.sitemaps.org/">sitemaps.org</a> to learn more about the protocol.</p>
</div>
