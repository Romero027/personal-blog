# Load Balancing

### \#1: Consistent Hashing

Only using consistent hashing is suboptimal because it balances loads about as well as choosing a random server for each request, when the distribution of requests is equal. But if some content is much more popular than others, it can be worse than that. Consistent hashing will send all of the requests for that popular content to the same subset of servers

### \#2: Round-Robin/Least-Connection

These two simple strategies works great for stateless services since they guarantee that each server will get equal amount of requests. However, for stateful services, they are still less-than-ideal: without hashing, these two solutions do not fully utilize the caches. 

One way to mitigate this cache problem is to use global\(regional\) cache. More specifically, the servers keep local caches, but also fall back to a regional shared cache. Writes will be send to both in-memory cache and the regional cache. On read, the server will first check its local cache and, if not found, try to fetch from the regional cache. Unfortunately, such strategy scales poorly, the shared cache traffic grows linearly with the number of servers. 

### \#3: Consistent Hashing with bounded load

[https://medium.com/vimeo-engineering-blog/improving-load-balancing-with-a-new-consistent-hashing-algorithm-9f1bd75709ed](https://medium.com/vimeo-engineering-blog/improving-load-balancing-with-a-new-consistent-hashing-algorithm-9f1bd75709ed) 

