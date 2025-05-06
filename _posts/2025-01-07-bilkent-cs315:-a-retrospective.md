---
title: "Bilkent CS315: A Retrospective"
date: 2025-01-07 16:00:00 +0300
categories: # [TOP, SUB]
tags: [bilkent]
author: uue
description: In this post I go over a course I took this semester at Bilkent University.
toc: true # Table of Contents
comments: false
math: true
mermaid: true 
---
## Introduction
It has been some time since I have resolved to write and reflect on my experience taking the `CS315 Programming Languages` course at Bilkent University. First, I will be going over the content of the course, the syllabus. I will be pointing out what they get right, and, in my estimation, where there is room for improvement. Then, I will be suggesting potential improvement opportunities and proposals.

I should add, however, that there is a specific reason why I am choosing to invest time into writing an article about this course, as opposed to the other courses that I am taking. I will say, right off the bat, that I did find this course overall boring, and I will also add that I felt like we could've learned more and more varied material in a semester. However, the primary reason that I am choosing to write is that I felt like there is a lot of missed opportunity with this course. 

I will be releasing this blog for public, however, you should know that I will not be trying to be expository. You might find it hard to follow if you are not familiar with the subject matter.

## Overview: The Syllabus
![Bilkent CS315 Syllabus](/assets/img/bilkentcs315.png)

This is the syllabus of the course. Right off the bat, I want to mention that `Logic Programming` was omitted from the course when I took it. From what I understand, it is usually omitted from this course.  

The first 12 weeks are usually divided into two cycles. I will call these the lex-yacc cycle (In this syllabus it is week 1-4, however, it takes longer I believe) and the imperative programming cycle. We have the thirteenth week, about functional programming.

### Lex-Yacc Cycle
For the lex-yacc cycle, the goal is a mini course project. In this cycle, we go over a formal way of describing programming languages. In my experience, this part of the course is woefully insufficient. We start by going over the notion of a `formal grammar`. First off, we need to talk about the concept of a `regular expression`. This is where the problem starts: We are given no reason why we should consider this specific subset of languages. Even more problematic is the concept of `context-free grammar`. This is explained extremely quickly, without giving any reason why it is important to consider (i.e, they are computationally very advantageous) or even what exactly they are. I would understand that these are better left to a automata theory course, but, in that case, why bring the concepts up in the first place? I guarantee, if you asked the students of this course what a `context-free grammar` means, you would get very inadequate answer, something like `It is the BNF form`.

However, it is even more problematic that we spend so much time on two programs, namely, `lex` and `yacc`. At this point, the student already has the prerequisite knowledge to use `lex` (i.e, writing regular expressions) in order to construct a lexer, or `yacc` (i.e, writing BNF form context-free grammars) in order to construct a parser. However, ridiculous amounts of time are spent describing the <b>SYNTAX</b> of `yacc`. Except for conflicts, which I will come to, the syntax of these tools can be given as a reading assignment, as the students are already aware of the general theory of what these tools are implementing, at least at a surface level. Instead, we are given lectures for 2 weeks explaining the syntax for these parser tools.

The parse conflicts deserve their own paragraph just because of how inadequately they are covered in the course. The conflict is not a property of a grammar, unlike ambiguity. It is generated when you try to parse a specific grammar with a parsing strategy. `yacc` is a `LALR(1) parser`. Understanding this specific parsing strategy is extremely critical to understanding the conflict this strategy will generate. However, this course doesn't actually cover parsing in any real way (except for the syntax of `yacc`, which doesn't count). The end result is a bunch of extremely confused students who are looking for ways to understand how to systematically detect conflicts for the midterm. They are out of luck, if they don't want to read a [48 page Stanford University document](https://web.stanford.edu/class/archive/cs/cs143/cs143.1156/handouts/parsing.pdf) that they found on the internet, none of which is covered in the class. However, without this knowledge, the best thing they can rely on is gut feeling for the midterm. In my estimation, this course doesn't cover the conflicts at all (in any real way, anyway). 

