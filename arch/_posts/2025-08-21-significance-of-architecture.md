---
title: Significance of Architecture
description: Is IT architecture still relevant in AI era of coding?
layout: post
categories: arch genai
pic: /arch/img/neward.jpg
---

When I'm interviewing new candidates for the software architect position, I always like to ask a few awkward questions first, like, "What is architecture?", "Who's an architect?" and "What do you do?" and then I just wait for that embarrassing moment of disbelief and the long silence. And then, maybe, just some of them will try to beat them by using these two rectangles and a barrel. Yeah, I was there too.

There are maybe two reasonably correct answers to these questions (by correct I mean simple, because simple question deserves a simple answer). The first answer is that the architect is the most senior person on the team, so he knows about the 'things' and how to avoid the 'traps'. It's just common sense, really. But this answer doesn't address the question about "architecture", so we still don't know what a "software architecture" is.

![Ted Neward](/arch/img/neward.jpg)
*Suprisingly you ask for an architecture and you always get this in response. / Ted Neward in Geekon/Kraków/Poland, 2019*

So, the second answer that's actually right is that 'an architect manages the architecture'. Architecture is a system that helps keep development and maintenance costs of software at a reasonable level.

Martin Fowler's post on [design stamina] is spot on. Good design, or architecture as it's also known, is an investment that takes time to pay off. You might not see the full benefits right at the start of your project, but as time goes on, you'll start to see the positive effects more and more. At some point, you reach the design-payoff line and you start cranking out work at a rate of knots. Then you know that you've got an architecture and it's good enough.

![Fowler: Design Stamina](/arch/img/design-stamina.png)
*Fowler's design stamina chart*.

There's a pretty straightforward reason why architecture is important and how it actually works. It's all down to human nature and the fact that our brains have limited capabilities. With all the lines of code and files these days, plus all the design patterns and technical stuff, it can get really confusing really fast. It's clear that it costs money. Either it takes longer and longer to understand the code that needs to be modified, or any changes introduce bugs that have to be corrected. In both cases, you just end up delivering slower, and nobody's happy about it. In my 23 years in the business, I've seen this so many times. Sometimes the code was all mine...

In addition, you see that writing software is actually a team effort, with different team members joining and leaving the project as time goes on. So, the foundation and the guidelines for the architecture are important, because they keep the project alive. Believe me, you don't want to end up with this extra level of freedom because of different cultures, skill levels and creative approaches, but you do need some standardisation. It's not exactly sexy, but it works.

![Marlon Brando](/arch/img/horror.jpg)
*A random architect Friday night, resting after another week of fighting with the code. / Apocalypsed*

The long-term goal of moderate further development costs is to establish a software architecture that is an economic pillar. There are two more: the first is obvious — support for the required functionality — and the second, which is sometimes forgotten, is a set of so-called 'non-functional requirements', i.e. industry standards that are not directly demanded but are usually essential, such as security, monitoring and performance. NFRs are sometimes forgotten, but the economic side of development is usually forgotten, especially by "green field specialists" who like to start a new project for the first three months and then hand it over to a software house-powered team for "maintenance". I've experienced this too.

![Pillars](/arch/img/pillars-of-architecture.excalidraw.png)
*Acropolis of the software engineering.*

I have deliberately excluded the 'economy' part, even though I could translate it into 'maintainability' and, like all 'abilities', easily squeeze it into the NFRs section. However, clients rarely explicitly request this kind of economy. In fact, it is almost always the deliverer's responsibility to ensure the project has the right margin. Developer sanity is also the responsibility of the deliverer; clients do not care.

However, there is a clear connection between architecture and the pleasure of coding, as well as the mental health of software developers who are forced to struggle with sequences of characters forming computer programmes.

![Swamp](/arch/img/we-live-in-a-bloody-swamp.jpg)
*Listen my son. I have built this software on a swamp. It sank into the swamp. So I built a second one. That sank into the swamp. So I built a third. That burned down, fell over, then sank into the swamp. But the fourth one stayed up. / Monty Python and the Holy IT Architecture.* 

But does architecture still matter in the year 2025? This is a pertinent question at a time when more and more coding tasks are being taken over by AI. Is architecture a human domain? If we eliminate humans, will it still be needed? AI never gets tired and, as time goes on, it will probably become cheaper and cheaper. It is unlikely to become mentally ill.

To answer "yes" here, one must assume that AI does not really have comprehension limits. Moreover, code without architecture can easily exceed polynomial complexity in terms of execution paths. Tireless AI could facilitate this growth by producing more and more PRs.

![Concerns](/arch/img/concerns.excalidraw.png)
*So-called clean architecture prioritises good concern separation, i.e. vertical separation into modules or business components and horizontal separation into layers. It treats inbound technical concerns, business logic and outbound technical concerns separately.*

Unless it is proven otherwise through empirical evidence, we cannot assume that AI will be able to work reliably with a complex codebase. So, if we don't know that for sure, we cannot just let vibe in. It will most likely end up in a disaster.

From my own experiments, I have noticed that a certain degree of order (architecture) actually helps AI produce better code and understand the existing code base more deeply. Good modularity (vertical separation) also helps AI build context properly and match the developer's prompt to the right elements of the code base. Horizontal separation (concern isolation) reduces informational noise in code, enhancing the models' comprehension capabilities.

I strongly believe that IT architecture is far from obsolete and must remain a cornerstone of the GenAI revolution.

[design stamina]: https://martinfowler.com/bliki/DesignStaminaHypothesis.html