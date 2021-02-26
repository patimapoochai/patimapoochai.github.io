---
layout: project
type: project
image: images/seacodelogo.png
title:  "SeaBank"
permalink: projects/seabank
# All dates must be YYYY-MM-DD format!
date: 2020-07-03
labels:
  - MongoDB
  - Express
  - React
  - NPM
  - GIT
  - PassportJS
  - SDL
summary: For a secure software design class, my team developed a secure banking application that is built with security in mind from the ground-up.
---

<img class="ui centered huge rounded image" src="{{ site.baseurl }}/images/seabank-home.PNG">

Secure development lifecycle is a new paradigm that I developed in quality assurance and secure software design class, and [SeaBank](https://tylerchinen.github.io/seabank.github.io/) is the application of this paradigm. My team created a web banking solution using the MERN ([MongoDB](https://www.mongodb.com/), [Express](https://expressjs.com/), [React](https://reactjs.org/), and [NPM](https://www.npmjs.com/)) stack.

<img class="ui centered huge rounded image" src="{{ site.baseurl }}/images/seabank-dashboard.PNG">

Users can deposit and withdraw a fictitious currency as well as securely wire their currency to another account. In addition to basic banking functionality, each function was built to withstand cybersecurity attacks and protect the users' data. We conducted regular testing and code review to fix any security issue that could expose the user data. The data transmission is also secure, where the frontend sends requests to the database through a self-validating REST API.

<img class="ui centered huge rounded image" src="{{ site.baseurl }}/images/seabank-restapi.png">

I contributed to this project by developing the REST API and the user database. Each function of the application's frontend is implemented and validated on the server using the Express framework. I created all routes of the API as well as coding the user database. I also integrated most of the routes on the frontend by creating mini-components on the frontend that would send requests through the API.

Working directly with APIs and requests helped me fully understand the inner working of a web application. Due to the emphasis on security, I had to build most features of the application on my own. Without using insecure third-party components, I learned how to validate user inputs and hash user passwords, and it has taught me a lot about the underlying construction of most web applications. I believe that, with the experience gained from this project, I would be prepared to work on any web project regardless of the technology stack.


<br/>
Website: <a href="https://tylerchinen.github.io/seabank.github.io/"><i class="large file icon "></i> tylerchinen.github.io/seabank.github.io</a>

Source: <a href="https://github.com/tylerchinen/SeaCode-Bank-WebApp"><i class="large github icon "></i> github.com/tylerchinen/SeaCode-Bank-WebApp</a>
