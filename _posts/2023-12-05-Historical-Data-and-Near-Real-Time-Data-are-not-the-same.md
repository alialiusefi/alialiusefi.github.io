---
title: "Historical Data and Near Real Time Data are not the same!"
excerpt_separator: "<!--more-->"
categories:
  - Blog
tags:
  - scalability
  - batch
  - integration
  - performance
---

Our platform service has recently integrated with a major client that generated a lot of data in small time frame. After a while, the amount of work needed to process the stored data starts to compound. 
This resulted into a big bottleneck for our service to support such big amount of data.
To properly scale, and mitigate the issue, we had to ask ourselves the following questions:

1. In the context of business domain, Do we really need to treat all data the same? 

For example, do old orders get updated as often as recent orders, or do old orders even need to be updated at all? 

**Key Idea**: Data that doesn't change often after some period of time can be updated much rarer, for example nightly instead of every 1 hr. For data that has reached its final state, can be simply archived or deleted.

2.  Do we need to fetch new data at the same time as updating the existing data? 

For example, the amount of data to fetch new orders in the last 5 minutes can be much less comparing to updating existing orders for the last 60 days. Doing that in the same job may lead to delays in new data processing throughput.

**Key Idea**: Updating existing data and fetching new data independently result into a more performant near real time integration.  

3. Do we need to have an iterator approach when updating existing data?

When data is already stored in database we already have direct access to objects by its identifier. This means we can parallelize the update process by calling the data store (an api for example) in batches.
 
**Key Idea**: Check your data supplier API and utilize it correctly with multiple threads. Just be careful with not hitting the rate limit ;).


Some food for thought :).
