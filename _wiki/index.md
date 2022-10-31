---
layout: wikiindex
title: wiki
toc: true
public: true
comment: false
regenerate: true
---

## wiki items


* [[BOJ]]
  * [[1247]]
    * [[quick-sort]]
    * [[insertion-sort]]
    * [[비트연산]]
    * [[dual-pivot-quick-sort]]
    * [[Scanner_vs_BufferedReader]]
* [[Linux]]
  * [[Crontab]]
  * [[리눅스_특수문자_정리]]
* [[WhiteShip]]
  * [[Inversion_of_Control]]
  * [[IoC_컨테이너]]


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
