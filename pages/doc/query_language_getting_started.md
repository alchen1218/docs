---
title: Query Language Quickstart
keywords: query language
tags: [query language, getting started, videos]
sidebar: doc_sidebar
permalink: query_language_getting_started.html
summary: Watch some videos, run a basic query, and apply filters and functions.
---
The Wavefront Query Language has been designed especially for time series data. Time series data is unique and requires a query language that accommodates the periodicity, potential irregularity, and streaming nature of the data.

Watch these videos to get you started:

<table style="width: 100%;">
<tbody>
<tr><td width="50%"><a href="https://vmwarelearningzone.vmware.com/oltpublish/site/openlearn.do?dispatch=previewLesson&id=60b992dc-dc7a-11e7-a6ac-0cc47a352510&inner=true&player2=true"><img src="/images/v_ql_intro.png" alt="introduction to query language"/></a></td>
<td width="50%"><a href="https://vmwarelearningzone.vmware.com/oltpublish/site/openlearn.do?dispatch=previewLesson&id=61f9391c-dc7a-11e7-a6ac-0cc47a352510&inner=true&player2=true"><img src="/images/v_ql_basics.png"/></a></td></tr>
</tbody>
</table>

## Basic Query

The simplest and most commonly used type of query retrieves an individual metric: `ts(<metricName>)`. If you are new to Wavefront, you might, for example, want to measure the time spent in the CPU idle task&mdash;`cpu.idle`&mdash;across all sources.
For this example you enter `ts(cpu.idle)` into a query field to produce the chart below:

![base query](images/base_query.png)


## Filtering by Source

The example chart displays many lines, particularly below 8M. To simplify the chart, you can filter by source using the optional `source=<sourceName>` parameter: `ts(<metricName>, source=<sourceName>)`. For example, use a `source="m*"` filter to show all sources that start with "m". The number of lines is reduced and the Y-axis scale changes from 30M to 5M:

![filtered query](images/filtered.png)

## Applying Aggregation Functions

For further exploration try one of the aggregation functions. Aggregation functions are the second most common query.  For example, use `avg()` to show the average value of the `cpu.idle` metric across all sources.  Or use `sum()` to get a total for all sources starting with "m". Here's the chart adding `sum()`:

![summed query](images/summed.png)

## Applying Mathematical Functions

Notice how the result of `sum(ts(cpu.idle))` is slowly increasing over time.  To increase your proficiency in the language and to learn the power of the other functions, you can take a metric example and experiment with the different functions. For example, we see that `sum(ts(cpu.idle))` gives us snapshots of the sum over time, but does not provide information such as how fast the sum is increasing. The query language has a `deriv()` function that shows the rate of change per second: `deriv(sum(ts(cpu.idle))`.

![summed rate query](images/deriv_sum.png)

## Next Steps

What's next depends on the type of data you're interested in, and how you want to interact with your data.

### Query Types for Different Data

Most Wavefront users query for metrics, but we support interacting with other data.

Charts for metrics also support the following types of queries:
* Query Wavefront events with [`events()` queries](query_language_reference.html#event-functions).
* Query histograms with [`hs() queries`](proxies_histograms.html#querying-histogram-metrics)

A separate set of interfaces is available for developers who are interested in traces and spans.

### Docs & Wizards

Wavefront documentation includes tutorials, reference, and guides on the query language.

- **[Query builder](query_language_query_builder.html)** and **[query wizard](query_language_query_wizard.html)** can help you come up to speed quickly while using the product.
- Log in to Wavefront and learn with our Tutorial and  Tour. The Tutorial includes an Interactive Query Language Explorer that shows examples for each function.
- [Wavefront Query Language Quick Reference](query_language_reference.html). gives an overview of the different types of functions that can be used in a query. Each function names is a link to a reference page for the function.
- For in-depth discussions and examples, see some of the [advanced function guides](label_query%20language.html).
