---
layout: post
title: Squash 
category: Portfolio
comments: true
tags:
    - Java
    - ECS
quote:
    quote: There is no mistakes or failures, only lessons.
    author: Denis Waitley
description: JavaFX breakout clone, using ECS pattern.
---

![javaFx breakout clone](/resources/images/squash.gif)

Break out clone written in [JavaFX](http://docs.oracle.com/javase/8/javase-clienttechnologies.htm) using [ECS](http://www.gamedev.net/page/resources/_/technical/game-programming/understanding-component-entity-systems-r3013) - Entity
Component System pattern. This was a coursework for programming module and my first big project. The end result and
the source code can be found on [GitHub](https://github.com/grrinchas/squash). Just `clone` or `fork` to get a copy, then build and run
with [gradle](http://gradle.org/).


##Task

Very simple - make breakout clone in Java. Everything else was left for my own interpretation.

##Requirements

- Destroy brick when it is hit by ball
- Bounce ball from bat
- Bat can not move out of the screen
- Bricks may have resistance

##Reflection

When I started this project I knew only basic things (java syntax, conditional statements, loops, methods and inheritance). But I had to start
somewhere and came across with <a rel="nofollow" href="http://www.amazon.co.uk/gp/product/1484204166/ref=as_li_tl?ie=UTF8&camp=1634&creative=6738&creativeASIN=1484204166&linkCode=as2&tag=dennisgrinch-21">Beginning Java 8 Games Development</a> book. Which gave me pretty good insight to JavaFX. How to setup game loop, timers, asset management etc.
Unfortunately, I was not impressed by the end result. Had few classes with hundredths of lines. My lack of understanding about [OOP principles](https://en.wikipedia.org/wiki/Object-oriented_programming) forced me to read <a rel="nofollow" href="http://www.amazon.co.uk/gp/product/0596007124/ref=as_li_tl?ie=UTF8&camp=1634&creative=6738&creativeASIN=0596007124&linkCode=as2&tag=dennisgrinch-21">Head First Design Patterns</a> book.

I redesigned the project from ground up mostly by decreasing coupling and ended up with more than 15 classes and interfaces. Which was good, had working game and code looked pretty clean. But then I started to think what if I want to extend it, for example add different power ups or make bricks (tomatoes in my case) to move. This will be very hard to accomplish, because I will have to extend my classes deeply, and will end up with too much redundant code. Lucky for me someone introduced me with [ECS](https://en.wikipedia.org/wiki/Entity_component_system) pattern which is very popular in game development.

I redesigned the project from ground up mostly by decreasing coupling and ended up with more than 15 classes and interfaces. Which was good, had working game and code looked pretty clean. But then I started to think what if I want to extend it, for example add different power ups or make bricks (tomatoes in my case) to move. This will be very hard to accomplish, because I will have to extend my classes deeply, and will end up with too much redundant code. Lucky for me someone introduced me with [ECS](https://en.wikipedia.org/wiki/Entity_component_system) pattern which is very popular in game development.
After redesigning again, and by redesigning I mean copying all the project files to new folder, then modifying them, I realized that this is too laborious and
has to be better solution to control all the versions. And this is where I realized importance of [Git](https://en.wikipedia.org/wiki/Entity_component_system) and [GitHub](https://github.com). Of course to learn about it, had to read [Pro Git](https://git-scm.com/book/en/v2) book which was free.

##Conclusion

In the end I had fully working and extensible breakout clone with just one lv. To download source code go to [GitHub](https://github.com/grrinchas/squash)
or run `git clone https://github.com/grrinchas/squash` from your favorite command line tool.
