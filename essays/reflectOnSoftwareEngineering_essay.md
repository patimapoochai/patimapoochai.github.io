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
Design patterns originated in architecture. Christopher Alexander et al abstracted template solutions to common problems that he encountered in their work. This inspired computer scientists to adapt it into a, now classical, design patterns for programming. This book soon became famous and design patterns are abstracted from commonly found programming problems and made into generalized templates and this technique is then broadly implemented by all projects and it became a standard concept for software engineers and everyone lived happily ever after…

## No, really. What *are* design patterns?
But that description is too complicated (and boring), even for me even though I’m studying software engineering concepts. Perhaps using an analogy of how common problems could evolve into a societal tradition. To me, there are many parallels of design patterns to a folk tale or cautionary story, passed down from generation to generation. A traditional Thai proverb comes to mind: “Don’t sleep under supporting beams, or ghosts would haunt you!” Of course, you wouldn’t get haunted by just sleeping under some support beams (at least I hope not). However, many older people have encountered a problem of having these beams fall onto them before, perhaps due to shoddy construction or unfortunate accidents. Instead of letting the younger generation learn this wisdom the hard way, people have created a saying to stop further harm. A common problem among the community was given a simple, singular solution.

Design patterns and proverbs are different, but they fundamentally solve the same problem: to effectively solve problems that have been solved many times before. In a software engineering setting, often a senior developer would encounter the same design issue multiple times in their career. It would be a waste not to create a template solution, decreasing the time needed to solve them again. These solutions tried and tested by many unfortunate software developers beforehand, would become formalized guidelines for newer developers, similar to how common wisdom would be passed down to the newer generation. Both are essentially a mechanism our community used to stop people from reinventing the wheel again.

## Design patterns save lives

Design pattern, in a more practical sense, becomes a method of creating a tool or method that saves time for other programmers. A good example is the factory design pattern, where objects are created its interactions with other components are different from their own underlying logic. In my software engineering class, I was assigned to create a component that would be placed within other components. A problem arises if I directly insert the component, for it requires multiple properties and a request to the server. Rather than hard-coding (read as Jerry-rigging) my component into other people’s components or telling them individually about the props for my components, I made a simplified interface component. The underlying logic is hidden; only accessible through the high-level component containing the bare essential input needed. Rather than solving the problem of integrating my component in multiple places, I created a template solution that I and my teammate could use instead.

With software engineering, reinventing the wheel is a common pitfall. Common problems, often with slight variation, pop up times and times again. Something like asking the user for input that has been solved before, currently, and will keep being solved again and again until the end of time. With design patterns, we could reduce the time needed for trivial problems and start making world-changing solutions.

<div>
  <div class="ui Huge center rounded image">
    <img src="https://www.rei.com/media/product/403141">
    <div class="ui bottom attached label">
          A real life example of a design pattern is a swiss army knife. This knife combines all the common tools that one would use regularly.
        </div>
  </div>
</div>