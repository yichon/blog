---
layout: post
title:  "MathJax Test"
date: 2020-08-01T12:16:16+0800
tags: mytest_tag test_label hidden_label
categories: my
hidden: true
mathjs: true
---
> You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

<!--more-->

{% include toc.html %}

### Liquid Syntax Error

Use {% raw %}`{% raw % } {% endraw % }`{% endraw %} to solve delimiter conflicts between Jekyll and MathJax such as {% raw %} `{{` and `}}` {% endraw %}.

### Inserting "Displayed" maths inside blocks of text
```
\displaystyle \sum_{i=0}^{10
\sum_{i=0}^{10}
```
This $$ \displaystyle \sum_{i=0}^{10} $$ is inserted in a block of text with `\displaystyle`.

This $$ \sum_{i=0}^{10} $$ is inserted in a block of text without `\displaystyle`.

### Symbols

These are directly from the keyboard:

```
+ - = ! / ( ) [ ] < > | ' : *
```

These are not: 
```
\forall x \in X, \quad \exists y \leq \epsilon
```

$$ \forall x \in X, \quad \exists y \leq \epsilon $$

#### Greek letters
```
\alpha, \Alpha, \beta, \Beta, \gamma, \Gamma, \pi, \Pi, \phi, \varphi, \mu, \Phi
```

$$
\alpha, \Alpha, \beta, \Beta, \gamma, \Gamma, \pi, \Pi, \phi, \varphi, \mu, \Phi
$$

#### Operators

```
\cos (2\theta) = \cos^2 \theta - \sin^2 \theta
\lim\limits_{x \to \infty} \exp(-x) = 0
a \bmod b
x \equiv a \pmod{b}
```

$$
\cos (2\theta) = \cos^2 \theta - \sin^2 \theta \\
\lim\limits_{x \to \infty} \exp(-x) = 0 \\
a \bmod b  \\
x \equiv a \pmod{b}
$$

### Powers and indices

```
n^{22}
k_{n+1} = n^2 + k_n^2 - k_{n-1}
f(n) = n^5 + 4n^2 + 2 |_{n=17}
```

$$
n^{22}  \\
k_{n+1} = n^2 + k_n^2 - k_{n-1}  \\
f(n) = n^5 + 4n^2 + 2 |_{n=17}
$$

### Fractions and Binomials

#### Horizontal Fractions

```
\frac{n!}{k!(n-k)!} = \binom{n}{k}
\frac{\frac{1}{x}+\frac{1}{y}}{y-z}
```

$$
\frac{n!}{k!(n-k)!} = \binom{n}{k}  \\
\frac{\frac{1}{x}+\frac{1}{y}}{y-z}
$$

>  The `\tfrac` and `\dfrac` commands force the use of the respective styles, `\textstyle` and `\displaystyle`. Similarly, the `\tbinom` and `\dbinom` commands typeset the binomial coefficient.

#### Slanted Fractions 
{% raw %}
```
^3/_7
{}^3\!/_7
x^\frac{1}{2}
x^{\sfrac{1}{2}}     // Not Working
x^{\nicefrac{1}{2}}  // Not Working

\newcommand{\rfrac}[2]{{}^{#1}\!/_{#2}}
\rfrac{3}{7}
x^{\rfrac{1}{2}}
```

$$
^3/_7          \\
{}^3\!/_7       \\
x^\frac{1}{2}    \\
x^{\sfrac{1}{2}}  \\
x^{\nicefrac{1}{2}}
$$

