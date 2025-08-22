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

[vibe-coding-definition]: https://en.wikipedia.org/wiki/Vibe_coding