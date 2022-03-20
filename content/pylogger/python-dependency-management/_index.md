---
title: "Python Dependency Management"
date: 2022-03-20
draft: false
hidden: false
weight: 100
---

I really enjoy Python. It is one of my go to languages, especially when I want to prototype something quickly. The standard library is quite mature, the third party library selection is massive, I am familiar with the syntax, and I find [The Zen of Python](https://peps.python.org/pep-0020/) (`import this`) to be a relatively strong set of ideals to guide development.

That said, I consider managing dependencies in Python to be a less than ideal experience. From my perspective Python lacks some of the quality of life improvements that many other languages have implemented with regards to managing dependencies: Rust has the fantastic Cargo, Node has the effective NPM and Java has a smattering of tools my favorite being Gradle. I preferred all of these tools to the way I have handled Python virtual environments in the past.

## My Current Knowledge

I have personally worked with multiple python virtual environment tools, `virtualenv`, `venv`, and `pyenv` to create isolated virtual environments. I would then use `Pip` to load third party libraries into the virtual environments. I have also used the impressive `conda` package and environment management tool. `Conda` is great, but is specifically geared towards data science. You can certainly use `conda` outside of data science, but I want a tool that isn't *flavored* in any way. I use Python as a general purpose programming language and regularly perform file manipulation, web request & response parsing, web application development, email management, text message management, and more so to use a data science geared package management tool seems like a miscommunication to my future self, or other developers.

This leads me to continue using `virtualenv` and `pip` as my virtual environment and package management tool-chain.

## Something Better?

I have heard promising things about `Python Poetry`, but haven't dived into using that tool yet. This project is simple enough that I can try out `Python Poetry` to assess it as an alternative to my current workflow. 

Worst case scenario: I add `Python Poetry` to the list of virtual environment management tools I've tried, but I continue to use `virtualenv` and `pip`. 

Best case scenario: I deem `Python Poetry` an improvement over my current workflow and I become a more effective Python developer!

Before we **explore** this new tool let's take a look at how I currently use `virtualenv` and `pip`.

## Articles

{{% children %}}