$$
\newcommand{\rfrac}[2]{{}^{#1}\!/_{#2}}
\rfrac{3}{7}     \\
x^{\rfrac{1}{2}}
$$

{% endraw %}

#### Continued fractions

```
\begin{equation}
  x = a_0 + \cfrac{1}{a_1 
          + \cfrac{1}{a_2 
          + \cfrac{1}{a_3 + \cfrac{1}{a_4} } } }
\end{equation}
```

$$
\begin{equation}
  x = a_0 + \cfrac{1}{a_1 
          + \cfrac{1}{a_2 
          + \cfrac{1}{a_3 + \cfrac{1}{a_4} } } }
\end{equation}
$$

#### Multiplication of two numbers

```
\begin{equation}
\frac{
    \begin{array}[b]{r}
      \left( x_1 x_2 \right)\\
      \times \left( x'_1 x'_2 \right)
    \end{array}
  }{
    \left( y_1y_2y_3y_4 \right)
  }
\end{equation}
```

$$
\begin{equation}
\frac{
    \begin{array}[b]{r}
      \left( x_1 x_2 \right)\\
      \times \left( x'_1 x'_2 \right)
    \end{array}
  }{
    \left( y_1y_2y_3y_4 \right)
  }
\end{equation}
$$


### Roots

```
\sqrt{\frac{a}{b}}
\sqrt[n]{1+x+x^2+x^3+\dots+x^n}
```

$$
\sqrt{\frac{a}{b}}  \\
\sqrt[n]{1+x+x^2+x^3+\dots+x^n}
$$


### Sums and integrals

```
\sum_{i=1}^{10} t_i
\displaystyle\sum_{i=1}^{10} t_i
\int_0^\infty \mathrm{e}^{-x}\,\mathrm{d}x
\int\limits_a^b    // `\limits` command
```

$$
\sum_{i=1}^{10} t_i                         \\
\displaystyle\sum_{i=1}^{10} t_i             \\
\int_0^\infty \mathrm{e}^{-x}\,\mathrm{d}x    \\
\int\limits_a^b
$$

#### `\substack` Command 

It allows the use of `\\` to write the limits over multiple lines: 

```
\sum_{\substack{
   0<i<m \\
   0<j<n
  }} 
 P(i,j)
```

$$
\sum_{\substack{
   0<i<m \\
   0<j<n
  }} 
 P(i,j)
$$

#### Other Symbols

```
\sum 	                        \prod 	                        \coprod
\bigoplus 	 		\bigotimes 	 		\bigodot
\bigcup 	 		\bigcap 	 		\biguplus
\bigsqcup 	 		\bigvee 	 		\bigwedge
\int 	 		        \oint 	 		        \iint
\iiint 	 		        \iiiint 	 		\idotsint
```

$$
\text{\sum} \quad \sum  \\
\text{\prod} \quad \prod      \\
\text{\coprod} \quad \coprod      \\
\text{\bigoplus} \quad \bigoplus      \\
\text{\bigotimes} \quad \bigotimes      \\
\text{\bigodot} \quad \bigodot      \\
\text{\bigcup} \quad \bigcup      \\
\text{\bigcap} \quad \bigcap      \\
\text{\biguplus} \quad \biguplus      \\
\text{\bigsqcup} \quad \bigsqcup      \\
\text{\bigvee } \quad \bigvee      \\
\text{\bigwedge} \quad \bigwedge      \\
\text{\int} \quad \int      \\
\text{\oint} \quad \oint      \\
\text{\iint} \quad \iint      \\
\text{\iiint} \quad \iiint      \\
\text{\iiiint} \quad \iiiint      \\
\text{\idotsint} \quad \idotsint
$$


### Brackets, Braces and Delimiters

```
( a ), [ b ], \lbrack b \rbrack, \{ c \}, | d |, \| e \|,
\langle f \rangle, \lfloor g \rfloor, \lceil h \rceil, \ulcorner i \urcorner,
/ j \backslash
```

$$
( a ), [ b ], \lbrack b \rbrack, \{ c \}, | d |, \| e \|,
\langle f \rangle, \lfloor g \rfloor,
\lceil h \rceil, \ulcorner i \urcorner,
/ j \backslash
$$

#### Automatic Sizing

using the `\left`, `\right`, and `\middle` commands

```
\left(\frac{x^2}{y^3}\right)
P\left(A=2\middle|\frac{A^2}{B}>4\right)
\left\{\frac{x^2}{y^3}\right\}            \\ `left\{` and `\right\}`
\left.\frac{x^3}{3}\right|_0^1
```

$$
\left(\frac{x^2}{y^3}\right)              \\
P\left(A=2\middle|\frac{A^2}{B}>4\right)  \\
\left\{\frac{x^2}{y^3}\right\}            \\
\left.\frac{x^3}{3}\right|_0^1
$$

> If a delimiter on only one side of an expression is required, 
then an invisible delimiter on the other side may be denoted using a period `(.)`. 


#### Manual Sizing

```
( \big( \Big( \bigg( \Bigg(
\Biggl(\biggl(\Bigl(\bigl((x)\bigr)\Bigr)\biggr)\Biggr)
\frac{\mathrm d}{\mathrm d x} \left( k g(x) \right)
\frac{\mathrm d}{\mathrm d x} \big( k g(x) \big)
```

$$
( \big( \Big( \bigg( \Bigg(                             \\
\Biggl(\biggl(\Bigl(\bigl((x)\bigr)\Bigr)\biggr)\Biggr)  \\
\frac{\mathrm d}{\mathrm d x} \left( k g(x) \right)     \\
\frac{\mathrm d}{\mathrm d x} \big( k g(x) \big)
$$


### Matrices and Arrays

> Columns separated using `&` and a new rows separated with `\\`
{% raw %}
```
 \begin{matrix}
  a & b & c \\
  d & e & f \\
  g & h & i
 \end{matrix}


```

$$
 \begin{matrix}
  a & b & c \\
  d & e & f \\
  g & h & i
 \end{matrix}
$$

#### Alignment of Columns in Matrix

```
 \begin{matrix} 
  -1 & 3 \\
  2 & -4
 \end{matrix}
 =
 \begin{matrix}
  -1 & \phantom{-}3 \\
  \phantom{-}2 & -4
 \end{matrix}

\left[
    \begin{matrix}
        0 & \hfil     1 & 5 & 2 \\
        1 & \ \llap{-}7 & 1 & 0 \\
        0 & \hfil     0 & 1 & 3
    \end{matrix}
\right]

\begin{array}{rr}
    1 & 1  \\
    1 & -1 \\
\end{array}
```

$$
 \begin{matrix} 
  -1 & 3 \\
  2 & -4
 \end{matrix}
 =
 \begin{matrix}
  -1 & \phantom{-}3 \\
  \phantom{-}2 & -4
 \end{matrix}
$$

$$
\left[
    \begin{matrix}
        0 & \hfil     1 & 5 & 2 \\
        1 & \ \llap{-}7 & 1 & 0 \\
        0 & \hfil     0 & 1 & 3
    \end{matrix}
\right]
$$

$$
\begin{array}{rr}
    1 & 1  \\
    1 & -1 \\
\end{array}
$$

#### Predefined Delimiters

> While it is possible to use the `\left` and `\right` commands to enclose matrices in delimiters of some kind, 
there are various other predefined delimiters

```
pmatrix 	( ) 
pmatrix* 	( ) 
bmatrix 	[ ] 
bmatrix*	[ ] 
Bmatrix 	{ } 
Bmatrix* 	{ } 
vmatrix 	| | 
vmatrix* 	| | 
Vmatrix 	‖ ‖ 
Vmatrix*	‖ ‖
```

$$

$$

#### Arbitrary Sized Matrices

```
A_{m,n} = 
 \begin{pmatrix}
  a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\
  a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\
  \vdots  & \vdots  & \ddots & \vdots  \\
  a_{m,1} & a_{m,2} & \cdots & a_{m,n} 
 \end{pmatrix}
```

$$
A_{m,n} = 
 \begin{pmatrix}
  a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\
  a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\
  \vdots  & \vdots  & \ddots & \vdots  \\
  a_{m,1} & a_{m,2} & \cdots & a_{m,n} 
\end{pmatrix}
$$

#### Insert Lines Between Columns or Rows

```
\begin{array}{c|c}
  1 & 2 \\ 
  \hline
  3 & 4
\end{array}
```

$$
\begin{array}{c|c}
  1 & 2 \\ 
  \hline
  3 & 4
\end{array}
$$


#### Add Additional Leading Space

```
M = \begin{bmatrix}
       \frac{5}{6} & \frac{1}{6} & 0            \\
       \frac{5}{6} & 0           & \frac{1}{6}  \\
       0           & \frac{5}{6} & \frac{1}{6}
     \end{bmatrix}

M = \begin{bmatrix}
       \frac{5}{6} & \frac{1}{6} & 0           \\[0.3em]
       \frac{5}{6} & 0           & \frac{1}{6} \\[0.3em]
       0           & \frac{5}{6} & \frac{1}{6}
     \end{bmatrix}
```

$$
M = \begin{bmatrix}
       \frac{5}{6} & \frac{1}{6} & 0            \\
       \frac{5}{6} & 0           & \frac{1}{6}  \\
       0           & \frac{5}{6} & \frac{1}{6}
     \end{bmatrix}
$$

$$
M = \begin{bmatrix}
       \frac{5}{6} & \frac{1}{6} & 0           \\[0.3em]
       \frac{5}{6} & 0           & \frac{1}{6} \\[0.3em]
       0           & \frac{5}{6} & \frac{1}{6}
     \end{bmatrix}
$$


#### Indexes on Matrix (Not Working)

```
M = \bordermatrix{~ & x & y \cr
                  A & 1 & 0 \cr
                  B & 0 & 1 \cr}
```

$$
M = \bordermatrix{~ & x & y \cr
                  A & 1 & 0 \cr
                  B & 0 & 1 \cr}
$$


#### Matrices in Text: `{smallmatrix}`

```
A matrix in text must be set smaller:
$$\bigl(\begin{smallmatrix}
a&b \\ c&d
\end{smallmatrix} \bigr)$$
to not increase leading in a portion of text.
```


A matrix in text must be set smaller:
$$\bigl(\begin{smallmatrix}
a&b \\ c&d
\end{smallmatrix} \bigr)$$
to not increase leading in a portion of text.


{% endraw %}

### Text in Equations

```
50 apples \times 100 apples = lots of apples^2
50 \text{apples} \times 100 \text{apples} 
 = \text{lots of apples}^2
50 \textrm{ apples} \times 100
 \textbf{ apples} = \textit{lots of apples}^2
```

$$
50 apples \times 100 apples = lots of apples^2  \\
50 \text{apples} \times 100 \text{apples} 
 = \text{lots of apples}^2  \\
50 \textrm{ apples} \times 100
 \textbf{ apples} = \textit{lots of apples}^2
$$

### Formatting Mathematics Symbols

```
\mathnormal{ABCDEF abcdef 123456 @1}  \\
\mathrm{ABCDEF abcdef 123456 @2}  \\
\mathit{ABCDEF abcdef 123456 @3}  \\
\mathbf{ABCDEF abcdef 123456 @4}  \\
\mathsf{ABCDEF abcdef 123456 @5}  \\
\mathtt{ABCDEF abcdef 123456 @6}  \\
\mathfrak{ABCDEF abcdef 123456 @7}  \\
\mathcal{ABCDEF abcdef 123456 @8}  \\
\mathbb{ABCDEF abcdef 123456 @9}  \\
\mathscr{ABCDEF abcdef 123456 @10}
```

$$
\mathnormal{ABCDEF abcdef 123456 @1}  \\
\mathrm{ABCDEF abcdef 123456 @2}  \\
\mathit{ABCDEF abcdef 123456 @3}  \\
\mathbf{ABCDEF abcdef 123456 @4}  \\
\mathsf{ABCDEF abcdef 123456 @5}  \\
\mathtt{ABCDEF abcdef 123456 @6}  \\
\mathfrak{ABCDEF abcdef 123456 @7}  \\
\mathcal{ABCDEF abcdef 123456 @8}  \\
\mathbb{ABCDEF abcdef 123456 @9}  \\
\mathscr{ABCDEF abcdef 123456 @10}
$$

> The above only format letters, numbers, and uppercase Greek, and **other math commands are unaffected**.
To bold lowercase Greek or other symbols use the `\boldsymbol` command; this will only work if there exists a bold version of the symbol in the current font. 
As a last resort there is the `\pmb` command (poor man's bold): this prints multiple versions of the character slightly offset against each other. 

```
\boldsymbol{\beta} = (\beta_1,\beta_2,\dotsc,\beta_n)
```

$$
\boldsymbol{\beta} = (\beta_1,\beta_2,\dotsc,\beta_n)
$$


### Accents

```
a' or a^{\prime} 	 	\\
a'' 	 	\\
\hat{a} 	 	\\
\bar{a} 	 	\\
\grave{a} 	 	\\
\acute{a} 	 	\\
\dot{a} 	 	\\
\ddot{a} 	 	\\
\not{a} 	 	\\
\mathring{a} 	 	\\
\overrightarrow{AB} 	 	\\
\overleftarrow{AB} 	 	\\
a''' 	 	\\
a'''' 	 	\\
\overline{aaa} 	 	\\
\check{a} 	 	\\
\breve{a} 	 	\\
\vec{a} 	 	\\
\dddot{a} 	 	\\
\ddddot{a} 	 	\\
\widehat{AAA} 	 	\\
\widetilde{AAA} 	 	\\
\stackrel\frown{AAA} 	 	\\
\tilde{a} 	 	\\
\underline{a}
```

$$
\text{ a' or a^{\prime}} \quad a' or a^{\prime} 	 	\\
\text{a''} \quad a'' 	 	\\
\text{\hat{a}} \quad \hat{a} 	 	\\
\text{\bar{a}} \quad \bar{a} 	 	\\
\text{\grave{a}} \quad \grave{a} 	 	\\
\text{\acute{a}} \quad \acute{a} 	 	\\
\text{\dot{a}} \quad \dot{a} 	 	\\
\text{\ddot{a}} \quad \ddot{a} 	 	\\
\text{\not{a}} \quad \not{a} 	 	\\
\text{\mathring{a}} \quad \mathring{a} 	 	\\
\text{\overrightarrow{AB}} \quad \overrightarrow{AB} 	 	\\
\text{\overleftarrow{AB}} \quad \overleftarrow{AB} 	 	\\
\text{a'''} \quad a''' 	 	\\
\text{a''''} \quad a'''' 	 	\\
\text{\overline{aaa} } \quad \overline{aaa} 	 	\\
\text{\check{a}} \quad \check{a} 	 	\\
\text{\breve{a}} \quad \breve{a} 	 	\\
\text{\vec{a}} \quad \vec{a} 	 	\\
\text{\dddot{a}} \quad \dddot{a} 	 	\\
\text{\ddddot{a}} \quad \ddddot{a} 	 	\\
\text{\widehat{AAA}} \quad \widehat{AAA} 	 	\\
\text{\widetilde{AAA}} \quad \widetilde{AAA} 	 	\\
\text{\stackrel\frown{AAA}} \quad \stackrel\frown{AAA} 	 	\\
\text{\tilde{a}} \quad \tilde{a} 	 	\\
\text{\underline{a}} \quad \underline{a}
$$

### Color

> The only problem is that this disrupts the default LaTeX formatting around the `-` operator. 
To fix this, we enclose it in a `\mathbin` environment, since `-` is a binary operator. 

```
k = {\color{red}x} \mathbin{\color{blue}-} 2
```

$$
k = {\color{red}x} \mathbin{\color{blue}-} 2
$$

### Plus and minus signs

```
\pm
\mp
```

$$
\text{\pm} \quad \pm  \\
\text{\mp} \quad \mp
$$

### Horizontal Spacing

#### Normal Spacing

> `\quad` is a space equal to the current font size. 
> 
> `\qquad` gives twice that amount.

```
 f(n) =
  \begin{cases}
    n/2       & \quad \text{if } n \text{ is even}\\
    -(n+1)/2  & \quad \text{if } n \text{ is odd}
  \end{cases}
```

$$ f(n) =
  \begin{cases}
    n/2       & \quad \text{if } n \text{ is even}\\
    -(n+1)/2  & \quad \text{if } n \text{ is odd}
  \end{cases}
$$

#### Small Spacing
```
Command 	Description 	          Size
\, 	        small space 	      3/18 of a quad
\: 	        medium space 	      4/18 of a quad
\; 	        large space 	      5/18 of a quad
\! 	       negative space 	     -3/18 of a quad 
```

$$ \text{\int y \mathrm{d}x} \quad \int y \mathrm{d}x $$

$$ \text{\int y\, \mathrm{d}x} \quad \int y\, \mathrm{d}x $$

$$ \text{\int y\: \mathrm{d}x} \quad \int y\: \mathrm{d}x $$

$$ \text{\int y\; \mathrm{d}x} \quad \int y\; \mathrm{d}x $$


#### Negative Spacing

```
\left(\!
    \begin{array}{c}
      n \\
      r
    \end{array}
  \!\right) = \frac{n!}{r!(n-r)!}
```

$$
\left(\!
    \begin{array}{c}
      n \\
      r
    \end{array}
  \!\right) = \frac{n!}{r!(n-r)!}
$$

#### Define Commands
```
\newcommand{\dd}{\mathop{}\,\mathrm{d}}
\int x \dd x
```

$$
\newcommand{\dd}{\mathop{}\,\mathrm{d}}
$$

$$
\int x \dd x
$$

### Manually Specifying Formula Style

> To manually display a fragment of a formula using text style, surround the fragment with curly braces and prefix the fragment with `\textstyle`. 
The braces are required because the `\textstyle` macro changes the state of the renderer, rendering all subsequent mathematics in text style. 
The braces limit this change of state to just the fragment enclosed within.

#### Without `\textstyle`

```
\begin{equation}
   C^i_j = \sum_k A^i_k B^k_j
\end{equation}
```

$$
\begin{equation}
   C^i_j = \sum_k A^i_k B^k_j
\end{equation}
$$

#### With `\textstyle`

```
\begin{equation}
   C^i_j = {\textstyle \sum_k} A^i_k B^k_j
\end{equation}
```

$$
\begin{equation}
   C^i_j = {\textstyle \sum_k} A^i_k B^k_j
\end{equation}
$$

#### Using Command
{%- raw -%}
```
\newcommand{\tsum}[1]{{\textstyle \sum_{#1}}}

\begin{equation}
   C^i_j = \tsum k A^i_k B^k_j
\end{equation}
```

$$
\newcommand{\tsum}[1]{{\textstyle \sum_{#1}}}
$$
{% endraw %}

$$
\begin{equation}
   C^i_j = \tsum k A^i_k B^k_j
\end{equation}
$$


### AMS Math package

> The AMS (American Mathematical Society) mathematics package is a powerful package that creates a higher layer of abstraction over mathematical LaTeX language; 
if you use it it will make your life easier. Some commands amsmath introduces will make other plain LaTeX commands obsolete: 
in order to keep consistency in the final output you'd better use amsmath commands whenever possible.
If you do so, you will get an elegant output without worrying about alignment and other details, keeping your source code readable. 
If you want to use it, you have to add this in the preamble: `\usepackage{amsmath}`


#### Dots in Formulas

```
\dots 	     generic dots (ellipsis), to be used in text (outside formulae as well). It automatically manages
             whitespaces before and after itself according to the context, it's a higher level command.
\ldots 	 	the output is similar to the previous one, but there is no automatic whitespace management; 
                it works at a lower level.
\cdots 	     These dots are centered relative to the height of a letter. 
             There is also the binary multiplication operator, \cdot, mentioned below.
\vdots 	 	vertical dots
\ddots 	 	diagonal dots
\iddots      inverse diagonal dots (requires the mathdots package)
\hdotsfor{n} 	to be used in matrices, it creates a row of dots spanning n columns. 
```

$$
\text{\dots} \quad \dots   \\
\text{\ldots} \quad \ldots \\
\text{\cdots} \quad \cdots  \\
\text{\vdots} \quad \vdots  \\
\text{\ddots} \quad \ddots  \\
\text{\iddots} \quad \iddots  \\
\text{\hdotsfor{7}} \quad \hdotsfor{7}
$$

```
A_1,A_2,\dotsc, 	for "dots with commas"
A_1+\dotsb+A_N 	        for "dots with binary operators/relations"
A_1 \dotsm A_N 	        for "multiplication dots"
\int_a^b \dotsi 	for "dots with integrals"
A_1\dotso A_N 	        for "other dots" (none of the above) 
```

$$
\text{A_1,A_2,\dotsc,} \quad A_1,A_2,\dotsc,   \\
\text{A_1+\dotsb+A_N} \quad A_1+\dotsb+A_N   \\
\text{A_1 \dotsm A_N} \quad A_1 \dotsm A_N   \\
\text{\int_a^b \dotsi} \quad \int_a^b \dotsi  \\
\text{A_1\dotso A_N} \quad A_1\dotso A_N
$$


### List of mathematical symbols



---

### references

<a name="latex-mathematics"></a>\[1\]. [LaTeX Mathematics][1]

<a name="latex-mathematics"></a>\[2\]. [MathJax basic tutorial and quick reference][2]

<a name="latex-mathematics"></a>\[3\]. [How to left-align entries in a matrix][3]



[1]: https://en.wikibooks.org/wiki/LaTeX/Mathematics  "LaTeX Mathematics"

[2]: https://math.meta.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference  "MathJax basic tutorial and quick reference"

[3]: https://tex.stackexchange.com/questions/45001/how-do-i-left-align-entries-in-a-matrix-with-beginmatrix  "How to left-align entries in a matrix"