To conclude, the lex-yacc cycle is very inadequate for a student who is trying to learn the material in a clear way. A lot of the rigour and detail is handwaved away, and we instead focus on the syntax of `yacc` (almost feels like we are back to CS101, learning Java), or some very surface level concept like that. You find students often ask whether they should put `=`, `:=` or `::=` in their BNF form notation, or something like that. The conflicts are the prime example of the surface level explanations in this course. The course project (lexing and parsing a language of your own creation) is an interesting and engaging experience, which is a silver-lining to the first 4-5 weeks. I just wished we were given the opportunity to be more creative. However, the grading criteria forces us to basically make a clone of `C` or some similar language.

### Imperative Programming Cycle
Before explaining my issues with this part of the course, I want to mention the disconnect between the previous part and this part of the course. There is no progression of ideas from the first part to here. What you learned there will stay there, and we will move on. It is almost like the first part of this course is really more appropriate for a `Compiler Design` course, but I will get to that in the next section.

For this section, I really don't have a lot of constructive criticism, I will only share my experience. Let me be honest: I found this part of the course plain boring. Just reading the names of the topics, (`names`, `bindings`, `scopes`, `data types`, `expressions`, `assignments`, ...) it would not be a stretch to assume that a 3rd year computer science student is at least implicitly aware of these topics (meaning they know the concept but don't know it's name). One has to recognize that this may cause a situation where the lectures become uninteresting for a lot of students. 

I want to immediately qualify my statements above. I don't mean to say that we learn nothing from this. There are many useful ideas here: activation records and the call stacks, coroutines, closures, etc... However, in between these, there are so much that we already are familiar with. 

I want to also critique the homework assignments. We are asked to implement CS101 level tasks in 7 different languages. I feel like this was a relatively useless assignment. I, for one, don't remember a single thing from any of these different languages, and I use ChatGPT less than many of my peers for homework assignments. I feel like more engaging and interesting homework assignments can be given. Here is an idea: implementing a call stack with activation records. Many of us, including me, have difficulty understanding static links and dynamic links, for example. I feel like implementing static scoping for myself using static links would've been very instructive, for example.

In conclusion, this section of the course is, first of all, very disconnected from the first part of the course. It has large sections where we are expected to go over material we are familiar with. I think instructors of this course should use more discretion in skipping or, at least, going over very quickly the topics they feel their audiences are mostly familiar with. The homework assignments do very little to make us critically engage with the material we are learning.

### Functional Programming Cycle
In the current form of this course, this section may as well be left out. Functional programming is a huge topic, and we are only studying it for only a single week. Instead of properly examining the computational model of functional programming, maybe doing a homework assignment or two with it, we quickly memorize some syntax of the functional programming language `Scheme`. 

Here is why I find this approach unsatisfactory for me: I didn't know anything about this paradigm before I took this course! It was a genuine opportunity to learn something new, however, we just fast forwarded our way through it.

## Suggestions for Improvement
I have two proposals to rectify the issues I see with this course.
- First Proposal: Keep the lex-yacc cycle, introduce more details on lexing and parsing, and shift towards a course on compiler design. 
    - Currently, the lex-yacc cycle feels pretty disconnected from the course. Implementing this would resolve that issue.
    - Furthermore, this would be the logical next steps for the projects. Maybe we can do a third or fourth project towards implementing our languages.
- Second Proposal: Remove the lex-yacc cycle, start with procedural languages. Do larger sections (and assignments) on functional or logic programming languages.
    - This proposal also addresses the disconnect issue. 
    - There is also more opportunity to focus on the applications of non-procedural paradigms. Many students are wondering about this (why are we learning this etc...).
    - Some applications:
        - How commonly used procedural programming languages use ideas from functional programming.
        - [Lean](https://en.wikipedia.org/wiki/Lean_(proof_assistant)) is a personal favourite of mine. It is also used by many AI models that try to solve math problems.
        - ...

I want to recognize that the faculty probably doesn't want to change this course into a course on compiler design. 
However, I think that the second proposal is completely reasonable to implement. Instead of focusing on procedural languages that most students are already very familiar with, focus on the topics which the students are mostly unaware of. Especially for the non-procedural paradigms, give more motivation so that the students are not wondering why they are learning this.

For comparison, here is the syllabus of the equivalent course from METU:

![METU CENG242 Syllabus](/assets/img/metuceng242.png)

As one can see, this syllabus begins with functional programming. I think this has some merit, as the instructor will be able to give examples of binding, storage, and parameter passing from functional languages. In our current syllabus, even though these topics are also relevant to functional languages, we can only give examples from procedural languages since functional programming languages are not covered yet. 
