---
title: Design by Agent (part 1)
layout: post
categories: genai arch
pic: /genai/img/koszmary-architektury.jpg
---
In my previous, Vibe coding related article, [The Rise of Vibe Coding][the-rise], I have prototyped an experiment to show whether such technique can effectively replace classical coding. At the same time, I have expressed fear, that the term, Vibe coding, will end up there, where all earlier misunderstood or misused IT concepts ended, like EJB, SOA, Agile, to mention just a few.

Let me just cite Andrey Karpathy:

> There's a new kind of coding I call "vibe coding", where you fully give in to the vibes, embrace exponentials, and **forget that the code even exists**. It's possible because the LLMs (e.g. Cursor Composer w Sonnet) are getting too good. Also I just talk to Composer with SuperWhisper so I barely even touch the keyboard. I ask for the dumbest things like "decrease the padding on the sidebar by half" because I'm too lazy to find it. **I "Accept All" always**, **I don't read the diffs anymore**. When I get error messages I just copy paste them in with no comment, usually that fixes it. The code grows **beyond my usual comprehension**, I'd have to really read through it for a while. Sometimes the LLMs can't fix a bug so I just work around it or ask for random changes until it goes away. It's not too bad for throwaway weekend projects, but still quite amusing. I'm building a project or webapp, but it's not really coding - I just see stuff, say stuff, run stuff, and copy paste stuff, and it mostly works.
> *Source: [Andrei Karpathy on X][karpathy]*

It is too good, I've found this particular X entry now, while writing this article, because I must admit, I do exactly the same, since I started using AI Agent for coding. I also accept all changes always, I blindly click "run" on tools (because GitHub Copilot always asks for confirmation, nobody knows why), i type "contninue" when Agent reaches internal limits. So I didn't know that Andrei wrote exactly this and I just started to use this approach anyway, because, honestly, there is no other choice. The reason why is very simple: if you do otherwise, meaning, review every single line of generated code, check everything with libraries API documentation and so on, you will end up on being as slow as before, except paying extra fee for a tool and tokens. It is an either-or kind of game.

The problem is that you can still generate a junk that pretends to work and cannot be relied on and this is definitely not something, that client pays for. So for mid to large size custom applications, there must be a way to use advantages of vibe and still sail your boat in the right direction.

I am more than certain, that the enterprise story of vibe coding will be following: first there will be an era of naive vibe coding that will generate a lot of legacy (a new term: green field legacy, remember that!). Then there will be an era of "controlled vibe coding", that is, for each `n` of young vibe coders there will be a `m` of senior code reviewers, checking twice each line of code being commited. That will be the end of the story - let's try to avoid that.

(But seriously, once I have worked in one company, which rolled out a "controlled agile", I am not kidding!)

Let's summarize this short introductionary rant with a cite from Martin Fowler:

> I'm convinced that this is another fundamental change: talking to the machine in prompts is as different to Ruby as Fortran to assembler. But this is more than a huge jump in abstraction. When I wrote a Fortran function, I could compile it a hundred times, and the result still manifested the exact same bugs. Large Language Models introduce a non-deterministic abstraction, so I can't just store my prompts in git and know that I'll get the same behavior each time. As my colleague Birgitta put it, we're not just moving up the abstraction levels, we're moving sideways into non-determinism at the same time.
> *Source: [Martin Fowler][fowler]*

Yes, don't even dare to store your prompts in GIT instead of the code. The code is there and wishful thinking will not change it.

## Resurrection of Design

There was a time when we had a designers in IT project. That is, design closed the gap between architecture and software developers. Solution was passed over by analysts into the architects to check, that it fits into the boundaries of the architecture and then designers took the job to model data structures and compose the code into the business components, classes, perhaps methods even. Developers, mostly junior employees got a design checklist and just filled in source files with content. That was before Agile came. And no, that wasn't just bound to the "waterfall" project. Trust me, I work for more than 20 years and *never* saw a true waterfall project in my life. Before agile we usually did an iterative-incremental development. And yes, iterative is not a synonym of Agile!

Software designer as a separate role has slowly died and its remnant can be found partially in software architect role and partially in development. I am not missing that role if architect and developers do their job properly. If so, then just preparing a separate design document is a waste of time, especially that it is a volatile document that can be used only during given iteration and gets outdated quickly afterwards (nobody is willing to update it in subsequent iterations and I fully understand this). So, usually architect is the one in charge of writing "technical consideration" or "solution details" in story description or just writes a solution design for bigger features.

I can imagine doing a formal design if only it is cheap to prepare. And guess what? Now we have an AI agent that will do it for me.

## A good design also requires a plan

[Last time][the-rise] I have presented a prompting strategy called "plan and execute". Apparently this strategy, and underlying system prompt in particular, is a general purpose tool. It can be used for implementing new features, changing existing functionality, troubleshooting, bugfixing and, in particular, planning.

I will continue with my already presented hobby project, [RBT][rbt], that is a Gradle plugin that can build assembly projects for Commodore 64 and other 8-bit targets. The project is useful by itself, but I also use it for experiments. One is to use Kotlin as a main programming language, second is to use Gradle as build system (instead of Maven, that I never liked), third is to use so-called Hexagonal Architecture. I use `copilot-instructions.md` file to put some essential information about the project and it's architecture, remembering to keep these informations short and concise. Let me reveal these chapters, as I kept them empty [last time][the-rise].

