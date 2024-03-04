---
layout: post
title: Circus Performance
---

In 2019, I went to Snowshoe, West Virginia with my girlfriend Kacy to meet up with my long-time friend Ben Emery ([twit][bentwit], [git][bengh]). For context Ben was on a cross country road trip to start a new life in Albequeque doing stats for the govt. We biked and hiked and things, but most importantly we found the most incredible carnival ride.

We were out driving dirt roads and found a cleared lot in the woods with shipping containers, a lot of trash, and this beautiful hoop ride on a trailer, parked out of sight. It had no locks, no barrier to stop us from trying it, SO WE DID.

![[circus_screenshot01.png]]

This was a blast; you can't help from laughing violently when you step in it and let go. Its not nearly as nauseating as it looks. Once you get over the fear of chopping your hands off in the whirring hoops. I felt like Jodie Foster in Contact

![[contact_machine.jpg]]
It immediately tickled my physics brain. I couldn't go to sleep that night, imagining analytical solutions so some nifty differential equations that would describe the motion of a triple pendulum...

*NERD ALERT* if you just came here for sexy, mesmerizing animations [skip ahead](#theJuice)

Okay, so lets break this down. There are three concentric hoops free to rotate on alternating axes (horizontal, vertical, horizontal). If you have ever studied the double pendulum problem, you'll know that this should be very similar to that but with some angular momentum complications. Excitingly, this means the solutions will be [chaotic](https://en.wikipedia.org/wiki/Chaos_theory)! That means every time you step in, you get a different ride than has ever been experienced.

Its been a while since I brought my physics mechanics-fu to bear :bear: on a real problem. To get back into it, I needed to tract the untractable, cut it into a bunch of pieces.

The processes was
1. Remember how to do lagrangian mechanics with a basic pendulum problem
1. Turn the pendulum into a hoop and weight, with all the right moments of inertia.
1. Solve two connected hoops
1. Figure a better coordinate system to make three hoops manageable.
1. Solve 3 hoops (pending)


## 1) Recall: How to physics

First and foremost, this is an exerise where I get to flex my mechanics chops which have atrophied considerably since undergrad.

I am going to throw Lagrangian mechanics at this problem. Lagrangian Mechanics is a clever 19th century way to do physics and end up with the same differential equations that you would with Newtonian Mechanics but with wayy less work. We're going to skip all of the writing of forces and hang the F=ma, and focus on energy. All we have to do to write a Lagranian is write all the energy resevoirs of the system in terms of all the system variables $$q_i$$ and their derivatives $$\dot q_i$$.

The Lagranian is the difference between the kinetic energy T and the potential energy V

So lets work this through a simple pendulum

[/]:<> (![Simple Pendulum](../images/circus/simplependulum.jpg)

### Newtonian Mechanics
Lets remember the way we hit this with Newtonian Mechanics. There is one parameter to concern our selves with, $$\phi$$, the angle of the pendulum. The only force involved is gravity, acting in the $$-\hat z$$ direction. The force on the mass, $$m$$ on the end of a string, $$L$$ meters long is $$-mg\sin{\phi}$$. The acceleration of the pendulum in the $$\hat\phi$$ direction is $$L \ddot\phi$$. Newton's first (?) law says $$-mg\sin{\phi} = mL\ddot\phi$$. Shockingly, the mass cancels and we have

$$ \ddot\phi = \frac{g}{L}\sin{\phi} $$

We know the solutions to this equation when the angle is small so that $$\sin{\phi} \approx \phi$$. The pendulum is a simple harmonic oscillator with frequency $$\omega = \sqrt{g/L}$$ and the motion goes like $$\phi{t} = A\sin{\omega t}$$. This works as long as the amplitude A follows the same small angle condition above. The period of the motion is constant for any amplitude, which why a pendulum is a great time keeping mechanism [provided you're not moving](linkToLongitude). 

## Meet Lagrange, the mechanic
![Lagrange](../images/circus/lagrange.png)
Alright, so this is pretty easy for a simple pendulum. Why complicated it with some new-fangled artillery. Well, once we have multiple coupled axes. The kinetic energy of the pendulum is $$T = \frac{1}{2}mV^2 = \frac{1}{2}mL^2\dot\phi^2$$. The potential energy due to gravity is $$V = mgh = mgL(1-\cos\phi)$$. Since we're free to choose the datum, or zero-point for the potential energy, we can toss the constant and just have $$V = -mgL\cos\phi$$. Our lagrangian comes together

$$L = T - V = \frac{1}{2}mL^2\dot\phi^2 + mgL\cos\phi$$

to find the equations of motion described by the Lagrangian we throw the Euler-Lagrange at it.

$$\frac{\partial L}{\partial q} - \frac{\,d}{\,d t} \left( \frac{\partial L}{\partial \dot q} \right) = 0$$

For ever system parameter, $$q$$, we have a differential equation. In this case, the whole system is described by the one free parameter $$\phi$$. Notice that derivatives w.r.t. $$q$$ and w.r.t $$\dot q$$ are treated differently, as though they are completely different paramters.

For our simple pendulum the $$\frac{\partial L}{\partial q}$$ partial derivative acts only on the potential energy, V part of the lagrangian, and $$\frac{\partial L}{\partial \dot q}$$ acts only on the kinetic energy, T part, after which we apply a total derivative with respect to time. This simply turns the angluar velocity $\dot\phi$ into an angular acceleration $\ddot\phi$. We end up with this equation.

$$-mgL\sin\phi - mL^2 \ddot\phi = 0$$

Which simplifies to the same equation we got from Newtonian Mechanics!! So we can be confident that this Lagrangian business works so long as we're meticulous about describing all the kinetic and potential energies in terms of the system variables.

## 2) Make that Pendulum into a Hoop
I am going to move in steps to morph the simple pendulum problem into the full treatment of the triple pendulum circus contraption. Lets start by turning the pendulum into a hoop. This will involve trading the point mass for an extended body: a hoop. While we're at it, we might as well formalize all the constants for the triple hoop case. At first blush, we could say all the hoops have the same radius and the same mass. Animating three hoops of the same size would look dorky if they're passing through each other. Maybe we just fake a larger radius for the outer hoop and a smaller radius for then inner hoop. That would look better, but those hoops are made of the same steel, a larger radius hoop should have a larger mass proportional to $$\frac{r}{r_0}$$. To solve this, I define a linear hoop density $$\lambda_hoop$$, then the mass of each hoop with radius r will be $$M_i = 2\pi R_i \lambda$$. You may think this is unnecessarily complicated, but supposing we want to analyse the characteristic time of the circus system from the video later on - its a correction that will mater.

I am using $$\lambda_hoop = 5$$ `kg/m` $$R1 = 2$$ `m`, $$R2 = 2.2$$ `m` and $$R3 = 2.4$$ `m`. The thickness of the hoop is $$r_hoop = 0.1$$ `m`, but this only matters for drawing purposes and won't figure into the physics. Notice that the hoops fit perfectly inside one another.

Remember, we're solving for the motion of a single pendulum right now, so lets just say the inner pendulum $$\phi_1$$ is allowed to move, and the outer two are held fixed, $$\phi_2 = \phi_3 = 0$$. When we turn the hoop into a pendulum, we inadvertently balance all the mass evenly about the center of rotation. This means that the potential energy V goes to 0. Too see the effect of this, notice that the simple pendulum equation becomes $$\ddot phi = 0$$, in other words the hoop will spin at a constant velocity. In reality, there is a human in the hoop, slightly off center. Lets assign the human a moment of interia $$I_h$$ and we can decide what is a reasonable value for this. Really, we still have something similar to our original pendulum, we just wrapped a hoop around it - redistributed the mass slightly. The hoop doesn't change the potential energy as it rotates because it center of mass and center of rotation are coincident. The potential energy is now, $$-mgR_h\cos\phi_1$$ where $$R_h$$ is the distance from the hoop center to the human's center of mass. The human rod is the new plumb-bob of the system - the mass that is lifted and lowered when the hoops rotate. 

The kinetic energy retains the origial term for the moving point mass, but in addition we need the rotational kinetic energy of the hoop, which involves the moment of inertia I. [Consulting the internet]("wikipediaMomentsOfInertia") we find the moment of inertia of a hoop about its major diameter. Applying this to all our hoops we can define $$I_i = \frac{1}{12}M_iR_i^2$$. The kinetic energy of the system is now $$T = \frac{1}{2}mR_h^2\dot\phi^2 + \frac{1}{2}I_1\dot\phi^2 = \frac{1}{2}\dot\phi^2 ( mR_1^2 + I_1 )$$

## 3) Add a Hoop in your Hoop

## 4) Better Coordinates

## 5) Add a Hoop in the Hoop in your Hoop


[/]:<> (link to the meat)
<a name="theJuice"></a>

[bentwit]:https://twitter.com/dbemerydt
[bengh]:https://dbemerydt.github.io
