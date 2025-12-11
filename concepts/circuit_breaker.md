## Circuit Breaker During API Request

**Summary**:
This technique is used to prevent hitting a service that has fallen and can't return an expected response.

**Explanation**
When a downstream dependency (API/Database/service) is down, you don't want to block you server by waiting on it;
that would block threads, cascade failures, and take down your whole app. 

There are 3 states:
1. closed - everything works, every call gets through, if too many failures happen withing a window — move to "open" state
2. open - every call fails °fast°, this protect your system from wasting resources 
3. half-open - after a cooldown period allow a certain number of calls, if they fail — switch back to 'open', if they succeed — switch to 'closed'

### Examples

You call an external API. It hangs for 5 seconds.
That’s fine once — but if 50 threads all hang, your service collapses.
Circuit breaker stops this by failing immediately after a threshold.
