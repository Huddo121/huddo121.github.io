---
layout: post
title:  "On Missing Language Features"
subtitle: "A quest for knowledge, and the curse that follows"
date: 2017-03-20 22:35:00+11:00
cover: seth_doyle_port.jpg
cover_artist: Seth Doyle
cover_link: https://unsplash.com/@sethdoylee?photo=tuiIo0uA-z8
tags:
---

Spending time learning about and honing your craft is undeniably a good thing. You can pick up new skills, become more proficient in the skills you already have, and find new ways of solving problems you face in your day to day life. However, despite those upsides, one *downside* is now you are acutely aware of situations where these tools would help immensely, and this realisation usually occurs in instances where you can't really leverage those tools.

This is exactly the problem I've faced since investigating functional programming concepts and playing around with different varieties of programming languages. My professional coding work is overwhelmingly Java, which often leaves me lamenting the lack of features like good collection literals, destructuring returns and pattern matching.

This phenomenon isn't just limited to programming language syntax and semantics either, it presents itself when people are suddenly facing feature disparity in software and other consumer goods. Deficiencies between cars, televisions, and software bother us most when we're aware that there is a better, faster or funner way.

{% include captioned_gfycat_image.html gfycat="infomercial-woes" caption="Nobody enjoys feeling like they're in an infomercial when trying to get their work done." %}

There are strategies to fend off such lamentations though. SOA and microservices provides us with opportunities to mix stacks where appropriate, though this still requires buy-in from the organisation if this is to be something maintained long-term by the team.

Virtual machines such as the JDK also provide options in this space too. By mixing Scala in to an existing Java project you can make use of Scala's features where they make sense in a very precise manner. For example, if a part of your application or business logic would be best served through judicious use of pattern matching, you can encapsulate that logic in a Scala object and the calling code can be none-the-wiser. It's also surprisingly simple to do with IntelliJ. A bit of playing around and I had an example up and running within a single coffee.

{% include captioned_gfycat_image.html gfycat="brilliant" caption="When the integration just works." %}

I spent a little bit more time in order to get the building working just with Maven on its own, and that too turned out to be surprisingly easy. The result of my playing was [this mixed language example <i class="fa fa-github"/>](https://github.com/Huddo121/Mixed-Language-Spring-Boot-Example/tree/master) is what I ended up with. So it's possible, and it's remarkably easy.

Unfortunately, I don't have any real sort of point to this post. Really I'm just venting. I think this must be something that a lot of developers face, just based on the amount I hear sentences like "but the such-and-such features are pretty anemic" or "It's so much nicer to do that sort of thing in otherLang".

**Side note:** This first quarter of the year has been pretty full on for me, and blogging was the first thing to suffer. Consider this a warm up post.
