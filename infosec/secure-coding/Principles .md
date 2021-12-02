---
description: notes from course (11.1021)
cover: >-
  https://images.unsplash.com/photo-1533709752211-118fcaf03312?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw3fHxjeWJlcnxlbnwwfHx8fDE2MzYyODE0OTQ&ixlib=rb-1.2.1&q=85
coverY: 0
---

# Principles of Secure Coding

## Week 1 Secure Programming Philosophy

> Now, there are many different models for developing software. We won't go into them, other than to say, "For example, Agile is the one that's currently the most popular. It's a form of rapid prototyping. Waterfall model is a more traditional one. It's slower. From the security point of view, Waterfall model is probably the one that will give you the most assurance because you're doing a lot of reviews on the way. With Agile, the focus is getting it out the door quickly, and getting it the way the customer wants. Programs that are developed using Agile programming can be very secure,but typically, the security checking is done after the fact or at the very end whereas with the waterfall model and other many other models, you can build security and as you go along. But regardless of which model you use, of developments you use, underlying all this is the quality of code, the quality of the programming. When programmers program, they make assumptions. They make assumptions about input, output, environment, services they're going to use, and so forth. Herein lies the problem.

> This was so long ago, there was no law in the Netherlands against breaking into computers. So, when they called the Dutch Police, the Dutch police said, "Okay. Let's check him out. Not a spy. We're not interested. Go away." So, how did the police stop the student? United States law doesn't apply in the Netherlands. Every time they tried to block the student, they couldn't get in. The way they stopped him was apparently to, according to the story I heard, to a very junior police officer who just joined the force and was very new. He suggested calling the student's mother and asking her to tell him to stop it. That's exactly what they did and the attacks stopped the next day and never occurred again. But it's a non-technical solution to a technical problem.

> So, where do you find these assumptions? Well, when I was learning to do this stuff, a wonderful mentor, gentleman by the name of Robert Abbott, taught me to look in manuals. He was right. Because when you read the manuals, they'll often say things like can, must, should, will, ought, have to, things like that.

> The easiest way incidentally to make sure this policy is followed, it's simply to remove all networking hardware from the system. If you don't have a wireless card, you can't turn it on. If you don't have ethernet cards or networking cards, you can't connect to a network. That solves the problem right there. Let's go on to the next. For that one, I should add also, you'll have to be careful if people put in USB sticks with wireless on them. So it's also a good idea to remove all networking software as well. So even if they do put something like that in, they won't be able to use it.

## Week 2 Secure Programming Design Principles

> Now, there's another principle that's closely related to this. It's called POLA, Principle of Least Authority. Now, POLA and least privilege are often seen as exactly the same things. However, some people do make a distinction and the distinction is quite interesting. Principle of least privilege speaks to permissions. It says, what can I do to an object, a file? Doesn't say anything about, what can I have you do to it? It's all very direct. What can I do? Authority controls what influence I have over other subjects. So, in other words, can I control how you interact with that object, for example, or can I control how something indirectly affects that file? Play video starting at :2:25 and follow transcript2:25 In that case, authority is slightly different than permission, and so POLA would speak to the authority and least privilege to the permissions. That distinction is not widely recognized yet. So, if you hear POLA and you think least privilege, you're either dead on or very close, close enough for our practical purposes unless you're a system developer.

* Principle of Least Privilege
* Fail-Safe Defaults
* Principle of Economy of Mechanism
* Principle of Complete Mediation
* Separation of Privilege Principle
* Principle of Open Design
* Principle of Least Common Mechanism
* Principle of Least Astonishment

> Now, one other thing, it's popularly believed or widely believed that this means source code should be made public or things should be made public. No. All this is saying is that your security should not depend solely on secrecy of design or implementation. It's perfectly fair to have that as one of a number of characteristics. But you need to be sure that if someone does discover the design or the implementation, that you're so secure. That's all it means.

> The idea here is to hide the complexity that security mechanisms introduce. So as an example, pretty much everyone nowadays is used to typing the password, that's expected. You go to a web server like a bank's web server something or you log in to your home system and it says log in ID and password. But suppose one day it suddenly said login ID, urine specimen for analysis so we can use your DNA to be sure you're who you claim to be. That would be astonishment because that never happens, at least as far as I know, I've never seen it. So that's an example of what Least Astonishment means. Don't surprise the user is the bottom line.

## Week 3 Robust Programming

> During this module, we will see examples of incorporating paranoia, stupidity, dangerous implements, and magical thinking, and what happens when you don't use them. It should be fun. At the end of this lesson, you'll be able to explain the issues that can arise from fragile programming. You'll also be able to discuss how design issues drive implementation and distinguish between robust and fragile code. Finally, you'll be able to explain what can go wrong in fragile code and write a robust version of fragile code.

> Robust programming has four principles. The first one is paranoia. Basically if you don't generate something, don't trust it. Assume that they really are out to get you.
>
> The second one is stupidity, which we already mentioned. Basically, assume the user hasn't read anything, including how to use the program, and handle cases where the program is just completely misused, where rather the inputs are bad or things like that.
>
> The third part is dangerous implements. Don't hand out dangerous implements.

> The lessons from this. Always check that the perimeter refers to a valid data structure and design your parameters so you can do this. Second, when you delete information, clean up. Overwrite it with no bytes if it's a string, make it null If it's a pointer, and so forth. That way it'll prevent errors later on because if you try to use it or reference it, you're either going to get an error or the program will crash and believe me it's a lot better for the program to crush and go on functioning correctly.

## Week 4 Methods for Robustness

> Formal methods for our purposes have three things. You begin with the specification of what the program is to do. You then design it and show that your design matches the specifications, or satisfies the specifications, to use the correct term. You then do the implementation and show that your implementation satisfies the design. By transitivity, that means it satisfies the specifications. Now, there is more beyond this, deployment and so forth, where formal methods really become somewhat informal themselves. So we're not going to focus on them because now we're only focusing on the program.

> In the 1960s when IBM 360s were being marketed, people found all sorts of undocumented instructions and relied on them. But the undocumented instructions were also often changed in the next release, because since they were undocumented, IBM said, well, we can make these meaningful and do something or changed them and have something documented and it messed up a lot of people. Same thing here. If the library functions descriptions don't say it does something, don't assume it does it.
