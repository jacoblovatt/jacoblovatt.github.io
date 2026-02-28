---
layout: post
title: How to Enable MathJax in Jekyll
subtitle: A short note about how to enable MathJax in Jekyll
tags: [mathematics, jekyll, mathjax]
---

Based on jojozhuang's [post on MathJax](https://jojozhuang.github.io/tutorial/jekyll-math-symbols-with-mathjax/).

This is a short post on enabling MathJax in Jekyll for Markdown.

## Step 1. Create mathjax.html in _includes
Create a file named `mathjax.html` in the `_includes/` directory with the following content:

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
        displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
        processEscapes: true,
      }
    });
  </script>
  <script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
```

## Step 2. Update _includes/head.html to include conditional logic
Update `_includes/head.html` to include the template above when `page.mathjax` is `true`.

{% raw %}
```liquid
{% if page.mathjax %}
  {% include mathjax.html %}
{% endif %}
```
{% endraw %}

_Note: I consulted [Michael Currin's cheatsheets](https://michaelcurrin.github.io/dev-cheatsheets/cheatsheets/jekyll/code-blocks/liquid-code.html) to display the Liquid snippet above (this is the [raw .md file](https://github.com/MichaelCurrin/dev-cheatsheets/blob/master/cheatsheets/jekyll/code-blocks/liquid-code.md?plain=1))._

## Step 3. Enable MathJax on a specific post
Create a post in the normal way (under `_posts/` with `YYYY-MM-DD-blog-post-name.md`) and include the `mathjax: true` directive in the front matter, like this:

~~~markdown
---
layout: post
title: M208 - Pure Mathematics - Book C - Linear Algebra
subtitle: Notes on M208 - Book C - Linear Algebra
tags: [mathematics, pure mathematics, linear algebra, open university]
mathjax: true
---
~~~
