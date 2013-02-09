jekyll-homepage
===============

Dies ist meine mit Jekyll gebaute/generierte statische Webseite
Was hab ich gemacht:

Zunächst Jekyll installiert:

* https://github.com/mojombo/jekyll

Dann Jekyll struktur erzeugt (entnommen http://www.terminally-incoherent.com/blog/2012/01/25/building-your-first-jekyll-site-in-5-minutes/):

* mkdir jekyll-homepage
* cd jekyll-homepage
* touch _config.yml
* mkdir _layouts
* mkdir _posts
* mkdir _site
* cd _layouts/
* vi default.html
```
    <html>
    <head>
	    <title>My Jekyll Test</title>
    </head>
    <body>
	    {{ content }}
    </body>
    </html>
```
* vi post.html
```
    ---
    layout: default
    ---
 
    <h2>{{ page.title }}</h2>
 
	    {{ content }}
```
* cd ..
* cd _posts/
* vim 2012-01-18-hello.markdown
```
    ---
    layout: post
    title: Hello
    ---
 
    Hello World!
```
* cd ..
* vi index.html

```
    ---
    layout: default
    title: Home
    ---
 
    <ul>
	    {% for post in site.posts %}
		<li><a href="{{ post.url }}">{{ post.title }}</a> 
		({{ post.date | date_to_string}})</li>
	    {% endfor %}
    </ul>
```
* jekyll
* jekyll --server
* http://localhost:4000

Jetzt ne echte Seite erzeugt

* template von http://www.freecsstemplates.org/ benutzt d.h. css und pics reinkopiert

* index.html hat das layout der default.html bekommen und der content-bereich enthält jetzt alle post der Kategorie home
``` 
   ---
   layout: default
   title: Home
   ---
 
	    {% for post in site.posts %}
               {% if post.categories contains 'home' %}
                  {{ post.content }}
               {% endif %} 
	    {% endfor %}
```
* oldEntries.html hat das layout der default.html bekommen und der content-bereich enthält jetzt alle post der Kategorie old
```
   ---
   layout: default
   title: Home
   ---
 
	    {% for post in site.posts %}
               {% if post.categories contains 'old' %}
                  {{ post.content }}
               {% endif %} 
	    {% endfor %} 
```
* default.html ist eine modifizierte Version von http://www.freecsstemplates.org
* post.html
```
   ---
   layout: default
   ---
 
   <div class="post"><h2 class="title">

   {{ content }}

   </div>
```
