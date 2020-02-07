---
layout: post 
title: jekyll-now 에서 pagination 적용 
tags: technology  
class: post-template
subclass: 'post tag-technology'  
author: tigim
---

jekyll-now에 보면, 관련 코드가 포함되어 있다. 근데 엉성하게 포함되어 있어 동작하지 않는다.

방법은 [jekyll pagination](http://jekyllrb.com/docs/pagination/)에 보면 나와 있다. 

짧게 말하면, _config.yml 에서 다음 추가  

```
# Pagination
paginate: 5
paginate_path: "/page:num/"	
```

다음으로 index.html 수정

```
---
layout: default
title: My Blog
---

<!-- This loops through the paginated posts -->
{% for post in paginator.posts %}
  <h1><a href="{{ post.url }}">{{ post.title }}</a></h1>
  <p class="author">
    <span class="date">{{ post.date }}</span>
  </p>
  <div class="content">
    {{ post.content }}
  </div>
{% endfor %}

<!-- Pagination links -->
<div class="pagination">
  {% if paginator.previous_page %}
    <a href="{{ paginator.previous_page_path }}" class="previous">Previous</a>
  {% else %}
    <span class="previous">Previous</span>
  {% endif %}
  <span class="page_number ">Page: {{ paginator.page }} of {{ paginator.total_pages }}</span>
  {% if paginator.next_page %}
    <a href="{{ paginator.next_page_path }}" class="next">Next</a>
  {% else %}
    <span class="next ">Next</span>
  {% endif %}
</div>
```

잘되는구나... ㅋㅋㅋㅋ