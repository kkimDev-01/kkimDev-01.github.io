---
layout: wikiindex
title: wiki
toc: true
public: true
comment: false
regenerate: true
---

## wiki items

* [[mathjax-latex]]
* [[할-일-목록]]
* [[autocmd테스트]]
* [[autocmd테스트2]]
* [[autocmd테스트3]]
* [[테스트4]]
    * [[테스트4-1]] 
    * [[테스트4-2]]
* [[BOJ]]
    * [[1427번:소프트인사이드]]
        * [[quick-sort]]
		* [[insertion-sort]]

---

## blog posts

<div>
    <ul>
{% for post in site.posts %}
    {% if post.public != false %}
        <li>
            <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">
                {{ post.title }}
            </a>
        </li>
    {% endif %}
{% endfor %}
    </ul>
</div>
