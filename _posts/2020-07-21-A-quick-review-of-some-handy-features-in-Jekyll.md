---
layout: post
title:  "A quick review of some handy features in Jekyll"
date:  2020-07-21T11:19:00+08:00
tags: ["Jekyll", "review_label"]
categories: ["Programming", "Webpage"]
keywords: Liquid site.tags site.categories
---

> This post is a summary of my initial blog building. 
It offers some guidance on how to use Jekyll and presents some clues to delve into details. 
To better understand some features, it interprets them using some javascript concepts. 

<!--more-->


<br>

{% include toc.html %}



## Liquid Basics

*Liquid is an open-source template language created by Shopify and written in Ruby. 
It has been in production use at Shopify since 2006 and is now used by many other hosted web applications.*

### Data Types
*Six types: String, Number, Boolean, Nil, Array, or EmptyDrop.*

Liquid variables can be initialized by using the {%- raw -%}`{% assign %}` or `{% capture %}` tags{% endraw %}.

 - **Boolean**: &nbsp;`true`, `false`. e.g. {% raw %}{% assign flag = true %}{% endraw %}

   Note that All values in Liquid are treated as `true`, except for `nil` and `false`.

 - **Number**: &nbsp;floats and integers. e.g. {% raw %}{% assign num = 10 %}{% endraw %} 


 - **String**: &nbsp;e.g. {% raw %}{% assign str = 'Hello World' %}{% endraw %}


 - **Array**: &nbsp;a list of variables can be accessed using indexes e.g. `site.posts[0]` or iterated through using {%- raw -%}`{% for post in site.posts %}`{% endraw %} statement.

Note: 

`.first` or `.last` can be used to access the first or last array element. 

