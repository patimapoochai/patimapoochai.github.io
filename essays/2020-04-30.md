---
layout: essay
type: essay
title: Repeat after me, Design Patterns Save (Metaphorical) Lives!
# All dates must be YYYY-MM-DD format!
date: 2020-04-30
labels:
  - Design Patterns
  - Problem Solving
  - Community
---

<div>
  <div class="ui Huge center rounded image">
    <img src="https://i.ytimg.com/vi/81W-pkHeTyo/maxresdefault.jpg">
    <div class="ui bottom attached label">
          Rather than forging the same tools a thousand times, a blacksmith uses a cast.
        </div>
  </div>
</div>

## What are design patterns?
Design patterns originated in architecture. Christopher Alexander et al abstracted template solutions to common problems that he encountered in his work. This inspired computer scientists to adapt it into a, now classical, design patterns for programming. This book soon became famous among the industry and design patterns are abstracted from commonly found programming problems and made into generalized templates. This technique is then broadly implemented by all projects and it became a standard concept for software engineers. Everyone lived happily ever after.

## No, really. What *are* design patterns?
But that description is too complicated (and boring), even for a student studying software engineering concepts. It is better to think of design patterns as an analogy for a creation of society's traditions. To me, there are many parallels of design patterns to a folk tale or cautionary story, passed down from generation to generation. An example of a traditional Thai proverb comes to mind: “Don’t sleep under supporting beams, or ghosts would haunt you!” Of course, you wouldn’t be haunted by sleeping under a support beam (at least I hope not). However, many elders have encountered a problem of having these beams fall onto them before. A common explanation for this proverb was described as the ancient construction method being unable to hold together during a house fire. Instead of letting the younger generation learn this wisdom the hard way, people have created a proverb to stop further harm. A common problem among the community was given a simple, singular solution.

Design patterns and proverbs are different, but they fundamentally address the same issue: to effectively solve problems that have been solved many times before. In a software engineering setting, often a senior developer would encounter the same design issue multiple times in their career. It would be a waste not to create a template solution, decreasing the time needed to solve them again. These solutions, being tried and tested by many unfortunate software developers beforehand, would become formalized guidelines for newer developers; like a common wisdom being passed down to the newer generation. Both are essentially a mechanism that our community uses to stop people from reinventing the wheel again.

## Design patterns save lives

Design pattern, in a more practical sense, becomes a method of creating a tool that saves time for other programmers. A good example is the factory design pattern, where objects are created its interactions with other components are different from their own underlying logic. In my software engineering class, I was assigned to create a component that would be placed within other components. A problem arises if I manually insert the it into other components, for it requires multiple inputs of properties and requests to the server. Rather than hard-coding (read as Jerry-rigging) my component into other people’s code or spending large amount of time teaching how the input works, I made a simplified interface component instead. The underlying logic is hidden; only accessible through the high-level component requiring the bare essential input needed. Rather than solving the problem of integrating my component in multiple places, I created a template solution that I and my teammate could use instead.

With software engineering, reinventing the wheel is a common pitfall. Common problems, often with slight variation, surface times and times again. Something like asking the user for input that has been solved before, being solved currently, and will keep being solved again and again until the end of time. With design patterns, we could reduce the time needed for trivial problems and start making world-changing solutions.

<div>
  <div class="ui Huge center rounded image">
    <img src="https://www.rei.com/media/product/403141">
    <div class="ui bottom attached label">
          A real life example of a design pattern is a swiss army knife. This knife combines all the common tools that one would use regularly.
        </div>
  </div>
</div>