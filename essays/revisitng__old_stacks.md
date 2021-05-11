---
layout: essay
type: essay
title: Seeing New Things with an Old Set of Eyes
# All dates must be YYYY-MM-DD format!
date: 2021-05-11
labels:
  - Meteor
  - Hawaiian Electric Industries
  - Full-stack Development
---

## Visiting an Old “Friend”
<div class="ui Huge center rounded image">
  <img src="{{ site.baseurl }}/images/projectmalama-landing.png">
  <div class="ui bottom attached label">
      Project Malama landing page.   
  </div>
</div>

Recently, I worked on [Project Malama](https://virtual-manoa-coders.github.io/) in a college web development course. This project is a collaboration between Hawaiian Electric Industries (HEI) and the Information and Computer Sciences department at the University of Hawaii at Manoa. In this upper-level web development course, my team had to deliver a web application that shows the impact of greenhouse gas (GHG) and our community’s transportation by the end of the course. An interesting note about this project is that we had to use the framework that we were taught earlier in the ICS program; [Meteor](https://www.meteor.com/).

<div class="ui Huge center rounded image">
  <img src="{{ site.baseurl }}/images/projectmalama-community2.png">
  <div class="ui bottom attached label">
      Project Malama community page, my main contribution to the project.
  </div>
</div>

It was very easy to dismiss Meteor. It was the web framework that was taught in our introductory web development course earlier in the program. The framework uses [React](https://reactjs.org/) for the frontend, and [MongoDB](https://www.mongodb.com/) for the backend. In my time in that course, the framework gave off an impression of a basic tool meant to be a stepping stone to learning better, more popular web development tools. It was very simple at a glance, and the class project that we built in that semester was a very basic application. The product was static web pages and had rudimentary reactivity. It is a tool that is unfit to create real-world applications. Personally, I feel that Meteor wasn’t as capable as other technologies, like [Django](https://www.djangoproject.com/), [Ruby on Rails](https://rubyonrails.org/), etc. However, when revisiting this framework for a project that has real-world requirements, I see this framework in a new light.

## Same Tools; Different Environment
In the recent intermediate-level web-development class, we were given a prompt by HEI. The electric company wanted an application that would visualize and communicate the impact of our transportation on the environment. This time, the course instructor didn’t give us strict requirements or told us how we should solve the problem. They gave us a real-world prompt and told us to bring this prompt to life using this same-old framework.

This new unrestricted set of requirements made me into learning a lot more about the framework. My team had to generate ideas that would satisfy the requirements while being attractive and appealing to the users. In addition, we have to implement all of these complex ideas within this seemingly simple framework as well. This led me to push my understanding of Meteor as much as possible to create something that the real-world client would want, and it is a very informative experience.

## Doing a Lot with <strike>a Little</strike> a Lot More

### Fan Out the Wings

<div class="ui Huge center rounded image">
  <img src="{{ site.baseurl }}/images/projectmalama-mmethod.png">
  <div class="ui bottom attached label">
      An example Meteor Methods used in the project. This Method returns the user's list of vehicles
  </div>
</div>

When we were asked to create and innovate with this same framework, we had to fully explore what this framework is capable of. For example, we learned how to create secure endpoints that have the same principles as the top framework. In the introductory course, the endpoints (a function that handles user requests) were non-existent. We took care of the user’s data as if both the user and the server were running on the same machine. When I try to look into the popular web stacks that the tech companies are using, this method of handling user requests was inapplicable to the other frameworks. This incompatibility gave off an impression that there’s a gap between the course material and the industry.

However, in the effort to make the user requests more secure for Project Malama, I’ve learned that Meteor could actually handle requests similar to the most popular frameworks. It does this with special functions called “Meteor Methods,” where a set of functions are created and can only be called on the server. This means that user requests in Meteor are handled like endpoints, and I could now apply this model of the user-server requests in other popular frameworks.

### Giving More to Meteor

<div class="ui Huge center rounded image">
  <img src="{{ site.baseurl }}/images/projectmalama-community.png">
  <div class="ui bottom attached label">
      A graph that shows community vs individual Green House Gas contribution on the Community Page.
  </div>
</div>

Another example is creating pleasing and meaningful user interactions that meet the requirements through public libraries. Since we were no longer locked into what we can use, and we can experience using third-party libraries to extend the usability of this project. The introductory course’s project requirements were simple, as we didn’t need to use a third-party library to meet the requirements. This time, however, the client requested something that can’t be simply done with just the framework alone. We had to find visualization libraries and integrate them with React and MongoDB by ourselves.

Learning how to integrate external libraries made me see Meteor as an extensible and adaptable framework for production, where one couldn’t rely solely on the framework to create real-world applications. We earned how to leverage community-made libraries and use them with the framework to its full potential. In the process of digging deeper into the introductory course material, even though what we were taught is often basic and abstract, I gained new insights on how to make them applicable in the real world.

## You are What You’re Willing to Learn
After submitting our product to our client, this project has taught me that one shouldn’t idly hope that the knowledge they have learned in class will be sufficient for the rest of their career. I couldn’t hope to be replicate my work in this project without revisiting and learning more about the Meteor framework. It is this need to dig deeper into what I’ve learned in the past that made my contributions to this project meaningful and made me feel like what I’ve learned really matters.

Just going through college classes won’t make what I’ve learned applicable in the real world. Rather, it is up to us as students to keep learning more about our toolset and make these tools applicable ourselves.

<br/>
<div style="font-size: 28px; font-weight: 500; border-top-style: solid; border-width: 1px">
  <blockquote>
    Ultimately, it is up to us to extend and revisit things we’ve learned during our time in school. Doing so will make the knowledge we've gained applicable to the real world.
  </blockquote>
</div>