`.size` can be used to get the number of elements in an array or *[object](#object-and-list)*. 

In the case of a string, `.size` will return the number of characters.



### Operators
```
== != > < >= <= or and contains
```

### Tags
 - **Control Flow**

{% raw %}
```
{% if %} {% elsif %} {% else %} {% endif %}
{% case %} {% when %}
{% unless %}
```
{% endraw %}

Note that: `elsif` is not "elseif". `unless` means "if not".

 - **Iteration** 
{% raw %}
```
{% for %} {% else %} {% endfor %}
{% break %} {% continue %}
{% cycle %}
{% tablerow %}
```
{% endraw %}

 - **Other tags**
{% raw %}
```
{% assign %} {% include %}
{% raw %} {% endraw % }
{% comment %} {% endcomment %}
```
{% endraw %}

### Filters
```
append prepend join split default date map where
plus minus times divided_by modulo floor ceil abs round at_least at_most
sort sort_natural reverse first last size
replace replace_first newline_to_br relative_url
strip strip_html strip_newlines rstrip lstrip
slice concat escape escape_once
remove remove_first truncate truncatewords compact
capitalize downcase upcase
uniq url_decode url_encode 
```

### TOC

*GitHub Pages can't run custom Jekyll plug-ins so when generating Tables of Contents (TOCs), you're stuck with either a JavaScript solution or using kramdown's `{:toc}` option. However, by using `{:toc}`, you are forced to have that code next to your actual markdown and you can't place it in a layout. This means every. single. post. will need to have the snippet. If you choose the JavaScript approach, that's perfectly fine but what if JS is disabled on the someone's browser or your page is just really long and it becomes inefficient.*

*This `toc.html` solution is written entirely in Liquid and can be used as an {% raw %}`{% include %}`{% endraw %}.
Use it in your template layout where you have {% raw %}`{{ content }}`{% endraw %}.*

[Jekyll Pure Liquid Table of Contents][6]

[A Jekyll TOC without Plugins or JavaScript][7]

Note: It seems that the table of contents can only be placed on the top of the post if using `toc.html`. When inserted into the post it doesn't work as expected.


## Built-in Jekyll Variables

### Object and List
There are two types of built-in variables: 
 - **Object variable** is like a javascript object with key/name pairs. 

e.g., suppose there is a `foo: bar` property in the `site` scope object, you can either use dot operator `site.foo` or string `site["foo"]` to access it.

To iterate through all the keys in an object use {%- raw -%}`{% for %}`{% endraw %} statement. e.g.:

{%- raw -%}
```
{% for item in site %}
  {{ item }}
{% endfor %}
```
{% endraw %}

 - **List variable** can be accessed using indexes like an array, e.g. `site.posts[0]` . 

To iterate through a list use {%- raw -%}`{% for %}`{% endraw %} statement. e.g.:
{%- raw -%}
```
{% for post in site.posts %}
  {{ post.title }}
{% endfor %}
```
{% endraw %}


Note that although we use javascript concept of *object* and *List(or Array)* to describe these built-in variables, 
the difference is they are immutable. 

e.g., the following code will still print out the title of the first post without any warning prompted:
{%- raw -%}
```
{% assign site.posts[0] = nil %}
{{ site.posts[0].title }}
```
{% endraw %}

The good news is they can be assigned to custom variables. e.g.:
{%- raw -%}
```
{% assign test = site %}
{% for post in test.posts %}
  {% assign pos = post %}
  {{ pos.title }}
{% endfor %}
```
{% endraw %}

### Global Variables

 - **`site`** An *Object variable* refers to the entire site scope containing built-in site variables and custom site variables set via configurations in `_config.yml`.

 - **`page`** An *Object variable* refers to the current page scope containing built-in page variables and custom page variables set via the front matter.

Note that the default value of undefined or unassigned custom variables in the above scopes is `nil`.

<br>
*For other global Variables (e.g. layout, content, paginator), see ["Jekyll Docs - Variables"][1]*


### Variables in the Site Scope Object

 - `site.posts` A reverse chronological **list** of all Posts in the `_posts` directory. (from newest to oldest)

 - `site.categories` An **object** stores all categories appeared in posts as property names whose values are **lists** of correspoding Posts.

 - `site.categories.CATEGORY_X` The **list** of all Posts with a category called "*CATEGORY_X*".

 - `site.tags` An **object** stores all tags appeared in posts as property names whose values are **lists** of correspoding Posts.

 - `site.tags.TAG_X` The **list** of all Posts with a tag called "*TAG_X*".

 - `site.collections` A **list** of all the collections (including posts).

<br>
*All the custom site variables set via `_config.yml` are available through the site variable.*


*For more site Variables, see &nbsp;[Jekyll Docs - Variables][1]*


### Variables in the Page Scope Object
To print out all the page variables in the first post: 
{%- raw -%}
```
{% for post in site.posts %}
  {% for item in post %}
    {{ item }}
  {% endfor %}
  {% break %}
{% endfor %}
```
{% endraw %}

The result:

`relative_path` `excerpt` `previous` `title` `categories` `collection` `url`
 `content` `id` `output` `date` `next` `tags` `path` `draft` `slug` `ext` `layout`

(Note that custom variables in the page object are not included in the result)

*More infomation about page variables, see [Jekyll Docs - Variables][1]*


### site.tags and site.categories

Note that as of version '4.1.1', Jekyll automatically fills the `site.tags` without user intervention with tags used in at least one post. 
Each tag property in the `site.tags` object points to a list of posts that contain it. 
However, it doesn't count in the tags in other non-posts collections. `site.categories` dittoes.

<br>
Print out `site.categories`: 
{%- raw -%}
```
{{ site.categories }}
```
{% endraw %}
The result:

```
{"my"=>[#, #, #, #, #, #], "cat2"=>[#, #, #, #, #], "cat1"=>[#, #, #, #]}
```
<br>
Access the values in `site.categories` object:
{%- raw -%}
```
{{ site.categories["my"].size }}
{{ site.categories.my.size }}
{{ site.categories.my[0].title }}
{{ site.categories["my"][0].title }}
```
{% endraw %}


However, when you loop through `site.tags` or `site.categories`, it will return an array(not property key) with two elements every time. 
The first element is the tag name or category name. The second element is a list of corresponding posts. 

{%- raw -%}
```
{% for tag in site.tags %}
  {{ tag[0] }} / {{ tag[1].size }}
  {% break %}
{% endfor %}

{% for tag in site.tags %}
  {{ tag.size }}
  {% break %}
{% endfor %}
```
{% endraw %}

This is inconsistent with the behavior of other "objects". e.g., the following code failed:
{%- raw -%}
```
{% for item in site %}
  {{ item[0] }} / {{ item[1] }}
  {% break %}
{% endfor %}
```
{% endraw %}


## Custom Array and Object

*You cannot initialize arrays or objects using only Liquid.*

### Custom Array

*You can, however, use the `split` filter to break a string into an array of substrings.*

Note that arrays created using the `split` filter are immutable. e.g., the following code will output: `aa-bb-cc`.
{%- raw -%}

```
{% assign arr = "aa|bb|cc" | split: "|" %}
{% assign arr[0] = "dd" %}
{{ arr | join: "-" }}
```
{% endraw %}


### Custom Object
You can also use custom collection to create a predefined, immutable map with key/value pairs. For example: 

1 . Create a new custom collection called `metas`, set `output` to `false` to stop outputting HTML files in this directory. 

```
collections:
  metas:
    output: false
```

2 . Put the following *Front Matter* key/value pairs into a file called `categories.md` in the `metas` collection.

```
---
name: categories
cat1: category one
cat2: category two
---
```

3 . Create a file called `category_info.html` containing the following code.
{% raw %}
```
{% assign category_info = nil %}
{% for meta in site.metas %}
  {% if meta.name == "categories" %} {% assign category_info = meta %} {% break %} {% endif %}
{% endfor %}
```
{% endraw %}


4 . Use {% raw %}{% include %}{% endraw %} to extract the "string map" from custom collection `site.metas`.
{%- raw -%}
```
{% include category-info.html %}
{% if category_info and category_info[page.category] %}<p>({{ category_info[page.category] }})</p>{% endif %}
```
{% endraw %}

<br>
Note that you are not supposed to loop through this "string map" using {% raw %}`{% for %}`{% endraw %} statement because there are also multiple built-in variables inside the "string map".

### Nested Square Bracket Operator

Another problem is that nested square bracket operator are not supported in Jekyll.

e.g., if you execute {% raw %}`{{ site.metas[0]["name"] }}`{% endraw %}, it will output `categories` as we defined above in step 2.


But the following code won't work:
{% raw %}
```
{% assign arr = "name|title" | split: "|" %}
{{ site.metas[0][arr[0]] }}
```
{% endraw %}

To fix this problem, you need to dismantle the nested square brackets:
{% raw %}
```
{% assign arr = "name|title" | split: "|" %}
{% assign temp = arr[0] %}
{{ site.metas[0][temp] }}
```
{% endraw %}

or

{% raw %}
```
{% assign arr = "name|title" | split: "|" %}
{% capture key %}{{ arr[0] }}{% endcapture %}
{{ site.metas[0][key] }}
```
{% endraw %}


## Others

### Date & Time
The date set in a page such as `date: 2020-07-10 16:54:16` is in UTC. The default timezone is `+0000` if no timezone attached.
It has nothing to do with the timezone setting in `_config.yml`, which tells Jekyll which timezone to display. 
So if your local timezone is `+0500` and your local time is `2020-07-10 16:54:16`, 
when you put `2020-07-10 16:54:16` in the page's front matter, it is `2020-07-10 16:54:16+0000` in effect.
By executing {% raw %}`{{ page.date | date: "%F %R%z" }}` {% endraw %}, the actual time displayed will be `2020-07-10 21:54:16+0500` which is not what we expect.

You should specify a date in the front matter like this: `date: 2020-07-10T16:54:16+0500`.

[More Information about date formatting][8]


### Github Pages

Uncommmnt these lines in the `Gemfile`:
```
source 'https://rubygems.org'
gem 'github-pages', group: :jekyll_plugins
```

Set `baseurl` in `_config.yml` to your repository name  if your repository name is not the github domain name `<username>.github.io`.

<br>
<font style="font-size: 12px;font-style: italic">(Note that all the Jekyll features discussed in this post are based on Jekyll Version 4.1.1, there may be changes made in the future version of Jekyll)</font>

## References & Resources  {#references}

*\[1\]. [Jekyll Docs - Variables][1]*
{: id="ref-jekyll-docs-variables" class="ref-item" }

*\[2\]. [Shopify Cheat Sheet][2]*
{: id="ref-shopify-cheat-sheet" class="ref-item" }

*\[3\]. [Liquid Basics][3]*
{: id="ref-liquid-basics" class="ref-item" }

*\[4\]. [Liquid for Designers][4]*
{: id="ref-liquid-for-designers" class="ref-item" }

*\[5\]. [Shopify Liquid Tutorial][5]*
{: id="ref-shopify-liquid-tutorial" class="ref-item" }

*\[6\]. [Jekyll Pure Liquid Table of Contents][6]*
{: id="ref-github-jekyll-toc" class="ref-item" }

*\[7\]. [A Jekyll TOC without Plugins or JavaScript][7]*
{: id="ref-a-jekyll-toc-without-plugins-or-javascript" class="ref-item" }

*\[8\]. [Date formatting][8]*
{: id="ref-date-formatting" class="ref-item" }

*\[9\]. [Types of Github Pages Sites][9]*
{: id="ref-types-of-github-pages-sites" class="ref-item" }


[1]: <https://jekyllrb.com/docs/variables/>  "Jekyll Docs - Variables"
[2]: <https://www.shopify.com/partners/shopify-cheat-sheet>  "Shopify Cheat Sheet"
[3]: <https://shopify.dev/docs/themes/liquid/reference/basics>  "Liquid Basics"
[4]: <https://github.com/Shopify/liquid/wiki/Liquid-for-Designers>  "Liquid for Designers"
[5]: <https://shopify.github.io/liquid/>  "Shopify Liquid Tutorial"
[6]: <https://github.com/allejo/jekyll-toc>  "Jekyll Pure Liquid Table of Contents"
[7]: <https://allejo.io/blog/a-jekyll-toc-without-plugins-or-javascript/>  "A Jekyll TOC without Plugins or JavaScript"
[8]: <https://learn.cloudcannon.com/jekyll/date-formatting/>  "Date formatting"
[9]: <https://docs.github.com/en/github/working-with-github-pages/about-github-pages#types-of-github-pages-sites>  "Types of Github Pages Sites"





