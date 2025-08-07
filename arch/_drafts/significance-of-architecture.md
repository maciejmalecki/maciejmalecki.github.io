---
title: Significance of Architecture
description: tbd
layout: post
categories: arch genai
pic: /genai/img/neward.jpg
---

When I'm interviewing new candidates for the software architect position, I always like to ask a few awkward questions first, like, "What is architecture?", "Who's an architect?" and "What do you do?" and then I just wait for that embarrassing moment of disbelief and the long silence. And then, maybe, just some of them will try to beat them by using these two rectangles and a barrel. Yeah, I was there too.

There are maybe two reasonably correct answers to these questions (by correct I mean simple, because simple question deserves a simple answer). The first answer is that the architect is the most senior person on the team, so he knows about the 'things' and how to avoid the 'traps'. It's just common sense, really. But this answer doesn't address the question about "architecture", so we still don't know what a "software architecture" is.

![Ted Neward](/arch/img/neward.jpg)
*Suprisingly you ask for an architecture and you always get this in response. / Ted Neward in Geekon/Kraków/Poland, 2019*

So, the second answer that's actually right is that 'an architect manages the architecture'. Architecture is a system that helps keep development and maintenance costs of software at a reasonable level.

Martin Fowler's post on [design stamina] is spot on. Good design, or architecture as it's also known, is an investment that takes time to pay off. You might not see the full benefits right at the start of your project, but as time goes on, you'll start to see the positive effects more and more. At some point, you reach the design-payoff line and you start cranking out work at a rate of knots. Then you know that you've got an architecture and it's good enough.

![Fowler: Design Stamina](/arch/img/design-stamina.png)
*Fowler's design stamina chart*.

There's a pretty straightforward reason why architecture is important and how it actually works. It's all down to human nature and the fact that our brains have limited capabilities. With all the lines of code and files these days, plus all the design patterns and technical stuff, it can get really confusing really fast. It's clear that it costs money. Either it takes longer and longer to understand the code that needs to be modified, or any changes introduce bugs that have to be corrected. In both cases, you just end up delivering slower, and nobody's happy about it. In my 23 years in the business, I've seen this so many times. Sometimes the code was mine...

In addition, you see that writing software is actually a team effort, with different team members joining and leaving the project as time goes on. So, the foundation and the guidelines for the architecture are important, because they keep the project alive. Believe me, you don't want to end up with this extra level of freedom because of different cultures, skill levels and creative approaches, but you do need some standardisation. It's not exactly sexy, but it works.

![Marlon Brando](/arch/img/horror.jpg)
*A random architect Friday night, resting after another week of fighting with the code. / Apocalypsed*

The long-term goal of moderate further development costs is to establish a software architecture that is an economic pillar. There are two more: the first is obvious — support for the required functionality — and the second, which is sometimes forgotten, is a set of so-called 'non-functional requirements', i.e. industry standards that are not directly demanded but are usually essential, such as security, monitoring and performance. NFRs are sometimes forgotten, but the economic side of development is usually forgotten, especially by "green field specialists" who like to start a new project for the first three months and then hand it over to a software house-powered team for "maintenance". I've experienced this too.

![Pillars](/arch/img/pillars-of-architecture.excalidraw.png)

add few examples from my past

add picture of separation

is architecture over in GenAI era

why it is not

[design stamina]: https://martinfowler.com/bliki/DesignStaminaHypothesis.html