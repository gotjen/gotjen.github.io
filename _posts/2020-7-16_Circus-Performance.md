---
layout: post
title: Circus Performance
---

In 2019, I went to Snowshoe, West Virginia with my girlfriend Kacy to meet up with my long-time friend Ben Emery ([twit][bentwit], [git][bengh]). For context Ben was on a cross country road trip to start a new life in Albequeque doing stats for the govt. We biked and hiked and things, but most importantly we found the most incredible carnival ride.

We were out driving dirt roads and found a cleared lot in the woods with shipping containers, a lot of trash, and this beautiful hoop ride on a trailer, parked out of sight. It had no locks, no barrier to stop us from trying it, SO WE DID.

[/]: <> (YouTube embedded)
<iframe width="560" height="315" src="https://www.youtube.com/embed/Dk8QO6jE5dA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

This was a blast; you can't help from laughing volently when you step into it an let go. Its not nearly as nauseating as it looks. Once you get over the fear of chopping your hands off in the whirring hoops. I felt like Jodie Foster in Contact

![contact](../images/contact_machine.jpg "Contact [1997]")

It immediately tickled my physics brain. I couldn't go to sleep that night, imagining analytical solutions so some nifty differential equations that would describ...

Okay, so lets break this down. There are three concentric hoops free to rotate on alternating axes (horizontal, vertical, horizontal). If you have ever studied the double pendulum problem, you'll know that this should be very similar to that but with some angular momentum complications. Excitingly, this means the solutions will be [chaotic](https://en.wikipedia.org/wiki/Chaos_theory)! That means everytime you step in, you get a different ride than has ever been experienced.

Its been a while since I brought my physics mechanics fu to bear :bear: on a real problem. To get back into it, I needed to tract the untractable, cut it into a bunch of pieces.

The processes was
1. Remember how to do lagrangian mechanics with a basic pendulum problem
1. Turn the pendulum into a hoop and weight, with all the right moments of inertia.
1. Solve two connected hoops
1. Figure a better coordinate system to make three hoops manageable.
1. Solve 3 hoops (pending)


## 1) Recall: How to physics

First and foremost, this is an exerise where I get to flex my mechanics chops which have atrophied considerably since undergrad.

I am going to throw Lagrangian mechanics at this problem. Lagrangian Mechanics is a clever 19th century way to do physics and end up with the same differential equations that you would with Newtonian Mechanics but with wayy less work. We're going to skip all of the writing of forces and hang the F=ma, and focus on energy. All we have to do to write a Lagranian is write all the energy resevoirs of the system in terms of all the system variables \\(q_i\\) and their derivatives \\(\dot q_i\\).

The Lagranian is the difference between the kinetic energy T and the potential energy V

\\[
\usepackage{mathrsfs}
\mathscr{L} = T - V
\\]

So lets work this through a simple pendulum

![Simple Pendulum]()



[bentwit]:https://twitter.com/dbemerydt
[bengh]:https://dbemerydt.github.io
