<h1>Archive</h1>
{% for post in site.posts %}
<h3><a href="{{post.url}}">{{post.title}}</a></h3>

{% if post.featured %}
  <figure class="entry-image">
    <a href="{{post.featured.url}}"><img src="{{ post.featured.preview }}" title="{{post.featured.caption}}" alt="{{post.featured.caption}}"></a>
    <figcaption><a href="{{post.featured.url}}">{{post.featured.caption}}</a></figcaption>
  </figure>
{% endif %}
  {{post.excerpt}}

  <small>posted on {{post.date}}</small>
{% endfor %}



