---
title: The Rise of Vibe Coding
layout: post
categories: genai
pic: /genai/img/YoungFrankenstein.jpg
---

With *Vibe Coding* is like with *Agile*: reasonable idea gets ruined by misunderstanding and this is progressing as the hype goes on. Let's try to understand first, what *Vibe Coding* really is.

We find an informal definition on [Wikipedia][vibe-coding-definition]. As Vibe Coding we understand a completely different approach to coding, that is, a chatbot based rather than assistant based. Developer no longer works with code directly but rather develop via series of prompts and let the GenAI agent provide an implementation. Developer focus on problem definition, testing and giving feedbacks, also via prompts. If we look at simplified coding assistant anatomy diagram (see below), agent mode (the heart of the vibe coding) is the rightmost, most sophisticated and autonomous component that is finally capable of comprehending written software as a whole.

![Anatomy](/genai/img/anatomy-of-coding-assistant.excalidraw.png)
*Anatomy of AI coding assistant, based on GitHub Copilot example*

According to forementioned definition, these few points characterize Vibe Coding:

1. Developer no longer "micromanage" the code.
2. Developer accepts generated code without full understanding of it.
3. Developer tests generated code in runtime and in case of misbehaviour, uses further prompting asking AI to fix the problem.

Apparent consequence of such understanding is that it is no longer needed to master a programming knowledge, know details of frameworks or libraries, has ability to model data or algorithms properly, etc. In other words, in order to write an application it is no longer needed to have a software engineer skills.

I have already suggested in [another article](significance-of-architecture), that uncontrolled code generation without a properly established architectural restrictions may lead to rapid quality degradation, exponential growth of complexity and in consequence to technical death of the code base. This is of course something I cannot prove (yet).

So, having this vague definition, plus an enormous hype wave of everything related to generative AI, we are on the clear path to trivialize yet another IT term, *Vibe Coding*.

## The Experiment

In my free time I do some Assembly coding for a rather outdated machine called Commodore 64, that has premiered around 1982. To make my life easier I decided to incorporate some modern stuff such as cross assemblers and CI/CD pipelines.

![Commodore 64](/cbm/img/commodore64.jpg)
*That's Commodore 64, the one I own since 1990.*

So, majority of my GitHub repositories are MOS 6502 assembly projects except one, called [Retro Build Tool][rbt-home-page], which is basically a plugin for Gradle build system, implemented in Kotlin Language.

RBT is not a big project, it's currently around 13K lines of code but the complexity makes it not trivial. We can assume it is a comparable to the typical microservice. If vibe coding works for this project, it should be useful for a real life microservice based code bases. If it fails, however, it cannot be used as a reliable development method for majority of existing software.

One problem I have noticed while working with RBT is, that while it offers a seamless experience when working with simple, straightforward, "one pass" assembly project, it fails miserably in more complex works. That is, once I started working on [Tony: Montezuma's Gold][tony] and wend beyond one-file demo version, I have discovered that RBT now needs to support multi module, multi part project that has two compilation targets: a 5'25" floppy disk and a ROM Cartridge in Magic Desk format. I had to include packing (as assets grow really big) as well as kind of linking, that is, an intermediate compilation step followed by additional compression and then yet another compilation to desired format (disk image/ROM image). I was able to do it within Gradle, by using a lot of custom at hoc tasks and CLI commands, but result was a bit messy, just to say.

[vibe-coding-definition]: https://en.wikipedia.org/wiki/Vibe_coding
[rbt-home-page]: https://c64lib.github.io/gradle-retro-assembler-plugin/
[tony]: https://monochrome-productions.itch.io/tony
