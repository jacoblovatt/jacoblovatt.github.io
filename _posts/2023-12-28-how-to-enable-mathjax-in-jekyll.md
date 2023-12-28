---
layout: post
title: How to Enable MathJax in Jekyll
subtitle: A short note about how to enable MathJax in Jekyll
tags: [mathematics, jekyll, mathjax]
---

Information from jojozhuang's [post on mathjax](https://jojozhuang.github.io/tutorial/jekyll-math-symbols-with-mathjax/).

# Using MathJax in Jekyll with Markdown

## Step 1. Create MathJax.html in _includes
Create a file named with mathjax.html in _includes/ directory  with the following content:

```html
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    TeX: {
      equationNumbers: {
        autoNumber: "AMS"
      }
    },
    tex2jax: {
      inlineMath: [ ['$','$'], ['\\(', '\\)'] ],
      displayMath: [ ['$$','$$'] ],
      processEscapes: true,
    }
  });
</script>
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
```

## Step 2. Update _includes/head.html to include conditional logic
Update file \_includes/head.html, include the above template file if page.mathjax is true.

{% raw %}
```liquid
{% if page.mathjax %}
  {% include mathjax.html %}
{% endif %}
```
{% endraw %}

_Note: I had to consult [Michael Currin's cheatsheets](https://michaelcurrin.github.io/dev-cheatsheets/cheatsheets/jekyll/code-blocks/liquid-code.html) to display the above Liquid code snippet (this is the [raw .md file](https://github.com/MichaelCurrin/dev-cheatsheets/blob/master/cheatsheets/jekyll/code-blocks/liquid-code.md?plain=1))_

## Step 3. To Enable MathJax on a particular post
Create a post in the normal way (under \_posts/ directory with _YYYY-MM-DD-blog-post-name.md_) and include the _mathjax: true_ directive at the start of the post like below:

~~~markdown
---
layout: post
title: M208 - Pure Mathematics - Book C - Linear Algebra
subtitle: Notes on M208 - Book C - Linear Algebra
tags: [mathematics, pure mathematics, linear algebra, open university]
mathjax: true
---
~~~

