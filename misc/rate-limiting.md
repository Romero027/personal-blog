# Rate Limiting

### What is rate limiter?

A _rate limiter_ is a tool that monitors the number of requests per a window time a service agrees to allow. If the request count exceeds the number agreed by the service owner and the user \(in a fixed window time\), the rate limiter blocks all the excess calls\(e.g. throw exceptions\). 

### [Rate Limiter and load shedders in Stripe](https://stripe.com/blog/rate-limiters)

In Stripe, they implement a few rate limiting strategies\(see the blog post for details\) to help keep the API available for everyone. 

Rate limiters are amazing for day-to-day operations, but during incidents \(for example, if a service is operating more slowly than usual\), they sometimes need to drop low-priority requests to make sure that more critical requests get through. This is called _load shedding_. It happens infrequently, but it is an important part of keeping Stripe available.

A _load shedder_ makes its decisions based on the whole state of the system rather than the user who is making the request. Load shedders help you deal with emergencies, since they keep the core part of your business working while the rest is on fire.

### How to build rate limiter in practice?

Suppose we are trying to build a rate limiter that restricts each user to N requests per hour:

* **Attempt 1: Fixed window counters**

For every user, there is one bucket for each of the unit time window and each bucket maintains the count of number of requests in that particular window. 

```text
{
 "1AM-2AM": 7,
 "2AM-3AM": 8
}
```

For example, if we only allow 10 request per hour, an exception is raised if the count exceeds 10. 

This simple algorithm runs in constant time and uses constant space\(We only need to maintain two entries if we use per hour rate limiting\). However, it is incorrect. Consider the case that if the server receives 9 request from 1:30 - 2:00, and 5 requests from 2:00 - 2:30, then the algorithm is going to allow 14 requests to go through from 1:30 to 2:30.

* **Attempt 2: Sliding window logs**

For every user, we have a queue of timestamps representing how many requests a user made within a specific time window\(e.g. an hour\). Whenever the server receives a new request, we remove all timestamps older than the window time. \(Instead of doing this at every request, we can also do periodic checking\). Then, we check the length of the queue, if `len(user_queue) < MAX_LENGTH` we process the request normally, and raise an exception otherwise.

This algorithm is correct, but it needs to store all the requests for all users in a time window, which may take too much space. In addition, the checking made for every request is also an expensive operation.

* **Attempt 3:  Token bucket** 

In Stripe, they use the [token bucket algorithm](https://en.wikipedia.org/wiki/Token_bucket) to do rate limiting. This algorithm has a centralized bucket host where you take tokens on each request, and slowly drip more tokens into the bucket. If the bucket is empty, reject the request. In our case, every Stripe user has a bucket, and every time they make a request we remove a token from that bucket.

This is much more efficient than attempt 2, because it only needs to store a fixed number of tokens at most and the checking only takes O\(1\). They implement their rate limiters using Redis. You can either operate the Redis instance yourself, or, if you use Amazon Web Services, you can use a managed service like [ElastiCache](https://aws.amazon.com/elasticache/).

**Note: Idempotence** 

In this blog post, the author also discuss the problem of "exactly once” semantics. Worded differently, how to implement a mechanism that makes sure we invoke an operation exactly once?  An example might be if we were designing an API endpoint to charge a customer money; accidentally calling it twice would lead to the customer being double-charged, which is very bad.

This is where _idempotency keys_ come into play. When performing a request, a client generates a [unique ID](https://stripe.com/docs/api#idempotent_requests) to identify just that operation and sends it up to the server along with the normal payload. The server receives the ID and correlates it with the state of the request on its end. If the client notices a failure, it retries the request with the same ID, and from there it’s up to the server to figure out what to do with it.

### Reference:

{% embed url="https://medium.com/@saisandeepmopuri/system-design-rate-limiter-and-data-modelling-9304b0d18250" %}















