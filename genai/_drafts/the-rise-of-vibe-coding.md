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
*That's Commodore 64, the one I own since 1990. Blurry badge proudly says: "Personal Computer".*

So, majority of my GitHub repositories are MOS 6502 assembly projects except one, called [Retro Build Tool][rbt-home-page], which is basically a plugin for Gradle build system, implemented in Kotlin Language.

RBT is not a big project, it's currently around 13K lines of code but the complexity makes it not trivial. We can assume it is a comparable to the typical microservice. If vibe coding works for this project, it should be useful for a real life microservice based code bases. If it fails, however, it cannot be used as a reliable development method for majority of existing software.

One problem I have noticed while working with RBT is, that while it offers a seamless experience when working with simple, straightforward, "one pass" assembly project, it fails miserably in more complex works. That is, once I started working on [Tony: Montezuma's Gold][tony] and wend beyond one-file demo version, I have discovered that RBT now needs to support multi module, multi part project that has two compilation targets: a 5'25" floppy disk and a ROM Cartridge in Magic Desk format. I had to include packing (as assets grow really big) as well as kind of linking, that is, an intermediate compilation step followed by additional compression and then yet another compilation to desired format (disk image/ROM image). I was able to do it within Gradle, by using a lot of custom at hoc tasks and CLI commands, but result was a bit messy, just to say. Other problem was that execution was all sequential and takes ages. It is because I know very little about Gradle API, the documentation is rather cryptic and this is my free time project, and apparently free time is not enough in my case. I know that Gradle can track changes in files and run only tasks that needs to be run and can even parallelize tasks but I had no idea how to write a plugin that uses all these features. So, I've created some entries into my backlog and let them rest for next 6 months.

This is a very good topic to start playing with agent and vibe coding, isn't it?

## How do I work?

I choose GitHub Copilot as a plugin into IntelliJ Ultimate IDE. The tool, as for now, supports code completion (via CODEX model), chat and edits function and, of course, an agent mode.

Agent mode supports following models without limitations (included in the license):
1. GPT-4o
2. GPT-4.1

Additionally, following premium models are available:
1. Claude Sonnet 3.5
2. Claude Sonnet 3.7
3. Claude Sonnet 4
4. Gemini 2.5 Pro
5. o4-mini (preview)

Gemini and o4 models were in *preview*, however today I have noticed, that Gemini model is now GA. Premium models have limits, all but *o4-mini* costs the same (1x tariff), *o4-mini* is way cheaper (0.33x tariff).

GitHub Copilot also supports instructions file (in markdown format) that can be stored in the project repository and shared across the team (however I don't have any team).

I rather prefer a careful examination and don't accept that much risk, therefore I gave up on naive usage of agent. I know that agent mode (as well as other coding agents) are internally implemented as ["plan and execute"][plan-and-execute] agent therefore I have orchestrated my Copilot so that it externalizes the plan into a file that I store in my project repo and use in subsequent iterations of my work with the agent.

So, my work basically requires three kind of prompts to work:

1. **User story prompt** specifying the feature that needs to be implemented that results in planning the work and storing the action plan in separate MD file.
2. **Execution prompt** asking to execute certain points from the action plan.
3. **Enhancing the plan prompt** asking to extend existing plan with new facts, decisions and points.

To orchestate agent so that it does exactly what I want (plan, execute, enhance, update the plan document and so on) would be a tedious work therefore I use copilot instructions file as a kind of *system prompt*. Let's see at my instructions file:

```markdown
# Coding guidelines
...a place to put your specific guidelines, including architecture guidelines...

# Testing guidelines
...a place to put some more details on how test automation is done...

# General notes on working approach relevant for Agent mode
## Tools
1. We use Powershell so always use syntax of powershell when running commands. In particular do not use `&&`.
2. Use `gradle build` to quickly compile the client code
3. Use `gradle test` to run all tests in the client code
4. use `gradle spotlessApply` to format the code according to the coding style
5. always run `gradle spotlessApply` after creating or editing any source files to ensure the code is formatted correctly

## Prepare plan
Always use this approach when user asks in agent mode to create an action plan.
At the beginning of each task, prepare a plan for the task. If not specified in the user prompt explicitly, ask user for a feature name to name the plan MD file accordingly.


1. Identify Relevant Codebase Parts: Based on the issue description and project onboarding document, determine which parts of the codebase are most likely connected to this issue. List and number specific parts of the codebase mentioned in both documents. Explain your reasoning for each.
2. Hypothesize Root Cause: Based on the information gathered, list potential causes for the issue. Then, choose the most likely cause and explain your reasoning.
3. Identify Potential Contacts: List names or roles mentioned in the documents that might be helpful to contact for assistance with this issue. For each contact, explain why they would be valuable to consult.
4. Self-Reflection Questions: Generate a list of questions that should be asked to further investigate and understand the issue. Include both self-reflective questions and questions for others. Number each question as you write it.
5. Next Steps: Outline the next steps for addressing this issue, including specific actions for logging, debugging and documenting. Provide a clear, actionable plan. Number each step and provide a brief rationale for why it's necessary.

After completing your analysis, create a Markdown document with the following structure:
```markdown
# Action Plan for [Issue Name]

## Issue Description
[Briefly summarize the issue]

## Relevant Codebase Parts
[List and briefly describe the relevant parts of the codebase]

## Root Cause Hypothesis
[State and explain your hypothesis]

## Investigation Questions
[List self-reflection questions and questions for others]

## Next Steps
[Provide a numbered list of actionable steps, including logging and debugging tasks]

## Additional Notes
[Any other relevant information or considerations]
Ensure that your action plan is comprehensive, follows a step-by-step approach, and is presented in an easy-to-read Markdown format. The final document should be named .ai/feature-{feature name}-action-plan.md
```
## Execute plan
1. When developer asks for executing plan step, it is always meant to be a step from the *next steps* section of the action plan.
2. When developer asks for complete plan execution, execute the plan step by step but stop and ask for confirmation before executing each step
3. When developer asks for single step execution, execute only that step
4. When developer asks additionally for some changes, update existing plan with the changes being made
5. Once finishing executing of the step, always mark the step as completed in the action plan by adding a ✅ right before step name.
6. Once finishing executing the whole phase, always mark the phase as completed in the action plan by adding a ✅ right before phase name.
7. If by any reason the step is skipped, it should be marked as skipped in the action plan by adding a ⏭️ right before step name. It should be clearly stated why it was skipped.
8. Always update action plan with relevant findings during plan execution, such as new questions, new contacts, new codebase parts, etc.
```

As you see, you can put a lot of useful information into the instructions file (but remember about LLM limits and try to be short and concise). Key information are included in **General notes on working approach relevant for Agent mode** section. In **Tools** subsection I specify how to use useful tools that are available for the project (that's right, my Gradle plugin project uses Gradle as a build tool itself and the build file contains everything that agent may need to work and verify the results). In **Prepare plan** subsection I specify exactly how the plan should be created and what should be its structure. This is a kind of *one shot* prompting technique, that deliberately covers **User story prompt** (see above). In **Execute plan** I specify exactly how agent should execute the plan and this covers both **Execution prompt** and **Enhancing the plan prompt** (also from above).

## Sample prompts

With all preparations already described, I can start *vibe coding* simply by choosing agent mode in Copilot plugin and placing a simple, problem describing prompt:

> Currently, all tasks in the problem are executed sequentially, not leveraging parallelization feature of Gradle. This results in very long build times for complex projects that execute compilation, preprocessing and postprocessing of long projects. Propose a new bounded context named 'flows' that will allow to organize tasks into chains (flows) that can depend on each other (outputs of one flow can feed input of another flows). This new flow mechanism should have a separate, new DSL syntax. Create an action plan.

I use *Claude Sonnet 4* as a model and get the following action plan:

```markdown
# Action Plan for Flows Parallelization Enhancement

## Issue Description
Currently, all tasks in the Retro Assembler Plugin are executed sequentially, not leveraging Gradle's parallelization features. This results in very long build times for complex projects that execute compilation, preprocessing, and postprocessing. The existing `flows` bounded context exists but lacks implementation. We need to enhance it with a new DSL syntax that allows organizing tasks into chains (flows) that can depend on each other, where outputs of one flow can feed inputs of another flows, enabling parallel execution.

## Relevant Codebase Parts
1. **flows/** - Empty bounded context directory that needs to be populated with domain logic and adapters
2. **infra/gradle/src/main/kotlin/com/github/c64lib/gradle/RetroAssemblerPlugin.kt** - Main plugin where tasks are currently registered sequentially
3. **shared/gradle//** - Contains existing DSL extensions and task utilities that need to be enhanced
4. **compilers/kickass//** - Kick Assembler integration that represents the main compilation flow
5. **processors//** - Various processor modules (charpad, goattracker, image, spritepad) that can be parallelized
6. **dependencies//** - Dependency resolution that can run in parallel with other tasks
7. **emulators/vice//** - Testing execution that depends on compilation outputs
8. **buildSrc//** - Contains Gradle plugin definitions that need enhancement for flow support

## Root Cause Hypothesis
The current architecture treats each task as an independent Gradle task with simple dependencies, but doesn't leverage Gradle's built-in parallelization capabilities. The missing piece is:
1. **Flow Definition DSL** - A way to define task chains with explicit input/output relationships
2. **Dependency Graph Analysis** - Logic to analyze which flows can run in parallel
3. **Task Orchestration** - Enhanced task registration that respects parallel execution capabilities
4. **Resource Management** - Proper handling of shared resources and output artifacts

## Investigation Questions
1. What are the current bottlenecks in build execution time?
2. Which tasks are independent and can run in parallel (e.g., different processors)?
3. What are the input/output dependencies between different task types?
4. How should the new DSL syntax look to be intuitive for users?
5. What Gradle features should we leverage for parallel execution?
6. How do we maintain backward compatibility with existing build scripts?
7. What validation is needed to prevent circular dependencies in flows?
8. How do we handle error propagation in parallel flows?
9. What monitoring/logging is needed for parallel execution debugging?
10. How do we test the parallel execution scenarios effectively?

## Next Steps

### Phase 1: Domain Model Design (Steps 1-3)
1. **Design Flow Domain Model** - Create core domain entities for Flow, FlowStep, FlowDependency, and FlowExecutor in the flows domain
   - Rationale: Establishes the foundational business logic for flow management

2. **Define Flow DSL Syntax** - Design the Kotlin DSL syntax for defining flows in build.gradle.kts files
   - Rationale: Users need an intuitive way to define parallel execution flows

3. **Create Flow Dependency Graph** - Implement logic to build and validate dependency graphs between flows
   - Rationale: Essential for determining which flows can execute in parallel

### Phase 2: Core Implementation (Steps 4-7)
4. **Implement Flow Domain Layer** - Build the core business logic for flow execution and dependency management
   - Rationale: Contains the parallelization logic independent of Gradle specifics

5. **Create Gradle Adapter Layer** - Build adapters that integrate flow domain with Gradle's task system
   - Rationale: Bridges domain logic with Gradle's parallel execution capabilities

6. **Enhance Plugin Registration** - Update RetroAssemblerPlugin to register flows instead of individual tasks
   - Rationale: Enables the plugin to orchestrate parallel execution

7. **Implement Flow DSL Extension** - Create Gradle extensions that provide the flow DSL to build scripts
   - Rationale: Provides user-facing API for defining flows

### Phase 3: Integration and Testing (Steps 8-10)
8. **Create Integration Tests** - Build comprehensive tests for parallel flow execution scenarios
   - Rationale: Ensures parallel execution works correctly and safely

9. **Update Existing Task Implementations** - Migrate existing processors and compilers to use flow system
   - Rationale: Leverages new parallelization for existing functionality

10. **Performance Benchmarking** - Implement benchmarks to measure parallelization improvements
    - Rationale: Validates that the changes actually improve build performance

### Phase 4: Documentation and Release (Steps 11-13)
11. **Update Documentation** - Enhance AsciiDoctor documentation with flow DSL examples and migration guide
    - Rationale: Users need clear guidance on using the new parallelization features

12. **Update CHANGES.adoc** - Document the new flow parallelization feature and breaking changes
    - Rationale: Maintains project change log as per guidelines

13. **Create Migration Examples** - Build example projects showing before/after flow definitions
    - Rationale: Helps users adopt the new parallelization features

### Phase 5: Advanced Features (Steps 14-15)
14. **Implement Flow Monitoring** - Add logging and metrics for parallel execution debugging
    - Rationale: Essential for troubleshooting parallel execution issues

15. **Add Flow Validation** - Implement compile-time validation for flow definitions and dependencies
    - Rationale: Prevents runtime errors from misconfigured flows

## Additional Notes
- **Backward Compatibility**: The implementation should maintain compatibility with existing build scripts while providing migration path to flows
- **Error Handling**: Parallel execution requires robust error handling and propagation mechanisms
- **Resource Contention**: Consider file system and memory resource management for parallel tasks
- **Testing Strategy**: Focus on BDD-style tests using Kotest's Given/When/Then DSL as per project guidelines
- **Gradle Integration**: Leverage Gradle's built-in parallel execution features rather than implementing custom threading
- **Performance Monitoring**: Include metrics collection to validate performance improvements and identify bottlenecks
```

I got a very detailed plan consisting of 15 steps divided into five phases. I got an identification of potentially relevant files and resources. I got a root cause hypothesis that describe why do I need this feature and why current implementation is not good enough for me. Eventually, I have a self reflecting questions showing how broad the context and the scope of thinking for selected model is. At any point I can review this plan, even manually correct it and then use it as an attachment to any interaction with agent mode. Even if I exceed token window, I can create a new conversation, put the plan into the context and continue with much smaller penalty comparing with situation when agent needs to plan the work from scratch.

## Preliminary conclusions

I have successfully implememented three features using this technique:

1. I have prototyped flow DSL and created placeholders for all types of flow steps that I would need.
2. I have implemented assemble step and integrate it with existing [Kick Assembler task][kick-ass].
3. I have implemented CLI step allowing to execute arbitrary CLI command in the flow.

I have tested generated code and checked, that it already supports file sensing and differential builds, however, it still does not use parallelization.

I have spent roughly 12 hours on coding with agent and burned about 25% of my monthly quota of premium tokens (YES!). That's a lot, but mostly I used Claude Sonnet 4 premium model.

I have tried different models once I discovered on how fast premium credits gets consumed. I always started with Claude Sonnet models and after a short while I decided to use version 4 from this family, as it seems to be the fastest one. I strongly prefer Claude for planning. Gemini model usually failed to execute but it was the time when it was still in "preview" phase. I falled back to GPT models and was completely appalled by their performance, especially GPT-4o - both models have very limited token window and require restarting conversation quite often, they have huge problems with using tools and capturing their output and GPT models were the only from the set that were prone to hallucinations. The only problem that was capable of executing the plan with good results is *o4-mini*, although execution can be very slow (it's still in *preview*). o4 seems to be a good choice keeping in mind it costs only 30% of Claude models.

I have some positive experience with GPT-4.1, even with creating a plan with that model, but this still requires a lot of work on my system prompt to be fully useful.

> **Hypothesis:**
> *GPT-4.1* is an economical approach for simple tasks, *Claude Sonnet 4* + *o4-mini* are best combination for most complex tasks. If you can afford, use *Claude Sonnet 4* solely.

[vibe-coding-definition]: https://en.wikipedia.org/wiki/Vibe_coding
[rbt-home-page]: https://c64lib.github.io/gradle-retro-assembler-plugin/
[tony]: https://monochrome-productions.itch.io/tony
[plan-and-execute]: https://langchain-ai.github.io/langgraph/tutorials/plan-and-execute/plan-and-execute/#planning-step
[kick-ass]: https://theweb.dk/KickAssembler/Main.html#frontpage
