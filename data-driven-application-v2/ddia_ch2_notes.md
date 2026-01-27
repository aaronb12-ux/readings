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
