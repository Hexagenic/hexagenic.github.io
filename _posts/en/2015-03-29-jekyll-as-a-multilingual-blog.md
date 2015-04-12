---
name: jekyll-multilingual
category: en
---

Setting up a multilingual blog has proven to not be an easy task.I have
experimented back and forth with different kinds of systems, and eventually
landed on [Jekyll] [jekyll] for a couple of reasons.

1. **It's static.**
	I like the idea of functionally generating HTML based on non-html content.

2. **Can be hosted on GitHub.**
	Adminstrating a full linux VPS seemed a bit overkill for a blog, so I'm
happy to find that GitHub can host Jekyll websites. 

3. **It's extendable.**
	Jekyll features a very easy system of extension through it's object model
and templating framework.

Making Jekyll Multilingual
==========================

Jekyll is not multilingual by default, but it is easy to add this
functionality as I found by reading [an article by Sylvain Durand]
[sylvianjekml]. 

One thing to note about how Jekyll builds up the data structures is that after
parsing all of the posts, it makes them avaiable through the `site.posts` array.
This array includes all posts of all languages, so it has to be filtered.


Finding The Next Post
=====================

One feature that I wanted to use in Jekyll that broke after adding multiple
languges was the `page.next` and `page.previous` variables that Jekyll exposes
for post pages.

These variables point to the next and previous post relative to the current one,
but will unfortunately point to posts of another language  most of time.
However, the object returned by these also have the `next` and `previous`
variables. Which means you can do `page.next.next.next...` to traverse the
array in relation to the current post.

We can explore forwards or backwards through the post array with a recursive
template. It will keep going until it either finds a post of the correct
language or the value `nil`.
 
```liquid
{% raw %}

{% assign next_post = include.next_post %}

{% if next_post == nil %}
	<!-- No next post -->
{% elsif next_post.lang == page.lang %}
	<a href="{{ next_post.url }}" >Next</a>
{% else %}
	{% include next_button.html next_post=next_post.next %}
{% endif %}

{% endraw %}
```

To use it:

```liquid
{% raw %}

{% include next_button.html next_post=page.next %}
{% include prev_button.html prev_post=page.previous %}

{% endraw %}
```


[jekyll]: http://jekyllrb.com
[sylvianjekml]: http://sylvaindurand.org/making-jekyll-multilingual/
