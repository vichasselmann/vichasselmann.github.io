---
layout: post
title: "AI Decision System in Unity"
categories:
  - programming
---

## Introduction

This post is the first of a series about build a better Artificial Intelligence Decision System in Unity. I'm not an expert in AI, but during the development of [Conflict 0: Revolution](http://www.vhasselmann.me/games/revolution){:target="_blank"} we came up with an interesting component and I would like to share what I learnt. Conflict 0: Revolution is a 2D + 3D turn based strategy combat game. It tells the story of a civil war in the country of Navaha, and the revolutionary army tries to take over the government.

There are different characters and classes in this game with specific characteristics in their behavior. So it was necessary to create a safe environment so that the design team could balance and create unique intelligences for each situation.

In this article, I will give an overview about data structure and architecture design (no code this time) used while building components, but in the following articles in this series I will focus mostly on building these components.

Almost all implementation work regarding the character decisions was done by me and **Marcos Castro**. The whole design would not really be possible without brainstorming with him and **Anderson Campus Cardoso**, who also made some improvements in the AI System later on.

# Prologue - AI in Video Games

I found an interesting article about [Artificial Intelligence application](http://sitn.hms.harvard.edu/flash/2017/ai-video-games-toward-intelligent-game/){:target="_blank"} in Video Games written by Harbing Lou on Harvard University's site, so if you are not familiar with AI's concept, read it first and come back here.

## Architecture

The architecture presented is a simplified version based on [Dave Mark & Mike Lewis session at GDC 2015](https://www.gdcvault.com/play/1021848/Building-a-Better-Centaur-AI){:target="_blank"} *Building a Better Centaur: AI at Massive Scale*. After watching that I decided to incorporate some of the ideas into our own system.

Gladly this architecture and concept can be used in every game that contains AI elements, so feel free and secure to explore its uses whenever you find useful for your game.

# The Hierarchy

One of the most important components in this architecture is the **Infinity Axis Utility System**. But what is **IAUS**?

In a nutshell it is a system that returns an **Action** to perform with the highest value. It's basically a list of Actions and each Action has a list of "Axis" (which we will call as Considerations in our future code).
{:refdef: style="text-align: center;"}
![Action Diagram Model]({{site.baseurl}}/img/aiseries/AIAction.png)
{:refdef}

<br />
So, we have **Considerations** in a list and each of these Consideration has its input and parameters. We might have as many as these we want or we judge necessary and then we will combine their output by multiplying together to get a score for that Action.

<br />
> To have an in-depth understanding in how IAUS works, I hardly recommend watching [this session](https://www.gdcvault.com/play/1018040/Architecture-Tricks-Managing-Behaviors-in){:target="_blank"}

> Also I hardly recommend watching [this session](https://www.gdcvault.com/play/1012410/Improving-AI-Decision-Modeling-Through){:target="_blank"} to understand how Utility Theory works and how it helps to improve AI Decision Modeling

<br />
# Overview

So before we go straight to code, let's get an overview of our system design. The figure below demonstrates what our Class Diagram will looks like once we code our algorithm for AI decision making.

{:refdef: style="text-align: center;"}
![Action Diagram Model]({{site.baseurl}}/img/aiseries/ClassDiagram.png)
{:refdef}

As you can see the components themselves are relatively simple and it is also simple implementing them.

- **Intelligence** (which works as a brain) is our main component and it holds all decisions from the AI Agent in a list. **GetHighestDecision** will return the choosen **Decision**, so our agent can behave according to the most valuable action during execution.
- **Decision** contains a weight that dictates how important it is and it will executes **EvaluateScore** to calculate Action Score.
- **Consideration Handler** works as a container for all considerations that must be taken into account by the AI Agent when making that decision.
- **Consideration** is the basic component that contains input and parameters.

# Where to now?

This was a brief introduction to AI Decision System. The next post in this series will be related on how-to apply this theory in **Unity**. We will also start coding our AI Decision System Tool and probably work in some prototype to demonstrate its usage.

That's all, folks!
