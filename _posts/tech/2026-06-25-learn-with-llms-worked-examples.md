---
title: "Learning with LLMs Without Brain Rot: Ask for Worked Examples"
tags: Tech
date: 2026-06-25 14:47:24 +0700
---

I've been using LLMs a lot to learn things lately, and there is one failure mode I keep seeing: I ask for a solution, I get a polished final answer, I nod along, and two days later I remember nothing. Reading the answer felt like learning. It wasn't.

There's a better way to ask, and it has a name from the learning-science literature: **worked examples**.

## Answer vs. worked example

If you tell an LLM "give me the solution to this problem", it hands you the destination. A worked example is different — it's the step-by-step breakdown of *how* you get there, like a math problem worked out on paper.

Why does that matter? Each step is small in terms of cognitive requirement. You can actually follow it, and your brain remembers the chain: if you remember four out of five steps, recalling those four nudges you into recalling the fifth, because your brain likes to associate related things. If you only read the final answer, there's no chain at all — just one isolated fact that floats away.

There's a study behind this too: "The Effect of Worked Examples on Learning Solution Steps and Knowledge Transfer" (Chen et al., 2023). Two groups of students learning math, one given worked examples and the other left to unguided problem-solving, on deliberately hard problems. The worked-example group had consistently lower cognitive load, around **50% higher retention**, better results on similar problems (near transfer), and even slightly higher motivation.

## The fading strategy

The same line of research suggests a gradual approach:

1. When you're new to a topic, start with **full** worked examples.
2. As you get more comfortable, peel back the steps — ask for partial solutions and fill in the gaps yourself.
3. Eventually attempt the problem unaided, and only ask for the next step when stuck.

The reason to be careful: when you're cognitively overloaded, you can't retain anything, and frustration makes it worse. Full worked examples keep the load manageable while you build up.

## What I actually do now

When I'm going through something new — a LeetCode-style problem, a math concept, an unfamiliar codebase — I don't ask for the answer anymore. My prompt is basically:

> "Give me a worked example: break down the steps to solve this, one small step at a time."

Then I go through the steps, and for each one I try to predict the next before reading it. If I can't, that's exactly where my gap is, and I ask follow-ups there.

LLMs are essentially a speaking encyclopedia that can break any problem down for you — a personal tutor on demand. If you take the answers, you won't learn. If you use worked examples, you'll probably learn better than most people who don't.
