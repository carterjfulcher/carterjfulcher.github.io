---
layout: post
title: "Corporation As Code"
date: 2023-08-05 14:01:45 -0700
categories: AI
---

I started writing this article several weeks ago. When proof reading it, I decided it sounded too dreamy and post-modern
so I decided to rewrite it and keep to just the technicals.

<ins>**I want to build a fully autonomous company.**</ins>

# The Vision

At the core, my idea originates from observing how a lot of start ups are run. A single founder or a small group of people,
are more than able to run a full company, leveraging the tools that are available to them. For exmaple, right now I
could go to [Upwork.com](https://upwork.com) and provision a team of 10 software engineers to build a product.

Management and execution takes a lot of time, and is typically what most of a founder's day is allocated to. I have two avenues
I want to explore with this project:

1. **Corporate Execution Engine** - Can I come up with an idea, and have an AI system fufill it? Is there a system that I can build
   that takes text as an input, and outputs an office lease, a team of engineers, a brand, and a distribution channel?

2. **Fully Autonomous Company** - Can an AI come up with a set of company ideas, develope prototypes, run them through a
   market validation process, and then execute on the most promising ideas?

[Infrastructure as Code](https://www.redhat.com/en/topics/automation/what-is-infrastructure-as-code-iac) (IaC) allows you to provision
servers in the cloud with a few lines of code (code -> realworld). My goal, is to make a compiler and runtime for the following code snippet:

```
company {
  name: "BestCRM"
  idea: "A CRM with a focus on small businesses, simply and easy to use"
  team: {
    engineers: 5
    designers: 2
    sales: 2
  }
}
```

# The Implementation

This will probably evolve over time, but I'm not just going to wrap an LLM with a chain

# Roadmap

1. Prototype "Execution Engine"

2. Prototype "Fully Autonomous Company"

3. Write a paper

4. Become world renowned

# Papers

Feel free to send me feedback: [`carter@fulcher.io`](mailto:carter@fulcher.io) or [`@carterjfulcher`](https://x.com/carterjfulcher) on Twitter

- [Tree of Thoughts: Deliberate Problem Solving
  with Large Language Models](https://arxiv.org/pdf/2305.10601.pdf)
