I found [this post](https://bartoszgorka.com/github-discussion-comments-for-jekyll-blog) per
accident, apparently we can have comments in static blogs by levering GitHub
discussions!

I host my blog on GitHub, which has a nice integration for building and testing
using GitHub Actions.  I don't need to host anything, and it's essentially free.

There's [disqus](https://disqus.com), a web service which solves the problem of hosting and managing
comments for static web sites.  I used disqus before, but since my relaunch I
was not so sure about having a dependency do some external service.

[Giscus](https://giscus.app) uses GitHub's Discussions -- which can be enabled on any repository
hosted on GitHub -- to add comments on a static website.  The Giscus bot automatically
adds discussions for a reaction / comment entry. 

The nice thing is that the `chirpy` theme I use already has a giscus integration!

## Configuration

Configuration is simple -- I found more explanation over at [Martin's Tech Journal](https://blog.martinp7r.com/posts/adding-giscus-comments-to-my-blog/).
I had to:

1. Install and authorize the `giscus` app in GitHub blog's repository
2. Configure the app on https://giscus.app 
   a. Enter my blog's repository
   b. Enabled the `pathname` mapping
   c. Use the "Announcements" Discussion Category
   d. Enable Reactions for the Main Post
3. Copying the values displayed in the script tag to the `_config.yml`
4. Enable `giscus` comments
```yaml
comments:
  active: "giscus"
  giscus:
    repo: "danywebxx/danywebxx.github.io" # <gh-username>/<repo>
    repo_id: "ricavato da pagina Giscus"
    category: "Announcements"
    category_id: "ricavato da pagina Giscus"
    mapping: "pathname" # optional, default to 'pathname'
    input_position: "bottom" # optional, default to 'bottom'
    lang: "it" # optional, default to the value of `site.lang`
    reactions_enabled: "1" # optional, default to the value of `1`
```
