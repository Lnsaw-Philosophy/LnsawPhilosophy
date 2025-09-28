> **Translation Note**: This is a machine-translated version. While we strive for accuracy, some nuances may be lost in translation. For the definitive reference, please consult the **Chinese version**.


# Lnsaw Philosophy

## Introduction
### The Birth of Instruction Thinking: A "Rebellion" Born from the Naming Dilemmas of RESTful

It all started on a night that left me deeply troubled.

I could never understand why "login," such a clear and unambiguous action, couldn't be directly named `api/login` within the RESTful paradigm, but had to be awkwardly abstracted into an operation on a "resource" like `api/token` or `api/session`. It felt like sacrificing the most direct expression to adhere to some dogma. "This is so unreasonable!"—that's exactly what I thought.

If login is an action, a request, then why can't it be an instruction?

This thought struck me like lightning. "Login is an instruction. Logout is also an instruction. When I use the `POST` method, aren't I essentially sending an instruction to the server?"

Following this line of thinking, I was amazed to find everything suddenly becoming clear:

What is `GET`? It's the instruction to "fetch" a resource.

What is `POST`? It's the instruction to "create" a resource.

`PUT`, `PATCH`, `DELETE`... They are all instructions!

The so-called "operations on resources" are, in essence, "instructions carrying different intents." Viewed from this new perspective, the mindset for API design instantly became incredibly clear and direct.

Thus, the initial **Instruction Thinking** was born at this moment.

And this starting point ultimately led me to construct a more complete cognitive paradigm—the Lnsaw Philosophy.

### From "Self-Justification" to "Emerging System"

Initially, this idea was purely for **self-justification**—to give myself a logically sound reason to confidently use `api/login`. I would have been perfectly satisfied if it just made sense internally. This was the most primitive form of **Instruction Thinking**.

However, when I tried to use this new perspective to observe the broader world, I discovered it was far more than just a "trick" for naming APIs. It acted like a master key, unlocking many past confusions. The deeper I thought, the more I realized that the concept of "instruction" was connected to a grand blueprint for understanding system interactions.

**Instruction Thinking** was no longer merely an excuse for my personal preference; it began to show the potential to become a **fundamental cognitive paradigm**. And to support this paradigm, another complementary concept—**Reality Thinking**—naturally emerged.

### The Birth of Reality Thinking

**Instruction Thinking** provided me with a sharp sword to analyze "dynamic interactions." But when I tried to wield this sword, I encountered a more fundamental problem: **What exactly are the sender, the receiver, and the object of the instruction?**

If the participants in the interaction aren't clearly defined, then the instruction itself loses its meaning.
+ When I say "user login," what is the "user"? Is it a row of data `User` in the database?
+ What is the "server" receiving the login instruction? Is it a `Server` class?
+ Isn't the "Token" generated after a successful login a real thing that exists?

I realized that before thinking about "how to interact," I must first answer the questions "**who is interacting**" and "**what is being interacted with**." We need a most fundamental perspective to identify and define these basic units that constitute the world.

This perspective must align as closely as possible with our intuitive understanding of the real world: a grain of sand, a cup, a person, a company—they are all objectively existing entities with their own attributes. We cannot ignore the inherent independence and integrity of these entities just because of the abstractions in software systems.

**Thus, as the indispensable foundation for Instruction Thinking—[Reality Thinking] emerged. Its core mission is exceptionally clear: discard all premature, purpose-driven abstractions, return to the origin, and think about and define the existence of all entities from a reality-based perspective.**

Only then can the solid "entities" defined by **Reality Thinking** become the reliable stage for the fluid "interactions" described by Instruction Thinking.

****

## Core Definitions

### Philosophical Framework

| **Philosophy** | **Description** |
| - | - |
| **Reality Thinking** | Think about the existence of all entities from a reality-based perspective. |
| **Instruction Thinking** | Think about the interactions between all entities from an instruction-based perspective. |

### Reality Thinking - Defining Entities
The world is composed of entities. Reality Thinking demands that we accurately recognize and name these entities, pursuing the correspondence between name and reality. It is the cornerstone upon which we build everything.

### Instruction Thinking - Describing Flow
Entities are not isolated; they interact through instructions. Instruction Thinking describes how "intent" drives "action" and triggers "change," thereby completing the flow between entities defined by Reality Thinking. If we view the world of entities defined by Reality Thinking as a physical system, then the role played by instructions is analogous to the role of energy in the physical world.

### Reality & Instruction - A New Perspective for Understanding the World
+ Use **Reality Thinking** to define the stage and the actors (entities).
+ Use **Instruction Thinking** to describe the plot and the lines (interactions) of the entities.