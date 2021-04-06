---
title: Investigating Slow Queries with the explain() Function (Beta)
keywords: query language, explain function
tags: [query language]
sidebar: doc_sidebar
published: false
permalink: query_language_explain_function.html
summary: How to investigate slow or large queries with the explain() function.
---

## Summary

```
explain(<tsExpression>)

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
</tbody>
</table>
 
## Description

If you have slow or large queries, use the `explain()` function to capture the query trace ID. You can then navigate to **Applications** > **Traces**, search for the trace ID, see how long it takes for the query to run, and investigate further.

In this Beta version, the `explain()` function is disabled by default. 

## How to Use the `explain()` Function


1. The following sample query returns the CPU usage percentage for the production environment.

    ```
    ts(~sample.cpu.usage.percentage, env=production) 
    ```

2. If the query is running slowly, apply the `explain ()` function.

    ```
    explain(ts(~sample.cpu.usage.percentage,  env=production))
    ```

    You can see the resulting trace ID under the Query Editor.
    
    ![Explain function result showing a trace ID](images/explain_function.png)

3. Copy the trace ID and navigate to **Applications** > **Traces**. 
4. Click **Trace ID** in the top left and paste the trace ID.
5. Click **Search**.
6. If you see no result within a short period of time, change the time window repeatedly. One to two minutes is the expected time for seeing the result as there's an ingestion delay. Refreshing the browser page or clicking the **Search** button multiple times doesn't work because it may show information that's in your browser cache.

Once the query trace is loaded, you can see the query critical path. It shows how long the query compiling and planning takes, as well as how long it takes for the query to run. 

Span details for each call include:

* Application tags. These are the application, service, cluster, and shard, as selected by the trace query.
* Other tags, including the trace ID. Expand the **Tags** section for the query parent call to see more information about the query itself and investigate further. For example, you can see information, such as:
  * The query start time and end time
  * The points scanned
  * The CPU seconds the call takes
* A clickable link to the corresponding Operation Dashboard that lets you examine the RED metrics associated with the call.

For details about trace details and critical paths, see [Traces Browser](tracing_traces_browser.html).
