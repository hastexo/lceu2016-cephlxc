### Manageable
## Application Containers
Lightning Quick Updates, Scaleable Security, Easy High Availability

[`florian.haas@hastexo.com`](mailto:florian.haas@hastexo.com)

[@xahteiwi](https://twitter.com/xahteiwi) / [@hastexo](https://twitter.com/hastexo)


A few
# words
to start with
Note: This talk covers an approach towards running application
containers that we think is quite suitable for production use, at
least we've been running it in product for 6 months.

A lot of people we talked to about this approach found it unusual
though, which always has me slightly wary of some important issues we
may have missed.

So with this talk I'll attempts to demonstrate, to explain, and to
hopefully kick off some good hallway discussions about what what we've
been doing. In other words, if you can poke holes into the ideas
described here, please do.


You should
# know
containers
Note:
- This talk assumes familiarity with basic linux container
  concepts. You should know what a container is, and ideally you would
  have occasionally run a container. It doesn't really matter whether
  your experience has been with LXC, LXD, Docker or whatever.
- It is not for complete container novices. Not being a container
  **expert** is perfectly OK though, I'm not one either.


You should
# know
high availability
Note:
- It also doesn't hurt if you've worked with high availability systems.
