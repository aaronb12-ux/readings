# Chapter 2: Defining Nonfunctional Requirements

When building an application, you will be driven by a list of requirements → these are called **functional requirements**.

**Nonfunctional requirements** are things like: app speed, reliability, scale, maintainability. These requirements are not written down but they are just as important as functional requirements.

We will be looking at the following nonfunctional requirements:
* How to define and measure the performance of a system
* What it means for a service to be reliable – namely, continuing to work correctly even when things go wrong
* Allowing a system to be scalable by having efficient ways of adding computing capacity as the load on the system grows
* Making it easier to maintain a system in the long term

## Case Study - Social Network Home Timelines

Imagine we are building a social network system like Twitter. And that users make 500 million posts per day (5,800 a second) and follow around 200 people and have around 200 followers.

### Characterizing Transaction Processing and Analytics

Imagine we keep all the data in a relational database. We have a table for users, posts, and for follow relationships.

And imagine that our main read operation is the home timeline which displays recent posts by people you are following:
```sql
SELECT posts.*, users.* FROM posts
    JOIN follows ON posts.sender_id = follows.followee_id
    JOIN users ON posts.sender_id = users.id
    WHERE follows.follower_id = current_user
    ORDER BY posts.timestamp DESC
    LIMIT 1000
```

Posts should be timely, meaning that once a post is made, we want users to see it within 5 seconds. This would be constantly doing the above query every 5 seconds. This is called **polling** and can be inefficient. It also does not help that the above query is very expensive.

## Materializing and Updating Timelines

Instead of polling, it would be better if the server actively pushed new posts to any followers who are currently online.

We can imagine that each user has a mailbox (home timeline) that stores all posts made by users they are following. Every time a user makes a post, we look up their followers, and insert that post into that mailbox of each follower. Now when a user logs in, we can simply display their mailbox of posts made by those that user is following.

The downside of this is that it requires more work and updating.

The process of precomputing and updating the results of a query is called **materialization**, and the timeline cache is an example of a **materialized view**. This view speeds up reading, but in return we have to do more work on write.

If a user is following many people and those accounts post a lot, that user will have a high rate of writes to their materialized timeline.

**Throughput metrics:**
* Posts per second
* Timeline writes per second

**Response time metrics:**
* Time it takes to load the home timeline
* Time until a post is delivered to followers

## Describing Performance

Most discussion of software performance consider two main types of metric:

### Response Time
The elapsed time from the moment when a user makes a request until they receive the updated requested answer. The unit of measurement is **seconds**.

### Throughput
The number of requests per second, or the data volume per second, that the system is processing. For a given allocation of hardware resources, there is a maximum throughput that can be handled. The unit of measurement is **somethings per second**.

### Why Response Time Increases as Load Increases

**Queuing:** When a request arrives on a highly loaded system, it is likely that the CPU is already in the process of handling an earlier request, and therefore the incoming request needs to wait until the earlier request has been completed.

### When Things Go Wrong

If there is a long queue of requests waiting to be handled, response times may increase so much that clients time out and resend their requests. This causes the rate of requests to increase even further. Even if the load is reduced again, the system may stay in this state until it is reset. This is called a **metastable failure**.

---

If throughput is likely to increase beyond what the current hardware can handle, the capacity needs to be expended; a system is said to be scalable if its maximum throughput is significantly increased by adding computing resources.

## Latency and Response Times

* The response time is what the client sees; it indicates all delays incurred anywhere in the system
* The service time is the duration for which the service is actively processing the user request
* Queuing delays can occur at several points in the flow: for example, after a request is received, it might need to wait until a CPU is available before it can be processed
* Queuing delays often account for a large part of the variability in response times. As a server can only process a small number of things in parallel, it only takes a small amount of slow requests to hold up the processing of subsequent requests - an effect known as head-of-line blocking

## Average, Median, and Percentiles

* Because the response time varies from one request to the next, we need to think of it not as a single number, but as a distribution of values that you can measure
* It's best to use a median for response times as 50% of users waited longer than the median, and 50% under.
* Tail latencies are high percentiles of response times

## Use of Response Time Metrics

* When making calls in parallel, it just takes one slow call to make the entire end-user request slow

## Reliability and Fault Tolerance

* **Fault:**
   * A fault is when a particular part of a system stops working correctly: for example if a single hard drive malfunctions, or a single machine crashes, or an external service has an outage
* **Failure:**
   * A failure is when the system as a whole stops providing the required service to the user; in other words, when it does not meet the service level objective

## Fault Tolerance

* We call a system fault tolerant if it continues providing the required service to the user in spite of certain faults occurring. If a system cannot tolerate a certain part becoming faulty, we call that part a single point of failure (SPOF), because a fault in that part escalates to cause the failure of the whole system
* For example, in the social network case study, a fault that might happen is that during the fan-out process, a machine involved in updating the materialized timelines crashes or become unavailable. To make this process fault-tolerant, we would need to ensure that another machine can take over this task without missing any posts that should have been delivered, and without duplicating any posts.
* A system normally has a fault tolerance that is limited to a certain number of faults. For example a system may be able to tolerate a maximum of two hard drives failing at the same time, or a maximum of one out of three nodes crashing
* In some systems, it may be necessary to try and increase the amount of faults by purposely triggering them, this is called fault injection and its used to test systems (like error handling)
