---
title: Traces, Spans, and Metrics
keywords: data, distributed tracing
tags: [tracing]
sidebar: doc_sidebar
permalink: trace_data_details.html
summary: Learn about the format for Wavefront spans, and naming conventions for the RED metrics derived from them.
---

A [trace](tracing_basics.html#wavefront-trace-data) shows you how a particular request propagates from one microservice to the next in a distributed application. The basic building blocks of a trace are its spans, where each span corresponds to a distinct invocation of an operation that executes as part of the request. 

Spans are the fundamental units of trace data in Wavefront. This page provides details about the Wavefront format of a span, as well as the RED metrics that Wavefront automatically derives from spans.


## Wavefront Span Format

A well-formed Wavefront span consists of fields and span tags that capture various attributes. These attributes enable Wavefront to identify and describe the span, organize it (possibly with other spans) into a trace, and display the trace according to the service and application that emitted it. Some attributes are required by the OpenTracing specification and others are required by Wavefront. 

Most use cases do not require you to know exactly how Wavefront expects a span to be formatted:
* When you instrument your application with a [Wavefront OpenTracing SDK](wavefront_sdks.html#general-purpose-sdks-for-custom-and-runtime-instrumentation) or a [framework-level SDK](wavefront_sdks.html#sdks-for-framework-instrumentation), your application emits spans that are automatically constructed by the Wavefront Tracer. (You supply some of the attributes when you instantiate the [ApplicationTags](tracing_instrumenting_frameworks.html#application-tags) object required by the SDK.)
* When you instrument your application with a [Wavefront core SDK](wavefront_sdks.html#core-sdks-for-sending-raw-data-to-wavefront), your application emits spans that are automatically constructed from raw data you pass as parameters. 
* When you instrument your application with a 3rd party distributed tracing system, your application emits spans that are automatically transformed by the [integration](tracing_integrations.html#tracing-system-integrations) you set up. 

It is, however, possible to manually construct a well-formed span and send it either directly to the Wavefront service or to a TCP port that the Wavefront proxy is listening on for trace data. You might want to do this if you instrumented your application with a proprietary distributed tracing system that Wavefront does not provide an integration for. 

### Span Syntax

```
<operationName> source=<source> <spanTags> <start_milliseconds> <duration_milliseconds>
```
Fields must be space separated and each line must be terminated with the newline character (\n or ASCII hex 0A).

For example:
```
"getAllUsers source=localhost traceId=7b3bf470-9456-11e8-9eb6-529269fb1459 spanId=0313bafe-9457-11e8-9eb6-529269fb1459 parent=2f64e538-9457-11e8-9eb6-529269fb1459 application=Wavefront service=auth http.method=GET 1533529977 343500"
```


### Span Fields

<table>
<colgroup>
<col width="20%" />
<col width="10%" />
<col width="40%" />
<col width="30%" />
</colgroup>
<thead>
<tr>
<th>Field</th>
<th>Required</th>
<th>Description</th>
<th>Format</th>
</tr>
</thead>
<tbody>
<tr>
<td markdown="span">`operationName`</td>
<td>Yes</td>
<td>The string name that indicates the operation represented by the span.</td>
<td>Valid characters: a-z, A-Z, 0-9, hyphen ("-"), underscore ("_"), dot ("."). <br> Length: less than 1024 characters.</td>
</tr>
<tr>
<td markdown="span">`source`</td>
<td>Yes</td>
<td>The string name of a host or container on which the represented operation executed.</td>
<td>Valid characters: a-z, A-Z, 0-9, hyphen ("-"), underscore ("_"), dot ("."). <br> Length: less than 1024 characters.</td>
</tr>
<tr>
<td markdown="span">`spanTags`</td>
<td>Yes</td>
<td markdown="span">See [below](#span-tags). </td>
<td></td>
</tr>
<tr>
<td markdown="span">`start_milliseconds`</td>
<td>Yes</td>
<td>Start time of the span. </td>
<td>Whole number of epoch milliseconds, which is the number of milliseconds that have elapsed since 00:00:00 Coordinated Universal Time (UTC) on January 1, 1970.</td>
</tr>
<tr>
<td markdown="span">`duration_milliseconds`</td>
<td>Yes</td>
<td>Duration of the span, in milliseconds.</td>
<td>Whole number greater than or equal to 0.</td>
</tr>
</tbody>
</table>

### Span Tags

Span tags are special tags associated with a span. Many of these span tags are required for a span to be valid. An application can be instrumented to include custom span tags as well. Custom tags should not use the reserved span tag keys listed in the following tables. 

**Note:** The maximum allowed length for a combination of a span tag key and value is 254 characters (255 including the "=" separating key and value). If the value is longer, the span is rejected.


The following table lists span tags that contain information about the span's identity and interrelationships. 

<table>
<colgroup>
<col width="20%" />
<col width="10%" />
<col width="55%" />
<col width="15%" />
</colgroup>
<thead>
<tr><th>Span Tags for Identity</th><th>Required</th><th>Description</th><th>Value</th></tr>
</thead>
<tbody>
<tr>
<td markdown="span">`traceId`</td>
<td markdown="span">Yes</td>
<td markdown="span">Unique identifier of the trace the span belongs to. All spans that belong to the same trace share a common trace ID. </td>
<td markdown="span">UUID</td>
</tr>
<tr>
<td markdown="span">`spanId`</td>
<td markdown="span">Yes</td>
<td markdown="span">Unique identifier of the span.</td>
<td markdown="span">UUID</td>
</tr>
<tr>
<td markdown="span">`parent`</td>
<td markdown="span">No</td>
<td markdown="span">Identifier of the span's dependent parent, if it has one. This tag is populated as the result of an OpenTracing `ChildOf` relationship.
A span without the `parent` or `followsFrom` tag is the root (first) span of a trace. </td>
<td markdown="span">UUID</td>
</tr>
<tr>
<td markdown="span">`followsFrom`</td>
<td markdown="span">No</td>
<td markdown="span">Identifier of the span's non-dependent parent, if it has one. This tag is populated as the result of an OpenTracing `FollowsFrom` relationship. Wavefrong ignores spans with this tag when calculating the critical path through a trace. A span without the `parent` or `followsFrom` tag is the root (first) span of a trace. </td>
<td markdown="span">UUID</td>
</tr>
</tbody>
</table>

The following table lists span tags that describe the architecture of the instrumented application that emitted the span. Wavefront uses these tags to aggregate and filter trace data at different levels of granularity. These tags correspond to the [application tags](tracing_instrumenting_frameworks.html#how-wavefront-uses-application-tags) you set through a Wavefront observability SDK. 

<table>
<colgroup>
<col width="20%" />
<col width="10%" />
<col width="55%" />
<col width="15%" />
</colgroup>
<thead>
<tr><th>Span Tags for Filtering</th><th>Required</th><th>Description</th><th>Value</th></tr>
</thead>
<tbody>
<tr>
<td markdown="span">`application`</td>
<td markdown="span">Yes</td>
<td markdown="span">Name of the instrumented application that emitted the span. </td>
<td markdown="span">String</td>
</tr>
<tr>
<td markdown="span">`service`</td>
<td markdown="span">Yes</td>
<td markdown="span">Name of the instrumented microservice that emitted the span.</td>
<td markdown="span">String</td>
</tr>
<tr>
<td markdown="span">`cluster`</td>
<td markdown="span">No</td>
<td markdown="span">Name of a group of related hosts that serves as a cluster or region in which the instrumented application runs.</td>
<td markdown="span">String</td>
</tr>
<tr>
<td markdown="span">`shard`</td>
<td markdown="span">No</td>
<td markdown="span">Name of a subgroup of hosts within the cluster.</td>
<td markdown="span">String</td>
</tr>
</tbody>
</table>

**Note:** Additional span tags may be present, depending on how you instrumented your application. For example, the [framework-level observability SDKs](wavefront_sdks#sdks-for-framework-instrumentation) associate span tags like `component`, `http.method`, and so on. You can find out about these tags in the README file for the the SDK on GitHub.

<!---
Because of operations are normally composed of other operations, each span is normally related to other spans -  a parent span and children spans.
--->



## RED Metrics Derived From Spans

<!---
Generated from spans emitted by tracing system integration or OpenTracing SDK (links). (framework-level SDKs generate 1st class metrics, correlated with spans, but not derived from them.)
Histogram distributions for D. Query with hs(), see all percentiles
Order of derivation vs. sampling and why you care. 
--->

When you instrument your application with a [tracing-system integration](tracing_integrations.html#tracing-system-integrations) or with a [Wavefront OpenTracing SDK](wavefront_sdks.html#general-purpose-sdks-for-custom-and-runtime-instrumentation), Wavefront derives RED metrics from the spans that are sent from the instrumented application. These out-of-the-box metrics are derived from your spans automatically, with no additional configuration or instrumentation on your part. You can use these metrics as context to help you discover problem traces.

RED metrics are measures of:

* Requests – the number of requests (spans) being served per second
* Errors – the number of failed requests (spans) per second
* Duration – per-minute histogram distributions of the amount of time that each request (span) takes

### Auto-Generated Charts
Wavefront automatically generates charts to display the auto-derived RED metrics and histograms. To view these charts:

1. Select **Applications > Inventory** in the Wavefront task bar. If necessary, scroll to find your application and its services.
2. Click on the service you want to see metrics for. 
3. If you instrumented your application with a Wavefront SDK, look for the charts in the **Overall** section. (If you used a tracing-system integration, the charts are in the only section on the page.)

The auto-generated charts let you view the Request Rate, Error Rate, and Duration (P95) for the service, as well as the "top" operations each category: the most frequently invoked operations, the operations with the most errors, and the slowest operations. You can click on an operation in one of these charts to view the just the traces that contain that operation. 

![tracing overall RED metrics](images/tracing_overall_RED_metrics.png)


### RED Metric Names
Wavefront constructs the names of the auto-derived RED metrics as shown in the following table. The name components `<application>`, `<service>`, and `<operationName>`) are string values that Wavefront obtains from the corresponding spans. If necessary, Wavefront modifies these strings to comply with Wavefront's [metric name format](wavefront_data_format.html#wavefront-data-format-fields).

<table>
<colgroup>
<col width="50%"/>
<col width="15%"/>
<col width="35%"/>
</colgroup>
<thead>
<tr><th>Metric Name</th><th>Metric Type</th><th>Description</th></tr>
</thead>
<tbody>
<tr>
<td markdown="span">`tracing.derived.<application>.<service>.<operationName>.invocation.count`  </td>
<td markdown="span">Counter</td>
<td markdown="span">The number of times that the operation is invoked. Used in the Request Rate chart. </td>
</tr>
<tr>
<td markdown="span">`tracing.derived.<application>.<service>.<operationName>.error.count`   </td>
<td markdown="span">Counter</td>
<td markdown="span">The number of invocations that are errors (i.e., spans with `error=true`). Used in the Error Rate chart. </td>
</tr>
<tr>
<td markdown="span">`tracing.derived.<application>.<service>.<operationName>.duration.micros.m`  </td>
<td markdown="span">Wavefront histogram</td>
<td markdown="span">The duration of each operation invocation, in microseconds, aggregated in one-minute intervals. Used in the Duration chart. </td>
</tr>
</tbody>
</table>


Wavefront associates each auto-derived RED metric with point tags `application`, `service`, and `operationName`. Wavefront assigns the corresponding span values to these point tags. The span values are assigned without being modified. 

Knowing the names of the auto-derived RED metrics lets you query and visualize these metrics just as you would any other metrics in Wavefront. For example, you can use the Duration metric in a [histogram query](proxies_histograms.html#querying-histogram-metrics) to obtain percentiles other than the one (P95) displayed in the auto-generated chart.

**Note:** Because the point tags preserve exact span values (and metric names might not), we recommend that you query for the auto-derived RED metrics using the point tags instead of metric names. 

### Trace Sampling and Auto-Derived RED Metrics

If you have instrumented your application with a Wavefront observability SDK, Wavefront always derives the RED metrics _before_ any sampling is performed. This is true regardless of whether the sampling is performed by the SDK or a Wavefront proxy. Consequently, Wavefront derives the RED metrics from a complete set of generated spans, so the metrics provide a highly accurate picture of your application's behavior. However, if you click through an auto-generated chart to inspect a particular trace, you might discover that the trace has not actually been ingested in Wavefront. You can consider configuring a less restrictive [sampling strategy](trace_data_sampling.html).

If you have instrumented your application using a 3rd party distributed tracing system, Wavefront derives the RED metrics _after_ sampling has occurred. The Wavefront proxy receives only a subset of the generated spans, and the auto-derived RED metrics will reflect just that subset. See [Trace Sampling and RED Metrics from an Integration](tracing_integrations#trace-sampling-and-red-metrics-from-an-integration).


<!---
<table>
<colgroup>
<col width="18%"/>
<col width="50%"/>
<col width="32%"/>
</colgroup>
<thead>
<tr><th>Menu</th><th>Description</th><th>Example</th></tr>
</thead>
<tbody>
<tr>
<td markdown="span"> </td>
<td markdown="span"> </td>
<td markdown="span"> </td>
</tr>
</tbody>
</table>


--->