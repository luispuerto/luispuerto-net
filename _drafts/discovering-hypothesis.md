---
title: 'Discovering Hypothes.is'
# date: 2018-12-01 00:00 +00:00
# last_modified_at: 2018-12-11 20:45 +00:00
header:
  overlay_image: /assets/images/blog/2019/HypothesisBannerGoogleForms.png
  teaser: /assets/images/blog/2019/HypothesisBannerGoogleForms.png
  caption: Hypothes.is logo
  overlay_filter: 0.2
categories: [Professional, Technology]
tags: [research, tools]
twitter: 
  image: /assets/images/blog/2019/HypothesisBannerGoogleForms.png
#   hashtags: [tag1, tag2] # Only for Twitter, they go before tags
# toc: true
# toc_label: "Unique Title"
# toc_icon: "heart"  # corresponding Font Awesome icon name (without fa prefix)
# toc_sticky: true
---

I just discovered [Hypothes.is](https://hypothes.is/) which is a service to annotate and highlight articles, posts or whatever text you want all over the web, and in any site. 

> The Hypothesis Project is a new effort to implement an old idea: A conversation layer over the entire web that works everywhere, without needing implementation by any underlying site.

All their tools are open source and free, and they can be checked [here](https://web.hypothes.is/developers/), which make the services even more attractive to me. They are also a [non-profict](https://web.hypothes.is/about/) if you are wondering if there any comercial interest in the tools they're developing. They also provide you with [outline.com](https://outline.com), which is similar to hypothes.is but transform the article in a more readable version. 

You can start using them right away, [creating yourself an account in hypothes.is](https://hypothes.is/signup) and fire up either hypothesi.is or outline in any web you want using a lightweight [bookmarklets](https://web.hypothes.is/start/) to share your annotations and highlights with others. 

{% include figure image_path="/assets/images/blog/2019/HypotheisisHighlight.png" alt="hypothes.is highlighting and annotation tool" caption="hypothes.is highlighting and annotation tool" %}{: .align-left style="max-width: 50%"}

The tool is really easy to use, you just click in the bookmarklet and then a small buttons in the side pops up.  Then, you just select text and a pop up will ask you if you want to highlight or annotate. You can also view the comments, notes and highlights from others users of the tool, which is even more interesting, and you can also comment in those annotation and reply to other annotations. Finally, it seems that it's possible to create groups, so you can share your notes with others privately or publicly. 

As you can see in the image below, outline do exactly the same, but in a clean interface that helps you to read the article without distractions. 

{% include figure image_path="/assets/images/blog/2019/HypothesisOutline.png" alt="outline interface with hypothes.is highlights" caption="outline interface with hypothes.is highlights" %}{: .align-center}

Web developers can also [implement this tool in their webs](https://web.hypothes.is/help/embedding-hypothesis-in-websites-and-platforms/), so when users select text the annotation and highlight tool pops up without the need to trigger it using the bookmarklet in a really [medium-like way](https://medium.com). I was thinking to implement it in my site, and I even was toying with it in a branch, but I finally decided to leave you the option of using it or not. Mainly for two reasons: 

- First of all. You choose. I don't want to impose anything into any one. 
- Second of all. I think the implementation is too intrusive to have it all the time there. You can implement it in two ways, classic or clean. 
  - The classic way makes the sidebar always present through its  buttons and somehow interfered with the design of the site. Mostly on mobile. 
  - The clean way doesn't show you anything until you select text, but them you are missing some features such us: marks of where the annotations are, or the ability to turn the annotations on and off. This wouldn't bee a problem if users can bring all the features triggering the bookmarklet, but it doesn't work like that. 

I really like the idea of being able to have a layer of notes and highlights all across the web. It's a great idea. I hope that the idea evolved in a sort of organization like the [Wikimedia Foundation](https://wikimediafoundation.org/) and it becomes a standard all across the web. I really think it's a great tool to share information with others, ever more if it's related to [research and science](https://web.hypothes.is/research/) or [education](https://web.hypothes.is/education/).  