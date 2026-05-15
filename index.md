<img src="DebayanChatterjee_Pic.jpeg" alt="My Profile Picture" width="200" style="border-radius: 50%; display: block; margin: 0 auto; margin-bottom: 30px;">

## Recent Posts

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a> 
      - {{ post.date | date: "%B %d, %Y" }}
    </li>
  {% endfor %}
</ul>
