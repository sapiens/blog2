Title : Understanding The Mindset of Event Sourcing
Tags : Domain driven design
Published : 2018-2-19
---

As Event Sourcing (ES) becomes more and more mainstream, usually bundled with DDD and CQRS, it's important for domain modellers, application architects etc to understand the paradigm behind the recipe.

In the 'classical' approach, the domain state is seen as a data structure or a collection of data structures (entities data). In ES we see the **domain state as a collection of changes**. You may wonder how we've got here and why this is the preferred way of working for many DDD practitioners.

If you're familiar with accounting or at least basic book-keeping, you know that (before computers) a book-keeper would write the daily relevant financial transactions into a ledger. Using their accounting 'language' they would record all the business events as they were.

An important thing here is that a book-keeper just writes down things, there's no attempt at interpreting or processing the events. Basically, a financially specialized scribe keeping a log of business actions.

When an investor or a manager wants to asses the health of the company, they usually look at 3 documents: balance sheet, income statement, cash flow statement. Each report offers a different view of the business highlighting different aspects. But how are these documents generated?

Well, someone (these days a software) would look at all transactions recorded in the general ledger and depending on what report is generated, specific information is extracted and aggregated according to that report's purpose.

By now, you've probably noticed that the ledger is basically an Event Store and that accounting is using CQRS (imagine that!!). So, we can say that ES is the book-keeping approach adapted for software development. A paradigm that has been used in any business in the world for hundreds of years. And if it works very well in the real world, it should work very well in a Domain Driven application, too.

The ES mindset sees the domain state as the accounting ledger (incidentally the Event Store is the actual 'ledger' as a technical detail), a collection of business actions that already happened (events). Like the book-keeper, we want to record operations and not just update some data structures like in the classical approach. Therefore properly identifying relevant domain events becomes very important and it's the first step when applying ES.

At this point you might wonder:"Okay, but what's wrong with the old approach?". In the end it's easier just to modify some data structure. It is, but it's also limited, because 1) all you have is the most current data and 2) accounting proved that recording operations instead of just modifying existing data is more flexible. Why reinvent the wheel poorly, when we have a hundreds of years old approach that works very well?!

Remember, in accounting, all events are written once then we can get _any_ type of report based on that information. In ES we call those reports projections.

Now, let's say I need the state of some entity. Since all we have is a collection of operations that happened (grouped by entity), we need to process (_apply_) the relevant events in a manner that makes sense for the business use case needing that state. This means that the events are 'processed' in order to extract the relevant information, so we can say that restoring an entity state is a form of creating a report. That's why, the resulted state should be considered a read model from CQRS point of view. Obviously, actual code implementations may vary depending on your style.

And speaking of restoring state or any other projection, the fact that it's based on the collection of events makes it _event sourced_, hence the name of the pattern.

Once you understand that, in the design process, we're focusing on the outcomes of business operations a.k.a domain events, that are persisted as they are, then projected (processed) according to reporting needs, you know the essence of ES.