> ### Coding guidelines
> 1. This application is a Gradle plugin for building Assembly projects for MOS 65xx family of microprocessors, using Kick Assembler as the only supported ASM dialect.
> 2. This application is implemented in Kotlin and uses Gradle as the build tool.
> 3. This application uses Hexagonal Architecture, with the domain layer containing the business logic and the adapters layer containing the glue code between the domain layer and specific technology, such as Gradle.
> 4. Top level directories denote bounded context, each having internal hexagonal structure.
> 5. There are dedicated at-hoc gradle plugins declared for each kind of module, all being located in the `buildSrc` folder.
> 6. Each module should have its own `build.gradle.kts` file, with the root `build.gradle.kts` file aggregating all modules and applying the necessary plugins.
> 7. There is an end user documentation stored in `doc` folder that is implemented in AsciiDoctor, keep it up to date with the code changes.
> 8. There is `CHANGES.adoc` file in the root of the project that contains the change log for the project, keep it up to date with the code changes.
>
> ### Testing guidelines
> 1. This application uses Kotest as testing library for unit and integration tests.
> 2. This application prefers using BDD style of testing, using Given/When/Then DSL of Kotest.
>
> *(for more check my GIT [repository][instructions])*

So, as you can see, I put all essential information related to the project, including the purpose, main technology stack, file layout, where to find documentation and so on. I used some important key words such as `bounded context`, `business logic`, `adapters`. I also use a very specific library to write unit tests (Kotest), so I named it here as well as which style of testing do I prefer (BDD).

