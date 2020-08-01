---
layout: post
title:  "Markdown Test"
date:   2020-07-10T16:54:16+0800
hidden: false
tags: mytest_tag test_label hidden_label
categories: my
series: playlist 2
hidden: true
comments: false
mathjs: true
---
> You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

<!--more-->

{% include toc.html %}


### ❯ Heading

# h1 Blah Blah (h1 not good inside the article)  
## h2 Blah Blah
### h3 Blah Blah
#### h4 Blah Blah

### ❯ Cross Line 

~~this~~

### ❯ Jump Link

*This jumps to reference.* [[1]](#jekyll-docs)

### ❯ Font Color

<p><font style="color:red">This is red color text.</font></p>

This is red blue text.
{: style="color: blue;"}

### ❯ Font Size

<p><font style="font-size: 30px;">This is big size text.</font></p>

This is big size text.
{: style="color: green; font-size: 30px;"}

<p><font style="font-size: 12px;">This is small size text.</font></p>

This is small size text.
{: style="color: green; font-size: 12px;"}

### ❯ List

- Color1 `#f03c15`
{: style="color: #f03c15;"}
- Color2 `#c5f015`
{: style="color: #c5f015;"}
- Color3 `#1589F0`
{: style="color: #1589F0;"}

### ❯ Task Lists

- [x] Finish my changes
- [ ] Push my commits to GitHub
- [ ] Open a pull request

### ❯ Images

<br>
`{: style="width: 500px; max-width: 100%;"}`
![Image of Yaktocat]({{ '/assets/images/bg-500x300-b.png' | relative_url }})
{: style="width: 500px; max-width: 100%;"}
*(Fig. 1 The is image Test.)*
<br>

<br>
`{: style="width: 40%;" class="center"}`
![Image of Yaktocat]({{ '/assets/images/bg-500x300-b.png' | relative_url }})
{: style="width: 40%;" class="center"}
*Fig. 1 The is image Test.*
{: style="color: red; text-align: center;"}
<br>


### ❯ Tables

```
Colons can be used to align columns.

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

There must be at least 3 dashes separating each header cell.
The outer pipes (|) are optional, and you don't need to make the 
raw Markdown line up prettily. You can also use inline Markdown.

Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3
```


| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |
{: class="info" style=""}

Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3
{: .info style=""}


| Header1 | Header2 | Header3 |
|:--------|:-------:|--------:|
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
|----
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
|=====
| Foot1   | Foot2   | Foot3
{: rules="groups"}

[Source](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#tables)
{: style="color: blue; font-size: 15px;"}

### ❯ Math Blocks
```
$$
\begin{aligned}
  & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
  = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
  & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
      \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
      \vdots & \ddots & \vdots \\
      \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
    \end{array} \right)
  \left( \begin{array}{c}
      y_1 \\
      \vdots \\
      y_n
    \end{array} \right)
\end{aligned}
$$
```

$$
\begin{aligned}
  & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
  = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
  & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
      \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
      \vdots & \ddots & \vdots \\
      \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
    \end{array} \right)
  \left( \begin{array}{c}
      y_1 \\
      \vdots \\
      y_n
    \end{array} \right)
\end{aligned}
$$


*([Source: Kramdown Math Blocks](https://kramdown.gettalong.org/syntax.html#math-blocks))*

### ❯ Code

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}


```ruby
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
```

~~~
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
~~~
{: .language-ruby}

~~~~~~
This is also a code block.
~~~
Ending lines must have at least as
many tildes as the starting line.
~~~~~~~~~~~~

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll](https://github.com/jekyll/jekyll)

### Abbreviations (kramdown)

This is some text not written in HTML but in another language!

*[another language]: It's called Markdown

*[HTML]: HyperTextMarkupLanguage
{:.mega-big}

### Comment (kramdown)
```
{::comment}
This text is completely ignored by kramdown - a comment in the text.
{:/comment}

Do you see {::comment}this text{:/comment}?
{::comment}some other comment{:/}

{::options key="val" /}
```
{::comment}
This text is completely ignored by kramdown - a comment in the text.
{:/comment}

Do you see {::comment}this text{:/comment}?
{::comment}some other comment{:/}

{::options key="val" /}

{::comment}
### A header without an ID
{::options auto_ids="false" /}
{:/comment}



## ❯ Link {#references}

<a name="jekyll-docs"></a>\[1\]. [Jekyll Docs][jekyll-docs]



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/



