# Release it

## Availability

Higher availability avoids losses but also increments actual costs.

Each "9" of availability increases the implementation cost by about
a factor of ten and the operational cost per year by about a factor
of two.

### Defining SLAs
It is highly advised to define and write down *precise* service-level
agreement with the system's sponsor in the beginning, not one year
after the system has been running.

Rather than stating the availability of "the system", it is better to
define the SLAs in terms of specific features or functions of the
system. Different features might require different levels of
availability depending on their importance to the business.

A particular feature cannot offer a better SLA than the worst SLA
of the external dependencies involved in this feature.

A good definition for a SLA should also state exactly what availability
means for each particular feature. 99.99% available is just too vague.

The book recommends to make use of synthetic transactions to assess
the current state of a particular feature. I don't fully agree with this
mechanism as I don't find it entirely reliable. I would prefer to
look at the actual traffic that users are generating and analize that
instead in terms of response time and server response.

### Load balancing

#### DNS Round-Robin
Quite old load balancing technique.
You set up a machine as DNS Round-Robin. Clients will send requests to
this machine, who will return the IP address of one of the servers
behind it. The client then will send a second request to that IP address
which will actually reach the server.

The DNS server has no information about the health of the servers so
it might return the IP address of a server that is down.

Any client system built on Java will cache the first IP address received
and will use it for all subsequent requests.

#### Reverse proxy
Addresses a few of the limitations of the DNS Round-Robin. You set up
a machine as Reverse Proxy which will receive all requests from clients.
It demultiplexes calls coming in to a single IP address and fans them out
to multiple IP addresses.

Clients don't know which actual server they are talking to. To them, it's
just one server and not many.

You can configure reverse proxy servers to reduce the load on the web
servers by caching static content. This reduces load from the servers
and internal network.

The most commonly used reverse proxy servers (at the time when the book
was written) were Squid and Apache. None of them detect servers health.

#### Hardware Load Balancer
Specialized network devices that serve a similar role to the reverse
proxy server. Examples are Cisco's 11500 Series Content Services Switch
or F5's BigIP products.

They provide better administration and redundancy features. They can check
the health of the servers in its pool.

They can be used as SSL accelerators.

The big drawback of these machines are their very high price.

## Operations

### System transparency
Transparency refers to the qualities that allow operators, developers and
business sponsors to gain understanding of the system's historical trends,
present conditions, instantaneous state, and future projections.

Debugging a transparent system is vastly easier, so transparent systems will
mature faster than opaque ones.

A system without transparency cannot survive long in production. If admins don't
know what it is doing, it cannot be tunned and optimized. If developers do not
know what works and does not work in production, they cannot increase its
reliability or resilience over time. If business sponsors do not know whether they
are making money on it, they will not fund future work.

Without transparency, the system will drift into decay, functioning a bit worse
with each release. Systems can mature well if, and only if, they have some degree
of transparency.

#### Historical trending
It is somewhat possible to predict tomorrow's system behaviour by extrapolating
yesterday's results. This applies to business metrics (customers, orders, conversion
rate, revenue, ...) as well as system metrics (free storage, average CPU usage,
network bandwidth, number of errors logged).

Common questions:
* How many orders did we take yesterday?
* How does that compare to this day last year?
* How much disk space did we consume during the first quarter?
* The last time we had a spike in traffic, which system was the limiting factor?
* How does the growth in customer traffic compare to the growth in CPU usage
over the past three years?

#### Predicting the future
Use historical trending to predict the future, of course with some level of
uncertainty.

Looking at historical data you might be able to see what bottlenecks you will have
in the future and you can start working towards eliminating them before they become
a reality.

#### Present status
Parameters to observe:
* Memory: min and max heap size
* Garbage collection: type, frequency, memory reclaimed, size of request
* Worker threads, for each thread pool: #threads, threads busy, threads busy more
than five seconds, max-concurrent threads in use (high-water mark), low-water mark,
number of times a thread was not available, request backlog.
* Db connection pools, for each pool: #connections, connections in use, high-water
mark, low-water mark, number of times a connection was not available, request backlog.
* Traffic stats, for each request channel: total requests processed, average response
time, requests aborted, requests per second, time of last request, accepting traffic
or not.

Parameters per application / service / system:
* Business transaction, for each type: #processed, #aborted, conversion rate, value.
* Integration points: current state, average response time from remote system, #failures
* Circuit breakers: current state, #failed calls, time of last successful call

### Designing for transparency
Transparency arises from deliberate design and architecture.
Adding transparency late in development can be done, but only with greater effort and
cost than if it had been built in from the beginning.

In designing for transparency, keep a close eye on coupling. It's relatively easy for
the monitoring framework to intrude on the internals of the system. Your monitoring
and reporting systems should be like an exoskeleton, built around your system, not
woven into it.

Decisions about what metrics should trigger alerts, where to set thresholds and the logic
to state the overall system health status should all be left outside of the application
itself.