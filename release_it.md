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
