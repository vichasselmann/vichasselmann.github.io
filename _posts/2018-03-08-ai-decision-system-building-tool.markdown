---
layout: post
title: "AI Decision System in Unity - Part I - Building our Tool"
categories:
  - programming
---

Continuing the series about AI Decision System in Unity that I started on [this post](http://www.vhasselmann.me/programming/2018/03/06/ai-programming-series.html){:target="_blank"}, today we will have an overview of what our tool is and how it looks.

> In this post we will not dig deep into how Considerations and Decisions code works, this content will be available in the next post with practical examples of Gameplay.

To assemble the intelligence behaviours of our game effectively, we will build a tool that will facilitate the process in the long run, and will also help to balance future changes. At this point, all we need are our tool for editing considerations and decisions for our AI Agent, so grab your coffee/tea, roll up your sleeves and get ready! :D

You can find full source code in a [repository](https://github.com/vichasselmann/aidecisionsystem){:target="_blank"} that I created only for this series. The implementation for the editor scripts is already there.

> Reminder: I'm using Unity's version [2017.3.1p3](https://unity3d.com/pt/unity/qa/patch-releases/2017.3.1p3){:target="_blank"}

## Consideration Tool

### This is where it all begins...

# Consideration Inspector

As described in my previous post, the **Consideration Handler** works as a container for all considerations that must be taken into account by the AI Agent when making a decision. To make it easier to manage and give it a more fancy look we are going to create a custom inspector that will look like this:

{:refdef: style="text-align: center;"}
![Consideration Inspector]({{site.baseurl}}/img/aiseries/ConsiderationInspector.png)
{:refdef}

<br />

This will make finding elements, editing them and re-organizing according to our needs easier by clicking **+** and **-** buttons or **dragging** selected element onto the list. Once a consideration is selected you can edit its parameters.

# Decision Inspector

**Decision** contains a weight that dictates how important it is and it will execute **EvaluateScore** to calculate its Action Score. A custom inspector for **Decisions** will also be needed. Our new inspector will look like this:

{:refdef: style="text-align: center;"}
![Consideration Inspector]({{site.baseurl}}/img/aiseries/DecisionInspector.png)
{:refdef}

From this point we will be able to code our behaviours by creating custom C# scripts and inheriting from Consideration and Decision. Still in this series I will show a practical example of how Utility Theory can be used and applied in a game in some simple **GamePlay**.
