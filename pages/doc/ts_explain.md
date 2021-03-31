---
title: Investigating Slow Queries with the explain() Function (Beta)
keywords: query language, explain function
tags: [query language]
sidebar: doc_sidebar
permalink: query_language_explain_function.html
summary: How to investigate slow or large queries with the explain() function.
---

## Summary

```
explain(<tsExpression>[,metrics|sources|sourceTags|pointTags|<pointTagKey>])

```
Returns a trace ID that allows you to search for the query traces, so that you can see how long it takes for the query with the respective trace ID to run.

## Parameters

<table>
<tbody>
<thead>
<tr><th width="20%">Parameter</th><th width="80%">Description</th></tr>
</thead>
<tr>
<td markdown="span"> [tsExpression](query_language_reference.html#query-expressions)</td>
<td>Expression that describes the time series for which you want to obtain the trace ID. </td>
</tr>
<tr><td>metrics&vert;sources&vert;sourceTags&vert;pointTags&vert;&lt;pointTagKey&gt;</td>
<td>Optional 'group by' parameter for organizing the time series into subgroups and then returning a count for each subgroup.
Use one or more parameters to group by metric names, source names, source tag names, point tag names, values for a particular point tag key, or any combination of these items. Specify point tag keys by name.</td>
</tr>
</tbody>
</table>
 
## Description

If you have slow or large queries, use the `explain()` function to capture the query trace ID. You can then navigate to **Applications** > **Traces**, search for the trace ID, and see how long it takes for the query to run.

In this Beta version, the `explain()` function is disabled by default. 

## How to Use the `explain()` Function

Let's consider that you have the Telegraf integration set up and running.

1. The following query returns the metrics for Telegraf CPU usage per guest.

    ```
    ts("telegraf.cpu.usage.guest")
    ```

2. If the query is running slowly, apply the `explain ()` function.

    ```
    explain(ts("telegraf.cpu.usage.guest"))
    ```

    You can see the resulting trace ID under the Query Editor.

3. Copy the trace ID and navigate to **Applications** > **Traces**. 
4. Click **Trace ID** in the top left and paste the trace ID.
5. Click **Search**.
6. If you see no result within a short period of time, change the time window repeatedly. Refreshing the browser page or clicking the **Search** button multiple times doesn't work because it may show information that's in your browser cache.

Once the query trace is loaded, you can see the query critical path. It shows how long the query compiling and planning takes, as well as how long it takes for the query to run. 

Span details for each call include:

* Application tags. These are the application, service, cluster, and shard, as selected by the trace query.
* Other tags, including the trace ID. Expand the **Tags** section to see more information about the query itself and investigate further. You can see the query itself, the user, start time and end time, overall time of the call (execution, compiling and planning, or both), the points scanned, and so on.
* A clickable link to the corresponding Operation Dashboard that lets you examine the RED metrics associated with the call.

For details about trace details and critical paths, see [Traces Browser](tracing_traces_browser.html).
