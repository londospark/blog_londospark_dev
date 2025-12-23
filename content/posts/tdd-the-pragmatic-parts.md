+++
title = "TDD the Pragmatic Parts"
date = "2025-12-23T13:21:22Z"
#dateFormat = "2006-01-02" # This value can be configured for per-post date formatting
author = "LondoSpark"
authorTwitter = "londospark" #do not include @
cover = "/images/TDD-Pragmatic.png"
tags = ["tdd", "pragmatic", "opinion", ".net", "csharp"]
keywords = ["Programming"]
description = "Why TDD should focus on Design over Development, and why it's okay to delete your tests."
showFullContent = false
readingTime = false
hideComments = false
draft = false
+++

So you've heard a great many opinions on TDD? Heck, you might even have heard different names for it too. Some people use **Test Driven *Development*** yet some will use **Test Driven *Design*** - I'm more a fan of the latter than the former, but I'm also not going to go full TDD-bro, it has its place, and even the way that I will implement it might horrify some.

First off: Why do I prefer to think in terms of Test Driven Design? Well dear reader, I'm so glad that you asked. You didn't ask? Never mind, I'm going to tell you anyway. I like the idea that you are using the tests for design purposes. Groundbreaking isn't it? Well no, but it is a fundamental idea. You see, there are a great many people who may not be fans of traditional TDD, who espouse the virtues of writing the usage code first. Now that's a form of TDD in itself, you're testing your API for ergonomics by designing it all around the usage.

You can think of writing the usage code first as "coding by wishful thinking" as in "I wish there was something called foo so that I could call it and things would be lovely." Let's have a look at doing that in a test. For this we're going to be using C# but this will work well in any language you choose to use:

```csharp
[Fact]
public void WeCanArchiveLogs()
{
    var archiver = new LogArchiver();
    archiver.ArchiveOldLogs(@"C:\Logs", daysToKeep: 7);
}
```

I think we have a lovely API there, we're just passing a folder and an amount of time to keep logs in the folder. We're not passing a hundred and twenty-eleven parameters into the function just to support DI and make the tests happy. What you might notice though is that we have no assertions. At no point do we call `Assert.Equal(...)` or any of the other methods. That's partly because we don't care at this point what the result is, it's also because setting up a load of mocks and stubs would make this test extremely complicated and probably remove a lot of the point of it. In a compiled language we could even go one further and add the `[Skip]` or `[Ignore]` attribute to it. What we're doing here is using a test to sketch out what the API looks like as well as ensuring that we keep that contract. If the test compiles then it passes, it's just a syntax check, just something to keep us honest with the ergonomics of the API. Yes, we're going to want to unit test what we can test in that way, and we're going to want integration and e2e tests, there's no doubt about that - but they're a bit too slow for a red-green-refactor loop, so we need to keep this quick.

Why not then just write the usage code in place and use that? That's the eventual plan, although for now this gives us a sandbox to play in. We don't have to commit to anything right now and we can share this with others without impacting the production system. We can either then get rid of the test that we just wrote, or we can extend it into some form of integration test. **Get rid of a test that we've written!? Heresy!** Well not really. Who said that red-green-refactor meant only refactoring the production code? If we can prove that the only ways to break this test will also break something else then is it giving us any value anymore? I would argue that it's just cruft, it's nonsense and it only serves to slow us down. If code is slowing us down and giving us no benefit then don't we get rid of it and refactor it away? Yes? So why should tests be any different? They're just code at the end of the day, so let's treat them as such. Let's be ruthless, let's get rid of tests that we no longer use!

By doing this we have embraced pragmatism over dogmatism and we're using the tests in TDD to help us with our design without carrying baggage around. So will you be looking more critically at your testing practices and how you use tests to help with your design rather than as a hindrance to speed?
