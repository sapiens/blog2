Title : Why Do We Bother With Eventual Consistency?
Tags : Architecture
Published : 2018-01-22
---

Eventual Consistency (EC) is viewed as a necessary evil most of the time. The simple CRUD unit of work, which ensures all data changes are performed successfully is deeply ingrained into our brains as developers. This is our default approach and only when dealing with distributed apps or Event Sourcing and CQRS etc we begrudgingly tolerate that eventual consistency, because we have to!

People who view EC as normal will tell you that this is how the real world works and usually a business transaction is by its nature eventual consistent and we're doing DDD so you should accept it, like it or not! But let's see, why most real world processes are EC?

Let's take the "buying from your favourite burger/coffe joint" example and let's pretend we're the manager and we like the CRUDy unit of work approach where _every change should be part of a unit of work_. The client orders, then pays with credit card,then waits for the order to be fulfilled. Let's say the payment failed. Great, the order is automatically canceled (rollback) and everyone is happy, right? Then the client tries again but paying cash this time. The order is sent again, payment is received and now we're waiting for the product. Once that's done, we consider that business transaction (process) complete. Now, imagine this process for an online shop who has to deal with shipments as well.

What's wrong with the above approach? For starters, it's kinda stupid to cancel the order (think about the waste and lost time), just because some step in the process has temporarily failed. Secondly, each customer would have to wait (synchronously) until the process end successfully. For a popular business this means low productivity and huge lines and unhappy customers. And I can guarantee that in a matter of hours if not sooner, someone will try to optimize the process by reverting to how the process works today in the real world i.e eventually consistent. 

Clearly, when dealing with a process that involves long running operations (each domain has their own standard of what 'long' means) **eventual consistency is the natural way to make the process scalable**. As you can see, nothing related to things like CQRS or technical methodologies, it's just the way the domain works.

But what if we don't deal with long running operations? And we're using Event Sourcing and CQRS i.e our technical implementation imposes the eventual consistency? Well, the reason why we choose such solutions is mainly to increase performance or scalability. And the good news is, DDD concepts like the Aggregate it still useful because it tells us what changes should be together i.e not distributed. And honestly there aren't many situations where it makes technical sense to store aggregate data in different databases.

CQRS allows us to scale queries very easily, but there's always this issue of "What if my read model doesn't have the latest data and my command model needs it". Don't worry, in an app with high traffic, by the time you've read some data from the db, business state has already changed. Simply put you'll _always_ be behind. For apps with moderate writes, 99% of cases you're going to deal with the latest data. And, like I've written [here](http://blog.sapiensworks.com/post/2017/08/23/Handling-Business-Rules-DDD), what matters is the _business impact_ , that is, if the business can wait for a couple of (milli)seconds. So, yes it depends on that specific problem of the domain.

In conclusion, we should accept the fact that the unit of work approach is not the best solution that we should always strive for and the fact that a domain transaction is a very specific and different thing from a database transaction.