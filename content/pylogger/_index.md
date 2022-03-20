---
title: "Pylogger"
date: 2022-03-19T21:34:35-05:00
draft: false
hidden: false
weight: 100
---

## Pylogger

Last week I wrote a quick and dirty key logging python3 script. I was writing curriculum on *systemd* and wanted an example of a custom application that would run as a background service.

However, the project needed to meet a few requirements.

## Project Requirements

1. The source code needs to be **accessible**
1. The application needs to have **utility**
1. The application needs to be **relatively easy** to run on Debian, or a Debian based distribution

The students of the program I was working on would be recent web development bootcamp graduates, or early career web professionals.

### Project Accessibility

To truly make the project accessible to the target audience the project source code would need to be:

- short
- simple
- syntax that can be understood by learners with little or no experience in the programming language

Python3 fits the bill. 

Python is widely recognized as having **beginner friendly syntax**.

The [`pynput`](https://pypi.org/project/pynput/) library will encapsulate away the **complexity** of monitoring keyboard input and cut down the **length** of the source code considerably.

Additionally, the provided [Monitoring the Keyboard Example](https://pypi.org/project/pynput/#monitoring-the-keyboard) from the documentation will serve as a very nice starting point for this application. 

{{% notice note %}}
Referencing the exact example the project draws its inspiration from should **empower** students to build their own version of the key logging application if they want to.
{{% /notice %}}

### Project Utility

The project is **objectively useful**, but **morally ambiguous**.

#### Objectively Useful

**End users can see a benefit** by viewing the record of their keystrokes from using a computer. This can be especially enlightening to coders as a way to analyze their typing patterns.

**Companies are also aware of the benefits** provided by key loggers and other employee monitoring software. This [Tech.io article on employee monitoring](https://tech.co/news/employee-monitoring-software-divides-opinion) does an adequate job of outlining some of the benefits companies have seen. Managing human resources in an efficient manner is usually a goal of companies and some companies may not mind using invasive software to achieve their goal.

Also **hackers have long used key logging software as a tool to harvest user data**, most often emails, usernames, and passwords. This project would show just how easy it would be for a human to deduce emails, usernames, and passwords by viewing the full keystroke log of a user.

#### Morally Ambiguous

Since a keystroke logger is both a tool of a black-hat hacker and a tool used by unethical companies to surveil their employees it may not be a good project to show learners.

I asked:

- Is this project creating new black hat hackers?
- Is this project enabling future company leadership to use invasive employee monitoring software?
- Would the inclusion of this project cause more harm than good?

I talked about my concerns with both my wife and curriculum development colleague. They both brought up interesting counterpoints:

- Teaching someone a tool doesn't mean they will use the tool in an inappropriate manner
- Bad actors will use any means necessary to achieve their goals. If learners want to learn about black hat tools, not mentioning them in this course won't stop them from finding them.
- Knowledge is power. How can the learners actively secure against keystroke loggers if they don't know they exist?
- The project isn't production grade.
- The project is highly engaging.
- The project is cool.

My colleague and I came to the conclusion that the benefits of the project outweighed the cons of the project. 

We also designed an alteration to the project that would instead log a sum of the keys typed, but not the order in which they were typed. This would drastically reduce a bad actors ability to use the tool to harvest personal data. Finally, we decided we would talk to other employees of our company, and other individuals in the tech industry to gain a greater perspective on including a project of this nature in our course. If the consensus agrees that this project should not be a part of this course, we would alter it to the version that simply counts characters instead of ordering characters.

This is a personal blog. I will show both the keystroke logger and the version that simply counts characters.

### Easy to Run

With the heavy shit out of the way we can talk about the ease in which students can run this application, specifically on a Debian based distribution.

Python comes as a part of most Debian based distributions. So students already have the interpreter necessary to run this program.

There is one third party dependency that students will need in order to run the program: `pynpt`. I have personally used both `venv` and `virtualenv` to create and manage virtual python environments. However, I've also read about, but never used [`Python Poetry`](https://python-poetry.org/docs/master/#installing-with-the-official-installer) which is supposedly a Python dependency management and packaging tool in Python. This could be a good chance for me to upgrade my Python skills by learning and using this new tool.

Again on this personal blog I'll use both `virtualenv` & `Python Poetry` just to compare what I'm learning to what I know.

When I first wrote the tool I used `virtualenv` as that's a tool I was very comfortable with, so the instructions may continue to contain that tool for learners in the program that is being created.

## Articles

{{% children %}}