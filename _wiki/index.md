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
  * [[11650]]
    * [[Arrays.sort()의_Comparator를_람다식으로_구현]]
  * [[10872]]
    * [[재귀]]
  * [[24060]]
    * [[병합정렬]]
	* [[Call_by_Value_VS_Call_by_Reference]]
  * [[10815]]
    * [[Collections.sort()에 대해]]
	* [[Tim Sort]]
	* [[Collections.binarySearch()에 대해]]
	* [[Comparable 과 Comparator의 이해]]
* [[Linux]]
  * [[Crontab]]
  * [[리눅스_특수문자_정리]]
* [[WhiteShip]]
  * [[스프링 프레임워크 입문]]
	* [[Inversion_of_Control]]
	* [[IoC_컨테이너]]
	* [[빈(Bean)]]
	* [[의존성_주입(Dependency_Injection)]]
	* [[AOP_소개]]
	* [[PSA_소개]]
  * [[스프링 프레임워크 핵심 기술]]
	* [[IoC 컨테이너 1부 - 스프링 IoC 컨테이너와 빈]]
	* [[IoC 컨테이너 2부 - ApplicationContext와 다양한 빈 설정 방법]]


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
