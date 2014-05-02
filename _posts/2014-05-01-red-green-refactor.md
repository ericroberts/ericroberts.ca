---
layout: post
title: Red, Green, Refactor
---

![Red, Green, Refactor](/images/posts/red-green-refactor.jpg)

Ever since DHH's keynote at Railsconf 2014 and his subsequent blog post [TDD is dead. Long live testing.](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html), I think it would be fair to say there has been a lot of discussion about Test Driven Development (TDD). Uncle Bob responded to DHH's original post with [Monogamous TDD](http://blog.8thlight.com/uncle-bob/2014/04/25/MonogamousTDD.html). A few posts from each followed, which I'll just list out here:

- [Test-induced design damage](http://david.heinemeierhansson.com/2014/test-induced-design-damage.html) - DHH
- [Slow database test fallacy](http://david.heinemeierhansson.com/2014/slow-database-test-fallacy.html) - DHH
- [Test Induced Design Damage?](http://blog.8thlight.com/uncle-bob/2014/05/01/Design-Damage.html) - Uncle Bob
- [When TDD doesn't work.](http://blog.8thlight.com/uncle-bob/2014/04/30/When-tdd-does-not-work.html) - Uncle Bob
- [Professionalism and TDD](http://blog.8thlight.com/uncle-bob/2014/05/02/ProfessionalismAndTDD.html) - Uncle Bob

A number of other people, some well known and some not so well known, have also written down their thoughts on the topic. Suffice it to say, the development world (or at least the Ruby community) seems to be abuzz with thoughts on TDD. I figured I may as well jump on the bandwagon myself, because hey, bandwagons are fun right?

One thing I've noticed about all of these posts, forum discussions, and tweets, is that there is a lot of focus on the speed of testing and the importance of decoupling. I must admit to being confused by this focus. TDD, as it was explained to me, is pretty simple:

1. <span>Write a failing test</span>
2. <span>Write some code that passes the test</span>
3. <span>Refactor</span>

Or, as the title of this post says, "red, green, refactor". Notice how it doesn't say anything about speed? Or whether the tests end up hitting the database? It doesn't talk about loading Rails in order to run your tests, or the difference between stubs, fakes, and mocks. It just says three simple things: write a test for some new functionality, write code that implements that functionality (thereby passing the test), and then refactor that code.

While many discussions have revolved around speed, DHH seems to also not agree with the test-first approach in general, because in [Test induced design damage](http://david.heinemeierhansson.com/2014/test-induced-design-damage.html) he criticizes BDD for being such an approach:

> At this point BDD proponents might well argue that, yes, testing units is not what we should be doing. We should be going outside-in. But as long as that's also done under the test-first regime, I don't think it generally help matters much. It still leads down a road of excessive mocking and artificial boundary installations.

From my point of view, I don't see why the simple act of testing first would lead down a road of excessive mocking. By itself, testing first says nothing about how I write my test. The point of testing first is to focus on the task at hand. What functionality am I implementing? What am I *not* implementing? Once I've figured out what I'm doing, I can get there the quickest way I know how, and finally, refactor that code to something I'm happy with. It's not unlike writing. First you create an outline, then you write your first draft, and finally you polish it until you're happy with it.

So where did all this preoccupation with speed and decoupling come from? Nothing in the steps we've talked about so far says whether 4.5 minutes for your test suite is too long or if you should be striving for times under one second. However, Wikipedia does have a fuller outline of the steps than what I've presented. Let's take a look (emphasis mine):

> 1. <span>Add a test</span>
2. <span>Run *all* tests and see if the new one fails</span>
3. <span>Write some code</span>
4. <span>Run *all* tests</span>
5. <span>Refactor code</span>

> <cite>[Test-driven Development](http://en.wikipedia.org/wiki/Test-driven_development) on Wikipedia</cite>

If you want to run all your tests every time you add a new one, you probably don't want to be waiting around too long for that to happen, it'll just slow you down. But all the focus on the amount of time that is right seems to me to be damaging to the discussion. If DHH is fine with waiting 4.5 minutes for his test suite to run, who cares? I think it's pretty awesome that he has thousands of assertions, most projects I see don't have anything close to that.

DHH also makes a point in his post [Slow database test fallacy](http://david.heinemeierhansson.com/2014/slow-database-test-fallacy.html) about not running all your tests everytime you add a new one, and it is an approach I find reasonable:

> I still wouldn't want to wait 80 seconds every time I make a single change to my model, and want to test that. Of course not! Why on earth would you run your entire test harness for every single line change in a particular model? If you have so little confidence in the locality of your changes, the tests are indeed telling you that the system has overly high coupling.

That said, I still do think speed is important. If your tests are too slow to be useful, you should fix that. Where that number lies is a matter of individual and team preference. I do think that faster is better, but I don't believe in pursuing that at the expense of more important concerns.

Red, green, refactor is to me the core of TDD. I also think decoupled logic in my application is a good idea. If TDD helps me to get there, and decoupling also increases the speed of my tests, those are just bonuses on top of a process I already follow.

So why all the fighting and confusion over TDD? Well, TDD is just a name. Names are pretty useful things, they help us have discussions about rather complicated topics. If you know what Object Oriented Programming is, and I know what Object Oriented Programming is, we can have a discussion about it without having to stop to define every detail along the way*.

But names can also be harmful. If you "know" what Object Oriented Programming is, and I "know" what Object Oriented Programming is, but our understanding of those two things differs greatly, our discussion will be rather difficult and confusing to follow. We're both assuming that the other has the same ideas in their head, which makes it hard to communicate effectively.

While reading [Sandi Metz](http://www.sandimetz.com/)'s book [Practical Objected Oriented Design in Ruby](http://www.sandimetz.com/poodr/), I noticed something interesting about the way she explained things. She doesn't tell you what things are called until after she's already explained the concept and shown you how to do it. So if you came into her book with the idea that polymorphism is confusing, you don't have time to get your guard up when she teaches you about it, since she doesn't tell you until after. You don't get time to project your own understanding of those concepts before she has a chance to show you how they work. It's a great way of explaining things, and one I'd like to try when I get the chance.

So what does this have to do with TDD? Well, TDD is a name that means a lot of things to a lot of people. To me, it means red, green, refactor. Everything else just helps me do that better.

Maybe when we have discussions about TDD, we should state what our definition of TDD is. If we found out we were all arguing about different things, maybe we could find some common understanding and begin our discussion there. Our first test might fail, but we can figure out how to make it pass and refactor from there.

<small>*However, growing a language can be fun. If you haven't already watched [Growing a Language](https://www.youtube.com/watch?v=_ahvzDzKdB0) by Guy Steele, you should! It's a great talk.</small>
