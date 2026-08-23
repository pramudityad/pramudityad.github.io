---
title: "Teaching at the Edge: AI Tutors and the Zone of Proximal Development"
tags: Tech
date: 2026-08-16 18:19:24 +0700
---

A century ago, psychologist Lev Vygotsky described a simple idea that still shapes education research: between what a learner can do alone and what they can't do yet, there's a band where learning actually happens — with help. He called it the **Zone of Proximal Development** (ZPD), and the help itself is *scaffolding*, best delivered by a "more knowledgeable other".

For decades the problem has been logistics. One teacher, thirty students, one pace. Scaffolding — adjusting help to each learner's edge of understanding — is labor-intensive, so most of us got the same lecture regardless of where we stood.

LLMs quietly change that math, and researchers are starting to formalize it.

## The research so far

A 2024 systematic review in *Education and Information Technologies* (Springer) used ZPD as its lens on AI in higher education: how AI tools help students identify and operate within their own ZPD, create collaborative learning environments, and provide scaffolding ([10.1007/s10639-024-13112-0](https://link.springer.com/article/10.1007/s10639-024-13112-0)).

"GenAI as More Knowledgeable Other" is now an active research theme in its own right — recent work examines GenAI in Vygotsky's MKO role for scaffolding within the ZPD in higher education.

There's also work on a subtler skill: how a tutor reacts to errors. Kakarla, Thomas, Lin, Gupta & Koedinger ([arXiv 2401.03238](https://arxiv.org/abs/2401.03238)) use LLMs to assess whether tutors correctly guide low-efficacy students to **self-correct** math errors rather than just pointing the errors out — the difference between scaffolding and answer-giving.

## What it looks like in practice

The pattern emerging from people building AI tutoring systems is consistent:

1. **Probe first.** Before teaching, find out what the learner actually knows — a few targeted questions, not a lecture.
2. **Teach at the edge.** Stay just beyond what they can do alone: too far ahead and it's noise, too far behind and it's boredom.
3. **One reasoning step at a time.** Standard chatbot behavior is to rush through an entire explanation before you can digest it; good tutors hold back.
4. **Fade the scaffolding.** Gradually remove hints and let the learner finish steps unaided — the goal is to need the tutor less.

Familiarity matters too: the same explanation lands better from a source you trust than from a random post, which is one reason building your own tutor around your own notes feels different from raw ChatGPT.

## The honest caveat

The empirical base is still thin — systematic reviews note more conceptual than peer-reviewed experimental work. But the direction is compelling: the oldest problem in education, meeting each student where they are, is the one thing an LLM can genuinely do at scale. The zone of proximal development turns out to be a pretty good spec for an AI tutor.
