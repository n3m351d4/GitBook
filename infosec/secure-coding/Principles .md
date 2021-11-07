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

