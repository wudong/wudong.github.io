---
layout: post
title:  "A Jekyll/Github Page sitemap generator"
date:   2014-06-23 12:08:26
categories: "web-develop"
disqus: true
keywords:
- Jekyll
- Sitemap
- Sitemap Generator
- Github Page

excerpt: "A sitemap generator for Jekyll/Github page without using plugin"
tags:
- Jekyll
- website development
---

Never a fan of WordPress, I have been back and forth between
site generators like [Jekyll](http://http://jekyllrb.com/) and
[Docpad](http://docpad.org/) for this blog. Problem of Docpad is it is more than a blog generator,
which incurs additional complexity that is not really necessary for a simple
personal blog like this! In the end I settled with Jekyll.
Even though it uses Ruby, a language
I barely know, the feature set is good enough for me.

Here is a simple sitemap generator for Jekyll which does not need a plugin.
Github Page (where this site hosted) cannot run any plugin so this is better
than some other options out there. Cannot remember where it is initially from,
I only made a few minor changes to make it complied
with google webmaster tool. If it is your copyright thing please let me know.

Simply copy the content below into a file, name it 'sitemap.xml', and put on your site's
root directory.

{% highlight xml linenos=table %}
{% raw %}  
---
layout: nil
---
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9
  http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd"
  xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  {% for post in site.posts %}
  <url>
    <loc>{{site.url}}{{ site.baseurl }}{{ post.url }}</loc>
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
    <loc>{{ site.url }}{{ site.baseurl }}{{ page.url }}</loc>
    <lastmod>{{ page.sitemap.lastmod | date_to_xmlschema }}</lastmod>
    <changefreq>{{ page.sitemap.changefreq }}</changefreq>
    <priority>{{ page.sitemap.priority }}</priority>
  </url>
  {% endif %}
  {% endfor %}
</urlset>
{% endraw %}
{% endhighlight %}

The 'layout: nil' on line 2 is important otherwise Jekyll will not interprete
the tags. Enjoy!
