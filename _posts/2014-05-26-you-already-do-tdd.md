# You're Already Doing TDD

I recently watched Uncle Bob's talk [Architecture the Lost Years](https://www.youtube.com/watch?v=WpkDN78P884) on YouTube. At one point he stops and asks the audience who practices Test Driven Development (TDD). By his reaction, it's apparent that not many people have their hands up.

I would argue that everyone without their hand up is a liar. They all do TDD. In fact, they have TDD'd every line of code they've ever written, and you have too.

In my post [Red, Green, Refactor](http://www.ericroberts.ca/2014/05/02/red-green-refactor/) I said how TDD was explained to me originally. Let's take a look:

> 1. <span>Write a failing test</span>
2. <span>Write some code that passes the test</span>
3. <span>Refactor</span>

So does everyone really do this? I guess they better if I'm going to call all those people liars. Let's take a look from the perspective of a bug being reported by a user in our system. The typical process probably looks like this:

- <span>A user reports that they can no longer purchase something from your site.</span>
- <span>You go to the purchasing system, and try various paths through until you find one that causes the error the user reported.</span>
- <span>You write some code, then try the permutation that had previously failed to see if it now works as expected.</span>

That doesn't sound so different from TDD, does it? Every time you open your browser and check to see the thing you expected to happen, that's a test. Every time you open your console and check the result of calling some method, that's a test. You've tested your code. How else would you know that it works?

You may have noticed I skipped a step. In my original definition of TDD step three is refactor. We've really only covered two steps here. In practice, I find that people who are not writing tests for their code often do not do the refactor step. It's just too exhausting. In the simplest case, testing that something works involves switching from your code to the browser, refreshing the page, and seeing what you expected to see on the screen. But most of the interesting things on your site (and therefore the most prone to bugs) involve more than that. In order to test the bug we described above you would likely have to follow steps like this:

- Find a product
- Add it to your cart
- Fill in your shipping details
- Fill in your credit card information
- Hit purchase button
- See if purchase was completed successfully
- Repeat steps above with different conditions to make sure we didn't break anything

When you have to do all that every time you make a change, are you really going to want to refactor? Even if you are willing to do that now, I wouldn't be willing to bet that future programmers who come across the code are going to want to touch any more than they absolutely have to.

That's because, while we have TDD'd our code, we haven't written [Self-Testing Code](http://www.martinfowler.com/bliki/SelfTestingCode.html).

[Martin Fowler](http://martinfowler.com/) says of Self-Testing Code that:

> You have self-testing code when you can run a series of automated tests against the code base and be confident that, should the tests pass, your code is free of any substantial defects.

You certainly don't have that in the scenario we described above. It sounds pretty great though, doesn't it?

So why don't people do it? Well, it's hard. It's another thing to learn. We are all busy, and in our field we spend a lot of time just keeping up with what is going on. But testing through the UI manually is slow. Testing through the console may be slightly faster, but it's still a lot of typing and manually setting up conditions every time you want to know if something works.

I can promise you that if you devote the time to learning how to test properly, it will payoff. It will payoff when you almost never have to open a browser to be confident that your application is still working. It will payoff when you don't have to manually enter the same conditions over and over to see if something gives you the results you expect.

I don't write perfect code, and I don't write perfect tests. Right now, when I run my test suite, I'm not 100% sure that every part of of my applications work. But the more I do it, the more my confidence rises. The more I do it, the faster I get at writing tests, and the less time I spend testing things manually. If you've never had the experience of writing an entire feature without opening the browser, and then the first time you test it manually it works as you expected it to, then you're missing out.

So remember, you already do TDD, but you could be doing it so much better.


