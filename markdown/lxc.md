# LXC
## containers

Note: LXC containers are a means of cleverly making use of Linux
kernel features, such as..


## Lightweight virtualization
### cgroups
### namespaces

Note: ... network and process namespaces, and cgroups, to isolate
processes from one another. What LXC does is basically set up
namespace and cgroup scaffolding for a particular process, and then
start that process, which becomes PID 1 in the container.


# Two
### container types

Note: Based on **what* exactly the process is that is being started
as the container's PID 1, we can distinguish between two different
container types:


## System
### containers

Note: A **system** container is one where the container manager (like
LXC) starts an init process, like SysV init or upstart or
systemd. This is the most common use case. However, this is by no
means required.


## Application
### containers

Note: In contrast, an **application** container is one where LXC
starts any other binary, like, say, MySQL or Apache or lighttpd or
HAproxy or whatever your conntainer is meant to run.


What's
# wrong
with system containers?

Note: So what's the problem with running system containers? Well
they're not so much problems as nuisances, but can be extraordinarily
painful nuisances at times.


### Resource utilization

Note: So one issue is that in a system container whose only purpose in
life is to, say, run an Apache webserver, there are a bunch of
processes that the container manages that are **not** Apache... like a
cron daemon, perhaps postfix, something to manage our network, etc.

And this may not hurt if all you're running is 50 Apache containers,
but if you're running 500 or 5,000 or 50,000, then having that excess
overhead may actually cut into your server density.


# Updates

Note: But a much bigger **operational** problem is updates,
specifically security updates. System containers generally run off
their own `chroot`-type root filesystem, so any packages that you
install there must be updates. Generally you'd do this with something
like `unattended-upgrades` (on Debian/Ubuntu), or by Puppetizing or
Ansiblifying your containers.

Or, and this is the approach generally favored by, for example, the
Docker camp, you don't patch containers at all, but instead you
rebuild them often and redeploy.

Now, put yourself in the shoes of someone who manages maybe 10,000
Apache instances, and the latest bombshell like Heartbleed drops. Now
you have to scramble to either patch or rebuild those 10,000, and
rather quickly. All for containers that are largely identical anyway;
otherwise they wouldn't be containerized.


How can we do
# better
with application containers?

Note: Now there's gotta be a better way, and indeed there is, if we
make use of another handy kernel feature.