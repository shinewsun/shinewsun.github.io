---
layout: page
title: Projects
---

(Major projects took days, weeks, or months while minor projects were one day or less.)

## Major Projects

---

<img src="/assets/circlepondbot.png" width="200px" align="left" style="margin: 20px 20px 0 0"/>
### CirclePondBot
A Discord.py bot which uses TensorFlow to "see" if the jets in UW's Drumheller Fountain are on.

Source code can be found at <https://github.com/WWRS/circlepondbot>.

[Invite the bot to your server here.](https://discord.com/api/oauth2/authorize?client_id=602156268551143424&permissions=51200&scope=bot)

<br clear="left">

<img src="/assets/visualizer.png" width="200px" align="left" style="margin: 10px 20px 0 0"/>
### [UW Game Development Club Visualizer](https://github.com/uw-gamedev-visualizer/uw-gamedev-visualizer)
Polls live audio playing through the speakers and displays various visualizations.

If you've been to a club meeting in-person since 2019, this was probably on the screen before the start of the talk!

Add your own visualizer and submit a pull request or contact me on Discord, `@RShields#5160`.

<br clear="left">

<img src="/assets/dubhacks_logo.png" width="200px" align="left" style="margin: 10px 20px 0 0"/>
### [Fix the MyPlan Crashes](https://devpost.com/software/fix-the-myplan-overloads)
(Presented at DubHacks 2019)

This is an innovative solution to a problem with the University of Washington's online registration system: consistent overloads.

Source code can be found at <https://github.com/WWRS/Fix-the-MyPlan-Crashes/blob/master/main.rb>.

<br clear="left">

## Minor Projects

---

<img src="/assets/towers_hanoi.png" width="200px" align="left" style="margin: 10px 20px 0 0"/>
### [Towers of Hanoi](https://rshieldsprojects.github.io/projects/towers_hanoi/)

November 2021

The [Towers of Hanoi](https://en.wikipedia.org/wiki/Tower_of_Hanoi) problem is a classic in computer science. I created a visualization of a few algorithms to solve it:

- Classic recursion
- Breadth-first search
- Depth-first search
- Q-learning

<br clear="left">

<img src="/assets/sudoku.png" width="200px" align="left" style="margin: 10px 20px 0 0"/>
### [Probabilistic Sudoku Solver](https://rshieldsprojects.github.io/projects/sudoku.html)

July 2021

There are many strategies for computer-solving sudoku puzzles. This project takes a novel approach: For each square, update the probability of it being a given number by looking at the probability of that number not appearing in any other square of its row, column, or subgrid. Perform this update repeatedly until every probability converges to 0 or 1.

This approach doesn't always converge to a solution, but it converges very quickly for most puzzles, regardless of how difficult the puzzle is for humans or other approaches.

<br clear="left">

<img src="/assets/quadtree.png" width="200px" align="left" style="margin: 10px 20px 0 0"/>
### [Quadtree](https://rshieldsprojects.github.io/projects/quadtree.html)

December 2020

[Quadtrees](https://en.wikipedia.org/wiki/Quadtree) are used to optimize 2-dimensional collision detection and to compress images.

This is a simple implementation of a quadtree, counting how many particles are in each branch. The "fuller" the branch, the redder its border.

<br clear="left">

<img src="/assets/orbit.png" width="200px" align="left" style="margin: 10px 20px 0 0"/>
### [Orbit](https://rshieldsprojects.github.io/projects/orbit.html)

December 2020

A simulation of a planet's orbit. Move the slider to change the eccentricity of the orbit.

<br clear="left">

<img src="/assets/triangles.png" width="200px" align="left" style="margin: 20px 20px 0 0"/>
### [Triangles](https://rshieldsprojects.github.io/projects/triangles.html)

July 2020

The [mice problem](https://mathworld.wolfram.com/MiceProblem.html) asks, if a mouse starts at each vertex of a regular *n*-gon, then each travels toward the next mouse over, what paths will they trace?

This project answers that for triangles. Move the slider to change the granularity of the simulation.

<br clear="left">
