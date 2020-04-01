---
layout: essay
type: essay
title: "Domestic Red Team&trade; - Final Project Idea"
date: 2020-03-31
labels:
  - Software Engineering
  - Meteor
---

## Overview
Problem: Many web developers in the ICS program aspire to further develop their school projects into an actual web applications. This dream has some difficulty, for users, investors, and companies that these student seeks would doubt the stability and quality of an application develop by an unexperienced undergrad. Cybersecurity students also share a simmilar lack of oppurtunity, where learning penetration testing for web applications is difficult since running hacking tools against such application is illegal.

Solution: Domestic Red Team&trade; allows students to list their web applications for penetration listing. A student pen-tester could test the app for vulnerability and submit them to the project. The student developing the app could patch the vulnerability, and mark them as fixed on the site. Investors and users could see the public listing of the patched vulnerability, giving more credibility for the web application/

## Approach
There are four user groups:
- unregistered
- tester
- web dev
- admin

An unregistered user could see the publically listed vulnerability of a project and browse testers on the site.

Aside from projetc listings and project page, a tester could also access to a tester-only bug submission page, where they could submit their engagement report.

The web devs would be able to privately see the submitted vulnerabilities of their project. They could mark the bugs as active, fixed, private, public, or invalid. This way, they could verify that the bug is actually valid and control what issues to present to the public. They could also define the scope of what is and isn't allowed to be tested.

The administrators would moderate the project list and bug reports. They would resolve any issues between the testers and the web devs.

### Mockup pages
The site could be broken up into six pages:
- Landing page that lists featured tester profile, bugs, and projects
- Login/Signup page
- Project listing
- Project page, having public, web dev, and admin views
- Vulnerability submission page for the tester
- Tester Profile page
- Admin control panel

## Use case ideas
The usage of the site focuses around bug reporting, submission, and listing.
- Unregistered user can see the landing page, navigate to projects listing, then click on a project to see its public bugs
- Testers could login or sign in, then navigate to the projects listing to choose a project
- A tester could type a report and submit it via a project's bug sumbission page
- Web devs could login or sign in, then navigate to their project page. They could see all of the submitted vulnerability. They would verify each vulnerability, and mark a fixed issue as public. and fixed.

## Beyond the basics

