---
layout: post
title: "Фрагменты кода"
date: 2016-10-01 16:25:06
tags: code jekyll
description: Пример сообщения, показывающий, как будут выглядеть примеры кода
---

## Введение

Для подсветки синтаксиса кода я использую тему Darcula от Intellij IDEA, которую я нашёл в этом посте [Тема Darcula для Pygments](http://smasue.github.io/pygments-darcula).

XML с номерами строк (флаг linenos), `{{ "{%" }} highlight xml linenos %}`:
{% highlight xml linenos %}
{% raw %}
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
  <channel>
    <title>{{ site.name }}</title>
    <description>{{ site.description }}</description>
    <link>{{site.baseurl | prepend:site.url}}</link>
    <atom:link href="{{site.baseurl | prepend:site.url}}/feed.xml" rel="self" type="application/rss+xml" />
    {% for post in site.posts limit:10 %}
      <item>
        <title>{{ post.title }}</title>
        <description>{{ post.content | xml_escape }}</description>
        <pubDate>{{ post.date | date: "%a, %d %b %Y %H:%M:%S %z" }}</pubDate>
        <link>{{post.url | prepend:site.baseurl | prepend:site.url}}</link>
        <guid isPermaLink="true">{{post.url | prepend:site.baseurl | prepend:site.url}}</guid>
      </item>
    {% endfor %}
  </channel>
</rss>
{% endraw %}
{% endhighlight xml %}

>JSON
{:.filename}
{% highlight json %}
{"employees":[
    {"firstName":"John", "lastName":"Doe"},
    {"firstName":"Anna", "lastName":"Smith"},
    {"firstName":"Peter", "lastName":"Jones"}
]}
{% endhighlight %}

>SQL
{:.filename}
{% highlight SQL %}
select count(*) as cm_content_nodes
from alf_node nd, alf_qname qn, alf_namespace ns
where qn.ns_id = ns.id
  and nd.type_qname_id = qn.id
  and ns.uri = 'http://www.alfresco.org/model/content/1.0'
  and qn.local_name = 'content';
{% endhighlight %}

>Java
{:.filename}
{% highlight java %}
private String getToken(HttpClient client) throws UnsupportedEncodingException{
  Cookie[] cookies = client.getState().getCookies();
  for (Cookie cookie : cookies){
    if (cookie.getName().equals("Alfresco-CSRFToken")){
      return URLDecoder.decode(cookie.getValue(), "UTF-8");
    }
  }
  return null;
}
{% endhighlight %}

Чтобы добавить имя к фрагменту кода, как в примерах выше, добавьте перед фрагментом следующую конструкцию:

{% highlight bash %}
{% raw %}
>Java
{:.filename}
{% highlight java %}
...
{% endraw %}
{% endhighlight %}
