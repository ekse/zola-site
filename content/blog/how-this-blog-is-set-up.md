+++
title = "Building a blog with Zola and Cloudflare Pages"
date = "2024-05-03"
slug = "my-blog-setup"

[taxonomies]
categories = ["Posts"]
tags = ["zola", "cloudflare", "umami"]

+++

I'm trying to get back into the habit of writing posts and I figured I could explain my current setup for this blog. This is meant both to help people that would be interested in standing up a similar site and help me to remember how this thing even works.

## My current setup: Zola and Cloudflare Pages

### Zola

[Zola](https://www.getzola.org) is a static site generator. In a nutshell, you edit Markdown files, generate the content with `zola build` and host the html files wherever you want. You can use `zola serve` to start a local web server, the content is hot reloaded everytime you save. The source code of my blog is hosted in a [github repo](https://github.com/ekse/zola-site).

The Zola website provides a good selection of [themes](https://www.getzola.org/themes/). I went with [Serene](https://www.getzola.org/themes/serene/) to which I did very light changes to the font and color schemes.


### Cloudflare Pages

For the hosting I'm using [Cloudflare Pages](https://pages.cloudflare.com/). When you create a site, you connect it to a git repository (github or gitlab), specify the framework to use and that's pretty much it. You get a `pages.dev` domain for your site, you can also define a custom domain. The free plan offers unlimited requests and bandwidth, and 500 builds per month which is plenty for my needs.

Now, deploying a Zola site works but I had to use an 😒ugly hack😒. If I was starting from scratch today I would look at deploying it on another provider like [Github Pages](https://www.getzola.org/documentation/deployment/github-pages/), [Netlify](https://www.getzola.org/documentation/deployment/netlify/) or [Vercel](https://www.getzola.org/documentation/deployment/vercel/). According to Cloudflare's documentation Zola is [supposed to be supported](https://developers.cloudflare.com/pages/framework-guides/deploy-a-zola-site/) but it complains that the `zola` command does not exist when building. My understanding is that they broke it with version 2 of Cloudflare Pages (there is an open [github issue](https://github.com/cloudflare/pages-build-image/issues/3)). 

My workaround is to store a precompiled build of zola in my repo and modify the build command to make it executable and call it manually. It works for the time being but I wouldn't be surprised if it breaks in the future.

![](/assets/blog_setup/cloudflare2.png)

The site is setup such that every time I push to the `main` branch it deploys the site. I can also do preview deployment by pushing to branches that begin with `preview-`. Cloudflare generates links like `ea49e660.ekse.pages.dev` that I can use to preview the content.

![](/assets/blog_setup/cloudflare1.png)

Other than the deployment bullshittery my experience has been quite pleasant so far, the site takes less than a minute to deploy. I found the management panel a bit confusing at first but I like it now that got used to where things are.

### Umami

[Umami](https://umami.is/) is a privacy preserving analytics service. You get page views by path and referrers which is frankly all I really care about. You can also see what browsers, platform (desktop or mobile) and countries visits are coming from which is nice to see. The interface is clean and very simple to use. The free tier includes 10K monthly events which is again more than enough for my needs.

![](/assets/blog_setup/umami.jpg)

Adding it was quite straightforward as the Serene theme supports it. I created my instance, enabled in config.toml, set my website_id and url in theme.toml and that was it.

```toml
# in config.toml
[extra]
analytics = "umami"

# in templates/theme.toml
[extra.umami]
website_id = "8a6655dc-02d7-4510-befc-e854f7da87a6"
src = "https://analytics.us.umami.is/script.js"
```



## Previous platforms

I used different components over the years to host my blog, I'll briefly describe them and why I decided to change.

### Blogger

I started my blog on [Blogger](https://www.blogger.com). It was fairly simple to create posts with the graphical editor but customization was done in html which wasn't great. I had to do syntax highlighting of code manually with a javascript library, which is something you get out of the box with tools like Jekyll, Zola and Hugo.

At some point Google also redesigned the look and feel of Blogger which in my opinion was worst than the previous look. 

### Github Pages

My next platform was [Github Pages](https://pages.github.com/). It's honestly quite cool, you create a repo named `yourusername.github.io`, add markdown pages to it and you automatically get a site on that domain.

The part that wasn't as nice is that it uses Jekyll which is in Ruby, I remember often losing time getting the packages to work locally with `bundler`. (I later learned that it is also possible to host content generated by other tools with some [Github Actions hackery](https://www.getzola.org/documentation/deployment/github-pages/)).

As a fan of Rust I decided to try Zola at some point. A neat advantage it has over Jekyll is that you can use a precompiled binary which saves time fighting with dependencies.

### Hosting on a VM on Oracle Cloud Free Tier

Around the same time that I started experimenting with Zola, I learned that you can get a free ARM virtual machine with the free tier of [Oracle Cloud](https://www.oracle.com/ca-en/cloud/free/).

My setup was to clone my git repository on that VM, generate the content and serve it with nginx. I would generally work on the content locally, but when I wanted to do quick changes I would edit the files directly on the VM, serve it with `zola serve` and browse from my system with an ssh port forward.

This setup worked well but its not as convenient as a *push and deploy* system like Github Pages and Cloudflare Pages. It also felt wasteful to have a VM spun up all the time for a blog that barely gets any traffic.

### Cloudflare Web Analytics

I gave [Cloudflare Web Analytics](https://www.cloudflare.com/en-ca/web-analytics/) a try, it is built as a privacy-preserving alternative to Google Analytics. However to achieve its privacy guarantees, Cloudflare bundles visit data together, for sites like mine that get rare visits the data wasn't really useful, I would see that my site had a number of visits in the past week but didn't which pages were visited. 