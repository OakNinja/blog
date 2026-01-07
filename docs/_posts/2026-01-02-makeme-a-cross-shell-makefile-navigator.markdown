---
layout: post
title: "MakeMe: A Cross-Shell Makefile Navigator"
date: 2026-01-02 23:45:00 +0100
categories: development
---

Makefiles are awesome. Language-agnostic, flexible, easy to read, and easy to write. You can have one make target called setup that can run npm install, pip install, etc., and you can easily read the makefile in order to understand what is happening and in which order. The one area where Makefiles fall short is discoverability. As projects grow, it can be a challenge to find the right target to run, and making yourself familiar with a new project's Makefile can take some time. 

That's why I wrote [MakeMeFish](https://github.com/OakNinja/MakeMeFish) - A simple tool built on top of `fzf` and other open source tools. One of the hardest parts was finding a way to parse the Makefile and extract the targets (Newer versions of Make include ---print-targets but I've so far never encountered that flag in the wild). I used [this](https://stackoverflow.com/a/26339924) and other similar Stack Overflow answers as a foundation and got some help and feedback from contributors to MakeMeFish, and MakeMeFish has been stable for several years now. The problem with MakeMeFish is that it only works with fish shell.

Over the years, I've gotten a lot of requests to support other shells, and I've been wanting to make it cross-shell for several years but haven't gotten around to doing it. When Gemini 2.5 Pro was released, I got curious on how well it could handle a rewrite of a _very_ specialized tool (sed, awk, fish, make, fzf, etc.). The first tries, unsurprisingly, failed miserably. When I changed the approach by being more explicit in _how_ I wanted to do the rewrite, things fared a lot better. I started out by making up a plan together with Gemini, where we co-wrote a plan in incremental steps. First, we discussed what language would be most suitable for the rewrite. I wanted to use Rust, but Gemini _insisted_ on using Go, quote: _“You're not building a new grep that needs to be the fastest in the world; you're building a wrapper around existing tools. Go is the ideal choice for that.”_. 

After settling on Go, I started doing the conversion in small incremental steps, guiding Gemini in a very similar way to how you would instruct an eager and competent junior developer to do it. The single most important instruction was to stop Gemini from trying to convert [this](https://github.com/OakNinja/MakeMeFish/blob/677bf57330a6772624bfabb1f270cb5d301be68b/functions/mm.fish#L51) into Go. Even for a human, that would be a huge challenge, and Gemini quickly used up all of its quota by just trying stuff that obviously would never work. To make it easier to resume development over time, I created a GEMINI.md file, a MEMORY.md file, and a TODO.md file. GEMINI.md contains the plan, MEMORY.md contains the memory of the conversation, and TODO.md contains the TODO list. Over time, these files evolve together with the project and work great across different sessions and models. 

The conversion went great up to the point where I tried to instruct Gemini to use [fzf as a library](https://gist.github.com/junegunn/193990b65be48a38aac6ac49d5669170) instead of as an external dependency. I tried multiple times, attacking the problem from different angles, but Gemini 2.5 simply couldn't do it. I settled with doing that part by myself. A few weeks later, Antigravity was announced together with Gemini 3. I downloaded Antigravity, opened my project, and Gemini 3 one-shotted the task of using fzf as a library. Impressive.

Now, I think MakeMe, the successor of MakeMeFish, is ready to be released, and you can find it [here](https://github.com/OakNinja/MakeMe).

## Footnote
A **bonus** with MakeMe is that it's a pretty good foundation for building other similar tools on top of fzf, using fzf as a library instead of a dependency. Please use it as a starting point for your own tools, and reach out if you have any questions!