Having all this in my repository, plus a "plan and execute" prompt as defined in [The Rise of Vibe Coding][the-rise], I open my Agent tab, select right model (that's one of Claude Sonnet family) and type my first prompt, ordering preparation of a plan:

> Currently, all tasks in the problem are executed sequentially, not leveraging parallelization feature of Gradle. This results in very long build times for complex projects that execute compilation, preprocessing and postprocessing of long projects. Propose a new bounded context named 'flows' that will allow to organize tasks into chains (flows) that can depend on each other (outputs of one flow can feed input of another flows). This new flow mechanism should have a separate, new DSL syntax. Create an action plan.

Honestly speaking, I was shaping this feature for a very long time. Not because it is a very difficult one, but rather that I really had no idea how to model it. I would need to do some prototyping to see what kind of DSL (Domain Specific Language, that is the main notation that Gradle uses as a "language" for its `build.gradle` files) works best for my purpose. I could sit with a pencil and sheet of paper and try to sketch it, but that is something "non coding"-related, that I don't like to spend my free time on.

I have imagined that perhaps I can arrange "steps" in something I called "pipelines", later renamed to flows. I thought that maybe flows can depend on other flows and possibly run in parallel if feasible. I had no idea, however, how to link steps together. In "classic" RBT notation such linking is very limited and built-in into the existing DSL, but this will not work here. I can have a flow that will convert graphical data into binary asset, then pack it, import and compile using assembler and then another flow that will gather these compiled programs and link it together into the floppy disk image using again the assembler. Each step can produce multiple files and consume also multiple files, this is not that easy.

So I made this prompt (above) and was curious whether AI Agent can propose something for me. After a short while an [action plan][action-plan] was generated and this is what I found there.

First, Agent has rewritten my "user story" into more elaborated form as "Issue Description".

> ## Issue Description
> Currently, all tasks in the Retro Assembler Plugin are executed sequentially, not leveraging Gradle's parallelization features. This results in very long build times for complex projects that execute compilation, preprocessing, and postprocessing. The existing `flows` bounded context exists but lacks implementation. We need to enhance it with a new DSL syntax that allows organizing tasks into chains (flows) that can depend on each other, where outputs of one flow can feed inputs of another flows, enabling parallel execution.


All that is essential is included there: what? (organize build into flows), the reason (performance), how? (run independent tasks parallely) and most importantly "that can depend on each other", meaning - not everything can run in parallel.

Then, Agent went straight into the design, first identifying relevant code parts (as instructed!):

> ## Relevant Codebase Parts
> 1. **flows/** - Empty bounded context directory that needs to be populated with domain logic and adapters
> 2. **infra/gradle/src/main/kotlin/com/github/c64lib/gradle/RetroAssemblerPlugin.kt** - Main plugin where tasks are currently registered sequentially
> 3. **shared/gradle/** - Contains existing DSL extensions and task utilities that need to be enhanced
> 4. **compilers/kickass/** - Kick Assembler integration that represents the main compilation flow
> 5. **processors/** - Various processor modules (charpad, goattracker, image, spritepad) that can be parallelized
> 6. **dependencies/** - Dependency resolution that can run in parallel with other tasks
> 7. **emulators/vice/** - Testing execution that depends on compilation outputs
> 8. **buildSrc/** - Contains Gradle plugin definitions that need enhancement for flow support

Yes, I did one thing manually before using AI Agent. I have created an empty bounded context `flows`, and forgot to remove it. So, Agent found it and enlisted as point 1. Point 2 is essential, this is the main plugin class and surely it must be modified to register new DSL constructs (flows). Then, Agent has found essential functionalities that would need to be plugged into the flows, that is 4 and 5. Very good, I am impressed.

Then, following instructions, Agent formulates a root cause hypothesis:

> ## Root Cause Hypothesis
> The current architecture treats each task as an independent Gradle task with simple dependencies, but doesn't leverage Gradle's built-in parallelization capabilities. The missing piece is:
> 1. **Flow Definition DSL** - A way to define task chains with explicit input/output relationships
> 2. **Dependency Graph Analysis** - Logic to analyze which flows can run in parallel
> 3. **Task Orchestration** - Enhanced task registration that respects parallel execution capabilities
> 4. **Resource Management** - Proper handling of shared resources and output artifacts

That was a creative part of the Agent. It is not particularily detailed, but as was my prompt. Point 1 (DSL) is obvious, dependency graph analysis (pt 2) is good, certainly it is a necessary logic to check correctness of the definition written in forementioned DSL as well as to detect right order of task execution (however, I have a feeling, that this is also something Gradle can figure out by itself, but let's see). Task orchestration is also a good point, I can imagine there must be a layer of gradle tasks dynamically created based on the DSL defition. It will be - for sure - much more complex than the one that is currently implemented in RBT. Resource management point is good too - the artifacts, either source or products of task execution can be shared across tasks and this indeed requires additional logic.

Let's now switch into the next part, which is perhaps the most interesting one: investigation questions.

> ## Investigation Questions
> 1. What are the current bottlenecks in build execution time?
> 2. Which tasks are independent and can run in parallel (e.g., different processors)?
> 3. What are the input/output dependencies between different task types?
> 4. How should the new DSL syntax look to be intuitive for users?
> 5. What Gradle features should we leverage for parallel execution?
> 6. How do we maintain backward compatibility with existing build scripts?
> 7. What validation is needed to prevent circular dependencies in flows?
> 8. How do we handle error propagation in parallel flows?
> 9. What monitoring/logging is needed for parallel execution debugging?
> 10. How do we test the parallel execution scenarios effectively?

All these questions are excellent. Particularily I like 4 and the way the Agent has thought about user experience. Backward compatibility (6) - I haven't asked for that, but indeed, I need this one. Circular dependencies (7) is also good - I haven't prompted but it has been clearly derived from my requirement, because you just match input with output and has an infinite execution cycle - we wan't to detect them and prevent.

After a quick check I have proceed with plan execution. And now another remark: I still do the design, but also I will let Agent to generate a few classes, to get a skeleton of the solution.

## Phase 1: design flow domain model

AI Agent has proposed following steps in phase 1:

1. **Design Flow Domain Model** - Create core domain entities for Flow, FlowStep, FlowDependency, and FlowExecutor in the flows domain
2. **Define Flow DSL Syntax** - Design the Kotlin DSL syntax for defining flows in build.gradle.kts files
3. **Create Flow Dependency Graph** - Implement logic to build and validate dependency graphs between flows

I have prompted as follows:

> Execute step 1

...and Agent started coding. During this process, it also modified original plan and deviates a bit, perhaps it got clearer understanding during scanning the code base. BTW, as far as I see, GitHub Copilot Agent uses directory scanning, GLOB file name matching and file searching to look for relevant code pieces. Perhaps it uses LLM to guess keywords to be searched for, perhaps it distills a domain vocabulary from the prompt and instruction file but here it is, and there is completely lack of magic how it is done. Apparently, it is assumed that naming conventions and consequence in applying domain name in code is sufficiently good. I haven't tried to run Agent on deliberately obfuscated code, and perhaps it will not work in that case.

Because I have instructed Copilot in `copilot-instructions.md` to update plan whethever anything new has been discovered, Agent has introduced following changes to the plan:

> ### Relevant code parts
> 2. **flows/adapters/in/gradle/** - Gradle integration adapters (FlowsExtension, FlowDsl) - **UPDATED LOCATION**

and...

> ### Phase 1: Domain Model Design (Steps 1-3) - **COMPLETED**
> 1. ✅ **Identify and Explore Relevant Codebase Parts** - Analyzed current sequential task registration and empty flows context
>   - Rationale: Understanding current architecture is essential for designing parallel flows
> 2. ✅ **Define Flow DSL Syntax** - Created Kotlin DSL syntax for defining flows in build.gradle.kts files in `flows/adapters/in/gradle/`
>   - Rationale: Users need an intuitive way to define parallel execution flows
>   - **Implementation**: FlowDsl.kt, FlowsExtension.kt, and examples located in adapters/in/gradle following hexagonal architecture
> 3. **Create Flow Dependency Graph** - Implement logic to build and validate dependency graphs between flows
>   - Rationale: Essential for determining which flows can execute in parallel

It has marked phase 1 as completed, although step 3 is still untouched. Minor inconvenience. Step 1 has been renamed to "identify and explore", step 2 has been enhanced with implementation log.

An Agent has created inbound adapter with proper build file (`build.gradle.kts`):

```kotlin
plugins {
  id("rbt.adapter.inbound.gradle")
}

group = "com.github.c64lib.retro-assembler.flows.in"

dependencies {
  implementation(project(":flows"))
  implementation(project(":shared:gradle"))
  implementation(project(":shared:domain"))
}
```

The Agent has included this newly defined module into `settings.gradle.kts`, so that the module is visible by the master build:

```kotlin
include(":flows:adapters:in:gradle")
```

It has generated a basic domain object in right module (`flows/domain`):

```kotlin
/*
MIT License

Copyright (c) 2018-2025 c64lib: The Ultimate Commodore 64 Library
Copyright (c) 2018-2025 Maciej Małecki

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
*/
package com.github.c64lib.rbt.flows.domain

import java.io.File

/**
 * Represents a named flow that can contain multiple steps executed in sequence. Flows can run in
 * parallel with other flows if they don't have conflicting dependencies.
 */
data class Flow(
    val name: String,
    val description: String = "",
    val steps: List<FlowStep> = emptyList(),
    val inputs: List<FlowArtifact> = emptyList(),
    val outputs: List<FlowArtifact> = emptyList(),
    val dependencies: List<String> = emptyList() // Names of other flows this flow depends on
) {
  /**
   * Returns true if this flow can run in parallel with the other flow. Flows can run in parallel if
   * neither depends on the other and they don't share conflicting artifacts.
   */
  fun canRunInParallelWith(other: Flow): Boolean {
    val hasDirectDependency = dependencies.contains(other.name) || other.dependencies.contains(name)
    val hasConflictingArtifacts =
        outputs.any { output ->
          other.outputs.any { otherOutput -> output.path == otherOutput.path }
        }
    return !hasDirectDependency && !hasConflictingArtifacts
  }
}

/** Represents a step within a flow that performs a specific action. */
data class FlowStep(
    val name: String,
    val taskType: String, // e.g., "assemble", "preprocess", "test"
    val configuration: Map<String, Any> = emptyMap()
)

/** Represents an artifact (file or directory) that is input to or output from a flow. */
data class FlowArtifact(val name: String, val path: File, val type: ArtifactType)

enum class ArtifactType {
  SOURCE_FILE,
  COMPILED_BINARY,
  PROCESSED_ASSET,
  DEPENDENCY_ARCHIVE,
  TEST_RESULT,
  BUILD_REPORT
}
```

It has even added a license disclaimer, although `gradle spotlessApply` would do it itself. The class makes sense, although I have feeling that some parts are redundant (like `ArtifactType`) but I accepted this (vibe way!) and continued.

In inbound adapter (properly!) a DSL data model has been generated. This is perfectly okay and adheres with hexagonal architecture: a DSL is a Gradle stuff and this belongs to the inbound adapter. Business logic and domain model should exist separately, and it does.

```kotlin
package com.github.c64lib.rbt.flows.adapters.`in`.gradle

import com.github.c64lib.rbt.flows.domain.ArtifactType
import com.github.c64lib.rbt.flows.domain.Flow
import com.github.c64lib.rbt.flows.domain.FlowArtifact
import com.github.c64lib.rbt.flows.domain.FlowStep
import java.io.File

/**
 * DSL builder for creating Flow definitions in build.gradle.kts files.
 *
 * Example usage:
 * ```
 * flows {
 *     flow("preprocessing") {
 *         description = "Process all assets in parallel"
 *
 *         parallel {
 *             step("charpad") {
 *                 from("src/assets/charset")
 *                 to("build/processed/charset")
 *             }
 *             step("spritepad") {
 *                 from("src/assets/sprites")
 *                 to("build/processed/sprites")
 *             }
 *         }
 *     }
 *
 *     flow("compilation") {
 *         dependsOn("preprocessing")
 *
 *         step("assemble") {
 *             from("src/main/asm")
 *             to("build/compiled")
 *         }
 *     }
 * }
 * ```
 */
class FlowDslBuilder {
  private val flows = mutableListOf<Flow>()

  fun flow(name: String, configure: FlowBuilder.() -> Unit): FlowDslBuilder {
    val flowBuilder = FlowBuilder(name)
    flowBuilder.configure()
    flows.add(flowBuilder.build())
    return this
  }

  internal fun build(): List<Flow> = flows.toList()
}

/** Builder for individual Flow definitions. */
class FlowBuilder(private val name: String) {
  var description: String = ""
  private val steps = mutableListOf<FlowStep>()
  private val inputs = mutableListOf<FlowArtifact>()
  private val outputs = mutableListOf<FlowArtifact>()
  private val dependencies = mutableListOf<String>()

  /** Adds a dependency on another flow. */
  fun dependsOn(flowName: String) {
    dependencies.add(flowName)
  }

  /** Adds multiple dependencies on other flows. */
  fun dependsOn(vararg flowNames: String) {
    dependencies.addAll(flowNames)
  }

  /** Creates a single step in this flow. */
  fun step(stepName: String, configure: StepBuilder.() -> Unit) {
    val stepBuilder = StepBuilder(stepName)
    stepBuilder.configure()
    val (step, stepInputs, stepOutputs) = stepBuilder.build()
    steps.add(step)
    inputs.addAll(stepInputs)
    outputs.addAll(stepOutputs)
  }

  /**
   * Creates multiple steps that can run in parallel within this flow. Note: This is for
   * documentation purposes - actual parallelization happens at the flow level, not step level.
   */
  fun parallel(configure: ParallelStepsBuilder.() -> Unit) {
    val parallelBuilder = ParallelStepsBuilder()
    parallelBuilder.configure()
    val (parallelSteps, parallelInputs, parallelOutputs) = parallelBuilder.build()
    steps.addAll(parallelSteps)
    inputs.addAll(parallelInputs)
    outputs.addAll(parallelOutputs)
  }

  internal fun build(): Flow =
      Flow(
          name = name,
          description = description,
          steps = steps,
          inputs = inputs,
          outputs = outputs,
          dependencies = dependencies)
}

/** Builder for steps within a parallel block. */
class ParallelStepsBuilder {
  private val steps = mutableListOf<FlowStep>()
  private val inputs = mutableListOf<FlowArtifact>()
  private val outputs = mutableListOf<FlowArtifact>()

  fun step(stepName: String, configure: StepBuilder.() -> Unit) {
    val stepBuilder = StepBuilder(stepName)
    stepBuilder.configure()
    val (step, stepInputs, stepOutputs) = stepBuilder.build()
    steps.add(step)
    inputs.addAll(stepInputs)
    outputs.addAll(stepOutputs)
  }

  internal fun build(): Triple<List<FlowStep>, List<FlowArtifact>, List<FlowArtifact>> =
      Triple(steps, inputs, outputs)
}

/** Builder for individual steps. */
class StepBuilder(private val name: String) {
  private val config = mutableMapOf<String, Any>()
  private val inputs = mutableListOf<FlowArtifact>()
  private val outputs = mutableListOf<FlowArtifact>()

  /** Specifies input sources for this step. */
  fun from(path: String, type: ArtifactType = ArtifactType.SOURCE_FILE) {
    inputs.add(FlowArtifact("${name}_input", File(path), type))
    config["inputPath"] = path
  }

  /** Specifies multiple input sources for this step. */
  fun from(vararg paths: String, type: ArtifactType = ArtifactType.SOURCE_FILE) {
    paths.forEach { path ->
      inputs.add(FlowArtifact("${name}_input_${inputs.size}", File(path), type))
    }
    config["inputPaths"] = paths.toList()
  }

  /** Specifies output destination for this step. */
  fun to(path: String, type: ArtifactType = ArtifactType.COMPILED_BINARY) {
    outputs.add(FlowArtifact("${name}_output", File(path), type))
    config["outputPath"] = path
  }

  /** Specifies multiple output destinations for this step. */
  fun to(vararg paths: String, type: ArtifactType = ArtifactType.COMPILED_BINARY) {
    paths.forEach { path ->
      outputs.add(FlowArtifact("${name}_output_${outputs.size}", File(path), type))
    }
    config["outputPaths"] = paths.toList()
  }

  /** Adds custom configuration for this step. */
  fun configure(key: String, value: Any) {
    config[key] = value
  }

  /** Adds custom configuration using a map. */
  fun configure(configuration: Map<String, Any>) {
    config.putAll(configuration)
  }

  internal fun build(): Triple<FlowStep, List<FlowArtifact>, List<FlowArtifact>> {
    val step = FlowStep(name = name, taskType = inferTaskType(), configuration = config.toMap())
    return Triple(step, inputs, outputs)
  }

  private fun inferTaskType(): String =
      when {
        name.contains("charpad", ignoreCase = true) -> "charpad"
        name.contains("spritepad", ignoreCase = true) -> "spritepad"
        name.contains("goattracker", ignoreCase = true) -> "goattracker"
        name.contains("image", ignoreCase = true) -> "image"
        name.contains("assemble", ignoreCase = true) -> "assemble"
        name.contains("test", ignoreCase = true) -> "test"
        name.contains("dependencies", ignoreCase = true) -> "dependencies"
        else -> "generic"
      }
}
```

Agent has even generated JavaDoc with usage examples. Perhaps I will remove it later, but this example actually gives me an impression, that we go in the right direction. Here I have learned something about Gradle API, I didn't know: it is possible to use builder pattern while defining DSL objects - that's definitely more handy approach than the one I used before! Big point for AI Agent!

Last part generated in this step is `FlowsExtension`, an archetype used in Gradle when defining new DSL:

```kotlin
package com.github.c64lib.rbt.flows.adapters.`in`.gradle

import com.github.c64lib.rbt.flows.domain.Flow
import org.gradle.api.Project

open class FlowsExtension(private val project: Project) {
  private val definedFlows = mutableListOf<Flow>()

  /** Defines flows using the DSL syntax. */
  fun flows(configure: FlowDslBuilder.() -> Unit) {
    val builder = FlowDslBuilder()
    builder.configure()
    definedFlows.addAll(builder.build())
  }

  /** Returns all flows defined in this extension. */
  fun getFlows(): List<Flow> = definedFlows.toList()

  /** Validates that all flow dependencies exist and there are no circular dependencies. */
  fun validateFlows(): List<String> {
    val errors = mutableListOf<String>()
    val flowNames = definedFlows.map { it.name }.toSet()

    // Check that all dependencies exist
    definedFlows.forEach { flow ->
      flow.dependencies.forEach { dependency ->
        if (dependency !in flowNames) {
          errors.add("Flow '${flow.name}' depends on non-existent flow '$dependency'")
        }
      }
    }

    // Check for circular dependencies
    definedFlows.forEach { flow ->
      val visited = mutableSetOf<String>()
      val recursionStack = mutableSetOf<String>()

      if (hasCircularDependency(flow.name, visited, recursionStack, definedFlows)) {
        errors.add("Circular dependency detected involving flow '${flow.name}'")
      }
    }

    return errors
  }

  /** Returns flows that can run in parallel (have no dependencies between them). */
  fun getParallelFlows(): List<List<Flow>> {
    val result = mutableListOf<List<Flow>>()
    val processed = mutableSetOf<String>()

    while (processed.size < definedFlows.size) {
      val readyFlows =
          definedFlows.filter { flow ->
            flow.name !in processed && flow.dependencies.all { it in processed }
          }

      if (readyFlows.isEmpty()) {
        break // Circular dependency or other issue
      }

      // Group flows that can run in parallel
      val parallelGroup = mutableListOf<Flow>()
      readyFlows.forEach { flow ->
        if (parallelGroup.all { it.canRunInParallelWith(flow) }) {
          parallelGroup.add(flow)
        }
      }

      if (parallelGroup.isNotEmpty()) {
        result.add(parallelGroup)
        processed.addAll(parallelGroup.map { it.name })
      }
    }

    return result
  }

  private fun hasCircularDependency(
      flowName: String,
      visited: MutableSet<String>,
      recursionStack: MutableSet<String>,
      flows: List<Flow>
  ): Boolean {
    if (recursionStack.contains(flowName)) {
      return true
    }

    if (visited.contains(flowName)) {
      return false
    }

    visited.add(flowName)
    recursionStack.add(flowName)

    val flow = flows.find { it.name == flowName }
    flow?.dependencies?.forEach { dependency ->
      if (hasCircularDependency(dependency, visited, recursionStack, flows)) {
        return true
      }
    }

    recursionStack.remove(flowName)
    return false
  }
}
```

At this point I have noticed, that something bad has happened: it seems that business logic starts to leak out to the adapter! This cannot be. So I prompted:

> Extract business logic (circular dependency management) to the separate service.

Agent has confirmed that this has been overseen and generated `FlowDependencyGraph.kt` file:

```kotlin
package com.github.c64lib.rbt.flows.domain

/**
 * Represents a dependency graph of flows that validates dependencies and determines execution
 * order. This class ensures that flows are executed in the correct order while maximizing
 * parallelization opportunities.
 */
class FlowDependencyGraph(private val flows: List<Flow>) {

  private val flowsByName = flows.associateBy { it.name }
  private val dependencyMap = buildDependencyMap()

  /**
   * Validates the dependency graph for circular dependencies and missing flow references.
   * @throws FlowDependencyException if validation fails
   */
  fun validate() {
    validateFlowReferences()
    validateNoCycles()
  }

  /**
   * Returns flows organized into execution levels where all flows in the same level can be executed
   * in parallel.
   */
  fun getExecutionLevels(): List<List<Flow>> {
    validate()

    val levels = mutableListOf<List<Flow>>()
    val completed = mutableSetOf<String>()
    val remaining = flows.map { it.name }.toMutableSet()

    while (remaining.isNotEmpty()) {
      val currentLevel =
          remaining.filter { flowName ->
            val flow = flowsByName[flowName]!!
            flow.dependencies.all { dep -> completed.contains(dep) }
          }

      if (currentLevel.isEmpty()) {
        throw FlowDependencyException(
            "Unable to resolve dependencies - possible circular dependency detected")
      }

      levels.add(currentLevel.map { flowsByName[it]!! })
      completed.addAll(currentLevel)
      remaining.removeAll(currentLevel.toSet())
    }

    return levels
  }

  /** Returns all flows that can run in parallel with the given flow. */
  fun getParallelFlows(flowName: String): List<Flow> {
    val targetFlow =
        flowsByName[flowName] ?: throw FlowDependencyException("Flow not found: $flowName")

    return flows.filter { flow -> flow.name != flowName && targetFlow.canRunInParallelWith(flow) }
  }

  /** Returns the topological order of flows for sequential execution. */
  fun getTopologicalOrder(): List<Flow> {
    return getExecutionLevels().flatten()
  }

  /** Returns all direct and transitive dependencies of the given flow. */
  fun getAllDependencies(flowName: String): Set<String> {
    val flow = flowsByName[flowName] ?: throw FlowDependencyException("Flow not found: $flowName")

    val allDeps = mutableSetOf<String>()
    val toProcess = flow.dependencies.toMutableList()

    while (toProcess.isNotEmpty()) {
      val dep = toProcess.removeAt(0)
      if (allDeps.add(dep)) {
        val depFlow =
            flowsByName[dep] ?: throw FlowDependencyException("Dependency not found: $dep")
        toProcess.addAll(depFlow.dependencies)
      }
    }

    return allDeps
  }

  /** Returns all flows that directly or transitively depend on the given flow. */
  fun getDependents(flowName: String): Set<String> {
    if (!flowsByName.containsKey(flowName)) {
      throw FlowDependencyException("Flow not found: $flowName")
    }

    return flows
        .filter { flow -> getAllDependencies(flow.name).contains(flowName) }
        .map { it.name }
        .toSet()
  }

  private fun buildDependencyMap(): Map<String, Set<String>> {
    return flows.associate { flow -> flow.name to flow.dependencies.toSet() }
  }

  private fun validateFlowReferences() {
    flows.forEach { flow ->
      flow.dependencies.forEach { dep ->
        if (!flowsByName.containsKey(dep)) {
          throw FlowDependencyException("Flow '${flow.name}' depends on unknown flow: '$dep'")
        }
      }
    }
  }

  private fun validateNoCycles() {
    val visited = mutableSetOf<String>()
    val recursionStack = mutableSetOf<String>()

    flows.forEach { flow ->
      if (!visited.contains(flow.name)) {
        if (hasCycleDfs(flow.name, visited, recursionStack)) {
          throw FlowDependencyException("Circular dependency detected involving flow: ${flow.name}")
        }
      }
    }
  }

  private fun hasCycleDfs(
      flowName: String,
      visited: MutableSet<String>,
      recursionStack: MutableSet<String>
  ): Boolean {
    visited.add(flowName)
    recursionStack.add(flowName)

    val flow = flowsByName[flowName]!!
    flow.dependencies.forEach { dep ->
      if (!visited.contains(dep)) {
        if (hasCycleDfs(dep, visited, recursionStack)) {
          return true
        }
      } else if (recursionStack.contains(dep)) {
        return true
      }
    }

    recursionStack.remove(flowName)
    return false
  }
}

/** Exception thrown when there are issues with flow dependencies. */
class FlowDependencyException(message: String) : Exception(message)
```

At the end Agent concluded by generating unit tests for newly generated service class:

```kotlin
class FlowDependencyGraphTest :
    BehaviorSpec({
      Given("a flow dependency graph with valid flows") {
        val compileFlow =
            Flow(
                name = "compile",
                description = "Compile source files",
                steps = listOf(FlowStep("assemble", "kickass")),
                inputs =
                    listOf(FlowArtifact("source", File("src/main.asm"), ArtifactType.SOURCE_FILE)),
                outputs =
                    listOf(
                        FlowArtifact(
                            "binary", File("build/main.prg"), ArtifactType.COMPILED_BINARY)))

        val preprocessFlow =
            Flow(
                name = "preprocess",
                description = "Preprocess assets",
                steps = listOf(FlowStep("process", "charpad")),
                inputs =
                    listOf(
                        FlowArtifact(
                            "charset", File("assets/chars.ctm"), ArtifactType.SOURCE_FILE)),
                outputs =
                    listOf(
                        FlowArtifact(
                            "processed", File("build/chars.bin"), ArtifactType.PROCESSED_ASSET)))

        val testFlow =
            Flow(
                name = "test",
                description = "Run tests",
                steps = listOf(FlowStep("execute", "vice")),
                dependencies = listOf("compile"),
                inputs =
                    listOf(
                        FlowArtifact(
                            "binary", File("build/main.prg"), ArtifactType.COMPILED_BINARY)),
                outputs =
                    listOf(
                        FlowArtifact(
                            "results", File("build/test-results.xml"), ArtifactType.TEST_RESULT)))

        val flows = listOf(compileFlow, preprocessFlow, testFlow)
        val graph = FlowDependencyGraph(flows)

        When("validating the dependency graph") {
          Then("it should not throw any exception") { shouldNotThrowAny { graph.validate() } }
        }

        When("getting execution levels") {
          val levels = graph.getExecutionLevels()

          Then("it should organize flows into correct levels") {
            levels shouldHaveSize 2
            levels[0] shouldContainExactlyInAnyOrder listOf(compileFlow, preprocessFlow)
            levels[1] shouldContainExactly listOf(testFlow)
          }
        }

        When("getting parallel flows for compile") {
          val parallelFlows = graph.getParallelFlows("compile")

          Then("it should return flows that can run in parallel") {
            parallelFlows shouldContainExactly listOf(preprocessFlow)
          }
        }

        When("getting topological order") {
          val order = graph.getTopologicalOrder()

          Then("it should return flows in dependency order") {
            order shouldHaveSize 3
            // Compile and preprocess should come before test
            val compileIndex = order.indexOf(compileFlow)
            val preprocessIndex = order.indexOf(preprocessFlow)
            val testIndex = order.indexOf(testFlow)

            compileIndex shouldBe 0
            preprocessIndex shouldBe 1
            testIndex shouldBe 2
          }
        }

        When("getting all dependencies for test flow") {
          val dependencies = graph.getAllDependencies("test")

          Then("it should return all direct dependencies") {
            dependencies shouldContainExactly setOf("compile")
          }
        }

        When("getting dependents of compile flow") {
          val dependents = graph.getDependents("compile")

          Then("it should return flows that depend on it") {
            dependents shouldContainExactly setOf("test")
          }
        }
      }

      Given("a flow dependency graph with circular dependencies") {
        val flowA = Flow(name = "flowA", dependencies = listOf("flowB"))
        val flowB = Flow(name = "flowB", dependencies = listOf("flowC"))
        val flowC = Flow(name = "flowC", dependencies = listOf("flowA"))

        val flows = listOf(flowA, flowB, flowC)
        val graph = FlowDependencyGraph(flows)

        When("validating the dependency graph") {
          Then("it should throw FlowDependencyException") {
            val exception = shouldThrow<FlowDependencyException> { graph.validate() }
            exception.message shouldContain "Circular dependency detected"
          }
        }

        When("getting execution levels") {
          Then("it should throw FlowDependencyException") {
            val exception = shouldThrow<FlowDependencyException> { graph.getExecutionLevels() }
            exception.message shouldContain "Circular dependency detected"
          }
        }
      }

      Given("a flow dependency graph with missing dependencies") {
        val flowWithMissingDep = Flow(name = "flowA", dependencies = listOf("nonExistentFlow"))

        val flows = listOf(flowWithMissingDep)
        val graph = FlowDependencyGraph(flows)

        When("validating the dependency graph") {
          Then("it should throw FlowDependencyException") {
            val exception = shouldThrow<FlowDependencyException> { graph.validate() }
            exception.message shouldContain "depends on unknown flow: 'nonExistentFlow'"
          }
        }
      }

      Given("a flow dependency graph with complex dependencies") {
        val sourceFlow = Flow(name = "source")
        val compileFlow = Flow(name = "compile", dependencies = listOf("source"))
        val preprocessFlow = Flow(name = "preprocess", dependencies = listOf("source"))
        val packageFlow = Flow(name = "package", dependencies = listOf("compile", "preprocess"))
        val testFlow = Flow(name = "test", dependencies = listOf("package"))
        val deployFlow = Flow(name = "deploy", dependencies = listOf("test"))

        val flows =
            listOf(sourceFlow, compileFlow, preprocessFlow, packageFlow, testFlow, deployFlow)
        val graph = FlowDependencyGraph(flows)

        When("getting execution levels") {
          val levels = graph.getExecutionLevels()

          Then("it should organize flows into correct hierarchical levels") {
            levels shouldHaveSize 5
            levels[0] shouldContainExactly listOf(sourceFlow)
            levels[1] shouldContainExactlyInAnyOrder listOf(compileFlow, preprocessFlow)
            levels[2] shouldContainExactly listOf(packageFlow)
            levels[3] shouldContainExactly listOf(testFlow)
            levels[4] shouldContainExactly listOf(deployFlow)
          }
        }

        When("getting all dependencies for deploy flow") {
          val dependencies = graph.getAllDependencies("deploy")

          Then("it should return all transitive dependencies") {
            dependencies shouldContainExactlyInAnyOrder
                setOf("test", "package", "compile", "preprocess", "source")
          }
        }

        When("getting dependents of source flow") {
          val dependents = graph.getDependents("source")

          Then("it should return all flows that transitively depend on it") {
            dependents shouldContainExactlyInAnyOrder
                setOf("compile", "preprocess", "package", "test", "deploy")
          }
        }
      }

      Given("a flow dependency graph with conflicting artifacts") {
        val flow1 =
            Flow(
                name = "flow1",
                outputs =
                    listOf(
                        FlowArtifact(
                            "output", File("build/shared.bin"), ArtifactType.COMPILED_BINARY)))
        val flow2 =
            Flow(
                name = "flow2",
                outputs =
                    listOf(
                        FlowArtifact(
                            "output", File("build/shared.bin"), ArtifactType.COMPILED_BINARY)))

        val flows = listOf(flow1, flow2)
        val graph = FlowDependencyGraph(flows)

        When("getting parallel flows for flow1") {
          val parallelFlows = graph.getParallelFlows("flow1")

          Then("it should not include flow2 due to conflicting artifacts") {
            parallelFlows shouldHaveSize 0
          }
        }
      }

      Given("an empty flow dependency graph") {
        val graph = FlowDependencyGraph(emptyList())

        When("getting execution levels") {
          val levels = graph.getExecutionLevels()

          Then("it should return empty list") { levels shouldHaveSize 0 }
        }

        When("getting topological order") {
          val order = graph.getTopologicalOrder()

          Then("it should return empty list") { order shouldHaveSize 0 }
        }
      }

      Given("a flow dependency graph with non-existent flow queries") {
        val flow = Flow(name = "existingFlow")
        val graph = FlowDependencyGraph(listOf(flow))

        When("getting parallel flows for non-existent flow") {
          Then("it should throw FlowDependencyException") {
            val exception =
                shouldThrow<FlowDependencyException> { graph.getParallelFlows("nonExistentFlow") }
            exception.message shouldContain "Flow not found: nonExistentFlow"
          }
        }

        When("getting dependencies for non-existent flow") {
          Then("it should throw FlowDependencyException") {
            val exception =
                shouldThrow<FlowDependencyException> { graph.getAllDependencies("nonExistentFlow") }
            exception.message shouldContain "Flow not found: nonExistentFlow"
          }
        }

        When("getting dependents for non-existent flow") {
          Then("it should throw FlowDependencyException") {
            val exception =
                shouldThrow<FlowDependencyException> { graph.getDependents("nonExistentFlow") }
            exception.message shouldContain "Flow not found: nonExistentFlow"
          }
        }
      }
    })
```

And then nightmare started, meaning trying to getting this code to the point where it compiles and all tests pass. Apparently a spotless plugin that is integrated in my Gradle build is to restrictive and usually newly generated code does not pass the checks, so I have updated my `copilot-instructions.md` file:

> 5. always run `gradle spotlessApply` after creating or editing any source files to ensure the code is formatted correctly

it helped a bit, but not always. I gave up and defined hot keys for this command and do it quite frequently manually. It is still not a big problem. I have noticed, that Copilot frequently misses output of the tool that is run, so after few tries I usually try to help him by running the build manually and pasting error message to the Agent chat window with command `please fix` - it always helps, no matter these are test failures or compilation errors. I must admit, that even with Claude Sonnet 4, these cycles take me the most time when working with agent. But still - I kind of like generated code.

## Summary

It has been a lot of text for a single blog entry so let's make a short pause and recap. I have intuitively started with design because this is how I normally work with new, bigger in size features. I am not disappointed, Agent made a great job and examples like these:

```kotlin
/*
 * flows {
 *     flow("preprocessing") {
 *         description = "Process all assets in parallel"
 *
 *         parallel {
 *             step("charpad") {
 *                 from("src/assets/charset")
 *                 to("build/processed/charset")
 *             }
 *             step("spritepad") {
 *                 from("src/assets/sprites")
 *                 to("build/processed/sprites")
 *             }
 *             step("image") {
 *                 from("src/assets/images")
 *                 to("build/processed/images")
 *             }
 *         }
 *     }
 *
 *     flow("dependencies") {
 *         step("resolve") {
 *             to("build/dependencies")
 *         }
 *     }
 *
 *     flow("compilation") {
 *         dependsOn("preprocessing", "dependencies")
 *
 *         step("assemble") {
 *             from("src/main/asm")
 *             to("build/compiled")
 *         }
 *     }
 *
 *     flow("testing") {
 *         dependsOn("compilation")
 *
 *         step("test") {
 *             from("build/compiled")
 *             to("build/test-results")
 *         }
 *     }
 * }
 */
```

are helping me imagining, how the new feature can be used, even before I can actually try it (because with phase 1 it is still not integrated with my RBT).

A few hints:
1. Do GIT commits frequently, because sometimes Agent can go crazy and start breaking things.
2. Try to use single conversation as long as possible, but commit frequently, because (see pt 1), once we reach end of the context window, strange things may occur.
3. Copilot Agent sometimes drops an error, that a limit of tool iterations has been reached. Don't worry and type `continue`.
4. If you don't like the design, you can always ask agent to enhance the plan with additional steps.

In next article I will show, how the implementation was integrated, there were many more source files generated in this story.

[the-rise]: the-rise-of-vibe-coding
[fowler]: https://martinfowler.com/articles/2025-nature-abstraction.html
[karpathy]: https://x.com/karpathy/status/1886192184808149383?t=7EBkBxJuW-5De2hqxso_jA&s=03
[rbt]: https://c64lib.github.io/gradle-retro-assembler-plugin/
[instructions]: https://github.com/c64lib/gradle-retro-assembler-plugin/blob/develop/.github/copilot-instructions.md
[action-plan]: https://github.com/c64lib/gradle-retro-assembler-plugin/blob/develop/.ai/62-pipelines/feature-flows-parallelization-action-plan.md
