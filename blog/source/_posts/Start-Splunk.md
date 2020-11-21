---
title: Start-Splunk
catalog: true
date: 2020-11-20 21:27:58
subtitle:
header-img:
tags:
- Jenkins
- DevOps
categories:
- From-Previous-Blog-General
---

# Start Splunk

## 1. Overview

``` lang=html
What is Splunk?
It is a software for searching, monitoring, and analyzing machine-generated 
big data via a Web-style interface.
```

Our product is getting closer to production stage. In this post, we explore how Splunk provides logging, monitoring, alerting and analyzing info.

---

## 2. Learn Basics

Splunk has lots of features. You will miss a lot if you don't follow tutorials.
I recommend `Free Fundamentals 1`, a course that is free and covers lots of features.

![Overview](1-overview.png)

Refer [here](https://www.splunk.com/pdfs/training/splunk-fundamentals-1.pdf) for the full pdf.

## 3. Practice Local

You can try with a free version.
Even if you have an enterprise license by your company, try installing locally because you can explore features only admin can do.

The tutorial helps you setup local Splunk.
Each module of the tutorial consists of three parts; lecture video, try local machine, and quiz.
7.X fundamentals course has 14 modules and following all of them might be boring but is certainly beneficial looking back.
Splunk will be a go-to place for any issues for your project, so spending time learning it is not a waste.

![Search](2-Search.png)

![Certificate](3-Certificate.png)

## 4. Search

Everything starts from a search command.

For example, if you want to setup an email alert for 5xx response in your application, you start with a search query. The query will search response code that's returned to the client.

Setup an alert based on the count of the query if the count is more than 1 for the past five minutes then send an alert.

In the search query, you need to

- Specify index
- Use fields
- Specify what you want to do with the result (table, rename, sort)
- Save these results using (dashboard, report, alert)

``` lang=html
index=*edc2prod | search cf_space_name=app-prod source_type=RTR response_code=500
```

---

## 5. Dashboard

![Dashboard](4-Dashboard.png)

Dashboard also starts from a search query.
For example, the total API calls in the dashboard relies on the search query like this;

``` lang=html
index=*edc2prod | search cf_space_name=app-prod | rex "Receiving\sa\ssynchronous\sscheduleRequest\sfor\stenantId:\s(?<TenantId2>\w{8}-\w{4}-\w{4}-\w{4}-\w{12})\s-\s"| stats count sum(TenantId2)
```

This extracts field `TenantId2` whose format is a GUID. The search text `Receiving...` is a log generated only once for every client call.

Similar way, you can create a report out of a search query.

## 6. Alert

Settings -> alerts -> New Alert


``` lang=html
index=*edc2prod | search cf_space_name=app-prod 
source_type=RTR("POST /yourEndpoint HTTP/1.1") (response_code="401") OR (response_code="404") OR (response_code="424")
```

![Alert1](5-Alert1.png)

![Alert2](5-Alert2.png)

Our company doesn't support `real-time` alert because it consumes too many resources.
As a work-around, I run alert-query every 5 minutes and get a noficiation for the past 5 minute's violation calls.

---

## 7. Tip

Our company's Splunk team masks some private data such as location coordinates.
With the masking, we can't really use Splunk for logging and analyzing purposes.
For this, we've implemented our custom logging framework using mongoDB.

Despite the masking, we are happy with what Splunk provides us especially the alerting.
Splunk is a really powerful tool that gives engineers more freedom to implement anything they want.
