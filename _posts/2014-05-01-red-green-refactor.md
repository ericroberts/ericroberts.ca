---
layout: post
title: Red, Green, Refactor
---

Ever since DHH's keynote at Railsconf 2014 and his subsequent blog post [TDD is dead. Long live testing.](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html), I think it would be fair to say there has been a lot of discussion about Test Driven Development (TDD). Uncle Bob responded to DHH's original post with [Monogamous TDD](http://blog.8thlight.com/uncle-bob/2014/04/25/MonogamousTDD.html). A few posts from each followed, which I'll just list out here:

- [Test-induced design damage](http://david.heinemeierhansson.com/2014/test-induced-design-damage.html) - DHH
- [Slow database test fallacy](http://david.heinemeierhansson.com/2014/slow-database-test-fallacy.html) - DHH
- [Test Induced Design Damage?](http://blog.8thlight.com/uncle-bob/2014/05/01/Design-Damage.html) - Uncle Bob
- [When TDD doesn't work.](http://blog.8thlight.com/uncle-bob/2014/04/30/When-tdd-does-not-work.html) - Uncle Bob
- [Professionalism and TDD](http://blog.8thlight.com/uncle-bob/2014/05/02/ProfessionalismAndTDD.html) - Uncle Bob

A number of other people, well known and not so well known, have also written their thoughts on the topic. Suffice it to say, the development world (or at least the Ruby community) seems to be abuzz with thoughts on TDD. I figured I may as well jump on the bandwagon myself, because hey, bandwagons are fun right?

One thing I've noticed about all of these posts, forum discussions, and tweets, is that there is a lot of focus on the speed of testing and the importance of decoupling. I must admit to being confused by this focus. TDD, as it was explained to me, is pretty simple:

1. <span>Write a failing test</span>
2. <span>Write some code that passes the test</span>
3. <span>Refactor</span>

Or, as the title of this post says, "red, green, refactor". Notice how it doesn't say anything about speed? Or whether the tests end up hitting the database? It doesn't talk about loading Rails in order to run your tests, or the difference between stubs, fakes, or mocks. It just says three simple things: write a test for some new functionality, write code that implements that functionality (thereby passing the test), and then refactor that code.

To be fair, the definition according to Wikipedia does sorta kinda mention speed if you twist the words around a bit:

> Test-driven development (TDD) is a software development process that relies on the repetition of a very short development cycle: first the developer writes an (initially failing) automated test case that defines a desired improvement or new function, then produces the minimum amount of code to pass that test, and finally refactors the new code to acceptable standards.
>
> <cite>[Test-driven development](http://en.wikipedia.org/wiki/Test-driven_development) on Wikipedia</cite>

It doesn't really say whether 4.5 minutes for your whole test suite is too long, or if you should be striving for times under 1 second. In its more detailed explanation of the steps it does say that you should be running *all* the tests before and after.

So where did all this preoccupation with speed and decoupling come from? Well, it certainly seems important that if you have to run all your tests every time you add a new one, you don't want to be waiting around too long for that to happen, it'll just slow you down. But all the focus on the amount of time that is right seems to me to be damaging the discussion. If DHH is fine with waiting 4.5 minutes for his test suite to run, who cares? I think it's pretty awesome that he has thousands of assertions; most projects I see don't have anything close to that.

DHH also makes a point in his [Slow database test fallacy](http://david.heinemeierhansson.com/2014/slow-database-test-fallacy.html) post about not running all your tests everytime you add a new one, and one that I think is reasonable:

> You might think, well, that's pretty fast for a whole suite, but I still wouldn't want to wait 80 seconds every time I make a single change to my model, and want to test that. Of course not! Why on earth would you run your entire test harness for every single line change in a particular model? If you have so little confidence in the locality of your changes, the tests are indeed telling you that the system has overly high coupling.

But in [Test induced design damage](http://david.heinemeierhansson.com/2014/test-induced-design-damage.html) he criticizes BDD for still being a "test-first" approach:

> At this point BDD proponents might well argue that, yes, testing units is not what we should be doing. We should be going outside-in. But as long as that's also done under the test-first regime, I don't think it generally help matters much. It still leads down a road of excessive mocking and artificial boundary installations.

I don't see why the simple act of testing first would lead down a road of excessive mocking. Testing first says nothing about how I write my test or the other parts of my code. By testing first, you are able to ensure that everything works as you think it should. Sure, you might miss something, but then you can always write a failing test for that before fixing it. That's the heart of what we call Test Driven Development.

But TDD is just a name. Names are useful, because they help us have discussions about rather complicated topics. If you know what Object Oriented Programming is, and I know what Object Oriented Programming is, we can have a discussion about it without having to stop to define every detail along the way*.

But Names can also be harmful. If you "know" what Object Oriented Programming is, and I "know" what Object Oriented Programming is, but our understanding of those two things differs greatly, our discussion will be rather difficult and confusing to follow. We're both assuming that the other has the same ideas in their head, which makes it hard to communicate effectively.

While reading [Sandi Metz](http://www.sandimetz.com/)'s book [Practical Objected Oriented Design in Ruby](http://www.sandimetz.com/poodr/), I noticed something interesting about the way she explained things. She doesn't tell you what things are called until after she's already explained the concept and shown you how to do it. So if you came into her book with the idea that polymorphism is confusing, you don't have time to get your guard up when she teaches you about it, since she doesn't tell you until after. You don't get time to project your own understanding of those concepts before she has a chance to show you how they work. It's a great way of explaining things, and one I'd like to try when I get the chance.

So what does this have to do with TDD? Well, TDD is a name that means a lot of things to a lot of people. To me, it means red, green, refactor. Everything else just helps me do that better.

Maybe when we have discussions about TDD, we should state what our definition of TDD is. If we found out we were all arguing about different things, maybe we could find some common understanding and begin our discussion there. Our first test might fail, but we can figure out how to make it pass and refactor from there.

<small>*However, growing a language can be fun. If you haven't already watched [Growing a Language](https://www.youtube.com/watch?v=_ahvzDzKdB0) by Guy Steele, you should! It's a great talk.</small>
