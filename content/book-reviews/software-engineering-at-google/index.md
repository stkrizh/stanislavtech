+++
title = "Software Engineering at Google"
date = 2026-07-29T15:00:00Z
draft = false
summary = 'I believe it would be interesting for anyone whose job is related to software to take a peek behind the curtain of a company like Google. The most appealing part is learning – even if only through reading – how a company of such scale operates and solves its technical problems. Fortunately, we have that opportunity – [the book is available online](https://abseil.io/resources/swe-book) completely for free.'
coverAlt = 'Silhouette of a building under construction beside a river'
coverCaption = '[Silhouette of a building under construction beside a river](https://unsplash.com/photos/silhouette-of-building-construction-beside-the-river-eo7YWqBIWYI)'
tags = ['software engineering', 'guide', 'testing']
categories = ['book review']
+++
I believe it would be interesting for anyone whose job is related to software to take a peek behind the curtain of a company like Google. The most appealing part is learning – even if only through reading – how a company of such scale operates and solves its technical problems. Fortunately, we have that opportunity – [the book is available online](https://abseil.io/resources/swe-book) completely for free.

## Programming integrated over time
The book begins with a catchy and precise definition of software engineering. It highlights a fundamental difference between programming and software engineering – two activities that are often considered the same. According to the book’s definition, the former is mainly the act of writing code, while the latter includes other critical factors, such as time, scale, and various operational trade-offs.

> One key insight we share in this book is that software engineering can be thought of as “programming integrated over time.”

I find the notion of time that differentiates software engineering from programming especially fascinating. Writing a piece of code is part of programming. Maintaining that code for years is part of software engineering. **Time adds a new dimension.** It forces us to consider many other crucial aspects of software, such as maintainability, readability, extensibility, and so on. This distinction is especially relevant now, as AI coding agents write more and more code and effectively handle much of the programming part.

## Engineering practices
The book comprises about two dozen chapters, each of which describes a specific engineering practice. Each chapter is fairly independent of the others, so it is possible to read them in any order. This structure also makes it easy to return to particular chapters from time to time to refresh your understanding of a topic.

Below, I’ll share a few excerpts from those chapters that especially resonated with me.

### Knowledge sharing

> People are at the core of software engineering: code is an important output but only a small part of building a product. 

Obviously, code is an important artifact of a software organization. But a **shared mental model of the problem domain** is arguably even more important. Without a clear and complete shared mental model, individuals push the product in different directions according to their personal views and beliefs. These uncoordinated efforts hamper maintainability and the product’s long-term success.

Interestingly, code itself can be seen as a tool for sharing knowledge. In a sense, **code is the ultimate specification of a product.** But other important practices also contribute to knowledge sharing, including documentation, code reviews, mentorship, and so on.

The mental model of a product is built and shared by people. Consequently, operational processes and communication should be organized in a way that helps refine and expand this shared knowledge.

### Style guides and rules

Style guides are often a controversial topic among software engineers. Countless hours have been spent arguing about commas and variable names in code reviews. After all, everyone has their own tastes and preferences. So how can style guides help in this regard?

> … the established rules and guidelines shape the common vocabulary of coding. A common vocabulary allows engineers to concentrate on what their code needs to say rather than how they’re saying it. By shaping this vocabulary, engineers will tend to do the "good" things by default, even subconsciously.

The book mentions several times that Google places a high value on simple, straightforward, and consistent code. One particularly compelling idea is that **consistency is valuable in itself**:

> … being consistent may feel constraining at times, but it means more engineers get more work done with less effort… When we solve our problems with the same interfaces and format the code in a consistent way, it’s easier for experts to glance at some code, zero in on what’s important, and understand what it’s doing. It also makes it easier to modularize code and spot duplication… It’s the consistency of having one answer rather than the answer itself that is the valuable part here.

### Testing

There are four chapters dedicated to testing, which clearly shows how highly Google regards this particular engineering practice. To me, these chapters are among the most valuable in the book.

Google places particular emphasis on the maintainability of tests. Without proper care, a test suite can quickly become an unmaintainable burden that harms the team’s productivity and morale. All the following advice shares a common goal of eliminating brittle tests and improving maintainability.

To begin with, the book presents the testing pyramid as the foundation of a healthy test suite:

> Our recommended mix of tests is determined by our two primary goals: engineering productivity and product confidence. Favoring unit tests gives us high confidence quickly, and early in the development process. Larger tests act as sanity checks as the product develops; they should not be viewed as a primary method for catching bugs.

Another crucial point is to test a component through its public API rather than its implementation details.

> Such tests are more realistic and less brittle because they form explicit contracts: if such a test breaks, it implies that an existing user of the system will also be broken. Testing only these contracts means that you’re free to do whatever internal refactoring of the system you want without having to worry about making tedious changes to tests.

For unit tests, one important piece of advice is to **test state rather than interactions.** Essentially, this means not relying too heavily on mocks and preferring other types of test doubles, particularly fakes. The argument is that interaction tests are very brittle and, consequently, much more expensive to maintain.

Consider the following example:
```python
def test_writes_to_database():
    accounts.create_user("foobar")
    database.create.assert_called_with(username="foobar")
```

This test verifies that the `create` method is called with a specific argument. In other words, it tests the interaction between components of the system. The problem is that such a test can produce misleading results in several ways:
1. If a bug in the system under test causes the record to be deleted from the database shortly after it is written, the test will still pass, even though we would want it to fail.
2. If the system under test is refactored to use a slightly different API to write an equivalent record, the test will fail, even though we would want it to pass.

A less brittle test would verify what we actually care about – the final state of the system under test:
```python
def test_user_is_created():
    accounts.create_user("foobar")
    assert database.get(username="foobar") is not None
```

The dedicated chapter on test doubles is particularly interesting. Google considers **fakes to be the most beneficial type of test double.** A fake is a lightweight implementation of a real component that behaves similarly but is not suitable for production (e.g. an in-memory database).

> If using a real implementation is not feasible within a test, the best option is often to use a fake in its place. A fake is preferred over other test double techniques because it behaves similarly to the real implementation: the system under test shouldn’t even be able to tell whether it is interacting with a real implementation or a fake.

By using fakes where feasible, we can make our test suite much faster and cheaper to run, in line with the testing pyramid.

## To sum it up
Apart from the chapters I’ve mentioned above, the book contains many other insightful ones. Philosophical reflections on software and highly practical advice are scattered throughout the book. Thanks to its structure, it is easy to find a particular topic you are currently curious about and return to it from time to time. If you are interested in software, this book is a highly valuable read.
