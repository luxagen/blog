---
title: Approaches to refactoring, technical debt and legacy code
layout: post
---

Sometimes a codebase has an overwhelming amount of "terrible" stuff that as a developer you almost can't help but just diving in and fixing it. Doing this without thinking too hard can result in many variations of failure, such as:

* Upsetting the people paying you because you are doing something they don't consider important.
* Never finishing the mammoth rewrite you took on, resulting in a mess of two different styles of code.
* Personal burnout.
* Fixing things that maybe weren't actually that important (even if they make you cry whenever you have to look at their source code), at the expense of failing to fix more urgent problems or building useful features.

## Must-read articles

Here's a couple of must-read articles on the subject:

### Refactoring


> "We take the next feature that we are asked to build, and instead of detouring around all the weeds and bushes, we take the time to clear a path through some of them."
>
> ~ Ron Jeffries

In short, don't make debt cleanup its own task, just tackle what gets in your way as you go, and leave the camp tidier than you found it.

I recommend reading the whole article here: [Refactoring -- Not on the backlog! by Ron Jeffries](https://ronjeffries.com/xprog/articles/refactoring-not-on-the-backlog/), it's not too long.

### Technical Debt

> "Tech debt": an overloaded term. There are at least 5 distinct things we mean [when] we say “technical debt”.
>
> 1. Maintenance work
> 2. Features of the codebase that resist change
> 3. Operability choices that resist change
> 4. Code choices that suck the will to live
> 5. Dependencies that resist upgrading

Read the full explanation of each type of debt here: [Towards an understanding of technical debt by Kellan Elliott-McCrea](http://laughingmeme.org/2016/01/10/towards-an-understanding-of-technical-debt/), a bit longer but an important piece of writing.

## Huge piles of debt

If your project is sooo "bad" that you feel like throwing it out, you might do well to heed this quote:

> "There is only one way to eat an elephant: a bite at a time."
>
> ~ [Desmond Tutu (via Psychology today of all places!)](https://www.psychologytoday.com/us/blog/mindfully-present-fully-alive/201804/the-only-way-eat-elephant)

Which to me means iterate your way to good.

It doesn't mean have no plan, in fact you should know where you are trying to get to and how realistic that is so you don't end up with a rewrite that can never be completed because it was too ambitious.

## Even more reading

### Is a mess a debt?

> "A mess is not a technical debt. A mess is just a mess."
>
> ~ Uncle Bob

[A Mess is not a Technical Debt by Uncle Bob](https://sites.google.com/site/unclebobconsultingllc/a-mess-is-not-a-technical-debt) suggests that the use of the term debt is for a considered short term trade-off just like taking out a loan, but a mess is nothing of the sort as there is no up-side to creating a mess versus having a clean but temporary solution to a problem.

### More taxonomy of bad code: the Reckless/Prudent vs Deliberate/Indavertent quadrant

Martin Fowler shows that you can categorize debt by whether it is reckless or prudent, and separately whether it was deliberately or inadvertently added to the code.

> "Technical Debt is a metaphor, so the real question is whether or not the debt metaphor is helpful about thinking about how to deal with design problems, and how to communicate that thinking. A particular benefit of the debt metaphor is that it's very handy for communicating to non-technical people."
>
> "The useful distinction isn't between debt or non-debt, but between prudent and reckless debt ... there's also a difference between deliberate and inadvertent debt."
>
> "The decision of paying the interest versus paying down the principal still applies, so the metaphor is still helpful for this case."
>
> ~ [The Technical Debt Quadrant by Martin Fowler](https://martinfowler.com/bliki/TechnicalDebtQuadrant.html)

These are useful definitions that can help clearly communicate complex issues with the code to people not directly involved in the code.

It's well worth reading much more of Martin Fowler's writing. There's so much to learn from Martin about programming good practice; the articles are all written in a very human and accessible style.

### Legacy tests

Now that TDD is widely adopted we're faced with cleaning up the output of those who [cargo-culted](https://en.wikipedia.org/wiki/Cargo_cult_programming) test coverage, or had well meaning but ultimately badly formed attempts at adding tests to their code, sometimes with prodigious volumes of test code.

I think "Legacy tests" is a useful term to describe these problematic tests. Nat Pryce outlines some useful hints that you have legacy tests on your hands:

> "Symptoms of legacy tests I have encountered include:  
> ...  
> Tests are named after issue identifiers in the company's issue tracker.  
> Bonus points if the issue tracker no longer exists."
> 
> ~ Nat Pryce, [Working Effectively with Legacy Tests](http://natpryce.com/articles/000813.html)

## The audio version from Codurance

[Codurance](https://codurance.com/) hosted an insightful round-table podcast episode with a group of people who are clearly very experienced. You can listen here: <https://codurance.com/podcasts/2019-01-21-legacy-code/> and will doubtless be inspired by some things in there. The conversation takes a little while to build momentum but it's worth the wait.

Here some highlights of what I learnt from listening to the show:

### The Book

* [Working Effectively with Legacy Code by Michael C. Feathers](https://www.amazon.co.uk/Working-Effectively-Legacy-Michael-Feathers/dp/0131177052/) is *the* book to read on the subject.

Feathers defines legacy code as code without tests.

> "Preserving behaviour is a large challenge. When we need to make changes and preserve behaviour, it can involve considerable risk."
>
> ~ Michael C. Feathers

This book is heavily geared towards unpicking the difficulties in getting untested code under test in object-oriented static typed languages such as Java, C# and C++, and modifying it safely.

### Named techniques and related libraries

* [Characterization testing](https://michaelfeathers.silvrback.com/characterization-testing) is the idea of creating tests to probe and demonstrate the existing behaviour of previously untested code.
	* Related to this is "approval tests" which allow you to easily incorporate snapshots of output (json, xml, logs etc) into your tests in order to capture existing behaviour and be able to spot any variations that pop up during refactoring.
	* [ApprovalTests.net](https://github.com/approvals/ApprovalTests.Net) is a dotnet library for implementing approval tests.
* Introducing [seams](http://wiki.c2.com/?SoftwareSeam) into software can be a useful technique for breaking down untestable monoliths into testable chunks on the way to better code.
* [Mutation testing (wikipedia)](https://en.wikipedia.org/wiki/Mutation_testing) (more info on [mutation testing at csharp academy](http://csharp.academy/mutation-testing/)) is a useful way of checking how good your test coverage really is. It is the idea of making (almost) random changes to the code under test to see what whether your tests spot the change in behaviour.
	* For dotnet this can be done with [Stryker.net](https://github.com/stryker-mutator/stryker-net)

### Approaches from hard-won experience

* Make as few changes as possible to get untested production code under test. The first cut of tests will likely be fragile.
* It's more important that legacy code that is already in production continues to behave as it currently does than that it behaves as originally specified. People and downstream systems may now rely on that "incorrect" behaviour.
* Does the organisation (culture, systems, pressures etc.) cause bad code to be created? If you don't fix that then you will always get more "legacy" code.
* The importance of competent technical leadership within an organisation for preventing the build up of catastrophic levels of technical debt.
* When communicating, quantify the cost of problems with the legacy code. E.g. "you are losing 1 in 5 developer-days to coping with bugs introduced due to the lack of automated regression tests".
* Have the hard conversations with the business about the cost of fixing the mess.
* Doing a rewrite is (almost) always the wrong answer.
* Get small wins, even if you are facing a huge challenge.

## Maybe modelling is the problem

Something that looks like bad code could be that way because of a failure to properly model the real world.

Domain Driven Design (DDD) has much to teach on the matter, and this is a great video on the why modelling could be the problem:
[Technical debt isn't technical - Einar Høst - DDD Europe 2019 - YouTube](https://youtu.be/d2Ddo8OV7ig)

## Thanks, now I'm even less sure what to do

Are you wrestling with something you don't like in a codebase you have to deal with? I'm guessing that's why you're here. Or maybe someone sent you this along with a rant about the tech debt in the codebase you own.

Either way, I suggest slowing down a bit, sitting down together (while maintaining social distancing), and considering how all the things that bother you about the code in front of you fit in to the various taxonomies detailed above. Then use that assessement to make a calm and rational plan about roughly where you want to get to. Then decide on the *one* next thing you will do towards that. Use the Ron Jeffries approach to get you there without wasting time on things that don't really matter.

Practically, that might mean if you have a user story, ticket or trello card for a feature, it might take longer to do as you include the work to "pay down" some of that badness along the way, knowing that it will improve your overall velocity over time. Be wary of pulling out separate "debt" stories to do, though that can work if the team dynamic is right.

To keep your stress levels down make it a shared team problem, have a bit of a laugh about it, take regular breaks in the great outdoors, and support each other.

Good luck!

I'll leave you with this little song I found on youtube about bugs:

<iframe width="560" height="315" src="https://www.youtube.com/embed/kuJI4hmvY8c" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Further Reading

If you want to avoid the task at hand by reading the whole internet in the hope that it will help, start here:

* A lengthy set of opinions on what to do with a huge 10 year old untested pile of spaghetti code:  <https://softwareengineering.stackexchange.com/questions/416242/is-it-the-correct-practice-to-keep-more-than-10-years-old-spaghetti-legacy-code>

* I'm still digesting some great content from this podcast episode: [Software Delivery in Small Batches: Automated Testing with Jason Swett](https://share.transistor.fm/s/a5ca21cb) which talks a lot about practical approaches to legacy code, including the idea that you actually can't always avoid writing some more untested code because of the cost-benefit trade-off.
