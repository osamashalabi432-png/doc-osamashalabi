# Disaster Recovery Planning

A Disaster Recovery (DR) plan is what keeps a company running when its main site goes down — for example, if a company's servers and firewalls all live in one building and that building or site becomes unavailable. Rather than treating that as a full outage, the DR plan provides another site holding the business-critical components, so operations can fail over instead of stopping.

!!! note
    DR site resources are typically weaker (lower capacity/specification) than the main site's — it's there to keep the business running, not to fully mirror production capacity.

## Why pre-assigned IPs matter

It's important to give all of the DR site's resources IP addresses ahead of time, so that failover between the main site and the DR site can happen with a single change rather than an emergency re-addressing exercise.

!!! important
    If DR IPs aren't pre-assigned, failover stops being "one click" and turns into on-the-fly network reconfiguration during an actual outage — exactly when there's the least time to get it right.
