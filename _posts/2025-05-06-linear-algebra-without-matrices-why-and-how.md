---
title: "Linear Algebra without Matrices, Why and How?"
date: 2025-01-07 16:00:00 +0300
categories: # [TOP, SUB]
tags: [bilkent]
author: uue
description: In this post, I gave my vision on how a first exposition on pure mathematics can be made by considering one of the most "applied" areas of mathematics. 
toc: true # Table of Contents
comments: false
math: true
mermaid: true 
---
## Initial Discussion
You have read the title. This post is exactly about what you just read. Or is it? Let's talk about it.

Usually one hears about how university level mathematics education is extremely abstract, and how it needs to be more motivated. I generally agree with this sentiment. I think the mathematics a **math major** learns is usually unnecessarily and even detrimentally abstract. Usually we learn the formalism first and then the applications pop out of nowhere, when, during the development of the theory, we think about the applications first. 

The pet story is how Galois' investigations on quintic polynomials lead to the field known as `group theory`, when, nowadays, Galois theory is one of the last pieces of algebra you will learn, typically towards the end of a usual **Algebra II** course. 

This phenomena can be seen in both analysis and algebra (and all that is in between) contexts. In group theory, you would first learn what a group is, rather than what a group is supposed to be used for. In topology, you would learn, first, what a topological space is and so on. Perhaps this is the best summary of this that I have heard: `As with all concepts, although the algebraic structure is completely dictated by the axioms, the axioms are completely dictated by how we intended to use the object in the first place` \[1\]. 

Given this duality, it is more convenient, though not necessarily more clear, when expositing a topic, to start with the axioms and build towards the use cases of the theory. It is more formal, it contains way less inexact or merely intuitive statements. One can usually formally verify whether a given statement is correct, although, they may be left wondering why they would even bother doing all this. 

Hearing all that, you would expect that, in linear algebra, you would first learn about what a vector space is, and then talk about what we may use vector spaces for. However, you are 100%, unequivocally, completely **WRONG**. Even for a course on linear algebra for mathematicians, you will still get the application first, namely, the solution of linear systems. You are then introduced to the concept of a **matrix** to abstract away from the more explicit languages of unknowns (such as $x$, $y$, $z$ and so on...). You learn about the Gauss-Jordan elimination as a natural follow up.

At this point, if you are lucky (from the perspective of this author), you are introduced to the concept of a vector space (or you might only be introduced to $\mathbb{R}^n$). Indeed, some courses draw the line between `abstract theory` and `useful calculation`. They use the term `abstract vector space`. It should be somewhat surprising to the mathematics student, that we are learning the central object of the study of linear algebra (vector space) in the middle of the semester, rather than in the first lecture.

I want to make it clear that I am not saying that this style of exposition is bad. Frankly, I agree we need more of it, for the **mathematics community.** However, it is my opinion that, an average engineering student will never get to learn about the traditional `axioms -> theory -> applications` style of exposition. These students usually take the Calculus sequence, which is usually very calculation focused, and they take a version of linear algebra, which is, again, extremely calculation focused. They never get to see the other side of mathematics.

I should write about why I would like more people to get a feel about the axioms first style of mathematics.
- First, I think it reinforces the idea that mathematics is deeper than just calculating with numbers. A lot of our high school education usually installs this idea. 
- Second, I think it makes the difference between the `mathematical object` and its `representation` clear, leading to a better understanding of the mathematical project. \
I first understood this when I randomly saw a very interesting and deep (at least to me) section on the Terry Tao blog: \
\
Note carefully that sample spaces (and their attendant structures) will be used to model probabilistic concepts, rather than to actually be the concepts themselves. This distinction (a mathematical analogue of the map-territory distinction in philosophy) actually is implicit in much of modern mathematics, when we make a distinction between an abstract version of a mathematical object, and a concrete representation (or model) of that object. For instance:

    - In linear algebra, we distinguish between an abstract vector space $V$, and a concrete system of coordinates $\phi: V \rightarrow {\bf R}^n$ given by some basis of $V$.
    - ...
    - Though it is rarely mentioned explicitly, the abstract number systems such as $\bf N$, $\bf Z$, $\bf Q$, $\bf R$, $\bf C$ are distinguished from the concrete numeral systems (e.g. the decimal or binary systems) that are used to represent them (this distinction is particularly useful to keep in mind when faced with the infamous identity $0.999\dots = 1$, or when switching from one numeral representation system to another) \[2\].
- The number system example was particularly interesting for me as I have never thought about it before. However, upon reflecting on it, it has really grown on me. Anyone who has experience with the construction of any of these number sets will surely agree. Our numerical system is just a representation of the underlying theory of the number systems, and it can be developed independently of any model, as weird as that may sound. We are talking about numbers without numbers after all. However, the relation between our numerical symbols (symbols such as $1$, $20$, $342$ and so on) and the set of natural numbers $\mathbb{N}$ have the same relation as a linear map and its representation as a matrix after we have chosen a basis.
- Third, I think it should be a piece of culture. We expose undergraduate students (at least here in Bilkent University) to history, antiquity and classical literature and philosopy, and more. I feel like the `axioms -> theory -> application` and `a set together with operations and morphisms between these sets that respect structure` is a piece of mathematical culture. Surely anyone studying mathematics is familiar with the second sentence, it is truly everywhere. However I think most students outside of the mathematics department, who may have a considerable knowledge of mathematics, will have a hard time deciphering what that sentence meant. 

I will, in this blog post (maybe it will be a series), try to develop the theory of linear algebra (i.e, linear transformations on vector spaces) without reference to matrices. I will not cover everything, so do not try to use this as a reference text. I should mention however, the concept of basis is absolutely essential to linear algebra, and we will be constructing bases from time to time. A careful individual may say that this is no different than doing the matrix representation. I disagree and I refuse to elaborate further on that.

I plan to start from the beginning. However, I will not be presenting proofs of everything, so prior knowledge of the topic is highly encouraged. I want to cover vector spaces, subspaces, dimension, linear maps, their kernel and range, rank-nullity theorem and isomorphisms between vector spaces (from here, after showing the isomorphism between a finite dimensional vector space over $\mathbb{F}$ of dimension $n$ with $\mathbb{F}^n$, it might be an appropriate time to jump into the representation of linear transformations as matrices). 

I also want to show the Jordan canonical form (which is already more than what engineering students usually learn) without mentioning matrices. This is near and dear to my heart. Most people think about matrices when they hear the Jordan canonical form, however, in my view, this confuses the theory. We are forced to say, for example, that the Jordan canonical form is unique, up to reordering of basis vectors. This is completely unnecessary. The Jordan canonical form is just the canonical decomposition of a vector space into its generalized eigenspaces, which is unique. The same is true with the Rational canonical form, where the decomposition is called the Primary Decomposition.

## References
\[1\] https://youtu.be/roP_HC7tiXw?si=GiP9qtLOdUHsJPKp&t=83 \
\[2\] https://terrytao.wordpress.com/2015/09/29/275a-notes-0-foundations-of-probability-theory/
