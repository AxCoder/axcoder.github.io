---
layout: post
title: Perfview in Dyn365FO. Part 5 - How to analyze X++ traces in perfview.
comments: true
---
You can use PerfView for X++ traces analysis in troubleshooting scenarios.

See the [First part]({{ site.baseurl }}{% post_url 2018-11-04-Perfview-in-dyn365fo %}) for the introduction and a list of posts.

### Introduction

Dynamic 365 for Finance and Operations uses [ETW](https://docs.microsoft.com/en-us/windows/win32/etw/event-tracing-portal) technology for doing it's own traces. 

Compiler generates code that inserts an event before and after each method call. Also database engine reports all queries as ETW events.  

Traces can be [collected](https://docs.microsoft.com/en-us/dynamics365/fin-ops-core/dev-itpro/perf-test/trace-trace-tutorial) from UI or by using Trace Parser.

After that it can be opened in trace parser for analysis. Trace parser provides list of events, tree of calls and list of SQL queries. 

That works good. But in some trouble shooting scenarios it can be usefule to search across events. In that case we can use Perfview. 

## How to use PerfView 

We can open eny ETL in PerfView since it shares the same ETW technology with TraceParser. 

Unfortunately, we cannot use stack viewer for X++ methods in this trace (you can use perfview to collect [.NET trace]({{ site.baseurl }}{% post_url 2018-11-04-Perfview-in-dyn365fo %}) ) though, but we can use this tool so see event sequence.

### Opening trace

Do the following steps on a machine with Dyn365FO installed:

![Opening X++ trace](/assets/perfview-opening-xpp-trace.png) 

- Launch PerfView and navigate to a folder
- Double click "events"
- Select all event types at the left:
    - Click on any event type
    - Presss Ctrl+A
- Press F5 to refresh

### Analysing trace

![Perfview events view](/assets/perfview-events-view.png) 

1. Setup columns to display: 
   ```FormattedMessage methodName sqlStatement *``` (where * means "all other columns" )
2. Filter events by text. Unfortunately Perfview shows limited number of events (MaxRet input box). We can reduce number of viewed events by:
   - excluding venet types from the left pane
   - filtering by text (Text Filter)
   - limiting by time range (Start, End)
3. After finding a time range where happens something that we are interested in, we can remove Text Filter and set Start and End instead. 
4. We can also use text filter to filter by activity id (since TraceParser catches all the session on the same AOS)


