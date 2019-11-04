# Creating a Github webpage to display your Repos and Posts.


#### Requirements
 - git
 - Github.com Account
 - Text Editor (Atom, Sumblime, Visual Studio)

 - ##### For Local Testing
    - Ruby
    - Bundler Gem



<div>

  <h3> Other Posts </h3>

  <ul>
    {% for post in site.posts %}
      <li>
        <a href="{{ post.url }}">{{ post.title }}</a>
      </li>
    {% endfor %}
  </ul>

</div>
