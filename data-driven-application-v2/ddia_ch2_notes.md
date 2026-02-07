## Chapter 2: Defining Nonfunctional Requirements

When building an application, you will be driven by a list of requirements -> these are called functional requirements

Nonfunctional requirements are things like: app speed, reliability, scale, maintainability. These requirements are not written down but they are just as important as functional requirements

We will be looking at the following nonfunctional requirements:
* How to define and measure the performance of a system
* What it means for a service to be reliable – namely, continuing to work correctly even when things go wrong
* Allowing a system to be scalable by having efficient ways of adding computing capacity as the load on the system grows
* Making it easier to maintain a system in the long term

## Case Study - Social Network Home Timelines

Imagine we are building a social network system like twitter. And that users make 500 millions posts per day (5800 a second) and follow around 200 people and have around 200 followers

### Characterizing Transaction Processing and Analytics

Imagine we keep all the data in a relational database. We have a table for users, posts, and for follow relationships

And imagine that our main read operation is the home timeline which displays recent posts by people you are following:
```sql
SELECT posts.*, users.* FROM posts
    JOIN follows ON posts.sender_id = follows.followee_id
    JOIN users ON posts.sender_id = users.id
    WHERE follows.follower_id = current_user
    ORDER BY posts.timestamp DESC
    LIMIT 1000
```

## Timeliness and Polling

Posts should be timely, meaning that once a post is made, we want users to see it within 5 seconds. This would be constantly doing the above query every 5 seconds. This is called **polling** and can be inefficient. It also does not help that the above query is very expensive.

## Materializing and Updating Timelines

Instead of polling, it would be better if the server actively pushed new posts to any followers who are currently online.

We can imagine that each user has a mailbox (home timeline) that stores all posts made by users they are following. Every time a user makes a post, we look up their followers, and insert that post into the mailbox of each follower. Now when a user logs in, we can simply display their mailbox of posts made by those that user is following.

The downside of this is that it requires more work and updating.

